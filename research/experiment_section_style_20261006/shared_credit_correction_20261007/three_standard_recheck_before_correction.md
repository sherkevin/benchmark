# 三份标准重检：submission preflight 后的窄切片状态

- 日期：2026-10-07
- 绑定主稿：`paper/aamas2027/main.tex`
- 绑定提交：commit `187e89c5`；source SHA `93a5d40c309c7d11ceb3898d4683882983a5f45614f8c447914bcab06b47dc8d`
- 绑定 PDF：`paper/aamas2027/build/submission_20261007_122046/main.pdf`
- PDF SHA：`8faa50ed36cc4080ff70822f050ebaa73499e8bcd08735b3d1b5a7bc37cf0ae4`
- 版式结果：9 页总计，正文 8 页，参考文献 1 页；submission preflight 通过。

## 先给结论

这次完成的是一个**明确收窄的投稿切片 preflight**：公开 TDC BBB truth、四个 checkpoint-bound fresh providers、一次调用、selected-only delayed feedback、$D\in\{0,1,4\}$，以及预登记的 controlled correctness drift。Benchmark/Baseline Gate A/B/C 和 declared-scope 方法证据可以进入联合复核；不能把它写成完整 agent、自然漂移、hERG/AMES 或完整 33-provider model-fresh 已经通过。

因此当前状态分两层：

1. **Benchmark + Baseline：可进入联合复核。** Gate A=15/16、Gate B=13/16、Gate C=14/16，均达到该 gate 的 declared-scope 通过线；论文结构和结果边界已写入主稿。
2. **方法论：回放与机制证据达到 declared-scope 门槛。** v2 import-free verifier 已独立重建 full state/action；Local/Shared/Frozen/Relative 机制消融已回填主稿。paper-level Gate 仍需第二位复核，且 family sharing 仍不可识别。详细纠正见 `independent_standard_recheck_20261007.md`。
3. **原始长期目标：仍 HOLD。** post-call verification/refusal、自然 lifecycle、跨端点、完整 33-provider model-fresh、端到端 agent 训练和 broad integration gain 尚无同等级 confirmation 证据。

主稿数字绑定审计 `research/experiment_section_style_20261006/paper_results_binding_audit_20261007.md` 已通过：旧主表计数、新机制 Local/Shared rescue-harm、Local−Shared 区间和正文关键数字均可从结果库重算。它不替代第二位审稿式复核，也不把新机制 receipt 改写成 independent full-state replay。

随后新增的 Shared-Credit import-free replay 已覆盖 natural/controlled 各 30 streams、4,710 decisions 和 9,420 个可用 state hashes，score error 为 0，reward/score mutation 均拒绝；这补齐了新机制主证据的回放链。

## 一、故事线与创新点

### 窄切片作者自评

评分采用生效标准的 0--4 分；这是作者侧复核，不替代第二位复核者。

| 项 | 分数 | 依据 | 当前缺口 |
|---|---:|---|---|
| S1 切口与重要性 | 4 | 引言给出正常返回但结果分歧的具体分子案例，并把问题落到 pre-call 选择 | 尚未有 agent 端到端能力结果 |
| S2 任务与 agent 角色 | 4 | 输入、菜单、选择、selected-only feedback 和 release clock 在正文与图中一致 | 验证/拒答仍是扩展接口 |
| S3 因果链 | 3 | 分歧→时序限制→provider-local residual→受控恢复的链条可读 | 长期“能力上限”愿景超出当前实证 |
| S4 创新必要性 | 3 | 冷启动等于 Frozen、只更新已选且已释放 provider，并有 matched controls 与 drift stress | integration gain 仍只在受控事件上成立 |
| S5 最近邻边界 | 3 | Related Work 和 baseline roster 覆盖固定、LinUCB、posterior、SGD 与现代 routing | GraphRouter/Laya 在线适配不完整 |
| S6 可证伪主张 | 4 | 自然 null、controlled gain、paired CI、rescue/harm 与 scope limit 都有位置 | 需要第二位复核者复查 headline 数字绑定 |
| S7 引言漏斗 | 3 | 7 节结构已收拢，六段功能基本齐全 | 仍需独立盲读记录 |
| S8 章节/RQ | 4 | 实验矩阵按 headroom、natural、drift、cost/validity 组织 | 后续扩展 RQ 不应混入当前主结果 |
| S9 段落转承 | 3 | 主稿已做 topic/support/bridge 重排 | Underfull box 仍存在，需最终语言审阅 |
| S10 定义、例子、术语 | 3 | 公式前解释、公式后 case，MIRA-Relay/JEV 命名已统一 | 仍需逐符号 case 表的最终核对 |
| S11 边界与主张校准 | 4 | 正文明确 natural null、controlled stress、hERG/AMES 和 verifier 的边界 | broad goal 尚未有结果 |
| S12 语气与呈现 | 3 | 结果用 paired difference、CI 和分母，未把 null 写成 generic gain | 需独立审查表格密度和残余模板句 |

