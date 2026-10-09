# Benchmark + Baseline 验收目标 Guide

当前唯一生效阅读版为 **v09**，固定入口是 [guide.pdf](guide.pdf)。v09 是一份“真实矩阵填完后可以关门”的验收合同；它不是效果预测，也不把旧实验数字当作候选方法结果。v07、v08 和更早版本完整保留在 `versions/`，其中 v07 是实测分布参考，v08 是已被反方审查修正的历史候选。

## v09 解决什么问题

之前的 Guide 还存在四个会让“通过”变得含糊的地方：任务有效性没有最小 headroom，pooled 压力可以掩盖单个 delay 失败，主估计量和多重比较没有唯一合同，已经查看过的 157 条确认记录也不能无条件叫 untouched。v09 把这些变成硬条件，并增加两个防回填的收据：

- `candidate_artifact_checker.json`：逐表/逐图核对 `run_id`、`method_id`、contract/input/source hash、primary comparator，禁止复制 v07 或旧策略的 SD、covariance、win rate、order 分布。
- `exposure_receipt.json`：记录 final cohort 的 manifest hash、freeze commit/time、truth/aggregate access audit、decision-impact audit 和 cohort 状态。已有 157 条只有通过这个检查才可承担最终 E6，否则开新的 sealed manifest。

## 关闭规则

Guide 只有在以下证据全部真实完成后才允许写 `CLOSED`：

1. H1–H10 全部 PASS；
2. B1–B8 每项至少 3 分，B9–B15 每项至少 2 分且不得为 0，总分至少 49/60；
3. Gate-A、Gate-B、Gate-C 的四个子项各至少 3 分、总分至少 12/16；
4. E1–E6 通过，包含主 baseline 的同合同 raw/recovery、E1 headroom、E2/E3 主比较、E4 三个 delay 的逐层判定、E5 成本/失败/恢复、final sealed E6；
5. 正文 `paper/aamas2027/main.tex#tab:confirmation` 与 candidate-specific raw evidence 一一对应；
6. `closure_scorecard_v09.md`、两份独立复核、`candidate_artifact_checker.json` 和 `exposure_receipt.json` 都完成，且两位复核者都推荐 `CLOSED`。

这些条件全部满足时，Guide 已经完成它的 Benchmark+Baseline 验收职责，不再另设隐藏门。任一条件失败都保留为 `HOLD`，不能换统计口径贴目标。

## v09 的核心实验合同

- **E1 任务有效性**：Best Fixed−Random pooled paired effect 至少 `+2.0 pp`，95% paired CI 下界不低于 0，且 5 个 outer fold 至少 3 个达到 `+1.0 pp`。
- **E2 主比较**：唯一 primary comparator 是 `matched_online_sgd`；controlled MIRA−SGD 至少 `+2.0 pp`，CI 下界至少 `+0.5 pp`。自然场景只做 `−1.0 pp` non-inferiority，不能把受控压力写成自然 superiority。
- **E3 机制归因**：唯一主要消融是 `mira_relative_reliability`，只删除 context-relative residual，其他输入、先验、预算和 release clock 不变；full−ablation 至少 `+1.0 pp`，CI 下界不低于 0。
- **E4 动态范围**：`D={0,1,4}` 逐层检查；每层 primary contrast 至少 `+1.0 pp`，Holm–Bonferroni 调整后 CI 下界不低于 0，不能用 pooled 正数覆盖某层失败。
- **E5 成本边界**：单线程、batch=1 的 selector+update policy p95 不超过 Frozen 的 2 倍且不超过 `100 μs`；provider inference 和端到端服务时间单独计时。
- **E6 最终确认**：四个 fresh providers × scaffold-disjoint final sealed cohort；truth、correctness、聚合结果和由其触发的决策必须在 candidate freeze 前不可见。已有 157 条若被证明影响过决策，只能作为历史/冻结参考。

E1 的 fold 内配对、E2–E4 的 stream/order strata 等权、scaffold-cluster bootstrap（10,000 draws）、primary/secondary 标记和 multiplicity 规则，都写在 PDF 和 [ADR-0048](../../../../docs/adr/0048-guide-v09-closure-contract-correction.md) 中。

## 文件

- 目标 PDF：`guide.pdf`
- 当前源稿：`guide.tex`
- v09 修订：`v09_修订说明.md`
- 机器清单：`manifest.json`
- 评分模板：`closure_scorecard_v09.md`
- 复核模板：`closure_review_author_v09.md`、`closure_review_replicator_v09.md`
- 状态模板：`closure_decision_v09.json`
- 历史审查：`reviews/guide_v08_adversarial_closure.md`、`reviews/guide_v08_closure_completeness_review.md`

Guide 与正式论文结果隔离：目标稿不写入 `main.tex/main.pdf`，真实结果必须有自己的 raw/config/recovery 和 hash。每个小任务结束时把状态和下一步写入 `TODO_LOG.md`。
