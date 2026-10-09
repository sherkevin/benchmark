# 157 条 confirmation 候选与 GraphB3 split 的重叠审计

日期：2026-09-30

## 结论

已把冻结的 157 条 scaffold-disjoint BBB confirmation 候选与 GraphB3 随包提供的 train/test 表逐分子比对。结果是：107 条出现在 GraphB3 train 表，27 条出现在 GraphB3 test 表，23 条不在两张表中。该结果让 GraphB3 的 provider-specific exposure 可量化，但**不能**把 27 条自动升级为独立 confirmation：当前发布 checkpoint 仍未绑定到 10 个 seed/split 中的具体一个。

## 机器证据

- 配置：`experiments/logs/task49_graphb3_confirmation_overlap_20260930/run_v1/config.json`
- 结果：`experiments/logs/task49_graphb3_confirmation_overlap_20260930/run_v1/results.json`
- 结果 SHA-256：`b1bc96b09dc9c78c161cc118206e035f4046614f5e1827aac595354593107868`
- 输入 confirmation manifest SHA-256：`c0af602bbc4ec49df448b916fb1e54dfce183926965d61c184cb736e68402805`

这次只读输入，未运行 GraphB3、未运行 selector、未读取任何新 confirmation 输出。

## 结果

| GraphB3 表 | 候选数 | BBB 标签 0 / 1 |
|---|---:|---:|
| train | 107 | 18 / 89 |
| test | 27 | 4 / 23 |
| 两张表都没有 | 23 | 未单独作为独立性证明 |
| 同时出现在 train 和 test | 0 | — |

候选 manifest 本身 157 行、157 个 canonical molecule、无无效 SMILES；与 GraphB3 train/test 的 canonical 交集没有重复交叉。

## 对 confirmation 的影响

1. GraphB3 的本地 test 表只覆盖 27/157 候选；不能把整份 confirmation 写成 GraphB3-unseen。
2. 107/157 已在 GraphB3 train 表中，说明 scaffold-disjoint 只解决 selector development overlap，不能替代 provider training exposure 审计。
3. 23 条不在 GraphB3 两张表中，是后续 provider-specific candidate 的线索；但其他 32 个 provider 的训练成员仍未知，不能把它们当作全菜单独立集。
4. 若论文需要 provider-specific 独立分析，可把 27 条作为 GraphB3 test-overlap stratum、107 条作为 GraphB3 train-exposure stratum、23 条作为 GraphB3-unmatched stratum 分层报告；正式 confirmation 仍需完成所有主 provider 的 lineage 冻结。

## 下一步

- 追查 GraphB3 当前 checkpoint 的 seed/split/member manifest；有了绑定后才能重新解释 27 条 test overlap。
- 对新训练的可审计 provider 子菜单逐 provider 生成同样的 train/test exposure manifest。
- 在 lineage 和方法冻结前，不打开 157 条 confirmation，也不从这次 overlap 审计生成效果数字。
