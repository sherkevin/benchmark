# Queue boundary bug 对既有 MIRA 结果的影响审计

日期：2026-09-30  
范围：只读代码审查与一个最小合成回归；不修改 runner、queue 或 policy，不重跑全量实验。

## 结论先行

1. **旧的 360-stream MIRA relative-reliability CPU effect（run_v1）不受这个 bug 影响，不需要因为这个 bug 重跑。** 该实验没有调用 `pharma_selection_stream.TerminalFeedbackQueue`，而是走 `run_structured_cpu_effect_v1.run_stream` 内部的 `DelayedFeedback`；它在绑定时传入 evaluator 的原始输出 label。MIRA 的 release 也只读取 `chemical_correct` 和 `feedback_status`，从不读取 `pending.label`。
2. **MIRA raw contract v5 的 pinned commit 确实包含过 queue bug，但该 bug 不改变 MIRA 的 action、计数器、阻断状态、版本或 state hash。** v5 (`bd525311...`) 的 `TerminalFeedbackQueue.complete` 把 evaluator 推导的 `record['recovered_label']` 写入 pending；MIRA release 只用 `chemical_correct`，所以 v5 的 MIRA 行为证据不应被整体撤销。不过 v5 的内部绑定语义不正确，不能作为“raw label binding 已验证”的证据；后续 contract 应使用修复后的 queue（`445611a6...`）。
3. **StructuredPolicy 对这个 bug 敏感。** 它在 release 中读取 `pending.label` 并用它构造学习目标；若把 recovered label 当作 pending label，错误样本会被再次取反，更新方向会错。旧的 structured effect runner 自己使用 `DelayedFeedback` 并绑定 raw label，因此旧结果本身不受影响；将来任何通过旧 `TerminalFeedbackQueue` 跑 StructuredPolicy 的结果都必须重跑或先做针对性回归。
4. **MatchedOnlineSGD 和 JointResidual/MIRA 系列不读取 `pending.label`，因此 action/state 不受该绑定 bug 影响。** 通过旧 queue 产生的 raw receipt 仍有 contract 语义瑕疵，但无需因 policy effect 改变而重跑；后续 receipt 应使用修复后的 queue。

这一区分只针对“绑定的 label 是否被 policy 用来更新状态”。它不把旧 raw receipt 的实现缺陷抹掉，也不把 contract smoke 当成 effectiveness 结果。

## Bug 的精确定义与修复

在 MIRA contract v5 固定的 queue 源（commit `bd525311afcaff7aee9d1e7fbb2fade7cd13b028`，`pharma_selection_stream.py` SHA256 `305e732cc8d7afb0cc8a0c0b711dd44ea13f92b5e3b931c213d26a469fb09514`）中，`TerminalFeedbackQueue.complete` 第 49--56 行调用：

```python
self.policy.bind(event_id, selection.provider_id,
                 'ok' if record['valid_return'] else 'error',
                 record['recovered_label'])
```

`recovered_label` 由 evaluator 的 correctness 推导，已经含有未来反馈。它不能作为决策时绑定的 selected raw output label。修复提交 `445611a6b69a8599ca810253595e0033dcf79285`（当前源 SHA256 `0bc0ae1077823b0364b0f92632cbafcadf696f28c2b31099f7c79c2d7baa2d5b`）先保存 transcript，再在 valid return 时取 `trace['history'][0]['label']` 绑定，并把 recovered label 留在 evaluator-only diagnostic 中（当前 `pharma_selection_stream.py:44-64`）。

## 旧 MIRA full effect：没有走坏的边界

旧实验的配置是：

- `experiments/logs/task49_mira_relative_reliability_cpu_20260930/run_v1/config.json`
- input SHA256：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`
- 事件 SHA256：`8ceae41fa4ba444e03014bc94123becf8790f288ede3c8e2fb855a070000517f`
- 360 streams，5 folds × 2 seeds × 2 scenarios × 3 delays × 6 methods；状态验证已通过。

调用链是：

```text
run_mira_relative_reliability_cpu_v1.py:8
  -> run_mira_veto_ablation_cpu_v1.run (lines 139-142)
  -> run_structured_cpu_effect_v1.run_stream
  -> local DelayedFeedback (lines 515-555)
