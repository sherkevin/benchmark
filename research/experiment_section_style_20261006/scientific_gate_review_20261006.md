# 三份验收标准的当前独立复核

日期：2026-10-06；最近复核：2026-10-07。

## 结论先写清楚

当前有两个不同层级的状态，不能混写：

1. `stage_b_scientific_submission_gate_v3/scorecard.json` 在它自己声明的范围内通过：公开 TDC BBB truth、四个 checkpoint-bound fresh providers、一次调用、selected-only delayed feedback、延迟 `{0,1,4}` 和 controlled correctness-drift stress。它的 Gate A/B/C 分别为 15/16、13/16、14/16。
2. 生效的三份投稿验收标准要求更强的“论文比较完整性”：最近方法必须有统一合同、adapted baseline 必须有可比的状态/成本/unknown/refusal 字段、机制贡献必须有更强的反事实证据。按这个更严格的论文 gate，当前不能写成三门全部通过。

这不是数字矛盾，而是两个 gate 的分母不同。前者回答“声明的一次调用动态切片是否可复核”，后者回答“论文是否已经用足够强的 benchmark、baseline 和机制证据支撑更广的贡献叙事”。

## 已达到的部分

| 标准 | 已有证据 | 当前判断 |
|---|---|---|
| Benchmark truth 与 split | TDC BBB 来源、版本、hash；157 个 scaffold-disjoint confirmation molecule；provider checkpoint manifest；自然 270/270、stress 150/150 contract/replay | 在 one-call fresh-menu 范围内通过 |
| 反馈合同 | selected-only、release clock、`D\in\{0,1,4\}`、runtime failure 与 chemical error 分开；决策记录不含 truth 或 unselected output | 通过 |
| 主结果关系 | natural MIRA/Frozen 89.81% 且 CI 跨 0；controlled stress MIRA 87.56% vs Frozen 73.38%，+14.18 pp；BayesianRouter 同 stress +6.90 pp | 结果关系合理，claim 必须保持 controlled scope |
| 实验排版 | 四行实验地图；natural/stress 两张主表；表先数字、后短解释；正文 8 页 | 已完成本轮写作目标 |

## 仍未达到三份标准的地方

### 1. Benchmark / Baseline 的最近邻闭合不足

PILOT、GraphRouter、RouterDC、RouterBench 已完成文献/源码审计或适配边界审计，但没有全部在当前 fresh-menu 结果表中以同一 input、feedback、delay、budget 和 recovery contract 完成效果证据。当前主表中的 BayesianRouter、Greedy LinUCB、matched SGD 是可审计的 adapted controls；不能把 audit-only 方法写成已完成的同层 baseline。

**修复动作：** 先完成最近方法合同矩阵，再决定哪些方法值得补跑。矩阵字段固定为 input representation、menu、warm start、feedback visibility、release clock、objective、failure/unknown/refusal、budget、state/cost receipt、可比性结论。没有完整合同的行保留 `audit-only`，不填伪结果。

### 2. 成本和状态字段还没有跨方法统一

当前 p95 是 policy-operation CPU overhead，能比较已记录的方法，但不是端到端服务延迟。旧 baseline 审计中仍有 `MISSING` 的 state bytes、refusal/unknown 和完整 warm-start 字段；不能据此写“实时性优于所有 baseline”。

**修复动作：** 下一版 raw-run schema 增加 `feature_sha256`、`warm_start_sha256`、`likelihood_id`、`budget_id`、`state_bytes_before/after`、`unknown_reason`、`refusal_action`，并对每个实际跑过的方法填入或明确 `MISSING`。不回填历史 receipt 的伪值；需要统一成本比较时只在新合同下重跑。

### 3. 机制必要性证据仍偏弱

现有 Frozen/no-update 与 matched Online-SGD 能说明“释放反馈和更新路径有作用”，E3 family-label permutation 只是一项敏感性诊断，不足以证明 provider-local residual 是唯一必要机制。当前 +14.18 pp 证明了在声明的 evaluator-side drift 下的 recovery 能力，不能单独分解成 residual、centered prior 或 provider-local state 各自的因果贡献。

**修复动作：** 在不改 confirmation 输入的前提下，设计并预登记最小 matched ablation：同一 frozen surface、同一 selected-only clock、同一 state budget，分别移除 provider-local residual、fit-rate centering 和 release gating；每个变体必须有同样的 action/state/evaluator receipt 和反事实失败条件。若只做 CPU replay，正文把它写成 mechanism diagnostic，不把它升级成 superiority claim。

