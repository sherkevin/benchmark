# 下一步计划：把医药直接选择任务和实时更新方法做实

日期：2026-09-26

状态：Owner 已批准持续执行，见 [ADR-0022](../../docs/adr/0022-continuous-aamas-research-loop.md)。研究范围沿用 [ADR-0021](../../docs/adr/0021-pharma-first-direct-selection-benchmark.md)；方法候选、协议细节和实验判据仍按下文逐项审查。计划编写时未启动实验，实际执行记录见 Task49/Task50 与 TODO_LOG。

目标很具体：给定分子、性质和候选工具，JEV 选择一个工具；题后收到该次选择的对错反馈，更新后服务下一题。最终要说明更新为什么有效、为什么够快，以及相比已有方法多解决了什么。

目前确定的是任务方向和 baseline 名单。BBB 指纹静态诊断与 calibrated switch 解释分析已经完成，但没有超过 Best Fixed；因此 backbone 和最终更新算法仍未定下来，当前没有该主任务的正向方法效果结论。已有 BBB 仍是 development 数据；旧 M2/M3 的复核收益结果不承担这条主线的效果证据。

最新小任务结论：Morgan 指纹 MLP 的 raw argmax 为 `0.8961/0.8906`，validation-selected calibrated switch 为 `0.8947/0.9100`，Best Fixed 为 `0.9155`。10 个 outer fold 没有一个 switch 超过 fixed。Laya 静态 head 也未通过闸门；普通 Online-SGD/Replay-SGD 只改善 Frozen warm start，仍低于 Best Fixed。

随后做了两项不占 GPU 的信息筛选。stream drift 的 winner 变化与 within-fold permutation null 接近，暂不把 order-only change point 当作方法来源。fit-only provider 残差关系虽然有 transfer AUC（五折均值 `0.7366`），但低秩、scalar 和 gated provider-transfer replay 都没有超过 independent selected-only，已停止这条主线。相似 hidden feature 的 context residual memory 在正式回放的 delay 1/4 达到 `0.8969/0.8976`，超过 independent bias 的 `0.8948/0.8927`，也超过随机 memory；delay 0 仍低于 independent。下一步冻结 memory 方法卡，先接统一 runner 做 replayable smoke，再做完整 development，confirmation 继续关闭。

### 2026-09-27 方法语义复核后的更正

上段的 context memory 数字使用了未校准 BT 排序分数，不能作为方法效果证据。独立审查发现 $\sigma(s_{BT})$ 没有 correctness 概率语义；原计划的 context-memory A800 smoke 尚未提交，避免重复错误定义。按新协议在 fit 区拟合 per-provider affine logistic correctness calibrator，再在概率空间读取 selected-only residual。新回放中 Frozen calibrated 为 `0.8831`，context memory 为 `0.8755/0.8727/0.8700`，independent 为 `0.8769/0.8769/0.8713`（delay `0/1/4`）；memory 三个 delay 均未超过 independent，停止该主线。v1 正向结果保留为历史负证据，不再写入主表。

当前下一步不是继续跑旧 smoke，而是提出一个 score 语义自洽、可在 CPU 上先证伪的新更新机制；通过后再做一次 A800 端到端 smoke。没有新方法通过前，confirmation 和多端点扩展都保持关闭。

## 1. 先回答现有数据究竟能支撑什么

复用 Task24 的真值候选清单，核对 hERG、AMES 的原始实验定义、标签方向、阈值和现有分子池交集。逐条记录分子身份、原始出处、标签冲突、缺失和排除原因。一个公开数据集很大，不代表它覆盖当前分子池，也不代表它与工具预测的是同一个端点。

交付 `data_contract_v0.md` 和逐分子来源清单，说明各端点实际可用的分子数、具体 provider、来源家族、真实失败、缺失预测、可公开字段和来源哈希。09-26 逐行审计更正：BBB 有 722 个带参考标签的分子，其中 716 个有完整 33-output；652 是七家族投票齐全且可评分的分子数，不是具体模型完整面板，不能混用。

分别记录两种隔离：选择器是否用过这些分子调方法；上游工具是否用过这些分子训练。池外分子、较晚数据或 scaffold 留出都不能自动证明工具未见过。相同分子跨端点复用时，也要防止身份或骨架跨区及统计重复计数。

**验收**：每个端点给出可用、缺证据或暂不可用的结论及具体数量。覆盖不足就列出需要补采的分子与工具结果；不拿工具共识填实验真值，不为满足三个端点的形式要求强行拼数据。

## 2. 把 baseline 名单变成可复现的适配方案

先写 `baseline_adapters_v0.md`：每个方法列出原论文输入、监督、决策和更新方式，再对应到分子、具体 provider、二元反馈，逐项记录改动。核清论文及代码版本、许可证和特征接口后，再实现。

