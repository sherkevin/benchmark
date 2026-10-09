# 管理者综合：故事线与方法论多 agent 辩论

- 日期：2026-09-29
- coordinator：`/root`
- 输入：Round 0 独立审查、Round 1 交叉挑战、Round 2 最终立场，以及三份生效主线文档、三份评价标准和 `current_acceptance_audit_v2`
- 状态：审查结论；未自动更新生效主线，未形成新的用户确认决议

## 1. 我如何管理这次讨论

三名审查者分别从 AAMAS 故事、方法理论和 benchmark/baseline 三个角度独立开始；Round 1 只攻击高影响假设；Round 2 对冲突给出最终立场。我不按多数票裁定，而是把每个结论拆成证据、推理、决定性测试和置信度。报告中保留了相互反驳的意见，尤其是“是否现在降级 method candidate”“fixed-pool 是否足以称 dynamic”“baseline 是否继续扩张”三个分歧。

本次辩论不是新的实验，也没有产生新的 accuracy、recovery 或 confirmation 结果。它的作用是找出哪些主张可以进入下一张 experiment card，哪些主张必须暂时停止。

## 2. 三轮讨论收敛出的共识

### 2.1 当前主方法没有被证明

当前 development master table 中，`Best Fixed=0.9155` 仍高于 Matched Online-SGD、Replay、Greedy、PILOT 和 BayesianRouter。post-call confidence escalation 的正信号不能回填为 pre-call selector；dynamic verifier 的负结果也不能被忽略。因而“dynamic JEV 已有效”目前不是可写入摘要或结论的事实。

更准确的现状是：我们有一个真实的工具可靠性问题、一个可复核的 routing protocol 候选、selected-only delayed feedback 的负诊断，以及一个仍需同合同证伪的单一 method candidate。

### 2.2 selector、verifier、updater 必须分开估计

`a_t=π_sel(x_t,e_t,A_t,H_t)` 是调用前选择；`v_t=π_ver(o_t,c_t,s_t)` 是拿到被选输出后的复核/升级/拒答。它们的输入、动作、成本和标签不同，不能共用一个 pooled accuracy headline。若未来要写组合方法，必须在同一预算下分别报告 selector-only、verifier-only 和 interaction utility。

### 2.3 selected-only 反馈带来真实的可识别性边界

在没有探索、propensity、fit-only prior 或结构共享条件时，两个未选 provider 环境可以产生相同可见历史，却有相反的最优动作。这个 P/Q 反例否定的是“无条件识别未选 provider 反事实”的强主张，不是所有 provider adaptation。下一候选必须把条件集合 `𝒜` 和探索成本写进 estimand；没有 `𝒜` 时只能报告 selected-provider calibration、已观测事件适应或不可识别性边界。

### 2.4 fixed-pool 可以支持一个较窄的动态范围

固定 33-provider menu、selected-only release 和 delay `{0,1,4}` 足以检验 `feedback-adaptive`：反馈释放后状态改变，之后 action 可能改变。它不足以支持 `environment-nonstationary`、自然 provider arrival/retire/version evolution 或普遍动态工具生态。后者需要单独的事件协议和 Frozen-pool 对照。

### 2.5 自建 routing layer 有条件可作主 benchmark

TDC truth 是公共 truth layer；provider menu、反馈时钟和事件合同是本项目的 routing layer。后者不能冒称社区 benchmark，但只要 truth/version/hash、provider lineage、冲突和 unknown 规则、无泄漏合同、独立 checker、overlap/confirmation 和 raw recovery 完整，它可以作为领域化的 AAMAS 主任务。当前 Gate A 还未闭合，所以目前只能称 development routing protocol。

### 2.6 baseline 采用因果角色最小完备集

不再以“方法数量”作为先进性证据。最小 roster 是：Random、Best Fixed、Frozen/Audit-Frozen、Matched Online/Replay、一个保真的 delayed/contextual nearest、一个通过 fidelity audit 的 modern router、Oracle；只有存在相应 RQ 时才加 static verifier/second-call controls。每个槽位都要有同菜单、同表示或明确匹配、同预算、同反馈时钟、raw run、recovery、成本和 paired uncertainty。

## 3. 仍然存在的分歧

| 分歧 | 当前两种意见 | 管理者处理 |
|---|---|---|
| H4 integration gain | 故事审查倾向“方法冻结前 UNKNOWN”；若冻结后无组合增益则 FAIL。 | 先冻结唯一 Proposed，再做 integration ablation；不提前判死，也不提前通过。 |
| dynamic 的含义 | 一方担心没有自然事件不能称 dynamic；另一方区分 feedback-adaptive 与 environment-nonstationary。 | 当前只允许 `feedback-adaptive` 术语；自然事件只作为更强扩展。 |
| protocol/interface 是否足够 AAMAS | 可能成为有价值的 protocol/negative diagnosis，也可能因缺少 agent-level utility 而不够。 | 保留为条件分支；必须闭合 Gate A/B/C，并证明 utility、可复核性或 agent impact。 |
| baseline 数量 | 需要现代强对手；也不能无限堆所有 router。 | 按因果角色最小完备；不可保真方法记 audit-only。 |
| Best Fixed 的作用 | 直接选择 accuracy claim 必须过 Best Fixed；其他 risk/cost/recovery claim 可有自己的 utility。 | 每个 RQ 预注册 primary utility，不用一个固定准确率门槛杀掉所有贡献。 |
| 一篇还是两篇 | 不能把 selector/verifier 混成一个结果；也不应现在 salami split。 | 先一篇拆 estimand/RQ/结果块，做 2×2；分稿留到独立 novelty/evidence 成立后。 |

