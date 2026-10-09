# ADR-0047：Guide v08 作为 Benchmark+Baseline 关闭目标的合同

- 状态：Superseded by ADR-0048（v08 目标保留为历史版本）
- 日期：2026-10-08
- 取代：无
- 关联：生效标准 `docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md` v006；Guide v08

## 背景

Guide v07 记录了固定已查看队列和既有策略的实测分布，不能回答“未来候选完成哪些实验后可以关闭 Benchmark+Baseline 验收”。如果只写均值目标，仍会漏掉公共 truth、baseline 保真、信息公平、失败分母、独立恢复、Stage-B confirmation 和双人评分。

## 决议

1. 以 `paper/aamas2027/experiment_tables/guide/versions/v08_20261008/guide.tex` 作为第一版完整 closure target。Guide 完成的含义是：真实 benchmark、真实主 baseline、E1--E6、H1--H10、B1--B15、Gate-A/B/C、原始证据、独立恢复、v006 scorecard 和两份独立复核全部完成。
2. 关闭阈值沿用 v006：H1--H10 全部 PASS；B1--B8 每项至少 3 分；B9--B15 每项至少 2 分且不得为 0；B 总分至少 49/60；Gate-A、Gate-B、Gate-C 各 4 个子项至少 3 分且总分至少 12/16。Gate 子项统一写作 `Gate-A1`--`Gate-C4`，不与 B1--B15 混用。
3. 当前主 baseline 的 canonical roster 为 `random_policy`、`best_fixed`、`retrospective_best_fixed`、`frozen`、`matched_online_sgd`、`replay_sgd`、`greedy_linucb`、`bayesianrouter` 和目标方法 `mira_context_reliability`（论文名称 MIRA-Relay）。`lin_ts_adapted` 是 audit-only，除非另建 ADR 把它提升为主 baseline。BayesianRouter 与 LinTS 不合并成一个结果行。
4. 当前 E6 范围固定为四个 fresh providers 上的 157 个 scaffold-disjoint confirmation molecules。替换数据集必须另建 Guide 版本和 ADR；“等价未读集”不能在 v08 内自行替换。
5. 正文确认展品固定为 `paper/aamas2027/main.tex#tab:confirmation`。Table 7 的拒答/失败展示不能代替标准 E6；关闭前必须把 confirmation 结果绑定到该 artifact ID 或在主稿中建立等价的明确 label（先更新本 ADR）。

## 预先冻结的目标阈值

这些是项目的 practical target，不是已取得结果，也不替代 v006 的评分规则。它们在任何新结果产生前冻结；后续修改必须新建 ADR。

- E1：fit-only Best Fixed 相对 Random 的 pooled paired 方向为正，且至少 3/5 fold 为正；否则 benchmark validity HOLD。
- E2：controlled 主 contrast 相对 matched Online-SGD/Replay 的点估计至少 `+2.0 pp`，95% paired CI 下界至少 `+0.5 pp`。自然场景允许到 `-1.0 pp` 的 non-inferiority 边界；只有 CI 下界大于 0 才能声称自然 superiority。
- E3：full 方法相对预先指定的主要机制 ablation 的 controlled contrast 点估计至少 `+1.0 pp`，95% CI 下界至少 0；只胜旧 baseline 不构成机制归因。
- E4：声明的 controlled pressure pooled contrast 使用 E2 阈值；每个 delay 单独报告，不把 pooled 正数写成每个 delay 都胜。
- E5：selector+update 的 replay/kernel p95 不超过 Frozen 的 2 倍且不超过 100 microseconds；这是本项目的工程目标，不能替代 B12/B13/B15，也不能被写成端到端 provider latency。
- E6：natural safety claim 的 CI 下界至少 `-1.5 pp`；controlled claim 的 CI 下界大于 0；只允许一次运行，禁止 label 回流、补跑和调参。

## 统计和审查规则

主比较的 primary comparator、paired episode/order 单位、95% CI 计算脚本、scaffold/family 聚类方式、multiplicity 处理、failure/unknown 分母和 rounding 必须在 E1--E5 的 preregistration 中逐项写出。候选不能继承 v07 的 SD、covariance 或 win rate。最终关闭必须生成 `closure_scorecard_v08.md`、`closure_review_author.md`、`closure_review_replicator.md` 和 `closure_decision_v08.json`；任一复核者不推荐 CLOSED，状态保持 HOLD。

## 后果

Guide v08 现在是可执行的验收目标，而不是预测表。真实结果若未达到某个 practical target，必须保留负结果并删去对应 claim 或修改方法；不能改变统计口径去贴目标。v07 继续作为实测参考版本保存，正式 `main.tex/main.pdf` 只接收真实结果。
