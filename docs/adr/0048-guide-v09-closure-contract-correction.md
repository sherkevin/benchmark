# ADR-0048：Guide v09 作为可关闭验收的统计与暴露修正版

- 状态：Accepted（研究执行目标；真实结果仍须逐项复核）
- 日期：2026-10-08
- 取代：ADR-0047（仅就 Guide 的 closure contract 取代；v08 文件和审查记录保留）
- 关联：`paper/aamas2027/experiment_tables/guide/versions/v09_20261008/guide.tex`
- 关联标准：`docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md` v006

## 背景

反方审查确认 v08 已经覆盖了大部分证据，但仍不能保证“真实矩阵填完即可关闭”：E1 只有方向和 fold 数，没有最小任务 headroom；E4 允许 pooled 压力掩盖单个 delay 失败；E2/E3 没有唯一的 primary estimand、统计单位和多重比较规则；并且项目中已有 157 条 confirmation 的 provider 输出和结果回执，不能把它们无条件称作 untouched。baseline 是否有可运行 modern nearest，以及正文数字是否来自候选自己的 raw receipt，也需要变成硬检查。

## 决议

1. Guide v09 是当前唯一生效的 Benchmark+Baseline closure target。v08、v07 和更早版本完整保留，只作为历史/参考，不能再作为关闭合同。
2. E1 固定任务有效性下限：fit-only Best Fixed 减 Random 的 pooled paired effect 至少 `+2.0 pp`，95% paired CI 下界至少 `0`，且 5 个 outer fold 中至少 3 个 fold 的 effect 至少 `+1.0 pp`。点估计按 fold 等权，fold 内按 scaffold-clustered molecule pair 计算。
3. E2 的唯一 primary comparator 是 `matched_online_sgd`；E3 的唯一 primary ablation 是 `mira_relative_reliability`，只删除 context-relative residual，保留 feature、menu、prior、seed、budget 和 release clock；E4 的 primary comparator 仍为 `matched_online_sgd`，并对 `D={0,1,4}` 逐层判定。
4. E2--E4 的主要点估计按共同 stream/order strata 等权，不能按 event 行加权；区间使用保持配对轨迹的 scaffold-cluster bootstrap（10,000 draws，固定 seed）。E4 的三个 delay contrast 构成一个 Holm--Bonferroni `q=.05` family；其他 baseline 和分层结果是 secondary，不能事后升级为 primary。
5. E4 除 pooled practical margin 外，每个 delay 的 primary contrast 必须至少 `+1.0 pp`，调整后 CI 下界至少 `0`；任何提前释放反馈或 delay contract 失败直接 HOLD。
6. E6 的“未读”定义为：confirmation truth、逐题 correctness、方法/延迟聚合结果和由其触发的决策在 candidate freeze 前不可见。输入和冻结 provider 前向输出可以为重放而缓存，但若曾影响方法、阈值、epoch、baseline 或 claim 选择，manifest 必须标记 `previously_inspected`，不能承担最终 E6。最终 E6 必须附 `exposure_receipt.json`；当前已查看的 157 条只有在 exposure audit 证明未发生决策影响时才可复用，否则使用新 sealed manifest。
7. 关闭包必须有可运行的 modern nearest（当前 canonical 为 `bayesianrouter`；若 freeze 前无法恢复，`lin_ts_adapted` 必须升级为 primary，不能用论文均值替代），并有 `candidate_artifact_checker.json` 校验每个正文表/图的 `run_id`、`method_id`、contract/input/source hash、primary comparator 和 exposure status。
8. E5 的 `p95 <= 2x Frozen` 且 `<=100 microseconds` 只约束单线程、batch=1 的 selector+update policy timer；provider inference、I/O 和端到端服务延迟单独记录，不能用 kernel cap 代替。

## 后果

如果真实矩阵、scorecard、两份独立复核和上述收据全部通过，Benchmark+Baseline 验收可以写 `CLOSED`；这时不需要另加一个隐含的“看起来合理”门。若任一硬条件、primary margin、逐 delay、exposure、artifact checker 或 independent recovery 失败，状态保持 `HOLD`，保留负结果并修方法/收窄 claim，不能改统计口径追分。
