# Guide v09：Benchmark 与 Baseline 选型审查

- 审查日期：2026-10-09
- 审查范围：只读审查 `versions/v09_20261008/guide.tex`、`README.md`、ADR-0048，并与当前生效 Benchmark 文档、baseline roster/eligibility、fresh confirmation 和已有 raw receipt 对照。
- 执行边界：本审查没有运行实验、没有打开 confirmation、没有修改 Guide 或正式研究文档。
- 审查性质：方法审查，不是 AAMAS 官方意见，也不是对最终录用的承诺。

## 结论

**Benchmark 选型：对“声明的 BBB 一次调用动态 routing 切片”是条件性 PASS；对“当前已经可以关闭 Benchmark+Baseline”是 HOLD。**

TDC BBB 的 truth 来源、P-C1 冲突处理、scaffold-disjoint 候选和 selected-only 延迟回放协议都有可追溯证据。把公共 truth layer 与项目自建 routing layer 分开，是正确的建模方式。问题在于 v09 还没有把 development 的 33-provider 菜单和投稿 E6 的四个 fresh provider 菜单逐个实验固定，也没有把当前 baseline roster 中的所有角色差异完整写进 Guide。因而目前只能说“这是一套可审计的公开数据 routing protocol”，不能把它写成已经被社区公认的独立 benchmark，也不能用 33-provider development 结果替四-provider confirmation 背书。

**Baseline 选型：角色设计合理，但 v09 与实际 baseline 证据尚未完全对齐，故 Gate-B closure 仍 HOLD。**

Random、fit-only Best Fixed、Retrospective Best Fixed、Frozen 和 matched Online-SGD 能分别承担下界、强固定控制、评估器上界、静态控制和普通在线更新控制；Greedy LinUCB 与 BayesianRouter-adapted 提供可运行的近期 online/router 对照。这一骨架足以检验“动态反馈是否超过普通更新”。但是：

1. v09 把 `matched Online-SGD / Replay` 放在同一 baseline 角色和 E2 主比较描述中，而当前 eligibility matrix 明确 Replay-SGD 使用历史 BT prior，尚未通过当前 common-direct-prior 的严格等价检查；它不能与 matched Online-SGD 合并为一个 primary comparator。
2. 当前生效 Benchmark 文档和 source manifest 将 PILOT 作为最近的概念对照，但 v09 的 baseline 表漏掉了 `PILOT-adapted`，没有说明是必须跑、单独 likelihood stratum，还是有理由排除。
3. GraphRouter 和 Laya/JEV-online 目前只有 audit/blocking 证据，不能写成已完成 online baseline；v09 应把它们的 audit-only/out-of-scope 身份和理由直接固化，否则“baseline 名单完整”仍需读者猜测。
4. BayesianRouter、Greedy 和 PILOT 的适配不是原论文的原生复现。已有合同正确地禁止跨 likelihood raw ranking，但 v09 的正文级表格还需要逐行绑定适配差异和独立 raw/recovery receipt。

因此，**v09 经过下列修订后才可作为“真实矩阵填完即可关门”的合同；在修订之前不能给 Benchmark+Baseline CLOSED。**

## 1. 可支持的事实（Evidence）

### 1.1 Truth 与 routing protocol

- 当前生效范围明确为公开 TDC BBB truth、四个 checkpoint-bound fresh providers、157 个 scaffold-disjoint confirmation molecules、一次调用、selected-only feedback 和 $D\in\{0,1,4\}$：
  `docs/研究主线/生效/03-Benchmark与Baseline.md`。
- TDC BBB test 的 canonical conflict、旧 development panel 的 molecule/scaffold overlap 和 157 条候选 manifest 均有独立清单与 hash：
  `task/task49-pharma-benchmark-baseline-freeze/gate_a_confirmation_manifest_v1/report.md`。
- fresh provider 菜单实际是 ECFP-LogReg、ECFP-MLP、ECFP-RandomForest 和 Graph message passing，四个 family；natural/stress raw events 和 replay verification 均存在：
  `provider_fresh_confirmation_v1/report.md`、`provider_fresh_drift_stress_v1/report.md`。
