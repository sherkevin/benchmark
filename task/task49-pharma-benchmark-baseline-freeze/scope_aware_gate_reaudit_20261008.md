# 投稿范围 Gate A/B/C scope-aware 独立重评

- 日期：2026-10-08
- 性质：只读审查；不改主版、不运行实验、不改变三份生效评价标准。
- 审查对象：当前投稿切片与整个项目的 broad claim surface。
- 评价标准：`docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md` v006。
- 评分规则：Gate A、B、C 各四项，每项 0--4；通过线为每门至少 12/16，且该门的附加子门槛全部满足。任一硬条件 H1--H10 失败，分数不能抵消。

## 结论先行

当前证据支持两个同时成立、不能混写的判断：

1. **窄范围投稿切片：Gate A=15/16、Gate B=13/16、Gate C=14/16，三门均达到标准线。** 这只覆盖公开 TDC BBB truth、四个 checkpoint-bound fresh providers、157 个 scaffold-disjoint 分子、一次调用、selected-only 反馈、延迟 `{0,1,4}`，以及明确标注为 evaluator-side controlled correctness drift 的压力条件。这个范围与 Stage-B v3 的状态 `PASS_FOR_DECLARED_ONE_CALL_DYNAMIC_SCOPE` 一致。
2. **整个项目的 broad scope：Gate A=11/16、Gate B=11/16、Gate C=10/16，三门仍 HOLD。** hERG/AMES、自然 provider lifecycle、完整 33-provider model-fresh、post-call verification/refusal 和端到端 agent 训练没有被当前证据闭合。窄范围通过不能把这些扩展变成已验证结论。

因此，**投稿 gate 可以按窄范围打开，不能按 broad 项目目标打开**。打开窄范围 gate 的前提是主稿、摘要、表格和 claim ledger 都严格使用窄范围措辞；若正文把 controlled drift 写成自然部署漂移，或把 33-provider development 写成 model-fresh generalization，Gate C 的结果合理性和 Gate A/B 的 scope 一致性会重新降为 HOLD。

## 1. 证据索引（source IDs）

| ID | 文件 | 已确认用途 | SHA-256（审查时） |
|---|---|---|---|
| S1 | `docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md` | 当前唯一生效的 Benchmark/Baseline 标准、H1--H10、Gate A/B/C 阈值及 R1--R8 | `3e6caca0aa41e665bd3055de16b00c133f5a3bdfc6c20b69d8fef84d6a3f56b6` |
| S2 | `task/task49-pharma-benchmark-baseline-freeze/baseline_scope_lock_and_receipt_join_v1/matrix.json` | 投稿范围锁定、9 个主行和 5 个 separate/audit 行、逐行 receipt 指针、角色和 claim scope | `1e4db4e7ca720439dd90c4d61e9ca624afce0bbbc2c69824c61c51edd96339cb` |
| S3 | `task/task49-pharma-benchmark-baseline-freeze/baseline_scope_lock_and_receipt_join_v1/independent_verification.json` | receipt join 独立核验；59 个证据文件 hash；窄范围 PASS、broad HOLD | `1a0692f34bbeb5fc7ff53676e0ebb4ab291e8e6b7068dc4d8e2ff790fd2c20ed` |
| S4 | `task/task49-pharma-benchmark-baseline-freeze/stage_b_scientific_submission_gate_v3/scorecard.json` | Stage-B 窄范围原 scorecard：A=15、B=13、C=14 | `69c4bc9d441719949ebec599409eda696db06eb38a94393a566ef0890749724a` |
| S5 | `task/task49-pharma-benchmark-baseline-freeze/provider_fresh_confirmation_v1/report.md` | 157 个 scaffold-disjoint fresh BBB confirmation；自然结果和独立回放 | `17d03e93267bf63dee21077dc97c3d2fd81e742b0c2f4c89d8a71c69d990a91f` |
| S6 | `task/task49-pharma-benchmark-baseline-freeze/provider_fresh_drift_stress_v1/report.md` | 同一 fresh 菜单上的 controlled correctness-drift stress；受控正结果，不是自然漂移 | `0d2c8452a88d5f1cb6fdffeda201ad6fe066c28c98dc8dc44c32994832ca65ce` |
| S7 | `task/task49-pharma-benchmark-baseline-freeze/provider_fresh_drift_stress_v2_bayesian/report.md` | BayesianRouter 同协议受控 comparator；适配边界和独立 recovery | `1176636a1654624ee78d950488059dc8d070977daa03c677c6fa203bd23367ee` |
| S8 | `task/task49-pharma-benchmark-baseline-freeze/primary_arrival_robustness_v1/report.md` | 128 个到达顺序完整重放、Online-SGD 主比较、独立 full-v2 核验和负结果 | `17a3191d7dc55e4be657b011d13b2ee91d36c0e4edba347b5adda8e9fa09569e` |
| S9 | `task/task49-pharma-benchmark-baseline-freeze/conditional_fusion_intervention_v1/report.md` | CGEF 终局融合候选的独立负结果和停止决定 | `2fc774db5c52383ea1655658ec8ce4d9fc323d906bf0497001e9e129de02e08a` |
| S10 | `research/AAMAS2027_benchmark_baseline_acceptance_audit_20260929.md` | broad scope 的历史审计基线：A=11、B=9（后续临时复核为 11）、C=10，及缺口说明 | `c34ca9b74c32986da9c6043a88e5834002144e644f97815d883b6db4c49bc1c5` |
| S11 | `task/task49-pharma-benchmark-baseline-freeze/gate_gap_prioritization_20261008.md` | receipt join 前后的 scope 冲突、角色整理和后续优先级 | `89203bb7fffb22814430756a16bc65d22cc957fc5d5623c35b7fb4d673eaec00` |
| S12 | `paper/aamas2027/main.tex` | 当前主稿的任务、结果、边界和 headline wording；不把本文档当成独立实验证据 | `0377e70fe9aec39158651b25bff6a898dbc9f4f13957cd478ec2cc1b02226930` |