加权作者分为 **85.5/100**。这只达到“可进入联合复核”的内部状态；尚未满足 submission-review-ready 所需的第二位盲读和全部核心段落复核。

### 故事硬门槛

窄切片 H1--H10 暂判 PASS：有 AAMAS area 对象、具体错误案例、最近邻对照、统一事件合同、独立结果、信息隔离、正文自洽、范围收窄、7 节因果顺序和段落审计材料。长期故事仍不能据此宣称已完成。

## 二、方法论

### 窄切片作者自评

| 项 | 分数 | 依据 | 当前缺口 |
|---|---:|---|---|
| M1 数学对象和接口 | 4 | $x,e,A,a,o,r,H,S$ 有正文定义、具体 case 和 receipts | 端到端 verifier 接口未纳入主实验 |
| M2 时间和反馈队列 | 4 | decision→termination→release→update 顺序、$D=0,1,4$ 和 exactly-once receipt 已验证 | 真实服务乱序/重启只在 replay 层覆盖 |
| M3 信息合同和隔离 | 4 | selector 不读当前 truth、未选输出或未来反馈；contract checker 通过 | 需保留最终 adversarial injection 结果索引 |
| M4 标签和失败语义 | 4 | chemical correctness、runtime failure、unknown/refusal 分开定义 | confirmation fresh receipt 主要是 scored returns |
| M5 机制必要性和近邻差异 | 3 | provider-local residual 与 matched Frozen/SGD/LinUCB/BayesianRouter 形成对照 | 还缺完整 integration ablation |
| M6 目标、估计量和更新推导 | 3 | posterior residual、复杂度和分数边界可直接计算 | 尚无更强理论界或自然漂移可识别性结果 |
| M7 公式—实现一致 | 4 | source/config/input hash、独立 full-state replay、逐事件 state hash 和 score/action 核对一致 | 后续代码扩展需维持同一 contract |
| M8 状态改变计算对象 | 3 | release ledger 改变后续 action，controlled stress 可重现 | 未证明在自然 fresh panel 上净提升 |
| M9 复杂度和实时预算 | 3 | update p95、state size、calls 和 CPU policy cost 有记录 | 尚无 K/d/pending 压力曲线 |
| M10 动态事件和可识别性 | 4 | feedback-adaptive scope 有 selected-only release、delay、state→next-action；没有冒称环境非平稳 | natural lifecycle 尚未做 |
| M11 延迟、探索和偏差 | 3 | 三种 delay、同流 paired comparison 和 selected-only contract 已有 | propensity/更广偏差分析仍有限 |
| M12 baseline 合同和初始化公平 | 3 | 主表统一 menu/features/feedback/delay/budget，privileged controls 单列 | modern adapter fidelity 仍有边界 |
| M13 消融、反事实和失败边界 | 3 | natural null、controlled stress、matched controls、rescue/harm 和负结果齐全 | 还需要更干净的 remove-state/interface 消融 |
| M14 恢复性和证据链 | 4 | raw JSONL、config、hash、checkpoint、v2 full-state trace、mutation rejection 已通过 | terminal/final-state hash 仍需加入未来 receipt |
| M15 主张校准和适用边界 | 3 | 主稿把 natural gain、controlled recovery、verifier/refusal 和 broad scope 分开 | 后续扩展不能回填当前主表 |

