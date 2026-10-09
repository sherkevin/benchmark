# Provider-fresh controlled correctness-drift stress v1

日期：2026-10-06
状态：**机制压力测试完成；不是自然泛化 confirmation。**

在同一 fresh provider menu 和 157 个 scaffold-disjoint candidates 上，预先固定在 stream 中点把每个 fold 的 fit-best provider 的 selected 返回标签翻转。selector 只能在 selected feedback release 后看到 correctness，不能看到 drift flag 或未选输出。

| 方法 | 平均 correctness | 相对 Frozen | scaffold bootstrap 95% CI |
|---|---:|---:|---:|
| Frozen | 73.38% | — | — |
| MIRA-Context-Reliability | 87.56% | +14.18 pp | [+9.61, +19.03] pp |
| Matched Online-SGD | 84.03% | +10.66 pp | [+6.05, +15.45] pp |
| Greedy LinUCB | 81.15% | +7.77 pp | [+5.08, +10.72] pp |

共 120 条 stream（4 methods × 5 folds × 2 seeds × 3 delays）。MIRA 的提升来自中点后的 selected correctness feedback 触发 provider ranking 改变；没有增加调用次数。

这项结果支持“实时 selected-only feedback 能处理已声明的 provider correctness drift”这一机制主张，不支持把受控翻转描述成自然环境漂移，也不能替代自然 fresh confirmation。原始事件在 `experiments/logs/task49_provider_fresh_drift_stress_20261006/v1/events.jsonl`，聚合在 `result.json`；独立回放完成后补写 `verification.json`。

## 2026-10-06 state-hash rerun addendum

The controlled stress was rerun after serializing the MIRA-Context fields that directly determine future scores: `fit_rates`, `successes`, `counts`, and `prior_count`. No data, candidate menu, prior, hyperparameter, seed, delay, drift rule, or aggregation rule changed.

- Receipt: `experiments/logs/task49_provider_fresh_drift_stress_20261006/v2_statehash_run2/`
- Streams/events: 120 streams; event contract and replay verification 120/120; forbidden decision fields 0; replay mismatches 0.
- MIRA-Context: 0.875584 (87.56%); Frozen: 0.733758 (73.38%); paired delta 0.141826 (+14.18 pp); scaffold bootstrap CI `[+0.096124,+0.190297]` (`[+9.61,+19.03]` pp).
- Receipt hashes: events `4cac68d3a5c3b262da2d05e93c7651386980a0727f70da38e87f225e33f8902e`; summary `1a41121e36b45a7cffff8b81331e70561d7222430b2ac86eb4e9100dd86cc221`; config `d74cf97e47bb5a4a214cae96d0700a363b6c18e014b8e8d902b683c68b06a892`.

This rerun closes the state-hash audit gap; the result remains a controlled evaluator-side correctness-drift stress and is not relabeled as natural deployment drift.
