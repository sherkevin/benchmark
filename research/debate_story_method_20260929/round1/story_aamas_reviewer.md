# Round 1：故事线独立交叉回应

审查对象：Round 0 manifest 与以下三份独立报告：

- `round0/story_aamas_reviewer.md`
- `round0/method_theory_auditor.md`
- `round0/benchmark_novelty_auditor.md`

交叉范围：只挑战或修订其他报告中的判断，不修改三份生效主线文档。重点复核故事评价标准 v004 的 H4/H5/H8 与 S3/S4/S6。

## POSITION

Round 0 的核心拒稿风险是成立的：动态 JEV 尚未冻结，pre-call selector、post-call verifier 和 delayed updater 的 estimand 未分解，当前 direct selector 没有超过 Best Fixed，且 selected-only 反馈的不可识别性没有被一般性消除。对 H4（统一机制）、H5（独立可证伪证据）、H8（结论范围校准）以及 S3/S4/S6 的低评价，我**支持**。

但两份方法/benchmark 报告有几处把“必要的最低可审计条件”写成了“所有条件都必须满足才能继续故事”。我保留这些风险，同时**挑战并修订**其门槛：

1. 固定探索率 $\epsilon>0$ 是解决 selected-only 识别性的一个方案，不是唯一方案；应把它改成“声明并检验支持假设”。
2. 多事件排序、crash/restart、duplicate 等是实现恢复性要求；它们不能全部作为 H4/H5 的故事淘汰线。预登记确定性 tie-break 和独立 replay 可作为第一阶段最低要求。
3. 不应为了满足“最近邻完整”而强制运行所有 Delayed UCB、LQM/Correlation-Aware、GraphRouter、Laya-online。H3/S5 要求公平的最近邻边界和可审计对照，不要求对输入/反馈合同不相容的方法伪装成同任务 baseline。
4. “平均调用超过 1.25 就停止 verifier”不能脱离效用函数独立成立；应由预注册的错误、调用、延迟和拒答成本共同决定。

这些修订不会把当前故事升级为可投稿；它们只避免把论文是否成立错误地等同于某个具体工程方案是否全部完成。

## TARGET

本轮检查以下四个判断是否把评价标准误读成过强的必要条件：

- selected-only 反馈必须有显式 $\epsilon$ 探索；
- delayed updater 必须立即覆盖所有乱序/崩溃场景；
- 最近邻必须逐一实现为同合同 baseline；
- verifier 必须满足固定调用次数阈值。

同时检查一个反向风险：在放宽上述条件后，是否会削弱 H4/H5/H8 和 S3/S4/S6 的实质要求。

## EVIDENCE

### 交叉共识（支持）

- 两份独立报告都把 selected-only 的不可识别反例作为主要风险；这不是措辞问题，而是若不声明先验、探索、结构共享或事件观测，无法从已选反馈推断未选 provider。
- 两份独立报告都指出 pre-call selector 与 post-call verifier 的目标不同；confidence escalation 的 `0.923823 / 1.206 calls` 不能回填 direct selection，dynamic verifier 的 `0.900277 / 1.490305 calls` 也不能被包装成动态方法收益。
- 两份独立报告都认为 fixed-pool replay 不能自动支持 environment-nonstationary、arrival/retire/version recovery 或跨端点泛化；H8 与 S11 的范围校准要求成立。
- 方法 auditor 关于 Frozen 表示、初始化和预算不可互换的警告，直接关系 H4/S4 的 integration gain；如果没有共同表示的 full/ablation，跨方法 accuracy 差值不能归因于 delayed state。

## REASONING

### 交叉点 1：对“必须使用固定 $\epsilon$ 探索”的挑战与修订

**POSITION：修订（部分挑战，保留核心识别性警告）。**

**TARGET：** `method_theory_auditor.md:P0.1` 要求预登记固定探索率 $\epsilon>0$，并把它作为 selected-only 学习未选 provider 质量的最低方案。