## 4. 评价标准快照

- **故事线标准 v004**：Round 0 临时 `62/100`；H4、H5、H8 为 UNKNOWN；不能标 `STORY READY`。S3/S4/S6 尚不能靠文字修复。
- **方法论标准 v004**：selected-only 可识别条件、唯一 Proposed、更新不变量、匹配实验和 integration gain 尚未闭合；H4 在冻结前保持 UNKNOWN，M5/M8/M13/M15 不能先给通过分。
- **Benchmark/Baseline 标准 v005**：Round 1 各 Gate 的临时评分约为 A=`9/16`、B=`9/16`，C 仍不足；以 `current_acceptance_audit_v2` 的 authoritative 判断为准，Gate A/B/C 全部 HOLD。不能用某一门的分数抵消另一门缺口。

## 5. 管理者当前决定（不等同于用户已确认决议）

1. **工作 scope** 暂定为 `BBB fixed-menu, selected-only, delayed, feedback-adaptive routing`；不使用未被事件证据支持的 `environment-nonstationary` 或“动态 JEV 已验证”表述。
2. **方法候选** 只保留一条、隔离、可证伪的 Proposed branch。它必须写清 `𝒜`（exploration/propensity、fit-only prior 或结构共享）、label/failure/unknown、utility、状态更新和 kill criterion。
3. **下一实验顺序** 是 Gate A/B/C 证据收口 → P/Q synthetic identifiability test → 唯一 Proposed 的同合同 CPU matrix → 2×2 selector/verifier utility → 一次独立 confirmation。未通过 CPU gate 不开新的效果型 A800。
4. **主线文档** 暂不改写；本综合报告、各轮报告和 TODO_LOG 保存本轮讨论。若要把 scope、创新点或 benchmark 资格升为生效内容，必须另行共同确认并按决议文件规则归档。

### Owner correction：目标 claim 保持

2026-09-29，Owner 明确要求不能因为当前实现版本的负结果而降低最终目标 claim，也不能直接使用 JEV 作为论文方法名。上面“protocol/negative diagnosis 作为当前主投稿定位”的建议因此只保留为**失败后的兜底分支**，不再作为当前主线定位。当前主线继续以 MIRA 为目标方法，并通过修复信息合同、状态更新、可识别条件和实验实现来逼近目标。

这条修正不改变辩论暴露出的证据约束：当前结果仍不能写成 MIRA 已经验证；它只改变研究定位和下一步资源分配。新的工作循环是：问题根因 → MIRA 候选修改 → CPU 反例/合同检查 → 一条 Nebula A800 任务 → 独立恢复与结果解释。只有所有合法候选都失败后，才重新讨论是否修改目标 claim。

## 6. 下一步动作矩阵

| 优先级 | 动作 | 通过条件 | 产物 |
|---|---|---|---|
| P0 | 完成 Gate A 双层 manifest、lineage、overlap、confirmation | A≥12/16，A2/A3/A4≥3 | authoritative manifest、checker、audit |
| P0 | 冻结 baseline role matrix 与统一 contract | B7/B9/B10/B14≥3；modern router 通过 fidelity 或标 audit-only | role matrix、raw-run/recovery 收据 |
| P1 | P/Q 不可识别性单元测试 | 无条件 updater 在 P/Q 产生同一可见轨迹；否则信息泄漏 | synthetic report、independent verifier log |
| P1 | 唯一 Proposed fixed-pool CPU replay | d∈{0,1,4}；至少 2/3 delay 相对 strongest matched 超出 ROPE；无标签/调用偷换 | experiment card、raw JSONL、paired CI |
| P1 | selector/verifier 2×2 | 分别报告 first-call、final accepted risk/coverage、calls、latency、interaction utility | factorial table、claim–evidence map |
| P2 | 独立 confirmation | frozen code/input、只读 recovery、与 development 分离 | confirmation report |
| P3 | 写作和投稿定位 | 通过 H1–H10、Gate A/B/C 后再写 positive headline | paper sections、abstract update |

## 7. 什么被吸收，什么暂不吸收

**吸收：** P/Q 反例；selector/verifier/updater 的 estimand 分离；feedback-adaptive 与 environment-nonstationary 的 scope 分层；按因果角色选 baseline；`full−strongest_matched` 和 claim-specific utility；Gate A/B/C 独立闸门；2×2 interaction；报告少数意见和停止条件。

**暂不吸收为主线事实：** dynamic JEV 优越性；自然 provider drift；GraphRouter/Laya 已完成保真复现；protocol/interface 已足够构成 AAMAS 主贡献；任一 post-call 正信号可以替代 pre-call selector 证据。

## 8. 观察结论

这次讨论没有把项目推向“再堆一个 online head”。它把争论压缩成两个可执行问题：在什么信息条件 `𝒜` 下，provider adaptation 可识别并有 utility；在什么证据条件下，自建 routing protocol 足够成为可复核 benchmark。两者都尚未通过。当前证据状态是 `HOLD / REWORK`，但目标方法仍是 MIRA；下一步继续做决定性审计、小规模 CPU 实验和经过 CPU 闸门的 Nebula A800 迭代，不把当前负结果写成最终结论。
