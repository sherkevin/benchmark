# Round 2：两个未决冲突的最终立场

审查基于当前生效故事线、方法论、Benchmark/Baseline、`current_acceptance_audit_v2` 以及 Round 0/1 交叉报告。本文不修改生效主线。

## A：现在收窄为 protocol/negative diagnosis，还是保留 method candidate？

### POSITION

**现在就把投稿主故事收窄为 protocol + negative diagnosis；保留一个被隔离、可证伪的 method candidate，但不能把它当作主线已成立的方法。**

这不是把动态 JEV 永久判死，而是把未有正向结果的候选从 headline、摘要和 confirmation 叙事中降级为条件分支。只有一条冻结候选在同合同 CPU development gate 通过后，才重新打开 method headline；通过后仍需独立 confirmation 才能写成已验证方法。

### TARGET

冲突在于：

- 继续保留 dynamic-JEV candidate，可能让全文继续围绕尚未成立的优越性组织；
- 立即删除 candidate，则可能把一个尚未完成最小证伪实验的方向过早降格，丢失清晰的可检验假设。

### EVIDENCE

1. 当前 development master table 中，Best Fixed=`0.9155` 仍高于 Matched Online-SGD、Replay、Greedy、PILOT 和 BayesianRouter；方法论明确写着普通 selected-only selector、transfer、memory 和 risk gate 没有形成正向主方法证据。
2. confidence escalation 的 `0.923823`、平均调用 `1.206` 是 post-call static verifier 证据，不能回填为 pre-call selector 或动态 JEV 结果；dynamic verifier 的 `0.900277`、平均调用 `1.490305` 反而是负结果。
3. Gate A/B/C 仍 HOLD：provider lineage、统一 baseline、动态 event、paired uncertainty 和独立 confirmation 尚未闭合。最终状态参数、更新规则和探索规则仍未冻结。
4. 组件级 prior art 已覆盖 contextual provider routing、selected-arm online update、delayed feedback、JEV observe、cascading 和 selective verification。没有正向 integration gain 时，把流程命名为 dynamic JEV 会越过 H4/H5/H8 与 S3/S4/S6 的边界。
5. 评价标准 v004 允许 contribution type 为 `interface/protocol` 或 `empirical evidence`，并不要求所有论文都必须是 accuracy-improving algorithm；但 H4 仍要求统一接口/不变量和 integration gain，H5 仍要求独立可证伪证据，H8 仍要求 scope 与阶段一致。

### REASONING

如果现在继续把 method candidate 放在中心，主张会随着下一轮结果在 selector、verifier、event recovery 和 protocol 之间移动，形成事后选故事。当前最诚实、风险最低的主线是：

- 公开 truth + 具体 provider routing protocol 的可复核 benchmark；
- selected-only delayed feedback 的信息边界和不可识别性；
- 强固定 incumbent 下普通在线更新和 transfer 的负诊断；
- post-call confidence 作为独立 verifier 信号，但不与 pre-call selection 混写。

候选仍应保留在 candidate ledger 中，因为没有完成“一条唯一冻结候选 + matched CPU gate”前，不能把“失败”扩展成整个 dynamic-JEV 方向不可能。保留候选的条件是：它必须有独立方法卡、唯一 utility、nearest comparator、support/propensity 假设和 kill criterion，并明确标为 exploratory/conditional。

评价影响：现在主故事可把 H8/S11 的范围校准修到可审查状态；但 method branch 的 H4/S4 和 H5/S6 仍是 UNKNOWN，直到 integration ablation、不可识别反例和强 matched baseline 完成。若继续把候选写成主方法，H8 应 FAIL，S4 不能超过 1/4、S6 不能超过 2/4。

### DECISION TEST

做一个**唯一候选、单次 CPU、同合同**的决定性 matrix：

1. 冻结一个 Proposed commit、特征、初始化、探索/support、failure/unknown 标签、delay `{0,1,4}`、调用预算和 utility；不得再从多候选中挑选。
2. 同时跑 `Best Fixed`、同表示 `Frozen`、`Audit-Frozen`、Matched Online/Replay、最强可审计 modern router 和 Proposed；保存 raw JSONL、独立恢复和 paired molecule/scaffold 结果。
3. 主门槛：三个 delay 中至少两个相对 strongest matched baseline 的预注册 utility 差值达到 ROPE 外（当前内部建议可沿用绝对 `+0.01`、至少 `4/5` folds 同向），且没有额外调用/标签泄漏。

判定：

- **未通过：** 立即锁定 protocol/negative-diagnosis 主故事，method candidate 只留在失败候选和附录审计，不再申请效果型 A800。
- **通过：** 只恢复“conditional method candidate”主张；先完成 Gate A/C 与一次独立 confirmation，确认前不能写“dynamic JEV 已有效”。
- **仅 post-call verifier 通过：** 主故事仍先写 protocol + verifier evidence，selector candidate 继续降级，不能用 verifier 正结果拯救 selector headline。

### CONFIDENCE

0.94。当前 negative development、方法未冻结和 Gate HOLD 都是已记录事实；保留候选而不把它写成主方法，是对“过早放弃”和“过度 overclaim”两种风险的最小同时控制。

## B：统一主结果是否必须把 selector 和 verifier 拆成两篇故事？

### POSITION

**不必立即拆成两篇论文，但必须先拆成两个 estimand、两个 RQ 和两个结果块；只有在二者各自具备独立 novelty/evidence，且组合没有额外 integration gain 时，才考虑分稿。**

