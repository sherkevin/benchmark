# Round 1：方法与理论交叉反驳

- 审查角色：method / theory auditor
- 日期：2026-09-29
- 输入：`research/debate_story_method_20260929/round0/README.md`、`round0/story_aamas_reviewer.md`、`round0/benchmark_novelty_auditor.md`，以及当前生效方法评价标准 `docs/研究主线/生效/评价标准/02-方法论评价标准.md`
- 目标：逐条挑战或修订其他报告中的方法假设；不修改生效主线

## 总体立场

POSITION: revise

TARGET: Round 0 的共同判定：当前不应把动态 JEV 写成已验证算法；但后续收口时需要区分“方法尚未定义”“固定池反馈自适应不可识别”“环境非平稳证据缺失”“准确率未胜出但其它 utility 可能改善”四个不同问题。

EVIDENCE: `story_aamas_reviewer.md` 临时评分 62/100，强调 H4/H5/H8 未过、方法未冻结；`benchmark_novelty_auditor.md` 要求 integration ablation、family 控制、强近邻和 dynamic event；方法评价标准规定 H1–H10 逐项 PASS/FAIL/UNKNOWN，M5/M8/M13/M15 需至少 3 分，且通过不能靠其它分数抵消（`02-方法论评价标准.md:29-44,60-71,87-101,121-129`）。

REASONING: 两份报告对“应暂停泛化主张”意见一致。需要修订的是若干过强的二元规则：方法未冻结时 H4 应是 UNKNOWN/HOLD，不自动等于算法已被证伪；`dynamic_scope=feedback-adaptive` 不要求自然 version drift；“超过 Best Fixed”应绑定预先声明的 utility，而不只能绑定 pooled accuracy。下列六条分别给出支持、挑战或修订，并指定 H1–H10、M5/M8/M13/M15 的影响。

DECISION TEST: 只接受能把方法假设写成输入、状态、事件、目标和反事实对照的修订；任何修订都必须在同一日志合同、同一菜单/预算和独立恢复器下重跑。若新定义无法落到唯一 estimand 和 kill criterion，保留 Round 0 的 HOLD。

CONFIDENCE: 0.95；判断同时受两份 Round 0 报告和生效评价标准约束。

## R1.1 关于“当前 H4 应直接 FAIL”

POSITION: challenge / revise

TARGET: `story_aamas_reviewer.md` 对流程拼接的判断，即 H4 应立即判 FAIL，S4 只能 1/4。

EVIDENCE: 故事报告指出组件组合缺少 integration gain，并写道“若没有完整组合相对去接口/去状态对照，H4 应判 FAIL”。方法评价标准却把 H4 的直接 FAIL 条件写成“只能换名称、无法给出整体机制必要性、消融或 integration gain”（`02-方法论评价标准.md:33-44`）；同一标准要求逐行填 PASS、FAIL 或 UNKNOWN，UNKNOWN 先补证据，不能按通过处理（`02-方法论评价标准.md:29-31`）。当前方法文档明确最终状态、探索和 backbone 尚未冻结（`02-方法论.md:10-14`）。

REASONING: 对一个尚未冻结的候选，已有 evidence 只能证明“当前没有满足 H4 的证据”，不能证明任何可能的 Proposed 组合必然只有换名字。若现在写 H4=FAIL，会把“方法定义缺失”与“已定义方法被反例推翻”混为一谈，也会让后续真正的统一状态/不变量实验无法被公平审查。更严格的状态应是：H4=UNKNOWN/HOLD，当前 S4≤2；只有候选冻结后仍无统一接口、状态不变量、matched ablation 或 integration gain，才升级为 FAIL。反过来，若方法最后只是协议日志，没有改变策略计算对象，则 H4=FAIL，贡献降为 protocol/interface。

评价条款/分数影响：方法论 H4、H10；M5、M8、M13 现阶段最高 2 分，不能进入方法通过；故事线 S4/S6 只能给 1–2 分。该修订保留“不得写成创新”的结论，同时避免提前宣判尚未存在的算法。

DECISION TEST: 冻结一个唯一 Proposed 后，逐项检查：(i) 输入是否超出 nearest baseline；(ii) 状态是否改变决策计算对象；(iii) 去掉 interface/state 是否有预登记反事实差值；(iv) full 是否在同成本 utility 上有 integration gain。四项均无时 H4=FAIL；只缺证据时 H4=UNKNOWN/HOLD；至少一项有独立、可恢复、可反驳证据时才可能 H4=PASS。