## 2. 狭义投稿切片的独立评分

### Gate A：Benchmark 选型（15/16，PASS）

| 子项 | 分数 | 证据 | 尚缺口/限制 |
|---|---:|---|---|
| A1 公共 truth 的权威性、版本和 hash | **4/4** | S2 固定 public TDC BBB truth、P-C1 冲突处理；S4/S5 给出 source/version/checkpoint/input 绑定；冲突原始收据保留 | 这是 BBB truth 的权威性，不等于 hERG/AMES 的 assay 等价性已解决 |
| A2 routing layer 协议、来源和 scope 一致性 | **4/4** | S2/S3 固定四 provider、one-call、selected-only、release clock、`D={0,1,4}`、failure semantics；59 条证据路径 hash 核对 | routing layer 是本项目可审计协议，不应在正文称为已有社区 benchmark |
| A3 duplicate、冲突、overlap、split、confirmation 隔离 | **4/4** | S2 明确 157 个候选与旧 development 在 canonical molecule 和 nonchiral scaffold 上隔离；S5 的 confirmation 只读；旧 722 panel 与官方 test 的 overlap 未被隐藏，而是排除后保留审计 | 该结论仅覆盖当前 157 个 candidate；不能外推到整个旧 33-provider panel |
| A4 provider lineage、family 相关性和独立性边界 | **3/4** | S2/S3 绑定四个 fresh checkpoint 与 provider manifest，并把 33-provider development、family correlation 和 model-fresh 未证实状态写成边界 | 四个 provider 的独立性和更广菜单的 model-fresh lineage 不是完全证明，故不加第 4 分 |
| **合计** | **15/16** | A1、A3、A4 均 ≥3；H1--H4 在声明范围内 PASS | **PASS_FOR_DECLARED_SCOPE** |

**判断：**这是一个“公共 truth + 项目 routing protocol”的合格窄范围 benchmark 结构。它不是一个已经被社区命名和广泛使用的现成 routing benchmark；论文必须把 truth layer 与 routing layer 分开写。

### Gate B：Baseline 选型（13/16，PASS）

