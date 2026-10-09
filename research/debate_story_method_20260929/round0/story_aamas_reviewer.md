# Round 0：AAMAS 故事线独立审查

审查对象：`docs/研究主线/生效/01-故事线与创新.md`、`02-方法论.md`、`03-Benchmark与Baseline.md`、`task/task49-pharma-benchmark-baseline-freeze/current_acceptance_audit_v2/report.md`。

审查范围：只判断中心主张是否已经形成可发表、可证伪且与方法一致的论文故事；不修改生效主线，不把 development 证据写成 confirmation，不替未冻结的方法补事实。

## POSITION

当前故事有一个真实且可证伪的问题定义，但还没有形成可投稿的方法论文中心主张。最强、最诚实的当前结论是：项目已形成“公共 truth + provider routing protocol + delayed selected-only feedback”的审计框架，并有一组能解释失败边界的 development 负结果；“动态 JEV 工具适应流程”仍是未冻结候选，不是已成立的创新。

建议编辑决定：**HOLD / Major Revision**。若投稿稿件仍把动态 JEV 写成已验证方法，按 AAMAS 方法论文标准应拒稿；若把贡献收窄为可复核 benchmark/protocol 和经过强基线检验的负诊断，则故事可以继续发展，但这会是不同的论文定位。

依据生效评价标准 v004 的临时 Round 0 评分为 **62/100（未达到 80 分返工线）**：H4/H5/H8 当前为 UNKNOWN 或待补证据，S4=1、S6=2、S3/S8/S9=2；因此至少触发 REWORK，不能进入 confirmation。该分数只评当前生效主线与现有 development 收据，不是 AAMAS 录用预测。

临时逐项分数（0–4；权重按 v004）：`S1=3, S2=4, S3=2, S4=1, S5=3, S6=2, S7=3, S8=2, S9=2, S10=3, S11=3, S12=3`。H1/H2/H6/H9 暂可视为 PASS；H3/H7/H10 需绑定最终稿件证据；H4/H5/H8 为 UNKNOWN，不能按通过处理。

## TARGET

中心主张被审查为：

> 在科学工具正常返回但可靠性未知时，调用前从 provider 菜单选择一个工具，获得延迟且只针对已选工具的 correctness 反馈，并把调用前选择、调用后验证/升级和后续状态更新连成一个可审计流程；在相同信息和预算下，该流程应比最强合法对照更可靠。

该主张包含四个可独立证伪的子主张：

1. provider 之间存在真实、可复核且可救的 correctness 差异；
2. selected-only、delayed feedback 能改变后续选择，而非只重述静态路由；
3. post-call confidence/状态能在相同调用成本下减少错误或安全地触发复核；
4. 三者组合形成超出 Frozen、Best Fixed 和近期 online router 的机制收益，并能在独立数据上复现。

## EVIDENCE

### 支持点

- **问题现实性有证据。** BBB development replay 给出 Best Fixed `0.915512`、Oracle `0.9972` 的 headroom，说明固定工具与完整面板之间存在可测差距（`current_acceptance_audit_v2/report.md`；`01-故事线与创新.md:43-49`）。
- **信息边界写得清楚。** 方法文档明确动作先于当前结果、只释放已选 provider、反馈可延迟、失败/未知真值分账（`02-方法论.md:8-24,56-62`）。这使任务可复现，也给出直接的泄漏判据。
- **模块分离有利于证伪。** selector、provider adapter、post-call verifier 和 delayed updater 具有不同输入输出，理论上可以分别做消融（`02-方法论.md:26-54`）。
- **负结果没有被隐藏。** Matched Online-SGD 只相对自身 Frozen 有小幅改善，仍低于 Best Fixed；Greedy、PILOT、BayesianRouter、transfer、memory 和普通 risk gate 尚未形成稳定 direct dynamic gain（`02-方法论.md:64-68`；`current_acceptance_audit_v2/report.md:20-25`）。
- **验证信号与选择信号有意识地区分。** confidence escalation 的开发 accuracy `0.923823`、平均调用数 `1.206` 被明确标为 post-call 证据，未回填成 pre-call selection（`01-故事线与创新.md:47-51`；`current_acceptance_audit_v2/report.md:24-25`）。
- **评价合同方向正确。** Benchmark 文档要求同菜单、同 delay、同 selected-only 权限、逐事件 JSONL、固定成本和一次性 confirmation（`03-Benchmark与Baseline.md:35-39,57-74`）。这为小实验解决争议提供了可操作入口。

