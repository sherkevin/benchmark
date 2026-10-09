# Provider-fresh BBB confirmation v1

日期：2026-10-06  
状态：**已完成并通过独立回放；自然 confirmation 的主要效果门未通过。**

## 运行范围

- 四个重新训练且 checkpoint 可绑定的 provider：ECFP-LogReg、ECFP-MLP、ECFP-RandomForest、Graph message passing。
- 157 个与旧 development panel 在 canonical molecule 和 nonchiral Bemis--Murcko scaffold 上都不重叠的 BBB candidate。
- 5 个 development scaffold folds × 2 seed × 3 delayed-feedback settings，8 个方法，共 240 条 stream、113,040 条事件。
- provider 只做 frozen checkpoint 前向；selector 决策时没有读取 confirmation truth、未选 provider 输出或未到期反馈。

## 结果

| 方法 | 平均 selected correctness | 相对 Frozen | scaffold bootstrap 95% CI |
|---|---:|---:|---:|
| Frozen | 89.81% | — | — |
| MIRA-Relative-Reliability | 89.81% | 0.00 pp | [0.00, 0.00] pp |
| Greedy LinUCB | 89.72% | -0.08 pp | [-0.58, +0.41] pp |
| BayesianRouter | 89.53% | -0.28 pp | [-1.75, +0.97] pp |
| Matched Online-SGD | 89.21% | -0.59 pp | [-1.80, +0.52] pp |
| Best Fixed | 88.15% | -1.66 pp | [-3.97, +0.51] pp |
| Random | 83.76% | -6.05 pp | [-9.93, -2.26] pp |
| Oracle | 90.45% | +0.64 pp | [-2.86, +4.07] pp |

区间为 scaffold 聚类 bootstrap，10,000 draws，seed 20261006。完整数值见 `aggregation_v1/result.json`。

## 判定

自然 fresh menu 上没有观察到 MIRA-Relative 的增益；它与 Frozen 的动作序列完全相同。预登记停止条件“相对 Frozen 的 pooled difference > 0 且区间下界 > 0”不满足，因此不能把这条自然批次写成动态收益证明，也不能通过删 provider、改 split 或调阈值追分。这个结果说明全局 reliability factor 在近似平稳菜单里不足以改变 molecule-conditioned ranking。

## 证据

- 预登记：`preregistration.md`。
- 原始事件：`experiments/logs/task49_provider_fresh_confirmation_20261006/v1/events.jsonl`。
- 配置与 checkpoint/input hash：同目录 `config.json`。
- provider 前向概率：同目录 `provider_predictions.json`。
- 聚合与区间：`aggregation_v1/result.json`。
- 独立回放：原始目录 `verification.json`，状态 `verified`；240/240 streams 通过，决策事件没有 truth/output 泄漏，replay mismatch 为 0。

## 后续方法方向

下一条主方法改为 context-relative MIRA-Relay：冻结的 molecule-conditioned prior 保留为基线，selected feedback 只更新“该 provider 相对其 fit-rate 的 residual”，并在冷启动时精确退化为 Frozen。它不能用这份自然 confirmation 调参；目前只在单独预登记的 controlled correctness-drift stress 中验证动态切换能力。