| 子项 | 分数 | 证据 | 尚缺口/限制 |
|---|---:|---|---|
| B1 角色覆盖 | **4/4** | S2 主矩阵明确 Random、Best Fixed、Frozen、Matched Online-SGD、Greedy LinUCB、BayesianRouter、MIRA-Relay、Retrospective Best Fixed 和 MIRA-Relative 的角色；GraphRouter/Laya-online 另列 audit-only，Replay-SGD 另列 historical control | 不把 audit-only 方法伪装成已运行主 baseline，主表角色是清楚的 |
| B2 与任务输入、反馈、目标和 dynamic scope 匹配 | **3/4** | S2/S3 对 9 个主行统一写入四-provider menu、64-bit Morgan+intercept、selected-only、延迟、one-call、failure semantics；Greedy/Bayesian 的 adapted likelihood 明确不作 native reproduction | adapted router 与论文原生协议不完全相同，不能做跨 likelihood raw ranking；因此保留 1 分缺口 |
| B3 源码/论文版本、适配和许可证保真 | **3/4** | S2 对 source/checkpoint/hash、适配差异和 `not_native_reproduction_claim` 做逐行记录；S3 对相关路径 hash 复核 | GraphRouter/Laya-online 没有完整 selected-only 在线 receipt；现代 router 是适配版本，不能声称原论文完整复现 |
| B4 同一 menu、feature、初始化、反馈、预算和 tuning contract | **3/4** | S2/S3 固定 menu、feature、selected-only feedback、delay、seed、one-call budget、failure semantics，并禁止 privileged ranking、missing-field imputation 和 confirmation 回流；S4 的 serialized MIRA state 与 replay verification 补强了该项 | live refusal、hERG/AMES、full 33-provider model-fresh 和端到端 agent 预算未覆盖；不能把窄范围公平性写成 broad efficiency 公平性 |
| **合计** | **13/16** | 四项均 ≥3 | **PASS_FOR_DECLARED_SCOPE** |

**判断：**baseline roster 对窄范围问题是“角色充分、适配边界显式”的集合，而不是所有现代 router 的原生复现榜单。最重要的公平性结果不是方法数量，而是禁止跨 likelihood raw ranking、禁止 privileged control 排进 deployable ranking、禁止从事件数量推算缺失字段。

### Gate C：实验矩阵与结果（14/16，PASS）

| 子项 | 分数 | 证据 | 尚缺口/限制 |
|---|---:|---|---|
| C1 矩阵覆盖 RQ、E1--E5 和声明的 dynamic scope | **4/4** | S4/S5/S6/S7 覆盖任务有效性、主比较、现代 adapted controls、selected-only delayed feedback、controlled stress、成本和失败语义；S8 补充固定 cohort 到达顺序；S2 将 extensions 排除在 primary claim | environment-nonstationary、provider arrival/retire/version 和端到端 agent 不在这个 dynamic scope |
| C2 全部主 baseline 在同一协议闭合 | **3/4** | S2/S3 的 9 主行均绑定同一 fresh four-provider contract；S5/S6/S7 以及 recovery receipt 可追溯到 raw events；不同 likelihood/privileged role 被分层 | Random/Best Fixed/evaluator-only control 的角色并非与在线方法同一“动态学习”估计量；adapted methods 仍需单独解读，故不加第 4 分 |
| C3 结果方向、控制关系、成本和主张边界合理 | **3/4** | S5 自然 fresh：MIRA=Frozen=89.81%，不把 null 写成 generic gain；S6 受控 correctness drift：MIRA=87.56%、Frozen=73.38%，并报告 matched controls；S7 补 BayesianRouter；S8 controlled MIRA−Online-SGD `+4.7363 pp`，自然 MIRA−Frozen `−0.2870 pp`；S9 CGEF pooled relative to current rule 约 0、relative to ignore-second `−0.000659`，按卡内停止 | controlled flip 是 evaluator-side stress，不是自然 provider drift；S8/S9 都不能扩大 claim 或替代新 confirmation |
| C4 paired uncertainty、failure/unknown 分母、恢复和负结果完整 | **4/4** | S2/S3 59 条 receipt/hash；S5/S6/S7 raw JSONL、config、checkpoint/input hash、contract/replay verification；S8 128-order full-v2 independent recovery、全部 61,440 streams 完整；S9 独立 CGEF 重算并保留负结果；自然 null、第 69/86/90 负排列和 CGEF 停止均未删除 | fresh verifier 是 contract/replay 与 aggregation 分离，不宣称第二套 provider inference；若未来标准提高到完全独立的第二算法实现，需要再补，但当前 v006 证据已足以给 4 |
| **合计** | **14/16** | C1--C4 均 ≥3；E1--E5 不缺 | **PASS_FOR_DECLARED_SCOPE** |

**窄范围 gate 判定：**三门均通过。这个结论支持“可以按声明的一次调用 BBB slice 进入投稿审查/主稿使用”，不支持“整个项目目标已经完成”。

## 3. Broad project scope 的独立评分

