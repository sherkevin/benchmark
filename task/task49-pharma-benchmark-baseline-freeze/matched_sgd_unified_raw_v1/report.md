# Matched Online-SGD unified raw receipt v1

日期：2026-09-30

> 历史 receipt。该目录的 verifier 曾被发现对菜单/read-set/summary fail-open；严格可引用的 v2 receipt 和 P0 修复见 `task/task49-pharma-benchmark-baseline-freeze/baseline_verifier_p0_repair_v2/report.md` 与 `experiments/logs/task49_matched_sgd_unified_raw_20261001/full_v6/`。下文数字保留作历史记录，不再作为当前验收入口。

## 结果

完整 CPU receipt 已完成：

- 60 条 stream：`frozen_direct`、`matched_online_sgd` × 5 folds × 2 seeds ×
  delays `{0,1,4}`；
- 8,664 个 decision、8,664 个 terminal、8,664 个 delayed feedback；
- runner 完成，独立 verifier `verified`，并重新计算每次 action、terminal cache
  输出、reward、版本和 direct-SGD 权重更新；
- 输入、feature/direct-prior snapshots、source hash 和 immutable commit 均有收据。

按 delay 的描述性 accuracy（每个值是 10 个 fold/seed stream 的平均）为：

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| frozen_direct | 0.901734 | 0.901734 | 0.901734 |
| matched_online_sgd | 0.891375 | 0.888587 | 0.896868 |

这些数字只用于和 MIRA 使用同一 representation、warm start、菜单和反馈时钟的
开发阶段配对分析；不打开 confirmation，也不替换旧 receipt 的历史结果。

## 可复核证据

- raw run：`experiments/logs/task49_matched_sgd_unified_raw_20260930/full_v2/`
- independent verifier：同目录 `verification.json`，60/60 streams、8,664/8,664
  events 通过；events SHA-256=`019d70beda63cf0eb337a8dc085eacc6bbfb89ef065ed782ca29db48730fa693`。
- immutable provenance verifier：同目录 `provenance.json`，6/6 source hashes 通过。
- immutable executable commit：`b4836607f97f34c8e546e02a27fe4d315e70c306`。

`full_v1/` 保留为失败现场：其 raw verifier 通过，但 provenance verifier 因绝对
source path 退出 1；修复后 `full_v2/` 才是权威 receipt。这个失败不被当作科学
结果，也没有通过重训掩盖。

## 解释边界

这条 receipt 关闭了 Matched Online-SGD 在当前 MIRA development representation
下的统一 raw-run/recovery 缺口。它没有关闭 PILOT、BayesianRouter、Greedy、Laya
的统一 fidelity，也没有关闭 provider lineage、endpoint equivalence、动态事件
和 confirmation 闸门。新的 receipt 与旧 Matched development run 使用的表示合同
不同，因此不把两者的数值直接混成一张历史表。

## 预期产物

- `config.json`：输入、checkpoint、source、超参和代码身份。
- `inputs.json`：本次 receipt 使用的冻结 rows、folds 和 provider menu。
- `fit_snapshots/`：逐 fold/seed 的 direct prior 与 64 维 feature 快照。
- `events.jsonl`：统一 decision/terminal/feedback 原始事件。
- `summary.json`：stream 计数与结果索引。
- `verification.json`：不导入 policy 的独立重放结果。

## 解释边界

即使 receipt 通过，也只关闭 Baseline 的一部分 raw schema/fidelity 缺口；
PILOT、BayesianRouter、Greedy、MIRA 和 provider/endpoint lineage 仍需分别闭合。
