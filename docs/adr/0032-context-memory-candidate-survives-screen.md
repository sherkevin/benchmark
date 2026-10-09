# ADR-0032：context residual memory 进入下一轮验证

- 状态：Accepted
- 日期：2026-09-27
- 依据：Task49 context residual memory replay v1

## 背景

普通 selected-only 更新和 provider-transfer 都没有形成稳定主方法。信息筛选显示 Morgan/hidden 表示上的局部 residual 有信号，因此测试了一个不传播到未选 provider 的有界 context memory。

## 结果

在同一冻结 BT prior、hidden feature、5 folds、2 seeds、delay `0/1/4` 回放中：

- Frozen：`0.8914/0.8914/0.8914`；
- Independent bias：`0.8948/0.8948/0.8927`；
- Context memory：`0.8927/0.8969/0.8976`；
- Random memory：`0.8920/0.8948/0.8907`。

真实 top-5 相似 memory 在 delay 1/4 超过 independent 和 random memory，但 delay 0 仍低于 independent。

## 决策

1. context residual memory 进入下一轮统一 runner 验证，暂定为动态 JEV 候选，不称最终方法。
2. provider-transfer 主线按 ADR-0031 停止；其负结果保留为消融和方法边界。
3. 下一轮先做 replayable smoke，再做完整 development；必须同时保留 Best Fixed、Frozen、Online-SGD、Replay-SGD、BayesianRouter-style 和 Random memory。
4. 若正式 runner 中两个以上 delay 低于 independent，停止 memory 线；不得靠调容量或 kernel 追分。

证据：`task/task49-pharma-benchmark-baseline-freeze/context_memory_replay_v1/report.md`、`experiments/logs/task49_context_memory_replay_20260927/summary_v2.json`。
