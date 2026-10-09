# Greedy LinUCB development baseline v2：合同与 CPU development replay

日期：2026-09-29  
状态：合同与全量 CPU development replay 已通过；结果仍不是 final main table、confirmation 或 A800 结果。

## 本轮修正

- 输入切换到 P-C1 authoritative BBB development manifest。
- 默认表示切换为已有 fit-only BT/CLS encoder 的 hidden-64，与 matched SGD、PILOT 和 BayesianRouter 主证据处于同一表示层；Morgan-1024 保留为单独适配 strata，不混入主排名。
- 事件日志补齐 `method_id`、`run_id`、`episode_id`、`decision_reads`、`released_feedback_ids`、`provider_event_ids`、`state_hash_before/after` 和 `update_ms`。
- verifier 独立重算 fit-only prior、hidden 表示、UCB 选择、延迟反馈、状态更新和状态哈希，不导入 Greedy policy 或 runner。
- seed 0/1 仍按确定性 protocol slot 处理，不能当作独立随机重复。

## 真实 smoke

| 项目 | 设置 |
|---|---|
| fold / seed / delay | `0 / 0 / 0` |
| provider | 33 |
| evaluation molecules | 145 |
| policy streams | online + Frozen，共 2 |
| decision / terminal / feedback | 各 145，共 290 事件 |
| 表示 | BT/CLS hidden-64，fold0 seed0 checkpoint |
| 输入 manifest | P-C1 authoritative v1 |
| GPU/API | 未使用 |

原始收据：`experiments/logs/task49_greedy_linucb_development_20260929/smoke_v2_fix/`。  
独立恢复结果：`verification.json`，状态 `verified`。

修正 drain 日志顺序后，delay=1 smoke 也通过：
`experiments/logs/task49_greedy_linucb_development_20260929/smoke_v3_fix/`。

## 负例核验

复制 smoke 目录后篡改第一条 online feedback 的 `state_hash_after`，独立 verifier 以 `feedback state_hash_after mismatch` 拒绝；篡改现场保留在 `experiments/logs/task49_greedy_linucb_development_20260929/smoke_v2_tamper/`。

## 中间失败

第一次 smoke 因相对 checkpoint 路径没有先 `resolve()`，source hash 生成失败；现场保留在 `experiments/logs/task49_greedy_linucb_development_20260929/smoke_v2/`。它没有开始回放，也没有产生效果结果。修复后使用新的 `smoke_v2_fix` 目录重跑。

第一次全量回放目录 `full_v2/` 在 verifier 中暴露出另一个协议日志问题：`run_bbb_stream` 在最终 drain 时先完成全部 policy update，再逐条写 feedback，因此末尾事件的 `state_hash_after` 不能代表该事件后的状态。该目录保留为失败证据，没有被用于结果分析。修复为逐条回调的 `drain_each` 后，重新运行 `full_v3_fix/`。

## 全量 CPU development replay

设置为 5 folds × 2 deterministic protocol slots × 3 delays × online/Frozen，共 60 条流、8,664 个事件。输入是 P-C1 authoritative BBB manifest，表示是已有 BT/CLS encoder 的 hidden-64；没有新模型/API 预测，也没有 GPU。

| delay | online accuracy | Frozen accuracy | online − Frozen | Best Fixed accuracy |
|---:|---:|---:|---:|---:|
| 0 | 0.8511 | 0.8566 | −0.0055 | 0.9155 |
| 1 | 0.8449 | 0.8566 | −0.0118 | 0.9155 |
| 4 | 0.8629 | 0.8566 | +0.0062 | 0.9155 |

Best Fixed 是每个外层 fold 只用 fit 区域选择的固定 provider，5 折合计 accuracy 为 0.9155。Greedy online 在三个 delay 上都没有超过 Best Fixed；delay=0 和 delay=1 也没有超过 Frozen。按预先的开发 go/no-go 规则（至少 0.02 的提升且多数 fold 为正），该候选不进入主方法或 confirmation。

结果收据：

- 原始事件：`experiments/logs/task49_greedy_linucb_development_20260929/full_v3_fix/events.jsonl`，SHA-256 `7d96d466f5ee5ae6098e19154a343077110860f1882e8e7d5551cb967f55ce1c`。
- 汇总：`full_v3_fix/summary.json`，SHA-256 `abb76658c7c6dfb0306c2b11b5e0e428269afd46bc6c2c6ad9ef33dd45694fb9`。
- 独立核验：`full_v3_fix/verification.json`，60 streams、8,664 events，状态 `verified`。

## 解释边界

这个负结果不能推出“所有在线线性更新都无效”。它只说明在当前固定 provider pool、selected-only delayed feedback、BT/CLS hidden-64 和 fit-only prior 合同下，普通 Greedy LinUCB 没有解决 warm-start 与探索损失的组合问题。它反而给主方法一个清晰的对照：若动态 JEV 只把冻结 selector 接上未经约束的在线线性头，容易低于强固定 provider；后续方法必须明确解决 prior calibration、探索风险和延迟信用分配，而不是继续堆普通 bandit baseline。

## 结论和下一步

合同、延迟队列、selected-only 信息边界和独立状态恢复已经通过；Greedy 作为现代在线基线的 CPU development replay 已完成并保留为负结果。它不支持申请 A800，也不支持把普通 LinUCB 写成我们的方法。下一步优先闭合 matched OnlineSGD/ReplaySGD 与 BayesianRouter 的统一主表，再决定动态 JEV 是否需要新的、带安全切换和校准先验的机制；不通过 Gate B/C 前不申请新的 GPU 任务。
