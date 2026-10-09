# 三份标准重检：submission preflight 后的窄切片状态

- 日期：2026-10-08（v2 主稿绑定更新）
- 绑定主稿：`paper/aamas2027/main.tex`
- 绑定提交：`paper/aamas2027/build/submission_20261007_161243/`；source SHA `4272e3689ddd3c88be317c948b0608bae30b0d32c3d65e1be4a5ec4d1c7ac045`
- 绑定 PDF：`paper/aamas2027/build/submission_20261007_161243/main.pdf`
- PDF SHA：`788488c2c3d02e613f38cca0b91af6c2cbb7c10cbd19724e6b0ebe346669269a`
- 版式结果：9 页总计，正文 8 页，参考文献 1 页；submission preflight 通过。

## 先给结论

这次完成的是一个**明确收窄的投稿切片 preflight**：公开 TDC BBB truth、四个 checkpoint-bound fresh providers、一次调用、selected-only delayed feedback、$D\in\{0,1,4\}$，以及在确认实验前预先指定的 controlled correctness drift。Benchmark/Baseline Gate A/B/C 和 declared-scope 方法证据可以进入联合复核；不能把它写成完整 agent、自然漂移、hERG/AMES 或完整 33-provider model-fresh 已经通过。

因此当前状态分两层：

1. **Benchmark + Baseline：可进入联合复核。** Gate A=15/16、Gate B=13/16、Gate C=14/16，均达到该 gate 的 declared-scope 通过线；论文结构和结果边界已写入主稿。
2. **方法论：回放证据部分通过，机制 Gate 仍 HOLD。** v2 import-free verifier 和 matched cyclic control 将声明范围内的 M5/M7/M8/M13 提升到 3/4、H7 提升为 PARTIAL/PASS_FOR_REPLAY；完整方法 Gate 仍 HOLD。详细范围见 `independent_standard_recheck_20261007.md`。
3. **原始长期目标：仍 HOLD。** post-call verification/refusal、自然 lifecycle、跨端点、完整 33-provider model-fresh、端到端 agent 训练和 broad integration gain 尚无同等级 confirmation 证据。

主稿数字绑定审计曾核对过旧版 Shared 表，但该表的机制解释和点估计/区间配对已撤回。旧审计保留作历史证据，不替代本次纠正后的逐行检查。

此前 Shared-Credit 的 import-free replay 和设计缺陷均保留为历史记录；当前有效的 matched evidence 是 v2 fixed cyclic permutation replay，主稿绑定记录见 `main_v2_integration_20261008.md`。

## 最新有效方法状态（H7 receipt closure v1，2026-10-08）

`fresh_receipt_closure_v1` 已在 natural/controlled 两个条件完成新的 receipt v3 和独立复核。两组各 90/90 streams、42,480 events；terminal/final state bytes、event chain、logical-clock reorder、代表性 checkpoint suffix 和 fail-closed mutation 均有原始证据。development provenance 也已用原 native helper 实际重建：2,888 个 probabilities、4,332 个 Frozen decisions，logit/fit-rate 最大误差均为 0。

因此当前**声明范围内**的有效方法分数更新为：M5=3、M7=4、M8=3、M13=3、M14=4，方法小计约 **52/60**；H7=`PASS_FOR_DECLARED_POLICY_REPLAY`。这只关闭 receipt/recovery 的窄切片证据，不把结果外推到自然 provider lifecycle drift、family sharing、verification/refusal、端到端 agent gain 或训练数据 lineage；完整 paper-level 方法 Gate 仍 HOLD，直到三份标准对 broader scope 做联合复核。

证据：`task/task49-pharma-benchmark-baseline-freeze/fresh_receipt_closure_v1/report.md`，实现和原始 receipt 见其 Evidence paths；v3 natural/controlled verification、provenance summary 和 delete/reinsert recovery 均有 SHA 绑定。v1 的 471/472 schema drift 失败保留，不计入通过统计。

## 一、故事线与创新点

### 窄切片作者自评（H7 closure 前的基线，历史保留）

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

### 窄切片作者自评（H7 closure 前的基线，历史保留）