- provider fresh natural 结果没有 MIRA 相对 Frozen 的提升，controlled correctness-drift stress 有正向 recovery。该差异已经被当前报告正确区分为自然 null 与 evaluator-side stress，而不是 generic online gain。

这些证据支持“任务可以作为一个公开 truth 上的可审计 routing protocol”。它们尚不支持“所有 provider 已达到 model-fresh 外部资格”，也不支持“自建 routing layer 是社区公认 benchmark”。

### 1.2 Baseline 的实际状态

当前 roster/eligibility 和 readiness 报告支持下列事实：

| 角色 | 当前证据 | 审查解释 |
|---|---|---|
| Random、Best Fixed、Retrospective Best Fixed | 有 development anchor/评估器控制 | 角色清楚，但不应作为 online learning baseline 计数。 |
| Frozen、matched Online-SGD | strict common-prior logistic receipt；独立恢复通过 | 可作为 E2 的唯一主要 comparator 候选。 |
| Replay-SGD | 有完整 development replay，但历史 BT prior 与当前 common-direct-prior 不同 | 只能作 separate historical/matched control，除非同合同重跑。 |
| Greedy LinUCB | 60 streams、三 delay、独立 verifier；适配为当前任务的 selected-only 版本 | 可运行的现代 adapted comparator；不能写成原论文零先验复现。 |
| BayesianRouter | native-BT prior receipt、60 streams、独立 verifier；adapted Gaussian posterior | 可运行的现代 adapted comparator；必须保持单独 likelihood stratum。 |
| PILOT-adapted | readiness 报告称已有 development replay/负结果 | v09 漏列；需要明确纳入 secondary 或写明排除理由。 |
| GraphRouter | 输入语义、单任务图 contract 和依赖仍阻塞 | 只能 audit-only；不能用弱化图模型代替。 |
| Laya/JEV-online | 静态表示诊断可用；没有 selected-only online update API/receipt | 只能静态表示诊断或 out-of-scope；不能写成在线 JEV baseline。 |

证据入口：
`task/task49-pharma-benchmark-baseline-freeze/baseline_role_roster_v1/roster.json`、
`baseline_eligibility_matrix_v2/matrix.json`、
`baseline_readiness_v1/report.md`、
`baseline_adapters_v0.md`。

## 2. 主要可证伪性与辨识性问题

### P0：development 33-provider 与 final 4-provider 的 estimand 没有在 v09 逐项绑定

v09 第 2 节把 33-provider Task36 叫 development、四-provider 叫主投稿切片；但 E1--E5 表格没有为每个实验写清楚使用哪个 menu/input manifest。E1 的 `Best Fixed-Random` headroom 若在 33-provider 上成立，不能自动证明四-provider confirmation 上存在同样 headroom；反过来也一样。E2 的 modern adapted receipts目前主要是 33-provider development，而 Stage-B fresh confirmation 是四-provider。

**为什么会影响辨识性：**同一个“accuracy improvement”可能来自菜单大小、provider lineage、输入 cohort 或先验不同，而不是动态更新机制。若不把 menu 绑定到每个 estimand，审稿人无法知道比较的是同一个任务。

**必须的决定测试：**为 E1--E6 增加机器字段 `scope_id`、`input_manifest_sha256`、`menu_provider_ids` 和 `provider_family_manifest_sha256`。至少明确：

- E1--E5 是 33-provider development 诊断，还是四-provider fresh 主实验；
- E6 只承担四-provider final confirmation；
- 两个 scope 不做 pooled raw ranking，且正文表中不混用其分母、标准差或 CI。

### P0：Replay-SGD 被 v09 描述成与 matched Online-SGD 同一 primary role

Guide 第 3 节写的是“Matched Online-SGD / Replay”，E2 也把 `SGD/Replay` 并列；然而当前 `baseline_eligibility_matrix_v2/matrix.json` 明确记录 Replay-SGD 的 `warm_start` 为历史 BT `score.weight`，缺少严格 feature/warm-start manifest binding，`comparison_allowed` 是 `separate_paired_control`。这不是文字差异，而是 prior 变化会改变冷启动和后续反馈的 estimand。