机制消融后，M5/M13 分别更新为 3，方法合计 **52/60**；在 declared scope 内方法证据达到最低门槛，但主稿尚未整合新结果，paper-level Gate 暂 HOLD。broad dynamic/environment-nonstationary、family-sharing 和端到端 agent claim 同样 HOLD。

## 三、Benchmark + Baseline

`task/task49-pharma-benchmark-baseline-freeze/stage_b_scientific_submission_gate_v3/scorecard.json` 已给出机器化复核结果：

| Gate | 分数 | 窄切片状态 | 主要限制 |
|---|---:|---|---|
| Gate A：Benchmark 选型 | 15/16 | PASS_FOR_DECLARED_SCOPE | wider 33-provider lineage/family independence 仍只得 3/4 |
| Gate B：Baseline 选型 | 13/16 | PASS_FOR_DECLARED_SCOPE | modern router adapter fidelity 与统一成本合同仍为局部 3/4 |
| Gate C：实验矩阵与结果 | 14/16 | PASS_FOR_DECLARED_SCOPE | 主张只覆盖 natural null + controlled drift recovery |

当前主表可以回答四个问题：

1. provider 之间是否存在可选择的 correctness 差异；
2. MIRA-Relay 是否在 stationary fresh menu 上保持与 Frozen 一致；
3. 在声明的 evaluator-side correctness drift 下，selected-only delayed feedback 是否能让后续 action 恢复；
4. 这些结果的调用数、更新成本、rescue/harm 和 replay receipt 是否可复核。

当前主表不能回答：自然 provider/version drift、post-call verification/refusal、端到端 Agent 训练、hERG/AMES 泛化、完整 33-provider model-fresh superiority，也不能单独证明 provider-local residual 是受控恢复的必要机制。

## 方法 replay 更新

`task/task49-pharma-benchmark-baseline-freeze/fresh_context_state_replay_v2/report.md` 记录了这次更新。natural/stress 两份 receipt 各覆盖 30 个 MIRA streams、4,710 次 decision、9,420 个可用 production state hashes；独立重算的 score 最大误差为 0。改变一个 reward 或 score 的复制 receipt 均被拒绝。该证据把 H7 从 UNKNOWN 推进到“完整回放通过”，但不替代同一合同下的机制消融，也不重新运行 offline fit 或 provider inference。

`task/task49-pharma-benchmark-baseline-freeze/fresh_mechanism_ablation_v1/report.md` 已补上机制对照：controlled drift 下 Local−Frozen 为 +14.18 pp、Shared−Frozen 为 +7.13 pp，Local−Shared 为 +7.05 pp（scaffold bootstrap 95% CI $[+3.11,+9.98]$）；natural 下 Local 与 Frozen 持平。该结果支持 selected-provider credit 的机制解释，但四个 provider 是 singleton families，不能外推 family sharing。

## 关键待办（按投稿影响排序）

1. 让第二位复核者独立重打故事和方法分数，补齐盲读五问、topic-sentence 审计和公式 case 表。
2. 重新核对正文表格每一行的 raw receipt、分母和 baseline role，特别是 MIRA-Relative、Matched SGD 与 BayesianRouter 的命名和初始化合同。
3. 把最小 remove-state/interface 消融和 provider-family/lineage 限制写成单独结果卡；没有新证据时不扩大 claim。
4. 如果继续做实验，优先做一项能直接关闭上述缺口的真实批次；不为自然 null 反复调参，也不新增无关 baseline。

## 当前结论

版式 gate 已过，Benchmark/Baseline 的 declared-scope gate 已过，但方法 Gate 仍 HOLD，三份标准的联合复核尚未完成。当前最危险的错误不是缺少更多文字，而是把受控 drift 的同实现恢复结果写成一般实时学习、独立方法证明或完整 Agent 能力提升。后续每次改稿或新增实验，都必须重新绑定这份范围表和三份生效标准。