这里的 broad 不是把窄范围结果重新加权，而是审查当前项目还保留的更大 claim surface：多端点（含 hERG/AMES）、完整 33-provider model-fresh、自然 lifecycle/arrival/retire/version drift、post-call verification/refusal、端到端 agent/tool-use gain，以及跨这些条件的 baseline 结论。

### Gate A：Benchmark 选型（11/16，HOLD）

| 子项 | 分数 | 证据 | 尚缺口 |
|---|---:|---|---|
| A1 公共 truth 权威性、版本和 hash | **3/4** | BBB truth 已冻结并可追溯；S10 记录 hERG/AMES 的来源审计仍有 assay/cutoff/training-lineage 未闭合 | broad 多端点 truth layer 仍不完整，不能用 BBB 的权威性代表所有 endpoint |
| A2 routing layer 协议、来源和 scope 一致性 | **4/4** | S2/S3 的 routing contract 对 one-call selected-only delayed replay 是机器可核对的 | broad 还包含 post-call、refusal、lifecycle 和 agent interfaces；它们不是同一个已闭合协议 |
| A3 duplicate、冲突、overlap、split、confirmation 隔离 | **1/4** | BBB 的 157 fresh slice 已隔离；S10 仍记录旧 722 panel 与官方 BBB test 的 canonical/scaffold overlap，以及 broad confirmation 未统一 | full 33-provider/multi-endpoint/model-fresh confirmation manifest 尚未闭合；这是 A3 的主要 blocker |
| A4 provider lineage、family 相关性和独立性边界 | **3/4** | S2/S3 明确四 fresh checkpoints 与 33-provider family correlation；未把 33 个变体当 33 个独立来源 | full menu 的训练成员、checkpoint-specific model-fresh exposure 和 cross-endpoint lineage 仍未完全证明 |
| **合计** | **11/16** | A3 低于 3，Gate A 不通过 | **HOLD** |

### Gate B：Baseline 选型（11/16，HOLD）

| 子项 | 分数 | 证据 | 尚缺口 |
|---|---:|---|---|
| B1 角色覆盖 | **3/4** | S2 已把主比较、adapted、ablation、historical 和 audit-only 角色分开；这改善了旧名单双重角色问题 | broad 的 GraphRouter/Laya-online、post-call/refusal 和端到端 agent 近邻仍无完整可部署 receipt |
| B2 任务匹配 | **3/4** | one-call selected-only delayed selector 的任务匹配已闭合 | broader agent/post-call/lifecycle objective 没有共同输入、反馈、目标和预算合同 |
| B3 版本/适配/许可证保真 | **3/4** | 主行 source/checkpoint/适配边界和 59 条 evidence hash 已记录；adapted 不冒称 native | GraphRouter/Laya-online 在线更新接口/receipt 未闭合，跨扩展方法的原生 fidelity 仍未知 |
| B4 公平合同 | **2/4** | 窄范围统一 menu/feature/feedback/delay/budget/seed 已核对 | broad 还缺 post-call/refusal、agent token/call budget、lifecycle event、full 33-provider model-fresh 和统一 live cost/unknown 合同 |
| **合计** | **11/16** | B4 低于 3，Gate B 不通过 | **HOLD** |

### Gate C：实验矩阵与结果（10/16，HOLD）

| 子项 | 分数 | 证据 | 尚缺口 |
|---|---:|---|---|
| C1 矩阵覆盖 RQ、E1--E5 和 broad dynamic scope | **3/4** | S4--S9 对窄 one-call feedback-adaptive scope 已有 E1--E5、自然 confirmation、受控 stress、arrival uncertainty 和负融合结果 | natural lifecycle、arrival/retire/version shift、hERG/AMES、post-call refusal、agent gain 没有完整 E1--E5/E6 矩阵 |
| C2 全部主 baseline 同协议闭合 | **2/4** | S2/S3 只证明窄 fresh slice 主行闭合，并把 GraphRouter/Laya-online 降为 audit-only | broad 方法集合跨协议、likelihood、接口和信息权限仍不统一；不能把窄 scope raw receipt 合并成 broad leaderboard |
| C3 结果方向、控制关系、成本和主张边界合理 | **3/4** | 窄范围结果方向自洽：自然 null、受控 positive、arrival 条件边界、CGEF negative；S12 主稿明确多处边界 | broad 没有自然非平稳或 agent-level 结果；任何 broad superiority/generic tool-use claim 都会越界 |
| C4 paired uncertainty、failure/unknown 分母、恢复和负结果完整 | **2/4** | 窄 scope 有 raw/recovery/CI/negative receipts，S8/S9 进一步增强审计 | broad endpoint/provider/lifecycle/refusal/agent 分母和独立恢复缺失；窄范围的完整性不能代替 broad completeness |
| **合计** | **10/16** | C2、C4 低于 3，Gate C 不通过 | **HOLD** |