### 既有 prior-art overlap（不构成“未搜到”证明）

本地一手来源审查已把以下近邻列为直接重叠：PILOT（selected-arm online routing）、PAK-UCB（带 context 的在线生成器/模型选择）、Greedy LinUCB/BayesianRouter（provider-specific online selection）、Cascade Routing（调用后继续 cascading）、AnyJev（闭式 JEV head 与 observe 更新）、Joulani 等 delayed-feedback 变换、META-DES/OLA（局部能力与拒答/选择）、KMTL-UCB/GOB.Lin/多任务 bandit（共享工具/任务结构），以及 AAMAS 2026 的 truthful reverse-auction provider selection。证据和一手链接见 `research/AAMAS2027_医药工具选择最近邻与方法审查.md`、`research/AAMAS2027_Problem_Formulation_Neighbor_Review.md`、`research/动态JEV创新性与线性状态审查_20260925.md`、`task/task49-pharma-benchmark-baseline-freeze/baseline_contract_v1.md`。

这些近邻并不证明本文一定没有增量，但说明“contextual routing + selected feedback + delayed update + abstain/second call + 小状态”每一项都已有先例。创新必须落在一个可重现实验或形式性质上：例如新的信息合同下的可识别性条件、明确的更新/安全门机制，或在匹配预算和反馈权限下稳定超越强近邻。

## REASONING

### Objection 1：中心创新目前是流程拼接，尚未达到方法贡献

**POSITION**

把 selector、verifier、delayed updater 放进同一审计流程是合理的系统设计，但不能自动成为新算法；如果没有一个独立机制带来可测收益，审稿人会把它归为已有 contextual routing、delayed bandit、selective prediction 和 cascading 的组合。

**TARGET**

`01-故事线与创新.md:32-41` 的“把信息边界、实时更新和调用后可信度连成一个可审计流程”是否足以支撑 AAMAS 方法论文的原创性。

**EVIDENCE**

当前方法文档仍把最终 backbone、状态参数化、探索规则和 selector/verifier 联合训练列为未决；普通 selected-only 更新候选均未过 development 主闸门（`02-方法论.md:10-14,46-68`）。最近邻审查明确指出，单独的 online head、closed-form JEV、delayed feedback、risk/abstain 和 cascading 均已覆盖。

**REASONING**

“流程可审计”是重要的可复现属性，但它改变了评测合同，不等于改变了学习问题的统计结构。若最终方法只是固定表示接标准 SGD/RLS/LinUCB，再加 confidence threshold 和第二工具调用，任何正结果都可能来自更强的静态表示、额外调用预算或 post-call information，而不是组合机制。当前结果恰好没有超过 Best Fixed，因此故事还没有一个可归因的因果干预。

评价条款/分数影响：这直接命中 **H4（统一接口、反例、消融和 integration gain）**，并使 **S4 创新必要性暂评 1/4**；若没有完整组合相对去接口/去状态对照，H4 应判 FAIL。也会压低 S3 因果链和 S6 可证伪主张。

**DECISION TEST**

冻结一个唯一 Proposed（完整伪代码、初始化、探索、状态、delay 处理和成本），在同一菜单/特征/seed/delay/预算下运行 `Frozen`、`Best Fixed`、Matched Online/Replay SGD、Greedy LinUCB、PILOT、BayesianRouter、`verifier-only`、`selector-only` 和 `full`。必须预注册逐组件消融，并给出 `full - strongest matched baseline` 的 paired uncertainty；若 full 只超过 Random/Frozen 或没有可归因的组件交互，删除“动态 JEV 方法创新”主张。

**CONFIDENCE**

高。该问题直接由方法未决项、现有负结果和已核对的近邻文献共同支持。

### Objection 2：把 pre-call selection 与 post-call verification 合成一个主结果会混淆 estimand 和预算

**POSITION**

调用前选择和调用后验证拥有不同可见信息、动作和标签成本。把 confidence、第二工具和拒答放进同一流程后，accuracy 提升不能直接归因于 delayed selected-only update；它可能只是多调用或使用了当前返回后的信息。

**TARGET**

`01-故事线与创新.md:10-12,32-41` 以及 `02-方法论.md:26-44` 对“同一 JEV 流程”与“选择/验证两个时点”的叙事衔接。

**EVIDENCE**