**EVIDENCE：** 该报告的 P/Q 构造是有效的：若策略只选 A，两个环境在 A 上反馈相同、在 B 上优劣相反，任何 `U(H_t)` 都无法区分 B。当前主线也没有冻结探索、propensity 或共享结构（`02-方法论.md:10-14,32-38`）。但主线允许 fit-only prior、provider/context transfer、版本事件或可重复局部上下文作为额外可观测结构；这些机制可以在不采用统一 $\epsilon$ 的情况下提供对 B 的支持，只是必须明确假设和适用范围。

**REASONING：** “固定 $\epsilon$”是识别性的充分设计之一，不能被提升为问题定义的必要条件。强制所有方法使用相同 $\epsilon$ 还可能把贡献错误地限制成 exploration policy，而候选主张也可能是“在声明的 fit-only provider geometry 下进行条件适应”。真正不可接受的是不声明任何支持来源，却宣称普遍学习未选 provider；这会触发 H4/H5，而不是因为没有某个具体 $\epsilon$ 数值。

评价条款/分数影响：我**支持** P0.1 对不可识别性的结论，但**挑战**其“$\epsilon$ 是唯一最低方案”的强表述。按 v004，H4 要求统一接口/不变量与反例，H5 要求每条主张有可反驳测试；S4 暂保持 1/4、S6 暂保持 2/4。若声明 fit-only/transfer 假设并做反事实审计，H4/H5 不必因没有统一 $\epsilon$ 直接 FAIL；若既无探索也无可检验共享结构，H4 应 FAIL，S3/S4/S6 仍不得达到 3。

**DECISION TEST：** 把实验分成三种预注册合同，而不是只运行一种 $\epsilon$：

1. 无探索、无共享结构的 P/Q synthetic impossibility control，验证系统能标出不可识别；
2. 固定 propensity 探索合同，报告曝光、support 和 suffix accuracy；
3. fit-only provider/context prior 合同，使用 transfer-vs-permutation 与 held-out support 检验结构是否真的可迁移。

三种合同中只允许在对应假设下解释结果。若方法在合同 3 有信号但在合同 1 不可识别，应把主张写成“conditional adaptation under declared prior”，而不是普遍 selected-only learning。

**CONFIDENCE：** 0.93。对不可识别反例的支持很高；对“必须 $\epsilon$”的挑战为中高，因为最终选择取决于 Proposed 的实际信息来源。

### 交叉点 2：对“全量事件工程场景必须成为 H4/H5 门槛”的挑战与修订

**POSITION：修订（挑战过早淘汰，支持恢复性要求）。**

**TARGET：** `method_theory_auditor.md:P1.1` 要求在效果比较前冻结同 tick 多事件、乱序、retry、duplicate、crash/restart 等完整状态递推；`benchmark_novelty_auditor.md:6` 进一步要求独立 checker 对非法 trace 一律拒绝。

**EVIDENCE：** 延迟事件 schema、exactly-once、state hash 和恢复器确实是复现 selected-only claim 的必要基础。当前 Greedy verifier 已有 60/60 streams 通过，但尚未证明未冻结 Proposed 也遵守同一不变量。另一方面，故事评价标准的 H4/H5 关注的是统一机制和可证伪主张；它没有要求在首轮故事审查前覆盖所有 crash/restart 组合。只要顺序是预登记、确定性且独立 replay 可重建，部分工程异常可以作为 H6/H7/H10 的后续实现审计。

**REASONING：** 如果同 tick 事件的顺序会改变动作，必须定义排序键，否则结果确实不可复现；这是 H5 的硬要求。可是“任意合法乱序、崩溃后恢复、retry 语义”属于从协议到部署的扩展，不一定是中心算法机制。如果把所有异常场景都作为 H4/H5 的前置门槛，会把 benchmark/protocol 论文和方法论文都推迟到同一完整系统验收，失去最小可证伪路径。更合理的分层是：先要求单一预登记 tie-break、exactly-once、raw JSONL 独立恢复；随后把 crash/restart、retry、跨进程恢复作为鲁棒性/可复现性扩展。

