# Structured CPU effect v1：smoke_v3 合约报告

日期：2026-09-29
状态：工程合约 smoke；不是效果实验，不支持方法优劣结论。

## 范围

本次只运行 fold 0、seed 0、delay 0，每个动态方法 1 个 evaluation 分子，共 6 条流。使用已恢复的 `bt_cls` 64 维 encoder、fit-only warm start 和 selected-only evaluator facade。没有读取 confirmation 数据，没有提交 Nebula/A800 任务。

## 结果

- 6 条流各产生 1 个 decision、1 个 terminal、1 个 feedback，共 18 条事件。
- 独立 verifier 状态为 `verified`，未发现 decision/terminal 泄漏 `truth`、`chemical_correct` 或 `recovered_label`。
- 每条流均只读取被选 provider 的一个 panel cell：`selected_panel_reads=1`，`selected_only_reads=true`。
- delay=0 仍在下一 tick 释放；feedback 没有提前更新。
- `frozen_structured` 版本保持 0；`matched_online_sgd` 和四个更新型结构化流各产生一次合法更新。

这些结果只说明当前实现满足最小的信息边界、延迟和日志合同。样本数不足以估计 accuracy、延迟分位数或任何方法增益。

## 可复核证据

- 运行配置：[config.json](../../../experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/smoke_v3/run/config.json)，SHA-256 `d9296b7a3b002df0bca7645dcd7bc893e698f3ee6be30ce27ed8f108ca4b0137`
- 原始事件：[events.jsonl](../../../experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/smoke_v3/run/events.jsonl)，SHA-256 `ca70ca138fd4fddc5b8f18f1c61dfe9bd60ab2e3aa819da44382aaadda59da88`
- 汇总：[summary.json](../../../experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/smoke_v3/run/summary.json)，SHA-256 `a5aa7dec0ffabf471d0650e535535e92fc3750e8b5df44aeb76e8053e5ef92ae`
- 独立复核：[verification.json](../../../experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/smoke_v3/run/verification.json)，SHA-256 `9b4453124be5196605438c49fe629ddee7a0dc9ba38b6329b09bf0bb5c3e60bf`

下一步是先加入并测试事件级 decision/update runtime 记录，再按预注册配置运行 180 条 CPU 流；通过独立复核和预登记停止条件后，才讨论是否值得进入 A800。
