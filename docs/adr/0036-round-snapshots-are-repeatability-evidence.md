# ADR-0036：round1/round2 只能作为重复性证据

- 状态：Accepted
- 日期：2026-09-27
- 依据：`task/task49-pharma-benchmark-baseline-freeze/round_snapshot_audit_v1/report.md`

## 背景

Task36 缓存为不少 provider 保留了 `round1/round2`。B3clf 的两轮确实出现 raw value 和部分 label 变化，这看起来像动态工具的候选素材。另一方面，动态 JEV 的论文叙事要求我们能证明两个 round 代表不同时间或不同部署版本，而不是同一模型的重复执行。

## 证据

1. 730 个分子、33 个 provider 中，32 个有第二轮，1 个没有。
2. 差异集中在 B3clf；manifest 明确把两轮解释为 PaDEL 概率抖动的留证。
3. 权威 census 的四个模型有完整 replay receipt。两轮输入、checkpoint、config、source 和 output hash 都相同，概率漂移为 0；运行只相隔约 40 秒。
4. Task36 多数 provider 没有逐轮 receipt。只有文件差异，无法推出部署版本发生变化。

## 决策

在 benchmark 和论文中，`round1/round2` 统一标成 **repeatability / execution-drift snapshots**。不把 `round2` 称为 later version，不把它作为 live tool evolution 的证据，也不据此构造时间外推主指标。

允许的用途是重复性压力测试：检验选择器在 provider 输出漂移和 label flip 下的稳定性，并把这项结果作为 robustness extension。

## 后果

- 当前 benchmark 的动态性来自 delayed selected-only outcome feedback 和在线状态更新，不来自工具版本时间轴。
- 若要恢复“动态工具版本”这条叙事，必须新增带版本/部署事件和逐轮 receipt 的数据；这应单独预登记，不能从现有缓存反推。
- 这项裁定不删除已有 round 数据，也不重跑旧实验；保留已有差异作为负面和稳健性证据。