**决定测试：**

- primary comparator 只固定为 `matched_online_sgd`，并要求其与 candidate 使用同一 direct prior、feature、menu、feedback clock、seed 和 update budget；
- Replay-SGD 单列为 secondary replay control；
- 若要把 Replay 纳入 strict main，必须先做同一 common-direct-prior 的新 receipt，不能重解释旧回执。

### P1：现代 nearest 的名单不完整，且“可运行”与“可公平比较”混在一起

BayesianRouter 和 Greedy 的适配已经有运行证据，但其 likelihood/prior 语义与 strict logistic stratum 不同。PILOT 在 source manifest 中被标为“closest conceptual baseline”，而 v09 没有任何行说明它的 status。GraphRouter/Laya 的阻塞也只存在于 roster/report，不在 Guide 本身。这样会造成两种相反误读：审稿人可能以为漏掉了最近方法，或以为 audit-only 方法已经完成。

**决定测试：**Guide 增加一张“modern baseline role crosswalk”，逐行写 `method_id`、论文/源码 commit、adaptation、likelihood/prior stratum、raw receipt、recovery、comparison_allowed、不能支持的 claim。最低要求：

- `matched_online_sgd`：strict primary comparator；
- `greedy_linucb_adapted`、`bayesianrouter_native_bt`：runnable adapted secondary；
- `pilot_adapted`：纳入 runnable secondary，或记录经共同确认的排除理由；
- `replay_sgd`：separate historical/secondary，除非重跑严格合同；
- `graphrouter`、`laya_online`：audit-only/out-of-scope，并保留阻塞报告。

### P1：E1 只有 pooled accuracy headroom，容易被类别比例掩盖

157 条 fresh confirmation 中 truth 为 1 的有 121 条、为 0 的有 36 条（`bbb_confirmation_candidate_manifest_v1/manifest.csv`）。仅用 selected correctness 和 Best Fixed−Random 的 +2 pp 下限，不能区分方法是否只利用多数类；也不能保证 headroom 在 minority class、failure/unknown 或 provider family 上存在。

**决定测试：**E1 至少同时输出 label-stratified correctness/balanced accuracy、每个 fold 的 class counts、failure/unknown 分母和 provider-family macro。`+2 pp` 仍可保留为总体任务可辨识性门，但不能代替分层可解释性；若某层分母过小，应标记为 descriptive 而不是伪造显著性。

### P1：future-Y 与独立 stream 的定义还不够硬

v09 规定 selected-only release 和 scaffold-cluster bootstrap，但没有写出独立 stream 的最低证据：当前若干“seed”是 deterministic protocol slots，不能自动当成独立随机重复；delay 条件也不是独立样本。fresh receipt 的 240/150 streams 是可复核的轨迹数，但仍需区分 `fold`、`policy_seed`、`delay`、`order_id`，并明确 correctness label 是 action 之后才到达的 future-Y。

**决定测试：**每个 primary contrast 的 raw manifest 必须有 `stream_id`、`fold_id`、`policy_seed`、`delay`、`order_id`、`future_label_source` 和 `independence_class`。至少保留外层 scaffold split 与真正的策略随机性/到达顺序分层；若 seed 只是 deterministic slot，统计合同不能把它描述成独立随机 seed。任何 future-Y 在 decision 前可见、或由 confirmation aggregate 生成的标签，都使该 stream 失格。

### P1：public truth 的权威性不能替 routing protocol 的社区认可

TDC BBB 是公共 truth 来源，但 Task36 provider routing layer 是本项目构建的实验协议。v09 已有一句“不把自建回放层直接称作社区 benchmark”，这是正确的边界；但关闭条件仍把“Benchmark+Baseline CLOSED”作为整体，没有要求正文明确使用“public-data routing protocol/benchmark construction”而不是“公认 benchmark”。

**决定测试：**在 Guide 和正文 baseline 段固定一句 scope：公共层是 TDC BBB，routing layer 是本项目公开、可重放的 protocol；所有 benchmark validity claim 只由 E1/E6 和完整 release artifact 支持。若论文坚持把它称为已有公认 benchmark，则本选型应 HOLD，直到有外部标准 routing benchmark 的同合同复现。