评价条款/分数影响：我**支持**对事件顺序和 exactly-once 的严格要求，它保护 H5、H6 和 S6；我**挑战**把所有异常排列直接判作 H4 FAIL。若存在确定性 tie-break 和独立 replay，H4 可继续按“机制尚未验证”而非“协议必然无效”处理；S3 暂评 2/4、S4 仍受 integration ablation 限制、S6 可在有限事件合同下达到 2–3 之间。若连单一顺序和恢复都没有，H5 FAIL，S6 最高 1–2。

**DECISION TEST：** 第一阶段固定事件 schema、排序键、duplicate 处理和 restart checkpoint，要求同输入的合法 replay 字节级重建状态/动作；第二阶段再注入乱序、crash、retry、duplicate，报告是否保持等价。若第一阶段通过，故事可以继续做 matched method test；第二阶段失败时，收窄为“单进程可恢复 feedback-adaptive protocol”，不把问题整体判死。

**CONFIDENCE：** 0.90。事件合同缺口真实，但其优先级应按论文贡献类型分层。

### 交叉点 3：对“所有近期 baseline 都必须实现”的挑战与修订

**POSITION：挑战并修订（支持最近邻边界，反对无差别扩张主表）。**

**TARGET：** `benchmark_novelty_auditor.md:4` 建议补 Delayed UCB、LQM/Correlation-Aware、GraphRouter、Laya-online 等，作为 H3/S5 的最低 baseline 集合。

**EVIDENCE：** 当前已有 Random、Best Fixed、Oracle、Frozen、Online/Replay SGD、Greedy LinUCB、PILOT-adapted、BayesianRouter-adapted；Gate B 仍缺 Random 统一收据、至少一个静态强 router 和 Laya-online/fidelity（`current_acceptance_audit_v2/report.md:16-18,42-46`）。但报告自己也承认 CAMS/full-label、GraphRouter、RouteLLM、TREACLE 等方法输入或反馈合同不完全相容，不能混入医药主表。评价标准 H3/S5 要求最近邻输入、反馈、目标、失效条件和公平比较方式清楚，不要求每篇邻近论文都变成 runnable row。

**REASONING：** 若把所有候选都硬塞入统一主表，会出现两个相反风险：不忠实适配被误报为强 baseline，或把不可部署的 privileged/full-panel 方法和 selected-only 方法错误排名。对于 H3/S5，更小而诚实的集合通常足够：一个 same-function direct provider selector（Greedy/PILOT/BayesianRouter 中至少选可审计者）、一个 matched delayed online control（Online/Replay SGD）、一个静态强 router 或行为审计、一个 post-call verifier/cascade control、以及 Random/Best Fixed/Oracle。其他工作应写 nearest-work matrix，明确“不运行”的信息/目标不相容理由。

评价条款/分数影响：我**支持** H3/S5 不能只列引用，必须给出最近邻边界；我**挑战**“所有 Delayed UCB/LQM/GraphRouter/Laya-online 都必须实际运行”的必要性。S5 当前可暂评 3/4（文献与差异已较丰富），S4 不会因少跑一个不相容方法自动改善；H4/H5 只要求 strongest fair comparator 和可反驳主表。若连一个同功能 direct selector、一个 matched delay control 都没有，H3 FAIL，S5 降至 1–2；若已有审计矩阵且明确不可比，H3 可保持 UNKNOWN/补证据而非直接 FAIL。

**DECISION TEST：** 建立三层表：`deployable same-contract`、`privileged diagnostic`、`paper-behavior audit`。主 headline 只使用第一层；第二层用来给出上界/不可部署边界；第三层记录输入、反馈、目标和适配差异。补齐 Random receipt、一个强静态 router 与一个 canonical delayed control即可先测试 H4/H5；其余方法若不能公平实现，保留源文献核对和不纳入主表的理由。

**CONFIDENCE：** 0.88。需要主线 owner 决定“方法论文”还是“benchmark/protocol 论文”的比较强度；但无差别 baseline 扩张本身没有证据必要性。

