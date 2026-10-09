# MIRA-Relay 语义合同审计（2026-10-02）

状态：实现审计记录；不是最终效果结果，也不替代双方确认的决议。

## 审计范围

逐项对照方法正文、Figure 1/2、`MIRARelayPolicy` 和
`TerminalFeedbackQueue`，检查“何时可见、谁能更新、失败如何计账”是否一致。

## 已修正

1. `TerminalFeedbackQueue.complete` 在 provider runtime failure 时原来把状态写成
   `error`，而 `MIRARelayPolicy.bind` 合同只接受 `failure`。这会让失败事件在 release
   之前抛异常。现已改成 `failure`，并加入失败路径回放单测。
2. Figure 1 v3 明确分开 runtime verifier 和 frozen endpoint judge，写出
   `fallback: ledger only`，并把反馈循环改成 release 后的“next visible decision”。
3. Figure 2 v7 去掉 v6 中可能让人误读的 `chemical | valid`，改成 provider output 与
   frozen endpoint judge；保留 selected-only、queue 和 release 边界。
4. 正文和 MIRA-Relay 预登记明确：challenger release 才更新 family residual/provider
   bias；fallback release 不更新 residual mean/variance 或 provider bias，但可以更新
   runtime/availability 与诊断正确计数，事件记录 `residual_update=false`。
5. unknown/refusal 不生成 correctness residual 的 0/1；成功返回的 unknown/refusal
   可以更新 runtime/availability 诊断账本。

## 当前合同

- selector 在 action 前只读 molecule、endpoint、menu 和已经 release 的 history。
- runtime verifier 只读 selected provider 的 output/status；frozen endpoint judge 在
  termination 后产生 chemical correctness。
- runtime failure 的 `system_success=0` 进入 challenger residual（fallback 除外）和
  hazard ledger；`chemical_correct=null`。
- valid return 的 scored event 才写 `n_scored/n_correct` 并更新 challenger residual；
  fallback 的 scored event只更新诊断计数和可用性账本。
- queue due tick 为 `decision_tick + 1 + delay`；release 发生在下一次可见决策之前。
- 未选 provider 的输出和 correctness 永远不进入 policy state。

## 仍需在 full replay 前验证

1. 独立 verifier 要重算 fit residual、Fisher precision、state hash、safe menu、完整
   propensity、hazard gate 和 delay release。
2. queue 当前按 `valid_return` 将反馈标记为 `scored`，unknown/refusal 的专门 replay
   路径尚未接入；在 full replay 前要用一个显式 unknown fixture 验证“无 correctness 更新、
   仍保留 runtime 诊断”的合同。
3. fit-only warm start 采用 summed Bernoulli NLL；每个 fold 的输入 hash、优化收据和
   source commit 必须写入 receipt。

本审计不读取 evaluation 结果，也不声称 MIRA-Relay 已有收益。
