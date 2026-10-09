# Round snapshot audit v1

日期：2026-09-27

## 要回答的问题

Task36 缓存里的 `round1/round2` 能不能被写成“工具随时间更新后的两个版本”，从而直接支撑动态 JEV benchmark？

## 证据

- 缓存有 730 个分子、33 个具体 provider。32 个 provider 有第二轮，`admetlab_3_0` 没有。
- 第二轮的差异集中在 B3clf：24 个 B3clf provider 有 raw value 变化，16 个 provider 的离散 label 也变化；没有 provider 的 status 发生变化。
- `data/tool_results/manifest.json`（built_on `2026-09-11`）明确写的是：ADMET-AI 两遍逐字节一致，B3clf 因 PaDEL 概率抖动而跑两遍留证。这里没有记录 checkpoint、源码、部署版本或模型更新时间。
- 权威 BBB census 的 `replay1/replay2` 有完整 receipt。四个模型两轮使用相同输入 hash、checkpoint/config/source 证据和 output hash；四个模型的概率漂移均为 0。两轮运行时间只相差约 40 秒，是重复回放。
- Task36 缓存的多数 provider 只有表文件，没有每轮执行 receipt。文件中出现两个值，不能单独证明是两个时间点的部署。

完整机器结果见：

`experiments/logs/task49_round_snapshot_audit_20260927/results_v1.json`

生成入口：

`src/probe_headless/audit_pharma_snapshot_rounds.py`

## 裁定

当前证据不支持把 `round2` 叫作“后来的工具版本”，也不支持用它证明 live tool evolution。最稳妥的解释是：这是重复运行产生的输出漂移/执行重复性材料，其中 B3clf 的漂移有明确的 PaDEL 运行抖动说明。

这批数据仍然有用，但用途要收窄：

1. 做 provider 输出重复性、数值漂移和标签稳定性的压力测试；
2. 检查 JEV 在 provider 输出轻微变化时是否保持选择稳定；
3. 作为动态方法的 robustness extension，而不是动态 benchmark 的时间轴。

## 真正的动态 benchmark 还缺什么

需要为至少一个 provider 保存两次有明确版本边界的运行凭据：执行时间、输入 hash、源码/权重 hash、参数和部署或更新事件。随后预先固定时间顺序和 train/evaluation 切分，不能看完正确率后再挑 round。若拿不到这类证据，论文应把主线写成“selected-only outcome feedback 下的在线适应”，不要声称工具本身在 benchmark 内发生了版本演化。