方法文档确实定义了 `a_t=π_sel(...)` 与 `v_t=π_ver(o_t,c_t,s_t)` 的不同输入，但实验边界仍把 direct selection、post-call verification 和 delayed updater 放在同一总故事下。confidence escalation 的 `0.923823`/`1.206` 是开发证据，不能回填 direct selection（`current_acceptance_audit_v2/report.md:20-25`）。验证成本、拒答损失和第二 provider 的失败/延迟语义尚未在主表闭合。

**REASONING**

如果 post-call verifier 只在已调用后工作，最合适的 estimand 是 risk–coverage 或 net utility；pre-call selector 的 estimand 是未来 episode correctness/ regret。两者共享日志不代表共享主指标。若第二调用成本不计入同一预算，组合可以通过“多买一次信息”获得看似提升；若拒答成本没有定义，accuracy 也不能代表决策可靠性。当前故事用一个“更可靠地使用工具”句子覆盖两个不同因果问题，容易被审稿人判作指标偷换。

评价条款/分数影响：命中 **H5（每条核心主张有独立证伪测试）**、**H8（结论范围与阶段一致）** 和 **S6 可证伪主张**；当前 S6 暂评 2/4。若主表继续把两种 estimand 合并，H5 应判 FAIL。

**DECISION TEST**

在一个冻结 development stream 上预注册四组：`selector-only`、`verifier-only`（固定 threshold）、`selector+verifier`、`always/never/random matched escalation`。固定总调用预算和失败/拒答损失，分别报告 pre-call correctness、post-call accepted risk、coverage、平均调用数、增量 feedback 数及端到端成本。只有组合相对每个对应强基线都有正的 net utility，且 selector 增益与 verifier 增益可分解，才保留统一主张；否则拆成两个结果或只保留其中一个。

**CONFIDENCE**

高。接口已经承认两个信息边界，当前证据也明确不能互相回填；缺口是主张层级而非措辞细节。

### Objection 3：方法没有冻结，中心主张不可复现、不可审稿

**POSITION**

故事要求一个“动态 JEV 工具适应流程”，但方法文档明确最终状态、更新规则、探索规则和联合训练仍未决；因此审稿人无法知道要复现哪个 Proposed，也无法判断负结果是候选失败还是方法尚未定义。

**TARGET**

`02-方法论.md:10-14,32-44,46-68` 与 `01-故事线与创新.md:34-51` 的方法—故事一致性。

**EVIDENCE**

当前所有普通 selected-only selector 候选没有通过 development 主闸门；动态 JEV 的状态和更新规则尚未冻结。验收审计把“完整 E1–E5、动态事件、paired uncertainty 和一次 confirmation”列为缺失硬条件（`current_acceptance_audit_v2/report.md:14-18,34-53`）。

**REASONING**

一个可证伪的故事至少需要一个确定算法、一个确定比较边界和一个确定 kill rule。候选未决时，故事可以随结果改变：若 direct selector 失败，可改称 verifier；若固定池无 drift，可改称 feedback-adaptive；若 GraphRouter 未完成，可删除该比较。这会把“主张”变成事后选择，而不是实验前的假设。当前文档虽写了主张纪律，但还没有把唯一 Proposed 落到可运行对象。

评价条款/分数影响：命中 **H5、H7（正文自洽包含方法与主要证据）和 S3/S6**；当前 S3 暂评 2/4、S6 暂评 2/4。无法冻结唯一 Proposed 时 H5 为 UNKNOWN，不能按通过处理。

**DECISION TEST**

先完成一张方法卡：状态变量、特征、初始化、探索/propensity、事件队列、乱序/重复反馈、failure/unknown、gate、超参、复杂度、恢复步骤及所有随机种子。用独立 verifier 从 raw JSONL 重建一个确定性小流；再按预登记 E1–E5 跑 development。若在规定截止时间仍无法冻结唯一 Proposed，kill dynamic-JEV-method claim，保留 benchmark/protocol 和 negative diagnosis 版本。

**CONFIDENCE**

高。该结论直接写在生效方法和验收审计中，无需推断。

### Objection 4：selected-only 反馈下，未选 provider 的适应不可识别；故事缺少支持假设

**POSITION**

只观察已选 provider 的 correctness 时，若策略长期不选某 provider，两个环境可以产生完全相同的历史，却对未选 provider 的正确率相反。复杂状态或 provider transfer 不能凭空识别未观测臂。

**TARGET**