**2026-10-07 新证据：** [development 残差分解](../../task/task49-pharma-benchmark-baseline-freeze/residual_decomposition_v1/report.md)独立重构 15 条流、2,166 次决策，最大分数误差 $2.22\times10^{-16}$。残差精确分为“相对冻结分数的误差项”和“已选样本的分数组成项”；沿同一已观察历史移除后项，会改变 401 次 shadow 选择（18.51%），不代表闭环准确率收益。selected-release 的平均冻结分数为 0.580164，正确率为 0.791782；实现采用 class-balanced logistic，分数校准是下一步必须区分的因素。已有 FamilyGuard 消融不再作为当前残差机制的充分证据。共同加性残差不改变排序，由代数和不变性检查覆盖；未居中的后验会改变冷启动，不能当作纯在线机制消融。下一步改为开发集的校准与中心化配对对照，保持旧 confirmation 只读。

**2026-10-07 后续诊断：** [fit参考恢复](../../task/task49-pharma-benchmark-baseline-freeze/fit_score_reference_v1/report.md)真实重建4个checkpoint的722分子预测和5折冻结头，4,332条Frozen logits及fit rates误差均为0。20个fit均分为0.525782–0.543146，正确率为0.693582–0.824324。组成项进一步分成相对fit样本组成变化（signed mean +0.016355）与fit分数基准差（−0.079047）。同历史分别移除两项改变292/2166和374/2166次shadow动作，仍不代表闭环收益。5项测试通过；缓存、逐行证据与报告已保存。下一步的[6格配对实验卡](../../task/task49-pharma-benchmark-baseline-freeze/calibration_centering_v1/experiment_card.md)把类别权重修正与在线中心化拆开，在natural/controlled两种环境检查，不读旧confirmation选方案。

**2026-10-07 六格闭环结论：** [配对诊断报告](../../task/task49-pharma-benchmark-baseline-freeze/calibration_centering_v1/report.md)已完成180条development流、25,992次决策；独立checker重构77,976条事件及全部counter/pending/action/aggregate，verified、0 issues。类别权重修正把OOF Brier从0.234888降至0.173292，log loss从0.661950降至0.533768。但corrected query中心化相对corrected marginal只有自然+0.508pp、controlled+0.369pp，均未达预登记1pp升级线；相对对应Frozen自然仅+0.139pp。故保留修正的诊断价值，不把Q替代原方法，也不扫超参找更大数。raw query的两个环境负结果、各折/delay负数和stress暴露差异均保留。下一步转向可预测headroom与greedy反馈缺失；新候选仍需同合同现代baseline和新的未读确认。

**2026-10-07 headroom与错误定位：** [当前四工具诊断](../../task/task49-pharma-benchmark-baseline-freeze/headroom_feedback_v1/report.md)独立核对722个开发分子、90条自然流和12,996次决策。471题全对、147题mixed、104题全错；fit-only Best Fixed为572/722，Raw Frozen 573，Corrected Frozen 579，逐题Oracle 618。原raw/M在启动前缀后的138个可救错误中136个已有正确候选反馈、2个全部正确候选从未被选；该解释不支持优先把主要缺口归因于完全未探索。E1/E5获得当前面板证据，不沿用旧33-provider上界。下一最小干预检查context条件化使用反馈；这不证明局部反馈充分、不识别探索因果效果，也未关闭新机制与baseline gate。

**2026-10-07 条件化干预结论：** [context residual](../../task/task49-pharma-benchmark-baseline-freeze/contextual_residual_v1/report.md)完成90条development闭环、12,996决策，状态及全部配对结果独立重构通过。固定Morgan64的context ridge相对原Q自然−0.139pp、controlled−0.646pp；虽略胜范数匹配的更强常数更新，仍未过原Q、LOO和动态保护规则，不升级或扫正则。下一步[补当前四工具学习型prior](../../task/task49-pharma-benchmark-baseline-freeze/fresh_learned_prior_v1/preparation.md)：现有原生BT/MLP/Laya结果实际均属旧33provider，不能当作当前强对照已完成。该项同时服务Baseline gate与冻结打分器的相对能力诊断。

**2026-10-07 当前四工具学习型对照：** [BCE/BT匹配训练](../../task/task49-pharma-benchmark-baseline-freeze/fresh_learned_prior_v1/report.md)完成单A800、20fits、4000epochs、2888OOF，30checkpoints和逐样本选择已由独立NumPy核验；logit最大差1.93e−7。BCE两seed78.53/78.95%，BT78.81/78.25%，均低于Corrected Frozen80.19%，各自五LOO全负；BT−BCE一正一负。补齐当前菜单的静态目标适配对照，但没有新增收益，不以换loss/网络当创新。下一步先审表示保真与第一层容量混杂，不直接搬用旧非chiral 1024缓存或旧33-marker Laya表示。该结果未关闭完整在线baseline及新确认缺口。