| 对照 | 实现顺序与用途 |
| --- | --- |
| Best Fixed、Oracle | 先验证评分器；前者只用训练区选工具，后者仅是事后上界 |
| Greedy LinUCB | 接通现代在线方法，核对单步适配保留了什么、删除了什么 |
| PILOT、BayesianRouter | 优先完成的最近邻，直接检验“离线先验加在线更新”是否已有充分解法 |
| GraphRouter | 首个静态强 router；复用可用开源实现并核对原论文，RouterDC 作为补充候选 |
| Frozen JEV、Online SGD、Replay SGD | 与候选方法共用 backbone 和初始化，检验更新本身的收益 |

静态强 router 的最低要求按 ADR-0021 的“至少一个”执行；Task49 报告末尾要求两者都跑的表述不增加当前必做工作。普通固定策略是参照，现代近邻与同 backbone 对照承担方法竞争力判断。

**验收**：适配表能回答每个方法看到了什么、学了什么、改了原算法哪里。全信息离线训练面板必须向同组方法等量开放；零目标标签冷启动与使用医药历史标签的初始化分别报告。未调用工具不补负标签。

## 3. 与数据审计并行，把实时更新方法推导完整

先写一页 `method_card_v0.md`，说清一个中心问题：反馈少、只来自所选工具时，怎样用有限计算及时修正选择，而不破坏已有能力。先比较可复用的候选，不把“在线更新”四个字本身当创新。

从 Task45 已审的 Laya 特征接口与小型在线头出发，明确冻结哪些参数、更新哪些参数、保留什么历史。用一个真实分子走完输入、选择、反馈、更新和下一次预测；每个新符号及公式都附实例，公式使用 `$` 或 `$$`。Laya 的选择概率不能直接当作工具正确率。

推导单次更新与逐步累积目标的关系，写清时间和内存复杂度、数值稳定条件、延迟反馈处理。如果更新 backbone，就必须说明旧特征和历史统计何时失效、如何重算，并把这部分成本计入。理论审查验证计算正确性和假设下的性质；实际准确率留给真实数据检验。

**验收**：给出一个主候选、一份可直接实现的伪代码，以及相对 PILOT、BayesianRouter、普通 SGD 的具体差异。优先检验冻结表示加小状态更新是否足够；只有明确的表示缺陷才引入更重的微调。若更新式与已有方法等价，就如实作为 baseline，继续寻找任务尚未解决的缺口。

## 4. 共用一套评价协议，再做第一轮小实验

复用 Task36 回放、judge、Task42 反馈时钟与日志合同。选择器只得到允许的输入和已释放反馈，完整输出矩阵留在 evaluator。记录先决策后更新、模型状态版本和每条反馈的消费次数。工具执行失败保留在分母；未采集输出与真实执行失败分开处理；未知实验真值不当作错误标签。

二值预测加对错反馈可以在题后恢复性质标签，因此不能声称真值完全不可获得。沿用 Task42 的分析，加入同已获标签的直接性质预测器作为辅助对照，检查选择工具本身的价值。

先固定两类流：随机顺序检验持续学习；按预先定义的骨架或性质组成变化检验受控分布变化。后者不称真实时间漂移；数据顺序不得按哪个方法赢来挑选。各方法共享初始化数据、流顺序、反馈延迟、标签数量和调参预算。

第一轮以已有真实缓存开展开发验证，至少含 Best Fixed、Frozen JEV、Online/Replay SGD、候选更新和已适配的最近邻。开跑前固定唯一主假设、比较方法、样本与种子、预算和失败处置，保存逐样本原始日志。

**验收**：同时报告累计错误、分段准确率、适应所需反馈数、选择及更新的 p50/p95 延迟和峰值内存。实时性包括编码与历史维护，不能只报小头更新耗时。检验目标可以是错误更少，或在预先规定的准确率容许损失内明显更快、更省内存。最小有意义差异与延迟目标在开跑前按任务和资源定值。

统计按分子/骨架处理；在线轨迹还需处理前后更新依赖，不能把输出格、seed 或事后打乱的日志行当独立样本。若差异很小且区间宽，结论是证据不足；若 Frozen、SGD 或最近邻已解释全部收益，回到方法假设，不扩大训练碰运气。

## 5. 有支持信号后，进入多端点和最终确认

在开发阶段修订并冻结方法、backbone、baseline、预算和主要指标，再执行 BBB/hERG/AMES 多端点评价。最终确认集的来源与隔离规则提前确定，确认结果在冻结后打开一次；失败结果照常记录，不把确认集变回调参集。

需要远端计算时沿用已跑通的 Nebula 入口，直连、一次只建一条任务、完整保存日志。当前计划不需要马上占用 GPU。确认集或端点资格不足时，明确当前可支持的结论范围和具体补数据成本，再决定后续投入。

最近一轮交付是数据资格表、baseline 适配表、实时更新方法卡。这三件可以并行准备；它们共同决定第一轮实验究竟测什么。

### 2026-09-27 post-call verifier 闸门