### 交叉点 4：对“平均调用数 1.25 是 verifier kill threshold”的挑战与修订

**POSITION：挑战（保留停止思想，修改为 utility-based gate）。**

**TARGET：** `method_theory_auditor.md:P1.4` 和其停止线建议：dynamic verifier 在 ≥2/3 delay 低于 static 或平均 calls>1.25 时停止。

**EVIDENCE：** 静态 confidence escalation 的开发结果为 accuracy `0.923823`、平均调用 `1.206`；dynamic verifier 为 `0.900277`、平均调用 `1.490305`，并且已有负结果。可见当前 dynamic verifier 不值得继续调参。但 `1.25` 的含义依赖调用价格、延迟惩罚、拒答损失和目标是 accuracy、risk–coverage 还是 net utility；这些权重尚未在主线冻结。

**REASONING：** 固定 calls 阈值适合作为内部资源保护线，不能自动成为论文 falsification 条件。一个 verifier 可能以 1.30 calls 换来足够大的 accepted-risk 降低而仍有正 utility；反之 1.20 calls 也可能因延迟和拒答成本而不值得。H5/S6 需要的是预登记唯一 utility 和最强同成本对照，而不是脱离效用的常数。当前负结果仍应保留，但其解释应是“在现有成本权重和静态对照下失败”，不是动态 verifier 在所有 utility 定义下被永久证伪。

评价条款/分数影响：我**支持** P1.4 对 confidence 跨 provider 校准、第二调用归因和 dynamic verifier 负结果的警告；我**挑战**把 `1.25` 当通用 kill criterion。H5 与 S6 的正确动作是冻结 `U = accuracy - λ_calls - ρ_latency - κ_refusal`（或明确 risk–coverage），再按 paired CI/ROPE 判定。否则 S6 只能 2/4；若 utility 已冻结且负差值超出 ROPE，H5 可判 PASS，即使平均 calls 不是 1.25。

**DECISION TEST：** 在 fit 区冻结 provider-specific calibration 和唯一成本函数，比较 static threshold、random same-cost、always-second、dynamic verifier；报告 first-call accuracy、final accepted risk、coverage、calls、latency、refusal 和 utility。以 utility 差值及 CI 作为停止线；`1.25` 只保留为资源上限或敏感性切点。若 dynamic 在现有预注册 utility 下 ≥2/3 delay 低于 static，停止 dynamic verifier；若只是 calls 超过 1.25 但 utility 仍优于对照，不能仅凭 calls 判死。

**CONFIDENCE：** 0.91。已有负结果可信，但固定调用阈值的普适逻辑不足。

### 交叉点 5：对“只能保留 benchmark/protocol 负结果”的过度收窄

**POSITION：修订（不提前宣布方法成立，也不提前宣布算法线必死）。**

**TARGET：** 两份报告多次建议：只要 full 没有超过 Best Fixed/strongest baseline，就永久停止 dynamic-JEV method claim，只保留 benchmark/protocol 与负结果。

**EVIDENCE：** 当前 direct selector 确实没有超过 Best Fixed，dynamic verifier 也有负结果；但故事评价标准允许 `contribution_type=interface or protocol / empirical evidence`，并允许 integration_gain 是 recoverability、latency、coverage 或 auditability，而非只能是 accuracy。`01-故事线与创新.md:53-64` 已明确“证据不足就缩小主张”，没有规定必须把所有论文降成负结果。

**REASONING：** “不超过 Best Fixed”足以杀死“动态 JEV accuracy 优越性”主张，但不必自动杀死一个明确的 protocol/interface 贡献，或一个以安全风险—成本曲线、可恢复性、事件后恢复为主指标的实证贡献。反过来，protocol 贡献也需要独立 comparator、事件审计和 confirmation 资格，不能用日志完整替代科学价值。更精确的 kill rule 应按 contribution type 分支：accuracy method 失败就降级；若 recoverability/auditability 或风险覆盖在同成本下有独立正结果，保留接口论文；若所有预登记 utility 都失败，再收窄为负诊断。