CONFIDENCE: 0.92；评价标准明确区分 UNKNOWN 与 FAIL，但最终候选尚未存在，因此是评分状态修订而非为当前方案辩护。

## R1.2 关于“必须超过 Best Fixed accuracy 才能保留方法”

POSITION: challenge / revise

TARGET: `benchmark_novelty_auditor.md` 的 K1，以及 `story_aamas_reviewer.md` 对“至少超过 Best Fixed 与强近邻”的统一 accuracy 门槛。

EVIDENCE: Benchmark 文档的主指标虽为 future correctness，但同时要求 regret、calls、latency、update p50/p95、memory、failure/unknown，并明确若 accuracy 不提高但调用成本、延迟、覆盖或可恢复性按预登记目标改善，仍可评价为整体贡献（`03-Benchmark与Baseline.md:57-68`；故事评价标准 `01-故事线与创新.md:105-121`）。方法评价标准 M8 也把任务相关收益列为 integration gain，可是主张必须有 matched controls（`02-方法论评价标准.md:91-101`）。

REASONING: Best Fixed accuracy 是 direct selection 的强 incumbent，但不是所有可能的 JEV claim 的唯一 estimand。若目标是同等 correctness 下减少调用、延迟或状态内存，强制 `accuracy > Best Fixed` 会错误杀掉有价值的 efficiency/recoverability 方法；若目标明确是 accuracy，则必须超过 Best Fixed 和最强 matched baseline。正确规则是先冻结 utility，例如
$$U=\mathrm{correctness}-\lambda_c\,\mathrm{calls}-\lambda_l\,\mathrm{latency}-\lambda_r\,\mathrm{refusal},$$
或冻结多目标 Pareto/非劣准则，然后在相同信息和预算下比较。utility 不能事后选择：若方法提高 accuracy 但超过调用上限，仍应判失败；若只提高 recoverability，主张必须改为接口/工程贡献。因而 K1 的“accuracy ≥ Best Fixed”应修订为“对声明的 estimand 超过最强合法对照并通过预登记成本约束；若声明 direct-selection correctness，才要求 accuracy ≥ Best Fixed”。

评价条款/分数影响：方法论 H4、H6、H10；M5、M8、M13、M15。没有预登记 utility 和成本边界时 M8/M15 最高 2 分；有 utility 但只和弱 baseline 比较仍不通过 M5/M12。benchmark B12/E5 的 calls、latency、memory、refusal 不能被 pooled accuracy 替代。

DECISION TEST: 在唯一 frozen candidate 上预登记一个主 utility 和一个 accuracy guardrail；同时报告 Best Fixed、Matched Online/Replay、strongest modern baseline、Audit-Frozen 和 verifier controls。要求 utility 的 cluster-bootstrap CI 超过 strongest matched baseline，accuracy 不低于预登记 ROPE、平均调用≤`1.25`（如主张二次调用），且 4/5 folds 同向。若只 accuracy 不胜出但 utility 非劣，论文改写为成本/恢复性贡献；若二者都不胜出，kill 方法 claim。

CONFIDENCE: 0.94；这是对“主张类型决定 kill metric”的修订，不改变当前 dynamic selector 尚未通过的事实。

## R1.3 关于“没有自然动态事件就必须删除 dynamic”

POSITION: challenge / revise

TARGET: 两份 Round 0 报告把 fixed-pool replay 缺少自然 drift 与删除“dynamic”并列，似乎要求任何 dynamic claim 都必须有 arrival/retire/version。

EVIDENCE: 方法评价标准明确区分 `feedback-adaptive` 与 `environment-nonstationary`：前者只要求 selected-only correctness 延迟释放后改变状态和后续动作；后者才要求 arrival、retire、version shift 或 distribution shift（`02-方法论评价标准.md:73-81`）。当前主 benchmark 确实没有自然版本证据，验收审计将 event stress 列为 Gate C 缺口（`current_acceptance_audit_v2/report.md:17-18,50-53`）。