调用后 confidence 的 fit-only 固定阈值回放通过 CPU 闸门：五折 pooled accepted accuracy `0.961606`，随机拒答 `0.915078`，差值 `+0.046528`，五折全部更高。该结果只支持 verification/refusal 扩展，不能回填 direct pre-call selection，也不能当作动态 JEV 效果。

下一步只做一条 CPU 候选：在同一 post-call confidence 状态上加入 selected-only delayed feedback 的在线校准/风险状态，比较 Frozen verifier、普通 selected-only 更新和随机拒答；先固定信息权限、延迟、阈值和失败处理，做独立回放。CPU 闸门不过就停止，不申请 A800；通过后才做一条端到端 A800 smoke。confirmation 继续关闭。

### 2026-09-28 confidence escalation 开发结果

在相同的 fit-only 0.8 confidence 阈值下，低置信度时追加第二 provider 的 pooled accuracy 为 `0.923823`，Best Fixed 为 `0.915512`，平均调用次数 `1.206`；相同 fallback 数量的随机追加为 `0.904432`。这把验证动作和调用成本放进了同一个流程，4/5 folds 不下降。

下一步仍然先做 CPU：让状态维护第二 provider 的近期可靠性和 confidence threshold，反馈按 selected-only 延迟释放；静态 escalation、随机 escalation、普通 selected-only 更新和 dynamic candidate 必须共用调用预算。动态版本只有在相同平均调用次数下稳定超过静态 escalation，才考虑 A800。

方法搜索边界已固定：不再为 direct pre-call selection 继续堆新的普通在线 head。最多执行一次 DG-SafeJEV 安全切换控制；若它不过预登记闸门，direct 动态主线停在负结果。后续资源优先投入调用后 verifier 的动态风险状态和论文中的 benchmark/baseline/证据整理。

## 已有资产的复用

- [Task24](../task24-fixed类口径分层与hERG真值接入/report.md)：复用真值候选及来源线索，重核当前端点与分子交集，不沿用旧题型要求。
- [Task36](../task36-pharma-provider-replay/report.json)、[Task42](../task42-p02-evaluation-contract/protocol_proposal.md)：复用真实缓存回放、判分、反馈隔离及可恢复标签分析，不重建环境。
- [Task45](../task45-dynamic-decision-model/implementation_card.md)、[Task46](../task46-open-source-composition/report.md)：复用已拉取源码、特征接口审查和组件清单；候选配置不当作最终最优方法。
- [Task49 来源表](sources.jsonl)：沿用已查明的论文、仓库和版本线索，优先补同任务适配与实现核对。

### 2026-09-28 延迟动态 verifier CPU 闸门

动态 verifier v1 已按预注册完成 CPU 回放和独立恢复。第一/第二 provider 分开记账，失败没有 numeric confidence 时强制追加第二工具，反馈 delay 0/1/4 按 release tick 更新。动态 pooled final accuracy 三个 delay 都为 `0.900277`，静态 q=.8 escalation 为 `0.923823`；平均调用 `1.490305`，超过上限 `1.25`，风险门没有一次放宽阈值。结果、独立恢复和逐字节重跑在 `task/task49-pharma-benchmark-baseline-freeze/dynamic_verifier_v1/` 与 `experiments/logs/task49_dynamic_verifier_20260928/run_v2/`，ADR-0046 已记录。

该候选因准确率和成本双失败停止，不调风险参数，不申请 A800。它作为动态状态的负证据保留；主线继续写静态 post-call verifier/escalation，direct selected-only 动态 JEV 不再堆普通 head。只有出现新的独立可观测反馈，才重新打开动态状态设计。

### 2026-09-29 development baseline 主表更新

Greedy LinUCB 已补齐 v5 provenance、BT/CLS hidden-64 common representation、P-C1 authoritative manifest 和独立状态哈希恢复，并完成 5 folds × 2 deterministic slots × 3 delays 的 60 条 CPU development 流。online accuracy 为 delay 0/1/4=`0.8511/0.8449/0.8629`，自身 Frozen=`0.8566`，fit-only Best Fixed=`0.9155`；没有通过预登记 go-signal，保留为现代在线负 baseline。

已有 development 证据已收成 `task/task49-pharma-benchmark-baseline-freeze/development_master_table_v1/report.md`。跨方法的稳定事实是：Best Fixed 明显强于所有当前动态头；Matched Online-SGD 相对自身 Frozen 有小幅正收益，但仍低于 Best Fixed；PILOT、BayesianRouter、Greedy、provider-transfer、context memory 和 delayed dynamic verifier 都没有形成可重复的主方法优势。

因此当前不再继续调普通 selector、provider graph、memory 或风险阈值，也不提交新的 A800。下一步工作改为两件事：

1. 用 development master table 更新三份验收文档，明确当前 Gate B/C 仍 HOLD 的具体证据缺口；
2. 保留 direct pre-call 负结果，补齐静态 post-call verifier/escalation 的多端点资格、动态事件边界和论文证据链；只有出现新的、预先定义的可观测反馈来源，才重新提出动态方法候选。