`01-故事线与创新.md:16-20,34-41` 对“有限 selected-only feedback 能改变后续选择”的一般表述，以及 `02-方法论.md:32-38` 的更新接口。

**EVIDENCE**

项目已有审查明确给出 indistinguishable-environment 反例：若策略永远选 A，环境 P/Q 的 A 反馈相同而 B 的正确率分别为 `0.95/0.05`，任何 selected-only updater 都无法同时识别 B；provider transfer 真实 replay 也未超过 Frozen（`research/task49_composition_20260928/prior_review.md`、`research/task49_composition_20260928/negative_diagnosis.md`；`current_acceptance_audit_v2/report.md:29-32`）。方法文档尚未冻结探索或 propensity 规则。

**REASONING**

故事把“菜单已知”和“历史可见”写得很完整，却没有说明未选 provider 如何获得支持：固定 fit-only prior、受控探索、可迁移上下文假设、provider/version event，还是调用后额外观测。没有其中至少一种，所谓“更新后续选择”最多是重排已选 provider；“跨 provider 传播”更是不可识别主张。该缺口是理论层面的，不会由更多 seed 或更大 backbone 修复。

评价条款/分数影响：命中 **H4（统一不变量/反例）、H5（可反驳测试）和 S4**；当前 S4 暂评 1/4。若没有先验/探索条件与合成不可识别性反例，H4 应判 FAIL，selected-only adaptation 不能作为中心创新。

**DECISION TEST**

先做最小两 provider synthetic test：固定同一 A 反馈，构造 B=0.95 与 B=0.05 两个环境；分别运行无探索、固定 propensity 探索、fit-only transfer 三种合同，检查是否能在预设置信界内区分。然后在真实 BBB development 上报告每 provider 的曝光、支持数、propensity、coverage 和 transfer-vs-permutation 对照。若无探索/先验条件下不能区分，论文必须把结论限制为“selected provider calibration”，删除泛化的 provider adaptation；若 transfer 只在 fit 区成立而 held-out 失败，删除 shared-state 主方法。

**CONFIDENCE**

高。反例是信息合同的直接推论，且项目已有同类审查记录。

### Objection 5：当前 benchmark 不能支撑“动态工具适应”或跨数据推广

**POSITION**

主 benchmark 是固定 33-provider BBB 缓存；没有自然 arrival/retirement/version drift，旧 development panel 与官方 test 存在 overlap，provider 训练成员和 checkpoint lineage 仍分级。由此只能支撑 lineage-aware、fixed-pool development 结论，不能支撑非平稳适应或普遍工具可靠性。

**TARGET**

`01-故事线与创新.md:22,70-76,101-108` 对“动态 JEV”和“科学工具”一般化的叙述；`03-Benchmark与Baseline.md:13-33,70-74` 的 benchmark 资格与实验边界。

**EVIDENCE**

验收审计明确：Gate A 缺 hERG/AMES 资格、最终 overlap 口径和 model-fresh lineage；Gate C 缺动态 event/recovery、跨端点或 confirmation（`current_acceptance_audit_v2/report.md:14-18`）。BBB 有 10 个 canonical conflict group；旧 722 条 development panel 与官方 BBB test 有 147 条 canonical、235 条 scaffold overlap；24 个 B3clf 变体不应当作 24 个独立家族（`03-Benchmark与Baseline.md:23-33`）。审计还明确不能写“环境非平稳工具适应”（`current_acceptance_audit_v2/report.md:27-32`）。

**REASONING**

一个 fixed-pool replay 可以检验 delayed feedback，却不能检验 provider arrival/retirement/version recovery。controlled version event 正控能证明日志和低维状态在“人为降级事件”下可学习，但不能证明真实 provider drift 或跨 provider 普适性。overlap 和训练成员问题还会把静态准确率解释成数据/模型记忆收益。故事当前把“医药场景”提升为“科学工具调用”一般问题，证据跨度大于 benchmark 覆盖。

评价条款/分数影响：命中 **H8（主张校准）**、**S11 边界和主张校准**；当前 S11 暂评 3/4 仅因为文档已主动列出限制，论文若仍作普遍结论则 H8 FAIL，S11 下降至 1–2/4。

**DECISION TEST**