### 4. 动态范围仍是 feedback-adaptive，不是自然生命周期漂移

controlled correctness drift 是 evaluator-side label flip；natural fresh menu 是 stationary checkpoint panel，MIRA 与 Frozen 没有 aggregate gain。没有自然 provider arrival、retirement、version trace，因此不能从当前结果推出“适用于真实 provider lifecycle”或“通用实时学习”。

**修复动作：** 论文主张继续限定为 delayed selected-only feedback 下的 declared correctness drift recovery。若要打开 lifecycle claim，另建 arrival/retire/version 合同和独立 confirmation；不能把当前 stress 改名为 natural drift。

### 5. event-level verifier 已补上，但 learned-state replay 仍未闭合

现已加入不导入生产 runner/policy 的独立 JSONL 合同审计器。v2 对 natural v2 与 controlled stress v3 共 420 条 stream、197,820 个事件做了 exact event allowlist、事件顺序、decision→terminal→feedback 绑定、release clock、selected-only 可见字段、opaque hash-chain、summary count 和 stream digest 检查，均 verified、0 issues。1 个 clean smoke 和 17 个 mutation 测试全部通过；包括修复 `_truth_proxy` 下划线字段漏检后的复核。证据见 [v2 报告](independent_receipt_checker_20261007_v2/report.md)。这支持“声明范围的 receipt 可做独立事件级合同审计”，不证明代码执行时不曾进行未记录的读取。

旧confirmation收据只记录 `state_hash_before/after`，没有 canonical learned-state bytes 或 bounded transition trace，因此审计器不能独立重算策略状态、随机数状态或 pending queue 的完整转移。`verified` 不能被解释为完整的独立策略重执行。

**新增可复用实现：** 六格development采用完整counter/pending/decision-time q快照与canonical bytes/hash，已由不导入runner的独立checker重算状态、queue、action和paired result。40项合成/篡改测试通过。此能力可用于下一轮共同baseline合同；它不回填旧confirmation缺失的状态字节，也不把历史事件级审计升级为完整状态重放。

## 当前可写与不可写

**可以写：** MIRA-Relay 冷启动等于 Frozen；在相同 fresh menu 和 delayed selected-only contract 下，natural stream 维持稳定；在预登记 evaluator-side correctness drift 中，MIRA-Relay 比 Frozen、matched SGD、Greedy LinUCB 和 BayesianRouter 恢复得更好；所有声明范围的 receipt 可 contract/replay 验证。

**暂时不能写：** 通用在线收益；自然 provider drift；完整 JEV/Laya 效果；GraphRouter/PILOT 等最近方法已公平完成主比较；端到端 agent 能力上限提升；MIRA-Relay 的 provider-local residual 已被唯一因果证明。

## 下一步顺序

1. [最近方法合同矩阵](nearest_work_matrix.md)与[统一收据规范](unified_receipt_schema_v1.md)已经存在；不再重复写方案。按其中 deployable / adapted / audit-only 结论推进实际对照与成本字段。
2. E3 六格、四工具headroom和context residual干预均已完成；后者未改善Q，保留负结果并停止该固定构造。当前四工具的匹配BCE/BT静态训练已完成并未过升级线。下一步审查表示保真变化和容量控制，再确定最小新卡；不重跑已核provider或描述性审计。旧confirmation保持只读。
3. 新 raw-run 一并记录必要的状态/成本字段，复用本轮独立事件和分数重构检查；不为扩充审计数量启动 Nebula。
4. 每完成一个小任务，重算三门：Benchmark、Baseline、实验矩阵与结果；任何一门低于标准都保持 HOLD。当前本地 PDF preflight 通过不等于三份完整论文标准已经通过。

## 2026-10-07 表示对照补充

当前四工具chiral1024 vs64零填充完成20A800 fits及独立核验。1024为77.84/79.09%，控制78.53/78.95%，CorrectedFrozen80.19%；未过双对照资源线，停止此构造。结果不能证明hash碰撞因果或否定所有表示。代码/实际权重/逐样本证据见fresh_representation_v1/report.md。E1控制更充分，但机制有效性未补齐，三份标准仍HOLD。下一步须利用已有结构化反馈原型判断是否有真正新干预，不继续增维/超参扫点。