```

`run_mira_relative_reliability_cpu_v1.py:8` 只导入 `run_mira_veto_ablation_cpu_v1.run`；`run_mira_veto_ablation_cpu_v1.py:15-18` 导入的是 `run_structured_cpu_effect_v1.run_stream`，没有导入 `pharma_selection_stream`。`run_structured_cpu_effect_v1.py:610` 创建自己的 `DelayedFeedback`；在 `:640-658` 先从 evaluator 取 `selected['label']`，然后执行 `policy.bind(..., label)`，再排队真实 `chemical_correct`。因此 old full effect 的 pending label 是 raw label，不是 recovered label。

同时，MIRA 的 release 代码（`mira_safeswitch.py:97-140`）只检查 `pending.status`、`chemical_correct`、`feedback_status`，更新 `n_valid`、`n_fail`、runtime failure streak、blocked 和 version；`pending.label` 不在计算路径中。relative-reliability 只覆盖 `_score` 和 state payload（`mira_relative_reliability.py:53-93`），继承同一 release。

所以，这个 queue bug 不能解释或污染 run_v1 的 relative-vs-matched effect；没有证据支持因它重跑 360 streams。

## MIRA contract v5：receipt 有缺陷，但 effect state 没有被改写

v5 runner 明确调用 `run_bbb_stream`（`run_mira_contract_raw_v1.py:21-25,216-231`），而 `run_bbb_stream` 在 `pharma_selection_stream.py:151-170` 创建 `TerminalFeedbackQueue` 并在 terminal 后调用 `queue.complete`。v5 的 config 固定了上面的 `bd525311...` queue，因此它确实落在旧绑定实现上。

但是 MIRA policy 的可变 state hash 只包含 `weight`、`n_valid`、`n_fail`、`runtime_failures`、`failure_streak`、`blocked`、`block_reasons` 和 `version`（`run_mira_contract_raw_v1.py:47-65`）；pending label 不在 hash 中。release 的更新参数是 queue 保存的 `chemical_correct` 和 `feedback_status`（旧 queue `:88-91`），不是 pending label。于是：

- action 选择、延迟时序、`n_valid/n_fail`、reliability factor、availability block、version 和 state hashes 不因 raw/recovered 绑定差异而改变；
- v5 的独立 verifier (`verify_mira_contract_raw_v1.py:109-145`) 检查的是 event identity、feedback timing、MIRA state/hash 与 selected-only schema，也没有检查 pending label 的语义；因此 v5 “verified” 只能说明 receipt lifecycle 通过，不能说明 label binding 正确；
- v5 terminal 中的 `selected_output` 仍来自 replay transcript，queue diagnostic 的 `recovered_label` 仍是 evaluator 诊断字段。该事实不等同于 policy 读到了它。

建议保留 v5 作为 **adapter correctness / lifecycle receipt**，在审计中标注 pinned source 含已修复的 binding defect；不要把它当作 raw-label binding 的单独证明，也不要把它当作 scientific effectiveness evidence（v5 自己的 scope 已写明 `effectiveness_estimate: None`）。后续 receipt 必须 pin `445611a6` 或更晚的修复源，并增加 pending-label regression。

## StructuredPolicy 与 Matched 的分界

### StructuredPolicy：会受影响（若使用旧 TerminalFeedbackQueue）

`run_structured_cpu_effect_v1.py:437-457` 读取 `z = pending.label`，并计算 `y = z if chemical_correct else 1-z`，再用 `y` 更新 `theta` 和 `phi`。例如 raw label=1、实际 `chemical_correct=0` 时，正确目标应为 `y=0`；旧 queue 先将 recovered label=0 绑定进去，release 再计算 `y=1`，目标被翻回错误值。当前旧 structured effect 通过自己的 `DelayedFeedback`，其 `run_stream:640-658` 绑定 raw `label`，所以该历史 effect 不受影响。

### MatchedOnlineSGD：不会受 pending.label 影响

`run_structured_cpu_effect_v1.py:474-489` 的 `MatchedOnlineSGDPolicy` 更新只使用 `chemical_correct`（失败使用 0）和 pending feature/arm，不读取 `pending.label`。`run_matched_sgd_unified_raw_v1.py:24,278-280` 虽走 `run_bbb_stream`，但旧 queue 的 label 绑定不会改变 matched 的 weight、version 或 action。raw receipt 仍应在新源上重做，理由是 contract 语义修复，而不是结果 effect 被污染。

### JointResidual 与 MIRA：不会受 pending.label 影响

`run_joint_success_cpu_effect_v0.py:113-117` 的 `_release_scored/_release_failure` 也只用 correctness target；MIRA safe-switch/relative/availability 继承同一类 release。它们均不读取 `pending.label`。

## 最小合成回归（只作边界测试，不是效果实验）

在不修改代码的临时 Python 片段中，对同一个错误样本分别将 raw label=1 与 recovered label=0 绑定，再释放 `chemical_correct=0`：

```text
MIRA raw=1 vs recovered=0: n_valid=[1], n_fail=[1], blocked=[False], version=1（完全相同）
Structured raw=1 vs recovered=0: theta 最大差 0.05，phi 最大差 0.025（不同）
Matched raw=1 vs recovered=0: weight 与 version 完全相同
```

这只是可执行的边界回归，验证上述代码路径；没有新增样本、没有估计效果、没有改变任何实验结论。

## 决定与后续动作

- **不重跑**旧 relative-reliability CPU full effect，也不撤销 relative-vs-matched 的已有估计。
- **保留并限定**MIRA contract v5：它是 lifecycle/selected-only receipt，pinned source 有 queue binding defect；结果不用于 effectiveness 或 raw-label binding claim。
- **后续所有 queue-based receipt**统一 pin `445611a6` 或更晚版本，并加入一个错误样本的 raw-label binding regression；StructuredPolicy 在 queue 上的任何正式实验在该回归通过前不得解释。
- 若未来发现某个策略的 release 读取 `pending.label`，按 StructuredPolicy 规则处理：旧 queue 运行需单独重跑，不能套用本审计对 MIRA/Matched 的结论。