## 4. Agreement / disagreement matrix

| 问题 | 现有材料的共同点 | 需要保留的差异 | 本次裁定 |
|---|---|---|---|
| 当前论文主范围是什么 | S2、S3、S4 都指向四 fresh providers、157 molecules、one-call selected-only、`D={0,1,4}` | S10 是更早的 broad audit，不能覆盖新 scope | 论文当前主范围采用窄 slice；S10 作为 broad 历史审计保留 |
| 窄 Gate A | S4=15/16；S3 独立 receipt join 也标 `PASS_FOR_DECLARED...` | A4 仍只有 3，不能写成 full model-fresh independence | 15/16 PASS，A4 的 1 分缺口保留 |
| 窄 Gate B | S4=13/16；S2 将 9 个主行和 5 个 separate/audit 行分开 | Greedy/Bayesian 是 adapted，不是 native reproduction；GraphRouter/Laya 不在主排名 | 13/16 PASS；adaptation 仍写进正文/附录边界 |
| 窄 Gate C | S4=14/16；S8 增强到达顺序和负结果；S9 关闭 CGEF 候选 | arrival 只针对 fixed inspected cohort；CGEF 不是主方法效果 | 14/16 PASS；这些材料只提高 C4/边界，不扩大 dynamic claim |
| fresh verifier 的“独立性” | contract/replay 与 aggregation 分离，主状态和输入可恢复 | S4 明确没有宣称第二套 provider inference；不是完全独立重写所有 runner | 当前 v006 可给 C4=4；若将来标准要求第二独立算法实现，应把 C4 上限改为 3 并补实现，不在本次追溯改分 |
| CGEF 是否证明第二工具无价值 | S9 只证明当前 Gaussian terminal fusion 候选没过停止线 | 不能推成第二工具一般无信息或一般有害 | 作为负候选停止证据，不改变 Gate A/B，不进入主效果 claim |
| broad 分数是否被 narrow PASS 覆盖 | 没有。S2/S3 明确 `broad_status=HOLD` | 历史 broad B 曾出现 9/16 和临时 11/16，原因是时间/scope不同 | 当前 broad 采用 11/11/10；不删除旧审计，只用 scope 字段解释差异 |

## 5. 已确认事实与候选主张

### 5.1 已确认事实（不把预测当结果）

- **F1（S2/S3）**：机器矩阵锁定了 4-provider、157-molecule、one-call、selected-only、delay `{0,1,4}` 的 fresh BBB 投稿切片；59 个 evidence path 的 hash 通过。
- **F2（S5/S4）**：自然 fresh 上 MIRA 与 Frozen 都是 89.81%，paired difference 0.00 pp；这不是自然 stationary online gain。
- **F3（S6/S4）**：evaluator-side controlled correctness drift 上 MIRA 87.56%、Frozen 73.38%，paired delta +14.18 pp，CI 为 `[+9.61,+19.03]` pp；这是受控机制压力，不是自然部署漂移。
- **F4（S7）**：BayesianRouter 在同一 controlled stress 为 80.28%，相对 Frozen +6.90 pp，作为 adapted comparator；不能称 native protocol reproduction。
- **F5（S8）**：128 个固定 cohort 到达顺序的完整回放中，controlled MIRA−Online-SGD 为 +4.7363 pp，近似均值区间 `[+4.3167,+5.1558]` pp，125/128 个 order 胜出；自然 MIRA−Frozen 为 −0.2870 pp，区间 `[−0.3723,−0.2016]` pp。该结果条件于已检查 cohort/到达分布。
- **F6（S9）**：CGEF 相对原 masked-logistic pooled utility 约为 0，相对 `ignore-second` 为 −0.000659，未过预登记停止线；独立复算通过，候选停止。
- **F7（S10/S3）**：broad 项目尚缺 hERG/AMES qualification、完整 33-provider model-fresh、自然 lifecycle、post-call/refusal 和 agent-level receipts；旧 broad 审计因此保持 HOLD。
- **F8（S12）**：主稿当前文字将自然结果写成 null、controlled 结果写成 recovery，并明确不把 routing layer 称为现成社区 benchmark；最终 claim 是否完全一致仍需一次稿件 claim audit。