REASONING: 因此两种结论应分开：固定 provider 菜单上，若 release→state→next-action 可重建，使用 `dynamic_scope=feedback-adaptive` 是有定义且可检验的；不能把它写成 environment-nonstationary 或自然 provider evolution。要求自然 drift 才能使用“dynamic”会过度收紧标准，把已明确允许的 feedback-adaptive 类误杀。相反，若故事继续说“工具版本变化、arrival/retire、非平稳环境”，Round 0 的挑战完全成立，必须补 event receipt 与 Frozen-pool 对照。Controlled version event 只能是机制正控，不是自然动态证据。

评价条款/分数影响：方法论 H9、M10、M15。正确收窄 scope 后，H9/M10 可以在 feedback-adaptive 范围内通过；未收窄时 M10/M15 最高 2 分。故事线 H8/S11 的范围校准取决于术语是否准确，Benchmark B7/Gate C 仍需声明事件缺口。

DECISION TEST: 同时做两个预登记表：(A) fixed-pool feedback-adaptive：delay `{0,1,4}`、pending queue、selected-only release、state hash、next-action recovery；(B) environment-nonstationary extension：arrival/retire/version event、Frozen-pool/Reset-on-event/oracle、event-after recovery。A 通过可保留 `feedback-adaptive`；B 未通过不得使用 natural drift/dynamic environment。若 A 也没有 release→state→next-action 证据，则删除 dynamic 一词。

CONFIDENCE: 0.96；这是直接依据生效评价标准的术语修订，能同时保留固定池在线研究与限制环境主张。

## R1.4 关于“provider family 控制应替代 pooled 主结果”

POSITION: support / revise

TARGET: `benchmark_novelty_auditor.md` 要求 family-macro、family-balanced menu、leave-family-out，否则不保留 general provider adaptation claim。

EVIDENCE: 33 个 provider 中 24 个 B3clf 变体高度相关，Benchmark 文档禁止把它们当成 24 个独立模型家族（`03-Benchmark与Baseline.md:29-33`）。主指标要求按 endpoint 和 provider family 分开报告；baseline 评价标准 B11/B12 要求 family correlation、exposure、成本和不确定性（`03-Benchmark与Baseline评价标准.md`）。

REASONING: 我支持把 family 相关性作为必要敏感性和不确定性审计，但不建议事后用 family-balanced menu 替换已定义的 primary estimand。换菜单会改变任务的 action distribution；family-macro 的目标是估计“均匀家族权重”下的性能，pooled accuracy 估计的是当前部署菜单下的 molecule-level policy value。两者都可以报告，不能在看结果后选更有利者。leave-family-out 也只在声称跨 family transfer 时是硬门槛；若主张范围是固定 deployed menu，应该把 family 结果作为边界与方差证据。

评价条款/分数影响：方法论 H6、H10、M12、M13、M15；不报告 family/exposure 会使 M12/M13 最高 2 分；报告但未预登记主 estimand 会触发 H10 的选择性报告风险。benchmark B11/B12 需要 pooled + family macro + exposure 三者并列。

DECISION TEST: 在方法冻结前注册 primary estimand（推荐 molecule-level deployed-menu utility）和 two sensitivity estimands（family-macro、leave-family-out）。所有方法用同一菜单、同一 family mask 和同一预算；报告 family exposure、provider selection share、fold/family cluster CI。若主结果只在 pooled 存在且 family-macro/leave-family-out 反向，删除跨-provider/family 泛化，只保留 deployed-menu 结论。

CONFIDENCE: 0.93；family 相关是硬方法风险，但其处理方式应是并列 estimand 而非事后替换。

## R1.5 关于“必须添加更多 baseline 才能证明 novelty”

POSITION: revise

TARGET: `benchmark_novelty_auditor.md` 建议补 Delayed UCB、同功能 provider router、CAMS、GraphRouter、RouteLLM、TREACLE 等一批基线。

EVIDENCE: 当前合同已有 C0–C8 角色分层，要求 matched Online/Replay、Greedy、PILOT、BayesianRouter、Frozen、Best Fixed、Oracle、verifier controls；Gate B 当前缺 Random 统一收据、至少一个强静态 router 和统一 raw-run contract（`03-Benchmark与Baseline.md:41-55`；`current_acceptance_audit_v2/report.md:16-18`）。评价标准 M5/M12 要求最近邻充分且公平，不要求无限增添未匹配方法。