| 项 | 分数 | 依据 | 当前缺口 |
|---|---:|---|---|
| M1 数学对象和接口 | 4 | $x,e,A,a,o,r,H,S$ 有正文定义、具体 case 和 receipts | 端到端 verifier 接口未纳入主实验 |
| M2 时间和反馈队列 | 4 | decision→termination→release→update 顺序、$D=0,1,4$ 和 exactly-once receipt 已验证 | 真实服务乱序/重启只在 replay 层覆盖 |
| M3 信息合同和隔离 | 4 | selector 不读当前 truth、未选输出或未来反馈；contract checker 通过 | 需保留最终 adversarial injection 结果索引 |
| M4 标签和失败语义 | 4 | chemical correctness、runtime failure、unknown/refusal 分开定义 | confirmation fresh receipt 主要是 scored returns |
| M5 机制必要性和近邻差异 | 2 | Local 与 Frozen/SGD/LinUCB/BayesianRouter 有同合同对照 | Shared-Credit 不能作为有效 wrong-credit 对照，机制必要性未隔离 |
| M6 目标、估计量和更新推导 | 3 | posterior residual、复杂度和分数边界可直接计算 | 尚无更强理论界或自然漂移可识别性结果 |
| M7 公式—实现一致 | 4 | source/config/input hash、import-free score/counter replay、逐事件可用 state hash 和 score/action 核对一致 | pending/terminal/final-state bytes 仍未进入旧 receipt |
| M8 状态改变计算对象 | 3 | release ledger 改变后续 action，controlled stress 可重现 | 未证明在自然 fresh panel 上净提升 |
| M9 复杂度和实时预算 | 3 | update p95、state size、calls 和 CPU policy cost 有记录 | 尚无 K/d/pending 压力曲线 |
| M10 动态事件和可识别性 | 4 | feedback-adaptive scope 有 selected-only release、delay、state→next-action；没有冒称环境非平稳 | natural lifecycle 尚未做 |
| M11 延迟、探索和偏差 | 3 | 三种 delay、同流 paired comparison 和 selected-only contract 已有 | propensity/更广偏差分析仍有限 |
| M12 baseline 合同和初始化公平 | 3 | 主表统一 menu/features/feedback/delay/budget，privileged controls 单列 | modern adapter fidelity 仍有边界 |
| M13 消融、反事实和失败边界 | 2 | natural null、controlled stress 和现代 controls 已有 | 现有 Shared 对照混合了静态初始重排与更新规则，仍缺可识别的机制消融 |
| M14 恢复性和证据链 | 4 | raw JSONL、config、hash、checkpoint、v2 score/counter transition trace、mutation rejection 已通过 | terminal/final-state hash 仍需加入未来 receipt |
| M15 主张校准和适用边界 | 3 | 主稿把 natural gain、controlled recovery、verifier/refusal 和 broad scope 分开 | 后续扩展不能回填当前主表 |

Shared-Credit 纠正后，独立复核的 M5/M13 仍为 2，方法严格分数保持 **46/60**，H7 仅为 PARTIAL（回放证据）。方法 Gate 和 paper-level Gate 均 HOLD。broad dynamic/environment-nonstationary、family-sharing 和端到端 agent claim 同样 HOLD。

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

`task/task49-pharma-benchmark-baseline-freeze/fresh_context_state_replay_v2/report.md` 记录了这次更新。natural/stress 两份 receipt 各覆盖 30 个 MIRA streams、4,710 次 decision、9,420 个可用 production state hashes；独立重算的 score 最大误差为 0。改变一个 reward 或 score 的复制 receipt 均被拒绝。该证据把 H7 从 UNKNOWN 推进到“score/counter/action 补充回放通过”，但 pending、terminal 和 final-state bytes 仍未进入旧 receipt，也不替代同一合同下的机制消融或重新运行 offline fit/provider inference。

`task/task49-pharma-benchmark-baseline-freeze/fresh_mechanism_ablation_v1/report.md` 已将 Shared-Credit 重分类为探索性 static-recentering 诊断：controlled drift 下 Local−Frozen 为 +14.18 pp，Shared−Frozen 为 +7.13 pp，但公共项在 `argmax` 中抵消，Local−Shared 同时改变初始排序，不能支持 selected-provider credit 的机制解释。四个 provider 是 singleton families，不能外推 family sharing。

## 关键待办（按投稿影响排序）

