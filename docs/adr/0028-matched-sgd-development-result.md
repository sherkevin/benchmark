# ADR-0028：普通 selected-only 更新不作为动态 JEV 贡献

- 状态：Accepted
- 日期：2026-09-27
- 依据：Task49 matched SGD full development v1

## 背景

我们在 722 个 BBB development 分子上，用同一 scaffold 划分、同一 Morgan BT representation、同一 `score.weight` warm start 和 selected-only delayed feedback，比较了 Frozen、Online-SGD 和 Replay-SGD。实验使用 5 folds、2 seeds、3 delays，完整日志已下载并由独立 NumPy 回放恢复。

## 结果

Pooled accuracy 为：Frozen `0.8913`，Online-SGD `0.8991`，Replay-SGD `0.8964`。在线更新相对 Frozen 有小幅提升，但 Best Fixed（只用 fit 分区选择一个 provider）为 `0.9155`；Online-SGD 低 `0.0164`，Replay-SGD 低 `0.0192`。30 条 paired stream 中，Online-SGD 正向 13、持平 7、负向 10；Replay-SGD 正向 12、持平 7、负向 11。

## 决策

1. 不把普通 Online-SGD、Replay-SGD 或 Morgan BT head 写成本文创新。
2. 不在这组 baseline 上看 evaluation 结果调学习率、buffer 或初始化。
3. 当前结果只支持“普通 selected-only 更新能改善一个较弱的 Frozen warm start”，不支持“动态更新已经优于固定选择”。
4. 下一阶段必须寻找真正的信息或表示优势：能从 selected-only outcome 中识别 provider correctness 的表示/目标，或一个经过理论说明的动态 JEV；confirmation 继续关闭。

## 后果

这条结果收窄了故事线，但没有关闭项目。后续候选方法必须在同一反馈合同下先通过可证伪的小实验，并和 Best Fixed、Frozen、Online-SGD、Replay-SGD 同时比较。若新方法仍不能超过 Best Fixed，应停止把实时 head 更新作为主贡献。

证据：`experiments/logs/task49_matched_sgd_development_20260927/report.md`、`recovered_v2/summary.json`、`analysis_v1.json`。