REASONING: “更多 baseline”不是自动的识别修复。Delayed UCB 若与 binary correctness、single-action、selected-only contract 不同，只能做行为审计或透明适配，不能冒充原方法；CAMS 的 full-label/active-label 权限若更强，应作为 privileged control；GraphRouter/RouteLLM 若输入 provider description、cost 或图边语义未冻结，重写会失去 fidelity。相比扩充名单，更关键是完成一个 canonical delayed comparator、一个同表示 matched head、一个强静态 router 和 verifier random-cost control。过多未匹配基线还会扩大 multiple-comparison 与计算负担。

评价条款/分数影响：方法论 H3、H6、H10；M5、M12、M13 关注“最强合法近邻”和公平合同，不是数量。若 baseline 未明确输入/反馈/预算，M12 最高 2 分；若只缺一两个同任务强对照，M5/H6 HOLD，但无需把所有外部方法都做成主表。

DECISION TEST: 为每个候选填写 `input/feedback/action/cost/fit labels/online labels/fidelity/license` 矩阵，按 deployable、privileged、diagnostic 分层。只有同一 action、selected-only、delay、预算且可恢复的方法进入主表；其它方法保留行为审计。先补 Random receipt、Delayed UCB 或等价 canonical delay control、一个强静态 router和同成本 verifier random null；若 Ours 仍不过最强合法集合，停止继续扩 roster。

CONFIDENCE: 0.91；这条是对 baseline 完整性的范围修订，不否定 Round 0 对强近邻的核心要求。

## R1.6 关于“当前可发表核心自动转为 benchmark/protocol”

POSITION: challenge / revise

TARGET: `story_aamas_reviewer.md` 和 `benchmark_novelty_auditor.md` 建议若 dynamic-JEV 方法失败，就直接保留 benchmark/protocol + negative diagnosis 作为可发表核心。

EVIDENCE: 当前三门 Gate A/B/C 仍 HOLD；Gate A 缺 endpoint、lineage、confirmation overlap，Gate B 缺完整主 baseline，Gate C 缺 E1–E5、paired uncertainty、dynamic event 和一次 confirmation（`current_acceptance_audit_v2/report.md:10-18,42-53`）。Benchmark 标准要求 truth/provider/event manifest、独立恢复和一次性 confirmation 才能支撑主结论（`03-Benchmark与Baseline评价标准.md:36-48,57-66,144-163`）。

REASONING: “没有新算法”不等于 benchmark/protocol 已自动完成。若没有独立 truth/lineage、同合同 baseline、family/endpoint 边界和 raw-event recovery，负结果也可能只是自建回放的实现/样本 artefact。更稳妥的修订是把它称为候选 study direction：先完成 Gate A/B/C，若所有方法 kill，才将论文类型正式改为 protocol + negative diagnosis；而不是在当前 development 状态下把 benchmark 贡献当成既成事实。该修订保护了 Round 0 的“不要硬保 JEV”结论，也防止用另一种标题绕过验收。

评价条款/分数影响：方法论 H1、H6、H7、H8、H10；M13、M14、M15 在 Gate A/B/C 未收口前最高 2 分。Benchmark B1–B8 任一低于 3 时只能写 development/stress，不得写主结果；故事线 H8/S11 需随新论文类型重新评分。

DECISION TEST: 建立 protocol-only 的独立验收：truth/version/hash、conflict policy、provider lineage、selected-only checker、family macro、主 baseline、成本/失败分母、raw JSONL recovery、molecule/scaffold-disjoint confirmation。全部通过后，若唯一 Proposed 在 kill line 失败，才转写 protocol/negative-study；任一资格失败则只能报告内部 development，不进入投稿主张。

CONFIDENCE: 0.95；这是由项目自身 Gate 定义推导出的必要条件。

## H1–H10 / M5-M8-M13-M15 交叉矩阵

POSITION: clarify

TARGET: 交叉报告对方法评价标准的共同通过状态。

EVIDENCE: Round 0 两份报告分别指出 H4/H5/H8 UNKNOWN、H9/B7 dynamic scope 风险、baseline fairness 和 integration ablation 缺口；本轮上述六条分别覆盖 H1–H10 的输入、时钟、信息边界、标签、可反驳性、匹配、公平、confirmation、dynamic scope 和负结果要求。

REASONING: 当前建议矩阵如下：

