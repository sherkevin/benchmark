# BBB 动态验证门 post-freeze 对照协议 v1

日期：2026-09-25

## 目的

回答一个明确问题：在固定首工具和固定候选工具下，动态 verification-gain gate 是否能在 held-out BBB 分子上降低 terminal loss，或在相同调用成本下提高正确率。

## 分析集与切分

- 数据只来自已冻结的 `PredictionCache` 和 `TruthStore`，不调用外部 API。
- 730 个分子中预先排除 8 个 unresolved reference，形成固定的 722 条 resolved 分析集。
- 由于这 722 个分子此前已经参与过 runner 调试和参数选择，本轮只能称为 post-freeze retrospective evaluation，不能称为未触碰的 confirmatory test。
- 先用 canonical SMILES 计算 Bemis–Murcko scaffold；没有环的分子使用 `ACYCLIC:<entity_id>`。按 `sha256(scaffold) mod 5 == 0` 固定为 held-out scaffold，其余 scaffold 是 calibration stream。切分规则不看预测、标签、策略结果或 seed。
- held-out 结果同时报告 molecule-weighted 和 scaffold-macro 两种统计。任何新参数若根据这次 held-out 结果修改，必须开启新版本实验，旧结果继续保留为 retrospective。
- 两个在线策略先在 calibration stream 上更新，再按 canonical entity id 顺序进入 held-out stream。held-out 内允许反馈在下一步到达，这是 prequential online evaluation；held-out 的当前结果不能回写到同一 episode 的动作。

## 策略

1. `never`: 只调用 CNN。
2. `always_morgan`: CNN 后固定调用 Morgan。
3. `always_mpnn`: CNN 后固定调用 MPNN。
4. `random_probe`: M1，探索概率 0.8，在 calibration 和 held-out 都按随机策略选择候选。
5. `dynamic_gate`: M2，使用 v5 safe-overlap HT/Freedman gate。

所有策略使用同一个首 provider、同一个 round、同一个 evaluator 和同一个 abstain loss。动态策略使用 20 个 seeds；静态策略没有随机性。

## 主指标

对 held-out 每个 episode 记录：

- terminal correctness；
- terminal task loss（正确为 0，错误为 1，拒答/失败为 1）；
- tool call count；
- net utility：`terminal_loss + call_cost * call_count`。

主实验使用 `call_cost=0.1`。敏感性分析使用 `0.0, 0.05, 0.2`。报告策略均值、跨 seed 标准差和 95% t 区间，并报告每个 dynamic seed 相对每个静态 baseline 的 paired difference。

## 主要比较与否证条件

- 主要比较：`dynamic_gate` 对 `random_probe` 和 `never` 的 held-out net utility。
- 如果 dynamic gate 在 `call_cost=0.1` 下没有降低 net utility，或者调用数没有低于 always baseline 且正确率没有改善，则不声称有实际收益。
- 如果队列不为空、propensity 不全为正、出现 unresolved、事件重复或 calibration/held-out 边界被破坏，则本轮实验作废。

## 结果边界

这是 provider-marginal dynamic gate 的正式基线实验，不是 context-conditioned JEV，也不验证 backbone fine-tuning。只有在这组对照通过后，才进入上下文特征和实时 JEV 参数更新实验。
