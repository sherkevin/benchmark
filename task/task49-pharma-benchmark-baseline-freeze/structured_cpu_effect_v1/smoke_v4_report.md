# Structured CPU effect v1：smoke_v4 合约报告

日期：2026-09-29  
状态：工程合约 smoke；不是效果实验，不支持方法优劣结论。

## 范围

本次只运行 fold 0、seed 0、delay 0，每个动态方法 1 个 evaluation 分子，共 6 条流。使用已恢复的 `bt_cls` 64 维 encoder、fit-only warm start 和 selected-only evaluator facade。没有读取 confirmation 数据，没有提交 Nebula/A800 任务。

## 结果

- 6 条流各产生 1 个 decision、1 个 terminal、1 个 feedback，共 18 条事件。
- 独立 verifier 状态为 `verified`，并检查了 fit checkpoint/snapshot hash、事件延迟、provider 绑定、terminal 信息边界和事件级 runtime 字段。
- 每条流均只读取被选 provider 的一个 panel cell：`selected_panel_reads=1`，`selected_only_reads=true`。
- delay=0 仍在下一 tick 释放；feedback 没有提前更新。
- `frozen_structured` 版本保持 0；`matched_online_sgd` 和四个更新型结构化流各产生一次合法更新。
- 每个 stream 保存 decision/update 的 p50/p95 纳秒运行时间；单样本分位数只用于检查字段完整性，不作性能结论。

这些结果只说明当前实现满足最小的信息边界、延迟、运行时日志和恢复合同。样本数不足以估计 accuracy、延迟分布或任何方法增益。

## 可复核证据

- 运行配置：[config.json](../../../experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/smoke_v4/run/config.json)，SHA-256 `f95aab97eccc3a15585550cb755b506d8560aa2f94d7ede898cb64d191d7563d`
- 原始事件：[events.jsonl](../../../experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/smoke_v4/run/events.jsonl)，SHA-256 `817f89aaf5f79478eb93b9aa07b0c28ef6965a1239faf9c970c8cbf9b520695d`
- 汇总：[summary.json](../../../experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/smoke_v4/run/summary.json)，SHA-256 `544619e84b84b77173613c2babc32be96b65fdad51eca1bba2af64762fc22cc3`
- 独立复核：[verification.json](../../../experiments/logs/task49_composition_20260929/structured_cpu_effect_v1/smoke_v4/run/verification.json)，SHA-256 `ae59b74733fba3fbf6928816140b45385a440aabe1b0e640eabb53a59adc5678`

下一步是按预注册配置运行 180 条 CPU 流，再独立聚合效果与 runtime。通过预登记停止条件后，才讨论是否值得进入 A800。