评价条款/分数影响：这涉及 **H4/H5/H8** 与 **S3/S4/S6** 的分层。H4 允许统一接口/不变量形成 protocol 贡献；H5 允许独立反例或实验；H8 要求不把 fixed-pool 结果泛化。因而当前 S4=1 适用于“算法创新”，但 protocol 分支可在完成 recoverability/成本实验后达到 2–3；S6 仍必须为每种 contribution type 写唯一指标与 kill condition。若没有任何 integration gain（包括可恢复性、覆盖或成本），才应永久停止所有“整体贡献”叙述。

**DECISION TEST：** 在 candidate ledger 中分别登记三种 contribution type：`algorithmic accuracy`、`risk/cost verifier`、`interface/protocol recoverability`。每种只选一个主 utility、一个 strongest fair comparator 和一个 falsification condition。若 accuracy 分支失败而 protocol 分支在独立 replay、tamper rejection、恢复时间和同合同成本上通过，则论文只能以 protocol/negative-evidence 形式写；若三者都失败，再执行 benchmark-only 收窄。

**CONFIDENCE：** 0.86。该修订是对评价标准贡献字段的直接应用，但 protocol 是否有足够 AAMAS significance 仍需后续独立判断。

## DECISION TEST

本轮保留的最小共识闸门如下：

1. **H4/S4：** 必须明确 contribution type。若写 algorithmic method，需共同表示的 full/去组件消融、不可识别反例和 $Δ_{int}$；若写 protocol/interface，需把收益改成 recoverability、成本、延迟或 auditability，并与无事件/无状态控制比较。
2. **H5/S6：** 每个主张只绑定一个 estimand、一个 strongest fair comparator、一个 utility/指标和一个 kill condition。selector、verifier、updater 不得共享 pooled accuracy 作为唯一证据。
3. **H8/S3：** `dynamic_scope` 至少写成 `feedback-adaptive`，除非完成预登记 event/recovery；BBB fixed-pool 结果不能外推到所有科学工具、自然 drift 或 model-fresh 泛化。
4. **最近邻最小集合：** 先闭合 same-contract direct selector、matched delayed control、static strong router/behavior audit、verifier control、Random/Best Fixed/Oracle；不相容工作写 nearest-work matrix，不伪造公平运行。
5. **识别性：** 不强制单一 $ε$，但必须声明 support 来源（propensity、fit-only prior、context sharing 或 event signal）并运行 P/Q synthetic impossibility control。
6. **事件恢复：** 首阶段只需确定性 tie-break、exactly-once 和独立 replay；后续再扩展 crash/restart/乱序鲁棒性。任何没有可重建事件顺序的实现直接触发 H5 FAIL。

## CONFIDENCE

### Round 1 结论

我对 Round 0 的主要拒稿判断保持支持：当前故事不具备可投稿的方法论文状态，H4/H5/H8 与 S3/S4/S6 尚未同时达标。Round 1 的修订是防止把可选工程实现误当成逻辑必要条件：

- 不把固定 $ε$、所有异常恢复场景、所有近期 baseline、固定 calls=1.25 视作普遍必需条件；
- 仍要求声明信息支持、可恢复事件顺序、最强公平对照、独立 estimand 和范围边界；
- 允许在 dynamic-JEV accuracy 失败后保留一个明确的 protocol/interface 或 risk–cost 贡献，但不允许用“可审计”四字代替 integration gain。

临时判断：**HOLD / REWORK**。若按上述分支重新登记主张，S3/S4/S6 有可能从 2/1/2 提升到 3/2–3/3；在当前 evidence 未补齐前，不得把任何分支标为 STORY READY 或 SUBMISSION REVIEW READY。

保留的未解决分歧：protocol/interface contribution 是否足以满足 AAMAS significance，不能由本轮故事审查单独裁定；需要后续 benchmark/agent utility 与投稿定位审查。