锁定 P-C1 清洗后的 BBB authoritative manifest，重新生成 scaffold-disjoint confirmation provider outputs，并逐 provider 绑定 checkpoint-to-train membership、阈值和版本。先在 development 预注册一个 arrival、retirement 或 version-shift 事件，报告 recovery steps、前后分段 accuracy、调用成本和 Frozen 对照；再做一次不回流标签的 confirmation。若只能完成 fixed-pool BBB，主张改成“在该 lineage-aware protocol 下的 feedback-adaptive routing”，删除自然 drift、跨端点和普遍科学工具结论。

**CONFIDENCE**

高。缺口和数字均由验收审计/benchmark 生效文档明确记录。

### Objection 6：主表和成本/不确定性合同尚未闭合，存在选择性报告空间

**POSITION**

当前开发表虽然比先前完整，但 Random 统一收据、GraphRouter fidelity、Laya/JEV online、E1–E5 全矩阵、paired uncertainty 和统一成本表仍缺。把不同表示、初始化和预算下的结果并排，会让“动态方法未超过 Best Fixed”或未来“超过强对照”都难以归因。

**TARGET**

`03-Benchmark与Baseline.md:41-68` 的 baseline/指标承诺与 `current_acceptance_audit_v2/report.md:14-18,42-53` 的 Gate B/C。

**EVIDENCE**

验收审计将 Random、GraphRouter（或同等静态强 router）、统一 raw-run/recovery、common feature、fit-only warm start、failure/unknown 分母和计算预算列为待办；Greedy 60/60 streams verifier 通过，但并不自动完成所有主 baseline（`current_acceptance_audit_v2/report.md:16-18,42-46`）。

**REASONING**

Best Fixed `0.9155`、Matched Online-SGD `0.8991`、confidence escalation `0.923823` 分属不同 estimand/成本结构。若主张“流程提升可靠性”，必须知道每个额外调用、反馈等待和拒答怎样计价；若只报告 pooled accuracy，family correlation 和易分子比例会产生 Simpson 型误导。方法故事虽要求同预算，但当前证据还未实现该合同。

评价条款/分数影响：命中 **H5/H6（独立证伪与选择性报告/信息边界）**、**S6**；S6 暂评 2/4。统一 runner、paired uncertainty 和成本表缺失时，H6 不能从文档口头承诺升级为 PASS。

**DECISION TEST**

完成一个统一 runner 和独立恢复器，固定 common feature、fit-only warm start、菜单、delay `{0,1,4}`、失败语义、seed 和调用预算；补齐 Random、至少一个强静态 router、所有主在线近邻，并按 molecule/scaffold 做 paired uncertainty、family macro、累计 regret、calls、p50/p95 update time、memory、failure/unknown 分母。任何候选若只在不匹配的成本或分母下占优，不能进入 headline。

**CONFIDENCE**

高。缺失项在 Gate B/C 中已逐项列出；“存在选择性报告空间”是由缺失合同推导出的审稿风险，不是对实际操作者的指控。

### Objection 7：故事中的“固定赢家不能适应分布”目前没有对应环境

**POSITION**

故事把 fixed winner 与 dynamic adaptation 的必要性作为因果起点，但主回放的 provider、菜单、checkpoint 和分子分布大多固定。若环境没有变化，固定赢家表现差只能说明静态条件能力不足，不能说明动态适应优于固定策略。

**TARGET**

`01-故事线与创新.md:24-30` 的“方法缺口”，以及 `03-Benchmark与Baseline.md:57-74` 的静态/在线实验划分。

**EVIDENCE**

当前审计明确不能写“环境非平稳工具适应”；controlled version event 是人为正控，不是自然 drift（`current_acceptance_audit_v2/report.md:27-32`；`TODO_LOG.md` 2026-09-29 controlled_version_event 条目）。Greedy/PILOT/BayesianRouter 在 fixed-pool development 没有稳定 direct gain。

**REASONING**

如果 distribution stationary，dynamic updater 可能只引入方差、探索成本和延迟，Best Fixed 合理地成为强 incumbent。把“固定赢家不够”写成普遍方法缺口会被反例击穿。当前故事实际上更适合说“我们检验在 feedback-adaptive 条件下是否有额外收益”，而不是预设动态一定应胜出。

评价条款/分数影响：命中 **S3 因果链**和 **S11 边界**；S3 暂评 2/4。没有事件对照时，不能把“动态适应”作为故事根因，只能写成待检验条件。

**DECISION TEST**