### 5.2 可写成候选主张，但必须绑定范围

| 候选主张 | 可以由哪些事实支撑 | 不能越过的边界 | 若出现何种结果应撤回 |
|---|---|---|---|
| P1：MIRA-Relay 可在 selected-only delayed feedback 下实现 fixed fresh BBB 菜单上的在线适应 | F1、F3、S6、S8 | 只覆盖 4 fresh providers、157 fixed cohort 和 controlled correctness drift；不是自然 lifecycle | release 后 state 不变、replay mismatch 或相对合法 matched control 的预登记主比较失败 |
| P2：MIRA-Relay 的已确认收益是受控 correctness recovery，而非 stationary improvement | F2、F3、F5 | 不能写 generic online gain、自然 drift recovery 或 broad agent gain | 自然 fresh 稳定正收益或 controlled 对照不再满足预登记边界时，需重写 headline |
| P3：source-aligned selected feedback 是一个可审计的机制变量 | S6、S8 的 matched credit/arrival receipt | 不能声称识别 family sharing、验证/拒答收益或跨 provider lineage 因果 | matched permutation/ablation 不再保留 source-aligned contrast，或状态与日志不一致 |
| P4：当前 baseline 组合足以审查窄范围主张 | S2、S3、S4、S5--S8 | adapted baseline 不得当 native reproductions；不能支撑 broad leaderboard | 任一主行 raw/recovery/hash/fairness 字段缺失或 confirmation label 回流 |
| P5：当前项目已经证明动态工具调用在广泛科学 agent 上有效 | 当前证据不足 | 需要 broad Gate A/B/C、自然 lifecycle、post-call/agent 和多端点 receipts | 现状直接判定为不可写，不应放入摘要或结论 |

## 6. 决定性测试与 kill criteria

这些是下一次修改主张或重新评分时的硬条件，不是新结果。

1. **主稿 scope 一致性测试（最高优先级）**：逐句检查摘要、引言、方法、主表标题、结论和图注。出现 “generic online gain”“natural provider drift”“33-provider model-fresh”“all scientific agents” 等超出 S2 的措辞，立即把对应 Gate C3、Gate A2 或 H8 标为 HOLD，先修文本。
2. **主行 receipt 完整性测试**：S2 的 9 个 primary rows 必须继续能指到 raw event、independent recovery、input/menu/checkpoint hash。任一关键文件缺失、hash 漂移或角色字段改变，Gate B3/B4 和 C4 不能给 3 以上。
3. **公平性/泄漏测试**：confirmation truth 不能进入 fit、epoch、threshold、menu、baseline 选择或 state；privileged retrospective control 不能进入 deployable ranking；跨 likelihood 不得 raw rank。任一失败触发 H5/H6/H8，三门整体 HOLD。
4. **结果合理性测试**：自然结果、controlled stress、arrival-order uncertainty、CGEF negative 必须分表；受控 flip 不得改称自然 drift；区间跨 0 时不得称稳定胜出。R1、R2、R4、R6 任一失败，Gate C 直接 HOLD。
5. **独立恢复测试**：对主表任一 headline 数字，必须从 raw event/config/hash/recovery 重建，不能从手工汇总倒推。出现 state/action/release mismatch 或不完整分母时，C4 归零或最高 2 分，窄 Gate C 关闭。
6. **Broad claim kill line**：若论文坚持 broad 多端点/lifecycle/agent claim，而没有新增对应的 authority、lineage、同合同 baseline、paired uncertainty 和 confirmation，必须维持 broad A/B/C HOLD；不得用 narrow 15/13/14 抵消。
7. **CGEF 候选停止线**：不要再调 covariance/shrinkage/Gaussian basis 来追过 `+0.01`。S9 的 candidate-specific negative result 已满足停止条件；若重新开卡，必须有新的问题、对照和独立验证。

## 7. 下一步排序（按 gate 收益/成本）

### P0：完成一次主稿 claim-scope 审计并与 S2 同步

- **动作**：只读核对 `paper/aamas2027/main.tex` 的摘要、主表、方法、结果、限制和结论；逐条绑定 S2 的 `claim_scope`。将 one-call fixed fresh BBB、feedback-adaptive、controlled recovery 和 natural null 作为唯一主句式。
- **理由**：当前窄 Gate 已过，最大剩余风险是文本把窄证据说成 broad 结论；成本低，直接决定 gate 是否可用。
- **停止线**：发现 broad wording 时先修正文，不新增实验，不改变 headline 数字。