当前建议是一篇 protocol-centered 论文：selector 与 verifier 是同一工具调用协议的两个时间点，但不能共用一个 pooled accuracy headline。统一稿件是否成立，取决于一个预登记的 net-utility 组合实验，而不是模块名称。

### TARGET

冲突在于：

- 若不拆，post-call 第二工具和 pre-call 选择会造成信息、成本和标签泄漏的混淆；
- 若直接拆成两篇，可能把同一 truth、provider panel、事件合同和负结果切成两个不足以独立成立的故事，形成 salami slicing。

### EVIDENCE

1. 方法论已经明确两个函数和不同输入：
   `a_t=π_sel(x_t,e_t,A_t,H_t)`；
   `v_t=π_ver(o_t,c_t,s_t)`。
   selector 在调用前工作，verifier 只能读被选 provider 的 output/confidence/status。
2. 当前正信号只在 post-call static confidence escalation；direct selector 未超过 Best Fixed，dynamic verifier 反而低于 static verifier。说明两条线目前不能用一个 pooled score 互相背书。
3. 两份 Round 0 审查都指出：第二次调用改变动作集合和成本；若只报最终 accuracy，always-two-call、confidence gate 和 selector 改善不可区分。
4. 但二者仍共享真实问题和协议：工具正常返回不保证正确、反馈可能 selected-only 且延迟、调用后证据会影响是否复核/拒答。若 selector 和 verifier 在同一预算下存在正的组合 interaction，拆成两篇反而会丢失实际 agent 决策链。

### REASONING

“拆 estimand”是必须的；“拆论文”不是。论文可以有一条上位问题：科学工具调用中的可靠性控制；其下分两个 RQ：

- RQ1：pre-call selected-only delayed feedback 是否改善未来 provider choice？
- RQ2：在同一 first-call selector 和调用预算下，post-call confidence/second-call/refusal 是否改善 accepted risk–coverage？

组合主张只在二者的净 utility 和交互项得到支持时成立。若只有 verifier 有正结果，最诚实的单篇结构是 protocol + verifier evidence + selector negative diagnosis；若只有 selector 有正结果，则 verifier 变成辅助控制。若二者各自都没有正结果，单篇仍可写系统性负诊断，但不能把“selector+verifier 流程”称为已验证方法。

直接拆成两篇的门槛应更高：每篇必须有自己的 failure、最近邻、独立 utility、强对照、confirmation 和 AAMAS significance；共享一个 benchmark 和一组 negative result 不足以构成两篇。当前材料显然还没有达到这个门槛，所以现在分稿会增加叙事重复和统计依赖。

评价影响：拆 estimand 直接修复 H5/S6，并帮助 S3 因果链；统一稿件是否保留组合创新取决于 H4/S4 的 integration gain。H8 要求将 selector、verifier 的适用范围分别限制为 fixed-pool/lineage-aware development 或已完成的 confirmation，不能因同稿而扩大范围。

### DECISION TEST

在同一冻结 development stream 做一个**2×2 factorial**，并把所有成本记入同一 utility：

| first-call selector | verifier | 目的 |
|---|---|---|
| Best Fixed / Frozen | off | selector incumbent |
| Proposed selector | off | pre-call selection 增益 |
| Best Fixed / Frozen | static threshold / random same-cost | post-call verifier 控制 |
| Proposed selector | static/dynamic verifier | 组合流程 |

每组报告：first-call chemical correctness、final accepted correctness/risk、coverage/refusal、calls、feedback count、latency、failure/unknown 和预注册 `U = accuracy - λ_calls - ρ_latency - κ_refusal`（或明确的 risk–coverage utility）。对 delay `{0,1,4}` 做 molecule/scaffold paired uncertainty，并报告组合交互项：

\[
\Delta_{int}=U(\text{Proposed selector+verifier})-
\max\{U(\text{Proposed selector only}),U(\text{best verifier control}),U(\text{Best Fixed})\}.
\]

判定：

- **selector 和 verifier 都无独立增益：** 统一稿只能是 protocol/negative diagnosis，不能拆成两篇方法论文。
- **只有一个模块通过：** 保留一篇，以通过的模块为主故事；另一个模块作为 negative/ablation，不单独成稿。
- **两模块都通过但 $Δ_{int}\le 0$：** 一篇可以写两个互补 RQ，但删除“组合机制”创新；不必拆稿。
- **两模块都通过且 $Δ_{int}>0$，并通过 Gate A/C 与 confirmation：** 保留一篇统一 agent workflow 论文，selector/verifier 作为两个明确章节和结果块；只有在两条线的目标、数据和近邻完全独立且统一稿件过长时，才评估分稿。

### CONFIDENCE

0.92。两个模块的时间顺序和证据边界已在方法论中明示；“不必立即拆稿”是由共享协议和潜在 interaction 推出的结构判断，最终仍由 2×2 实验决定。

## FINAL POSITION

1. **A 的最终答案：** 主投稿叙事现在收窄为 protocol/negative diagnosis；method candidate 保留为隔离的、单候选、可证伪条件分支。未通过 CPU gate 前，不把 dynamic JEV 写成主方法。
2. **B 的最终答案：** 不立即拆成两篇；先在一篇中拆 estimand、RQ、成本和证据。只有独立贡献和组合 interaction 的结果都清楚后，才决定统一稿或分稿。
3. 未解决点：protocol/interface 贡献是否达到 AAMAS significance，不能由本轮单独裁定；需要后续 benchmark authority、agent utility 和投稿定位复核。
