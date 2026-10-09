# Greedy LinUCB unified raw effect v1

日期：2026-09-30  
状态：CPU development baseline qualification 已完成；不是 Proposed 方法、A800 或 confirmation 结果。

## 收据与核验

- receipt：`experiments/logs/task49_greedy_unified_raw_20260930/full_v1/`
- 输入：722 分子、33 provider、5 个 scaffold-disjoint folds；SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 矩阵：`greedy_frozen/greedy_online × 5 folds × 2 deterministic slots × delay {0,1,4}`，60 streams，8,664 decision/terminal/feedback。
- 独立 verifier：`src/probe_headless/verify_greedy_unified_raw_v1.py`，从 immutable checkpoint 重建 hidden-64，独立重算 fit-only ridge prior、UCB score/argmax、delay release、selected reward 和 rank-one state transition；状态 `verified`。
- raw events SHA-256：`db6322383a05506154076f3ef5ed24b0847c5a8197fdf0a001e99550fde3cc13`。raw JSONL 保留在本地，机器收据、配置、summary、analysis 和 verifier 已保存。

## 结果

下面的 pooled accuracy 以每个 delay 的 1,444 个 evaluation decisions 为分母；paired stream 差值另保存在 `analysis.json`，不把两种聚合混用。

| delay | Greedy online | Greedy frozen | pooled online − frozen |
|---:|---:|---:|---:|
| 0 | 0.851108 | 0.856648 | −0.005540 |
| 1 | 0.844875 | 0.856648 | −0.011773 |
| 4 | 0.862881 | 0.856648 | +0.006233 |

Greedy 在 delay 0/1 低于同一 fit-only prior 的 frozen control，delay 4 只提高约 0.62 个百分点。按预注册的“至少两个 delay 为正且多数 fold 为正”开发判据，普通 LinUCB 没有稳定收益；保留为现代在线 baseline 的负结果。

fit-only Best Fixed 的五折 evaluation accuracy 为 `0.915575`（pooled 分母口径保留在 `analysis.json`），明显高于两种 Greedy 变体。这个结果只说明当前 fixed provider pool、hidden-64、selected-only delayed correctness 合同下，普通线性在线更新没有解决 warm-start 与探索损失的组合问题；不能推出所有在线方法都无效。

## 证据边界

- 这批使用当前 MIRA/native/matched 共用的 `a73f1c…` 输入。旧 Greedy `full_v3_fix` 使用 P-C1 `94580e…`，作为历史证据保留，不与本表混合。
- `greedy_online` 和 `greedy_frozen` 共用 fit-only prior、feature、menu、顺序和 feedback delay；未选 provider output、truth、future feedback 不进入 policy。
- 这是统一 raw contract 和 baseline 资格证据，不是 MIRA superiority 证据。PILOT-adapted 的统一 receipt、provider lineage、endpoint equivalence、成本/特征公平和 confirmation 仍未闭合。

## 下一步

将本 receipt 并入 Gate B 的统一合同证据，Gate A/B/C 分数暂不自动上调；下一项按同一输入和合同处理 PILOT-adapted，随后再重新审查是否值得做一次完整主表，而不是提交 A800。