| 条款 | 当前状态 | 本轮解释 |
|---|---|---|
| H1 输入/动作/反馈/终止/失败可执行 | UNKNOWN/HOLD | 状态/事件/label 仍有候选冲突（R1.1、R1.5） |
| H2 pre-call 无 truth/未选输出/未来反馈 | UNKNOWN→需反例验证 | 合同写明约束，但必须做 injection checker |
| H3 公式/伪代码/实现一致 | UNKNOWN/HOLD | 多事件 queue、failure 语义、上游 fidelity 未闭合 |
| H4 机制相对近邻有统一接口/必要性 | UNKNOWN/HOLD，不先判 FAIL | 候选未冻结；冻结后无 integration gain 才 FAIL（R1.1） |
| H5 核心主张有独立反驳条件 | UNKNOWN/HOLD | selector/verifier/updater estimand 和 kill metrics 需拆分 |
| H6 matched baseline、初始化、预算公平 | HOLD | common feature、cost、family、verifier controls 缺口仍在 |
| H7 raw event→state→metric 可恢复 | UNKNOWN/HOLD | Greedy 收据不等于未冻结 Proposed 收据 |
| H8 confirmation 冻结且不回流 | HOLD | Gate A/C 未过，不能自动转 protocol 论文 |
| H9 dynamic_scope 与事件匹配 | `feedback-adaptive` 可检验；`environment-nonstationary` 未通过 | 不能用固定池支撑自然 drift（R1.3） |
| H10 每条机制有 falsification，负结果保留 | HOLD | 需冻结 utility/ROPE、候选 ledger、family sensitivity |
| M5 机制必要性/近邻差异 | ≤2/4 | 无统一状态、matched ablation 或 canonical nearest neighbor 不得给 3 |
| M8 状态改变计算对象 | ≤2/4 | 仅日志/模块拼接不算；utility 可是 accuracy、cost 或 recoverability，但必须预登记 |
| M13 消融/反事实/失败边界 | ≤2/4 | 需要 full、去 state/interface、Audit-Frozen、event/no-event、family null |
| M15 主张校准与适用边界 | ≤2/4 | 建议收窄为 fixed-pool feedback-adaptive；protocol-only 也需 Gate A/B/C |

DECISION TEST: 只有 H1–H10 全部 PASS、M5/M8/M13/M15 至少 3 分、且方法总分满足项目闸门，才进入 confirmation。若 H4 从 UNKNOWN 变 FAIL，或 H6/H8/H10 任一 FAIL，停止效果型实验并先修合同；不能用 H9 的 scope 收窄抵消 H4/M5 的机制缺失。

CONFIDENCE: 0.96；矩阵逐项对应生效评价标准硬门槛。

## Round 1 决策建议

POSITION: revise

TARGET: 下一轮 coordinator synthesis 的合并规则。

EVIDENCE: 两份 Round 0 报告对“当前不支持已验证 dynamic-JEV algorithm”一致；本轮只修订判定粒度：H4 未冻结时 UNKNOWN 而非直接 FAIL，`feedback-adaptive` 与 `environment-nonstationary` 分层，accuracy kill criterion 改为 claim-specific utility，protocol-only 仍须完成 Gate A/B/C。

REASONING: 建议 coordinator 采用以下优先顺序：

1. 先冻结 `dynamic_scope=feedback-adaptive` 或 `environment-nonstationary`，不得混写；
2. 再冻结 reward label（`system_success`、`chemical_correct`、failure cost 分开）与唯一 primary utility；
3. 再冻结一个 Proposed 和一个 canonical matched nearest neighbor，完成 full/去 state/去 interface/Audit-Frozen 消融；
4. 最后按 H1–H10、M5/M8/M13/M15 重新评分，确认是否进入一次性 confirmation。

当前仍建议 HOLD；若候选无法提供 selected-only 的识别条件、独立恢复和 matched utility gain，最终应改写为 Gate 完成后的 benchmark/protocol + negative diagnosis，而不是把流程重新命名成 JEV。

DECISION TEST: synthesis 必须保留本轮三项分歧：

- **是否需要自然 event 才能称 dynamic**：不需要；自然 event 只对 environment-nonstationary 分支是硬条件。
- **是否必须 accuracy 超过 Best Fixed**：只对 direct-selection accuracy claim 是硬条件；其它 utility 必须提前冻结并满足成本/公平约束。
- **H4 当前是 FAIL 还是 UNKNOWN**：方法尚未冻结时是 UNKNOWN/HOLD；冻结后无 integration gain 才升级 FAIL。

CONFIDENCE: 0.95；这些是对两个 Round 0 报告的针对性修订，不改变当前 Gate A/B/C HOLD。