## 3. 推荐的最小修订集

这些修订不要求立即跑新实验，但在任何 closure scorecard 之前必须落入 v10 或等价的机器合同：

1. **scope crosswalk**：E1--E6 逐行绑定 33-provider development 或四-provider fresh，输入、菜单、family、分母和能支持的 claim 不混用。
2. **baseline role crosswalk**：把 Replay、PILOT、GraphRouter、Laya 的角色和完成条件写入 Guide；禁止将 Replay 继承为 strict matched primary。
3. **modern fidelity fields**：每个 adapted baseline 的论文/源码 commit、适配差异、likelihood/prior、调参预算、raw events、recovery 和不能支持的 claim 逐行绑定。
4. **future-Y/independence receipt**：每条 primary stream 写清 future label 的 release tick 和独立性类别；确定性 schedule、delay、fold 不冒充随机重复。
5. **class/family sensitivity**：E1 增加 label-stratified/balanced 与 family-macro 报告；整体 accuracy 门仍可作为必要而非充分条件。
6. **closure role rule**：只有所有声明为 primary 的 baseline 都有 live parity raw/recovery，且 adapted/ audit-only 行明确不进入 primary ranking，才允许 Gate-B PASS。

## 4. 当前判定表

| 项目 | 判定 | 依据 |
|---|---|---|
| 公共 truth 权威性 | PASS（声明范围内） | TDC source/version/hash、P-C1 conflict audit、157 candidate manifest。 |
| routing protocol 可审计性 | PASS（声明范围内） | selected-only、release clock、failure semantics、fresh raw/replay receipts。 |
| benchmark 任务有效性 | CONDITIONAL / HOLD for closure | E1 menu 未绑定；总体 accuracy 未要求 class/family sensitivity；self-built routing layer 不能称社区公认 benchmark。 |
| 控制 baseline 覆盖 | PASS（角色设计） | Random、Best Fixed、Retrospective、Frozen、matched SGD 覆盖基本反事实。 |
| 现代 nearest 覆盖 | CONDITIONAL / HOLD | Greedy/Bayesian 可运行；PILOT 漏列；GraphRouter/Laya 需显式 audit-only。 |
| strict fairness | HOLD | Replay prior mismatch；adapted likelihood stratum 需逐行隔离；fresh 与 development scope 需分开。 |
| 独立性与 future-Y | CONDITIONAL / HOLD | raw streams 存在，但 v09 缺 minimum/independence receipt 和 future-label 字段。 |
| 当前能否关闭 Benchmark+Baseline | **HOLD** | 上述 P0 缺口和 E6 exposure/primary live-parity 证据尚未全部闭合。 |

## 5. 对论文主张的边界建议

在修订完成前，最多支持以下表述：

- “我们在公开 TDC BBB truth 上构建了一个可重放的 one-call, selected-only delayed-feedback provider-routing protocol。”
- “在四-provider fresh menu 的自然 confirmation 上，MIRA-Relay 与 Frozen 持平；在预登记 evaluator-side correctness-drift stress 上，动态反馈能够恢复 provider ranking。”
- “Greedy LinUCB 和 BayesianRouter 是当前任务的 adapted modern comparators；它们的结果不冒充原论文原生复现，并与 strict logistic controls 分层报告。”

不能从当前选型证据直接推出：

- 33-provider development 的 gain 可迁移到四-provider final confirmation；
- Replay-SGD 与 matched Online-SGD 是同一 strict primary comparator；
- Laya/JEV 已经有可运行的 selected-only online baseline；
- TDC truth + 自建 routing layer 已经等同于社区公认 benchmark；
- natural deployment drift、hERG/AMES 或 agent-level tool-use gain 已被该矩阵验证。

**最终审查意见：**v09 的方向是对的，且比 v08 接近可执行合同；但它目前仍是“条件合格的目标规格”，不是已被现实证据闭合的 benchmark+baseline 验收。先完成 scope/role/future-Y crosswalk，再填 closure scorecard；在此之前保持 HOLD，不应通过换 denominator、混合 likelihood 或继承旧 receipt 来提前关门。