1. 让第二位复核者独立重打故事和方法分数，补齐盲读五问、topic-sentence 审计和公式 case 表。
2. 重新核对正文表格每一行的 raw receipt、分母和 baseline role，特别是 MIRA-Relative、Matched SGD 与 BayesianRouter 的命名和初始化合同。
3. 把最小 remove-state/interface 消融和 provider-family/lineage 限制写成单独结果卡；没有新证据时不扩大 claim。
4. 如果继续做实验，优先做一项能直接关闭上述缺口的真实批次；不为自然 null 反复调参，也不新增无关 baseline。

## 当前结论

版式 gate 已过，Benchmark/Baseline 的 declared-scope gate 已过，但方法 Gate 仍 HOLD，三份标准的联合复核尚未完成。当前最危险的错误不是缺少更多文字，而是把受控 drift 的同实现恢复结果写成一般实时学习、独立方法证明或完整 Agent 能力提升。后续每次改稿或新增实验，都必须重新绑定这份范围表和三份生效标准。

## Author-side evidence update: fresh mechanism ablation v2 (2026-10-07)

这是一项新的、与 v1 Shared-Credit 分开的 matched mechanism replay，尚未由本文件
的独立分数自动吸收。设计卡和代码 gate 已由独立审查通过，结果报告见
`task/task49-pharma-benchmark-baseline-freeze/fresh_mechanism_ablation_v2/report.md`。

- natural 与 controlled-drift 各运行 90 streams（5 folds × 2 个 deterministic schedule
  replicas × 3 delays × 3 methods），每个条件 14,130 decisions、42,390 events；replica
  不是 seed robustness。
- 三臂为 Frozen/no-update、MIRA-Relay/Local 和 fixed cyclic permutation credit。Local
  与 permutation 共用 source-indexed anchor/counters；后者只把源 residual 增量写到固定
  destination 槽，冷启动 score/action 与 Frozen 完全相同。
- import-free verifier 两个条件均通过：90 streams、14,130 decisions、56,520 state
  checks、14,130 score checks、最大 score 重算误差 0，release 前逐事件 score/argmax
  一致，严格 release clock/ordinal、输入 SHA、实现 SHA 与 source-delta 均通过；把
  第一条 release tick 加 1 会被拒绝。
- 唯一主 estimand 是等权 124 scaffold 的 stream-level correctness difference，并以
  同一 scaffold 向量做 10,000 次 bootstrap。natural Local−Frozen 为 +0.10 pp
  [−1.08,+1.42]，Local−permutation 为 −0.30 pp [−1.42,+0.76]；controlled
  Local−Frozen 为 +14.40 pp [+9.79,+19.23]，Local−permutation 为 +15.67 pp
  [+10.65,+20.74]。

这组证据关闭了“v1 对照初始重排、且点估计与区间混用”的具体漏洞，并使
selected-provider credit 在声明的 evaluator-side drift replay 中具有可检验的
matched contrast。它仍不证明自然 provider drift、family sharing、verification/refusal
或端到端 agent gain。独立 follow-up re-review 建议 M5=3、M7=4（限 declared replay
scope）、M8=3、M13=3、M14=3，方法严格分数约 **51/60**；H7 为
PARTIAL/PASS_FOR_REPLAY。由于完整 Gate 仍要求更广的端到端证据，方法 Gate 继续
HOLD，主稿不自动改写。

## Main-text binding update (2026-10-08)

The v2 mechanism paragraph is now bound to the paper-level master. It uses the
same separate equal-weight scaffold-mean estimand as the v2 report and leaves the
pooled primary tables unchanged. The promoted build is
`paper/aamas2027/build/submission_20261007_161243/main.pdf` (9 pages total,
8 body pages), with source SHA `4272e3689ddd3c88be317c948b0608bae30b0d32c3d65e1be4a5ec4d1c7ac045`
and PDF SHA `788488c2c3d02e613f38cca0b91af6c2cbb7c10cbd19724e6b0ebe346669269a`.

The text reports the controlled Local--permutation contrast `+15.67 pp`
(`[+10.65,+20.74]`), permutation--Frozen `-1.27 pp` (`[-3.66,+0.95]`), and
natural Local--permutation `-0.30 pp` (`[-1.42,+0.76]`). It calls the cyclic arm
a mechanism control, not a deployable baseline, and retains the singleton-family
and evaluator-side-drift boundaries. The full record is
`main_v2_integration_20261008.md`.

The paper-level page and binding gates pass for this declared slice. The complete
scientific gate remains HOLD; this is an evidence-binding update, not a claim that
the broader method, agent, verifier, or natural-drift objectives are complete.