预先定义三段流：stationary、provider version-shift、menu arrival/retire；在每段都对比 Best Fixed/Frozen 与动态候选，报告 change-point 前后和 recovery，而不是只报 pooled accuracy。若动态方法只在 shift 段有收益，主张限定为事件后的恢复；若 stationary 与 shift 都不超过强对照，删除“动态适应是缺口”并保留负结果。

**CONFIDENCE**

中高。固定池事实明确；“因果起点不成立”的结论取决于最终事件实验，但当前已有充分理由要求收窄表述。

## DECISION TEST

### 三个必须做的修正

1. **冻结唯一 Proposed 与 estimand。** 在实验前提交方法卡和 claim table：明确到底主张 direct pre-call selection、post-call verification，还是二者组合；给出状态、探索/propensity、delay 队列、成本、失败/拒答损失和 kill rule。把其他候选降为 baseline/ablation，不再用“流程”代替算法。
2. **完成同合同的强基线与因果分解。** 统一 runner 补齐 Random、强静态 router、Matched SGD/Replay、Greedy/PILOT/BayesianRouter、verifier-only 和 random matched escalation；固定同一菜单、特征、初始化、delay、预算和 seeds，报告 paired uncertainty、family/scaffold 分层及端到端成本。主结果必须同时回答“是否超过 Best Fixed”和“收益来自 selector、verifier 还是额外调用”。
3. **闭合资格与动态/独立验证。** 绑定 provider checkpoint-to-train membership，完成 P-C1 后的 molecule/scaffold-disjoint provider outputs，预注册至少一个 arrival/retire/version event 和 recovery 指标，然后冻结代码只开一次 confirmation；在未闭合前，所有文字限于 BBB lineage-aware development。

三项修正分别对应评价标准的 **H4/H5/H6/H8** 和 **S3/S4/S5/S6/S11**。其中 H4、H5、H8 目前不能按 PASS 计；修正完成前总分不应超过 79，且不得进入 confirmation。

## CONFIDENCE

### 最强拒稿点

若提交稿件仍把“动态 JEV 工具适应”作为已验证新方法，最强拒稿理由是三件事同时成立：

1. 方法尚未冻结，更新规则与探索规则仍是候选，无法复现或证伪唯一 Proposed；
2. 当前 development 主表没有超过 Best Fixed/强 matched online baselines，且 post-call confidence 的正结果不能回填 pre-call selection；
3. 主张依赖的 delayed selected-only adaptation、动态事件和跨数据推广尚无完整 Gate B/C 或一次独立 confirmation，而组件级先例已经覆盖 contextual routing、delayed feedback、JEV observe、cascading 和 abstention。

这不是“再调一次学习率”可以解决的缺陷，而是中心贡献、信息可识别性和证据合同尚未闭合。即使未来 full 方法在 BBB development 上胜出，只要没有与最强匹配基线、同成本 verifier 控制和独立 confirmation 同时成立，仍不能把组合称为普适动态 JEV。

### Kill criterion

满足以下任一条件，永久停止“动态 JEV 方法优越性”主张，并把论文收窄为 benchmark/protocol 与负诊断：

- 在唯一冻结的 Proposed、同一信息/菜单/预算合同下，`full` 在三个 delay `{0,1,4}` 中至少两个不超过 strongest matched baseline（允许的最小门槛为预注册绝对提升 ≥0.01、至少 4/5 folds 为正）；或 full 只超过 Frozen/Random、没有超过 Best Fixed 与现代近邻；
- selected-only synthetic identifiability test 在没有预先声明的探索/先验时失败，且真实数据上的 transfer-vs-permutation 不显示 held-out 支持；
- 任何未选 provider 输出、当前题真值、confirmation 标签或未来版本信息进入 selector/update；独立 verifier 无法从 raw JSONL 重建逐事件状态；
- 无法完成 provider lineage、molecule/scaffold-disjoint confirmation 和预注册动态 event，导致只能报告 fixed-pool BBB development；
- 组合的任何正收益在扣除第二调用、拒答和反馈等待成本后消失，或只存在于 pooled accuracy 而不在 family/scaffold 成对不确定性下成立。

若 kill criterion 触发，保留的可发表核心应是：一个公开 truth 与具体 provider routing protocol 的可复核 benchmark、selected-only delayed-feedback 的信息合同/不可识别性边界、以及“普通在线头和简单 transfer 不足以超过强固定赢家”的系统负结果。上述核心仍需按 benchmark Gate A/B/C 完成资格、统一 baseline 和一次 confirmation，不能以当前 development 记录直接宣称完成。
