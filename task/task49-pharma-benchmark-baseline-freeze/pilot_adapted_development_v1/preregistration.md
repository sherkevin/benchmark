# PILOT-adapted development replay v1

日期：2026-09-27

## 问题

在同一 33-provider BBB development 任务上，fit-only PILOT-adapted preference prior 加 selected-only cosine/UCB/ridge 更新，是否比完全冻结的同一 prior 改善后续选择？这是一项 baseline qualification，不是 Proposed 方法实验。

## 固定协议

- 输入：`fingerprint_diagnostic_v1/inputs.json`，5 个 scaffold-disjoint outer folds；每折 fit 只用于两阶段 prior，evaluation 只按原顺序流入。
- fit seeds：`20260927`、`20260928`；projection/provider epochs=100，projection 64 维，triplet margin=0.2，AdamW lr=0.001、weight decay=0.01，provider L2=0.01。
- 在线：已有 `PilotOnlineKernel`，cosine mean + UCB，`alpha=0.5`；ridge precision 使用 fit provider accuracy 的倒数；delay 为 `0/1/4`。Frozen 和 Online 共享完全相同的 prior、features、流和 alpha，Frozen 只把 `learn=False`。
- evaluator 只在 action 后生成所选 provider 的 cached `system_success`，到期才释放；未选输出不进入 policy。失败按 0，unknown 不补标签。
- 对照只报告同一流的 Best Fixed 和 evaluator Oracle 作为控制/上界；不把它们当作 online baseline。

## 指标与停止线

按 delay 汇总 molecule accuracy、累计错误、每流准确率、feedback update 次数、选择和更新时间。若 Online 相对 Frozen 在至少两个 delay 没有正均值，停止把 PILOT prior + 普通在线更新作为当前任务的有效机制；即使超过 Frozen 但仍低于 Best Fixed，也只能保留为 baseline 结果。禁止看到结果后调 alpha、epoch、precision 或流顺序。

## 证据边界

这是 CPU development replay，没有新工具调用，不打开 confirmation。保存逐事件 JSONL、输入/source hash、每折每 seed prior、配置和独立恢复摘要；若恢复失败，不使用任何汇总结果。
