# Baseline readiness v1

日期：2026-09-29 更新

这份报告只盘点“能不能作为论文 baseline 运行和解释”，不把 adapter smoke 或失败
任务写成效果结果。

| baseline | 当前证据 | 状态 | 论文使用边界 |
|---|---|---|---|
| Best Fixed / Oracle | frozen BBB panel 与独立聚合 | 可用 | 训练区固定参照 / evaluator 上界 |
| Frozen static selector | fingerprint diagnostic 已恢复；低于 Best Fixed | 可用 | 静态能力控制，不能当新方法 |
| matched Online-SGD | structured CPU effect 中完整运行 | 可用 | 同表示、selected-only、延迟匹配的强直接控制 |
| Replay-SGD | 已完成 5 folds × 2 seed slots × 3 delays 的真实 development replay；90/90 streams，独立 NumPy recovery 通过，结果低于 Best Fixed | 可用，负结果 | 可进入 development 主表；不能写成 Proposed 或 confirmation 结果。人类可读解释见 [matched SGD report](../../../experiments/logs/task49_matched_sgd_development_20260927/report.md) |
| Greedy LinUCB | 已完成 5 folds × 2 seed slots × 3 delays 的真实 BBB development replay；60/60 streams、独立 verifier 通过；online 未超过 Best Fixed，delay 0/1 也未超过 Frozen | 可用，负结果 | 可进入 development 主表；明确为 fit-prior、hidden-64 的透明适配，不能写成原论文零先验复现 |
| PILOT-adapted | 5 fold × 2 seed × 3 delay development replay；Online 未超过 Frozen/Best Fixed | 可用，负结果 | 现代近邻，保留负结果 |
| BayesianRouter-adapted | 已完成 [5 folds × 2 seeds × 3 delays 的真实 BBB selected-only development replay](../fingerprint_diagnostic_v1/online_bayesianrouter_effect_result.md)；online 均未超过 Frozen | 可用，负结果 | 保留正式负结果；不得把旧失败任务重用为证据，也不得把 adapted 结果写成原论文完全复现 |
| GraphRouter | 源码/论文审计完成；BBB 静态适配的 provider 描述、单 task 图语义和 `torch_geometric` 依赖尚未冻结 | 未完成，阻塞已具体化 | 只有完成 [GraphRouter 适配审查](../graphrouter_static_adapter_v1/blocking_report.md) 中的输入和 fidelity 合同后才运行；不能当在线 JEV |
| Laya/JEV | Laya forward 已在 A800 完成；静态表示诊断 5 folds × 2 seed slots × 4 heads 已恢复，低于 Best Fixed；selected-only 动态适配尚未完成 | 静态可用，在线阻塞 | 可作为表示负诊断/静态对照；不能写成在线 JEV baseline。见 [Laya static report](../laya_static_diagnostic_v1/report.md) 和 [在线适配审查](../laya_online_adapter_v1/blocking_report.md) |
| joint-success residual | 120-stream CPU gate 已运行；比 direct SGD 有正点差，但未超过 Frozen | 候选未通过主闸门 | 低成本校准候选和负结果，不提交 A800 |

## 当前结论

主 benchmark 的 baseline 名单仍然合理，但“名单存在”不等于“全部已完成可比较”。
当前可以支撑的直接选择开发比较是 Best Fixed、Oracle、Frozen、matched Online-SGD、
Replay-SGD、Greedy LinUCB、PILOT-adapted、BayesianRouter-adapted，以及 Laya 的静态表示负诊断。
GraphRouter 尚未完成 BBB 适配；Laya/JEV 还缺
selected-only 在线动态实验，因此不能把静态 Laya 结果当作动态 JEV 结果。

joint-success residual 的全量 CPU 结果不支持进入 A800：它没有在两个 delay 上超过
Frozen，所有 fold-bootstrap 区间都跨 0。下一步先补 baseline fidelity 和 provider
lineage/endpoint 资格；如果没有强近邻完成，就不能声称候选方法领先。

Greedy LinUCB 的全量结果也不支持进入 A800。它的 online accuracy 在 delay 0/1/4
分别为 0.8511/0.8449/0.8629，Frozen 为 0.8566，fit-only Best Fixed 为 0.9155。
这把“普通在线线性头本身就是解决方案”的可能性排除了，但不排除带校准先验、保守切换或
动态事件处理的后续方法。

## 交付前硬要求

1. 每个 baseline 都有固定源码/论文版本、输入权限、warm start、延迟、失败语义和日志恢复器。
2. 现代 baseline 失败必须保留失败原因，不能用弱化版替代后宣称公平比较。
3. 所有方法在同一 scaffold fold、seed、菜单、feedback clock 和 confirmation freeze 下运行。
4. 任何 baseline 只跑 adapter smoke 时，正文只能写“实现已核”，不能写效果排名；已有
   development effect 的方法还必须配套人类可读的聚合、paired delta、成本和边界说明。
