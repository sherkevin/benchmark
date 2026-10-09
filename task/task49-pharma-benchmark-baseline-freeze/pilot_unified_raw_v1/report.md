# PILOT-adapted unified raw effect v1

日期：2026-09-30  
状态：CPU development baseline qualification 已完成；不是 Proposed 方法、A800 或 confirmation 结果。

## 收据与核验

- receipt：`experiments/logs/task49_pilot_unified_raw_20260930/full_v1/`
- 输入：722 分子、33 provider、5 个 scaffold-disjoint folds；SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 矩阵：`pilot_frozen/pilot_online × 5 folds × 2 fit seeds × delay {0,1,4}`，60 streams，8,664 decision/terminal/feedback。
- 独立 verifier：`src/probe_headless/verify_pilot_unified_raw_v1.py`。它不导入 runner 或 `PilotOnlineKernel`，独立重算归一化 query、cosine+UCB score/argmax、selected-only delayed reward、Gaussian rank-one update、state/version/hash、provider precision、事件守恒和 summary；状态 `verified`。
- verifier 的边界：PILOT 的 projection/provider vector 是运行前 fit-only PyTorch prior，当前 verifier 独立重算其投影应用、query normalization 和 fit-only precision，但不把 AdamW 训练过程伪装成 NumPy 等价重训；prior 文件、fit statistics、输入和源码身份均封存。
- raw events SHA-256：`541819721b2e90187412a34958ae7f078b0f405467af42063652748fcc1a68e1`。raw JSONL 保留在本地，机器收据、配置、summary、analysis、snapshot manifest 和 verifier 已保存。
- smoke 过程中的两个真实实现失败（PILOT 二次归一化导致 feature hash/state 重放不一致）保留在 `smoke_v1`、`smoke_v2`；修正后的 `smoke_v4` 通过。state hash 与 UCB score 篡改均被拒绝，记录在 `tamper_state_v2/`、`tamper_score_v2/`。

## 结果

下表 pooled accuracy 的每个 delay 分母为 1,444 个 evaluation decisions；paired stream 均值和每个 fold/seed 差值在 `analysis.json` 中保存，两个聚合不混用。

| delay | PILOT online | PILOT frozen | pooled online − frozen | paired stream mean | 正差值流数/10 |
|---:|---:|---:|---:|---:|---:|
| 0 | 0.884349 | 0.885042 | −0.069 pp | −0.068 pp | 4 |
| 1 | 0.882271 | 0.885042 | −0.277 pp | −0.275 pp | 4 |
| 4 | 0.883657 | 0.885042 | −0.139 pp | −0.139 pp | 3 |

fit-only Best Fixed 的五折 evaluation accuracy mean 为 `0.915575`，明显高于两种 PILOT 变体。

PILOT online 在三个 delay 都低于同一 fit-only prior 的 frozen control，且没有达到预登记的“至少两个 delay 的 equal-stream mean 为正”开发门槛。因此，PILOT-adapted 离线先验加普通 cosine/UCB/ridge 更新在当前任务上保留为现代负向 baseline；不申请 A800 诊断，不因这个结果调 alpha、epoch、precision 或数据顺序。

## 证据边界

- 两种 PILOT 变体共用同一 fold/seed prior、query features、provider menu、evaluation 顺序和 delayed feedback clock；未选 provider output、truth、future feedback 不进入 policy。
- 这是统一 raw contract 和 baseline qualification 证据，不是 MIRA superiority 证据。它也不是官方 PILOT 复现：本任务没有 LLM preference 文本或成本 knapsack，正文应写 `PILOT-adapted`。
- 旧 `task49_pilot_adapted_development_20260927/run_v1/` 保留为历史 receipt；它的旧 `status=started`/非统一字段不与本 receipt 混表。新 receipt 关闭了这条 baseline 的统一合同缺口，但不提升 Gate A/B/C。

## 下一步

Greedy 与 PILOT 的 unified baseline receipt 现在都具备；继续补 provider training membership/lineage、endpoint assay equivalence、cross-method feature/warm-start/budget/cost fairness、动态事件压力和独立 confirmation。Gate A/B/C 仍保持 HOLD，下一步不提交新的 A800 任务。
