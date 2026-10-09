# ADR-0031：停止 provider-transfer 主线

- 状态：Accepted
- 日期：2026-09-27
- 依据：Task49 gated provider-transfer replay v3

## 背景

fit-only provider residual screen 显示了稳定的 pairwise transfer AUC，但低秩共享状态 v1、scalar provider bias v2 和带 surprise/margin gate 的 v3 都在真实冻结 BT 回放中没有超过 independent selected-only baseline。

## 结果

- 低秩 v1：`0.8893/0.8893/0.8886`（delay 0/1/4），低于 Frozen。
- scalar v2：`0.8955/0.8941/0.8927`，三个 delay 都低于 independent `0.8983/0.8948/0.8989`。
- gated v3：`0.8941/0.8948/0.8934`，三个 delay 中两个低于 independent。

## 决策

1. 停止 provider-transfer 作为主方法继续扩展；不再搜索它的学习率、图阈值、clip 或 gate。
2. 将这些 replay 保存为“信息存在但简单传播失败”的负证据，方法比较中保留必要消融。
3. 下一候选改为有界 context residual memory：反馈只进入对应 selected provider 的局部记忆，未来相似请求读取局部残差。该候选必须重新写合同并先做零机时回放。

证据：`task/task49-pharma-benchmark-baseline-freeze/provider_transfer_replay_v3/report.md`、`experiments/logs/task49_provider_transfer_replay_20260927/v3_results.json`。
