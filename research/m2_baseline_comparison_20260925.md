# M2 与基本策略的匹配回放对照

**日期：** 2026-09-25  
**定位：** 管线检查，不是最终性能实验

## 配置

- 30 个 Task36 frozen BBB 分子；
- 首 provider 固定为 `deeppurpose_bbb_molnet_cnn`；候选 probe 为 Morgan 和 MPNN；
- round 1，feedback delay 1，seed 0；
- 对照：never-verify、always-verify、random probe（M1，$\eta=0.5$）和 gain gate（M2，探索概率 $0.5$，调用成本 0）；
- 没有新工具/API 调用，所有结果来自冻结 `PredictionCache` 和 `TruthStore`。

## 结果

| 策略 | Accuracy | 平均调用数 | Probe rate | Release 数 |
|---|---:|---:|---:|---:|
| never-verify | 0.700 | 1.000 | 0.000 | 0 |
| always-verify | 0.900 | 2.000 | 1.000 | 30 |
| random probe | 0.833 | 1.433 | 0.433 | 13 |
| gain gate | 0.933 | 1.900 | 0.900 | 27 |

这组数字只能说明四条 runner 在同一批 replay 上的会计口径能跑通。样本只有 30，策略共享同一 provider 顺序，gain gate 还使用了 zero call cost 和 one-hot probe 特征，不能据此声称它已经优于 baseline。

## 检查结果

- 4 个策略都没有 unresolved episode；
- 所有实际 selection 的 propensity 都为正；
- 所有 release event id 唯一；
- 所有 queue 在 flush 后为空；
- 原始 per-episode 结果和事件流已保存。

## 文件

- 配置：`experiments/logs/m2_baseline_comparison_20260925_config.json`
- 原始结果：`experiments/logs/m2_baseline_comparison_20260925_raw.json`
- 事件流：`experiments/logs/m2_baseline_comparison_20260925_events.jsonl`
- 汇总：`experiments/logs/m2_baseline_comparison_20260925_summary.json`

下一轮应扩大到预先冻结的 evaluation split，并把调用成本、风险约束、多个 seed 和置信区间写进正式协议；在此之前，这份结果只作为实现闭环证据。
