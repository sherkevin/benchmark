# BayesianRouter controlled-drift stress v2

日期：2026-10-06
状态：**完成并通过合同/回放验证；作为补充 baseline 证据。**

这是在已有 controlled correctness-drift stress 上补齐 BayesianRouter 的同协议比较。没有改变候选集、checkpoint、truth、fold、seed、delay、翻转规则或 MIRA 参数；原 v1/v2 receipt 保留不改写。

| 方法 | 平均 correctness | 相对 Frozen | scaffold bootstrap 95% CI | action changes | selection/update p95（均值，µs） |
|---|---:|---:|---:|---:|---:|
| Frozen | 73.38% | — | — | 0 | 17.9 / 1.5 |
| BayesianRouter | 80.28% | +6.90 pp | [+3.50,+10.41] pp | 1,654 | 137.3 / 92.6 |
| Matched Online-SGD | 84.03% | +10.66 pp | [+6.05,+15.45] pp | 2,566 | 14.3 / 20.0 |
| Greedy LinUCB | 81.15% | +7.77 pp | [+5.08,+10.72] pp | 874 | 128.4 / 92.3 |
| MIRA-Context | 87.56% | +14.18 pp | [+9.61,+19.03] pp | 1,884 | 32.5 / 3.5 |

BayesianRouter 使用 30 条 stream（5 folds × 2 seeds × 3 delays），与 Frozen 成对；整个收据共 150 条 stream、70,650 条事件。BayesianRouter 的正/负 correctness changes 为 492/167，余下 4,051 个事件没有改变 correctness。bootstrap 使用 124 个 Bemis--Murcko scaffold clusters、10,000 draws、seed 20261006，并按 scaffold-resampled event-row denominator 聚合；这一实现复现了已有 stress 表的区间合同，已有三行区间逐项重现。

验证结果：`verification.json` 为 `verified`，150/150 streams 通过 event contract/replay，0 forbidden decision fields，0 replay mismatches。该压力测试仍是 evaluator-side label flip 机制测试，不是自然生命周期漂移或真实部署轨迹；BayesianRouter 也只作为现代 adapted comparator，不改变 MIRA-Relay 的主 claim。

原始收据：`experiments/logs/task49_provider_fresh_drift_stress_20261006/v3_bayesian_statehash_run1/`。
聚合：`result.json`。
