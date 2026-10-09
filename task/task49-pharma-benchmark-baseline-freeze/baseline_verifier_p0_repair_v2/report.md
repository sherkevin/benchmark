# Unified baseline receipt P0 repair v2

日期：2026-10-01

## 为什么要修

对 Matched Online-SGD 和 adapted BayesianRouter 的旧 raw verifier 做独立审查时，发现它们只按 `event_id` 聚合事件，没核对原始顺序、菜单、decision read-set、provider event ID 和根 summary。把首条菜单改成不存在的 provider、把 read-set 改成 `truth/full_panel`、把 `stream_complete.n_correct` 改掉，旧 verifier 仍会返回 `verified`。这不是科学负结果，而是 baseline 证据合同的 P0 缺陷。旧收据和旧 verifier 保留为历史失败现场，不再作为严格 baseline 证据。

## 修复内容

`src/probe_headless/verify_unified_raw_contract_v2.py` 是独立 NumPy replay，不导入策略实现。它现在固定检查：

- canonical frozen input SHA、cache 中所有 BBB 源文件 SHA，以及声明 commit 的 git tree；
- 完整 `5 folds × 2 seeds × 3 delays × 2 methods` roster；
- 原始 event 顺序：feedback release → decision → terminal，`stream_complete` 必须在本 stream 最后；
- decision 的完整菜单、候选菜单、精确 read-set、空 provider event IDs 和 selected provider；
- terminal 的 selected cache output、entity/provider/tick、status、valid return、system success、recovered label 和 state hash；
- feedback 的 exact schema、reward/status、due/release tick、provider/event IDs、版本和 state transition；
- 每条 stream 和根 summary 的计数、方法 roster、config/event hash；
- 任意额外的 `oracle_label`、`truth_proxy` 等字段都会 fail closed。

`run_matched_sgd_unified_raw_v1.py` 和 `run_bayesianrouter_unified_raw_v1.py` 已加入 cache manifest、相对 snapshot 路径和 verifier/source manifest。Bayesian runner 的根 summary 也补上了 method roster。

## 可引用收据

| baseline | 收据 | 独立结果 |
|---|---|---|
| Matched Online-SGD / Frozen direct | `experiments/logs/task49_matched_sgd_unified_raw_20261001/full_v6/` | v2 verifier：60 streams、8,664 decisions/terminals/feedback，`verified` |
| adapted BayesianRouter / Frozen BT mean | `experiments/logs/task49_bayesianrouter_unified_raw_20261001/provenance_repaired_v1/` | v2 verifier：60 streams、8,664 decisions/terminals/feedback，`verified` |

Bayesian 收据的 raw events 来自同一目录下的 `pre_cache_manifest_v1`；`provenance_repaired_v1` 只补入已从 `PredictionCache` 重新计算的 cache hashes、根 summary 的 `methods` 字段和对应 hashes，未修改 inputs、snapshots、events 或 action。中途未完成的 `aborted_incomplete_v1` 和路径错误现场均保留，不能用于结果分析。

## 变异证据

`experiments/logs/task49_baseline_verifier_p0_20261001/mutations_v2/results.json` 中的 8 类 mutation 全部被拒绝；对最终 matched v6 收据增加 `oracle_label` 的 mutation 也被拒绝，见 `mutation_final_oracle_label/result.json`。

## 结论边界

这一步只关闭 baseline raw receipt 的可复核性缺口，不证明 BayesianRouter、Online-SGD 或 MIRA 的效果，也不把 common direct prior 当作 native BayesianRouter prior。Gate B/C 的分数不因这次工程修复自动上调；统一成本、native prior lineage、MIRA 主方法效果和 held-out confirmation 仍需分别通过验收。