### P1：把 S2/S3 receipt join 压成论文可读的 baseline role/fidelity 表

- **动作**：主表或附录逐行标明 Random/Fixed/Frozen/Matched/Greedy/Bayesian/MIRA 的 role、adapted/native、comparison stratum、raw/recovery receipt；GraphRouter/Laya-online 放 audit-only 注释；Retrospective Best Fixed 只作 headroom。
- **理由**：将 13/16 的公平性证据变成审稿人可读的表，防止把“数量多”误解为“比较公平”。
- **停止线**：如发现任一主行缺关键回执，先降级角色，不用补写或从事件数量推算。

### P2：做一次不涉及新效果跑批的独立 reviewer gate walkthrough

- **动作**：用 S1 的 R1--R8 和 Gate A/B/C 表逐项让未参与实现的复核者抽查主表和 headline 数字；尤其检查 adapted likelihood、confirmation 边界和 128-order 条件范围。
- **理由**：Stage-B scorecard 已有机器证据，但 reviewer 可读性和 claim 复述仍需验证；比重复运行 CGEF 或重加旧 cohort 更有价值。
- **停止线**：若复核者无法在规定时间内找到证据路径，降相应分数，先补文档或收窄主张。

### P3：只在 P0/P1 暴露不可替代缺口时做定向 baseline 卡

- **动作**：如果某个主比较 baseline 的 raw/recovery/fairness 字段确实缺失，再为同一 4-provider、157-molecule、delay `{0,1,4}` 合同写一张最小实验卡；否则不重复跑已完成 receipt。
- **理由**：当前主 baseline roster 已闭合到窄范围，新增弱化版 GraphRouter/Laya-online 或再调 CGEF 的边际收益低。
- **停止线**：新 baseline 不能在同一 input/menu/feedback/budget 下完成时，降为 audit-only，不进入主榜。

### P4：Broad 扩展单独立项，不阻塞窄范围投稿稿

- **动作**：若仍要实现 broad 目标，依次为 hERG/AMES endpoint qualification、full model-fresh provider lineage、natural lifecycle event、post-call refusal utility 和 agent-level tool-use gain 写独立卡；每项先过 Gate A/B/C 子门。
- **理由**：这些是新的科学问题，不是靠窄范围表格补齐的排版缺口；混入当前稿会降低而不是提高可审查性。
- **停止线**：任何扩展缺 truth/lineage/同合同 baseline/independent recovery，就保持 HOLD，不把它写成已完成目标。

## 8. 尚未由文件裁定、需要人确认的问题

1. 是否正式把“公开 TDC BBB + 四 fresh providers + one-call selected-only delayed feedback + controlled correctness recovery”定为本次投稿的唯一主 claim surface，并把 broad 目标明确放入未来工作？
2. 主文是否保留 post-call refusal 作为 secondary development analysis；若保留，是否在标题、表格和讨论中明确它不属于 primary Gate C？
3. 对 C4 的严格解释是否接受“contract/replay 与 aggregation 分离、但不重新实现 provider forward”的独立性；若不接受，需把窄 C4 暂记为 3/4 并补第二实现。
4. GraphRouter、Laya-online 的 audit-only 角色是否进入正文脚注/附录，还是只留 artifact；两种选择都不能把它们写成已完成在线 baseline。
5. 如果最终坚持 broad claim，优先补哪个不可替代证据：多端点真值/lineage、自然 lifecycle、post-call/refusal utility，还是端到端 agent 任务？当前文件不能替人决定科学资源排序。

## 9. 最终状态

| scope | Gate A | Gate B | Gate C | 投稿状态 |
|---|---:|---:|---:|---|
| 窄范围：fresh BBB one-call dynamic scope | **15/16 PASS** | **13/16 PASS** | **14/16 PASS** | 可进入窄范围投稿审查；必须保持 claim scope |
| Broad project scope | **11/16 HOLD** | **11/16 HOLD** | **10/16 HOLD** | 不能打开 broad 科学投稿 gate |

本文件不改变主版 `paper/aamas2027/main.pdf`，也不替代三份生效主线文档。它只把当前证据、范围、分数和下一步放在同一份可审查记录中，避免用历史 broad HOLD 或当前 narrow PASS 互相覆盖。
