# Matched effect matrix v3：验收审查

日期：2026-10-04  
范围：五折、seed 0、`ProbeMIRA-Relay` / `ProbeIndependentResidual` / `ProbeNoUpdate`，BBB development replay。  
证据：`experiments/logs/task49_mira_matched_effect_matrix_20261004/v3_fold{0,1,2,3,4}_seed0*/summary.json`、各目录 raw JSONL 与 verifier receipt；对应合同为 `matched_effect_matrix_v3_preregistration.md`。  
结论性质：这是验收审查，不是论文效果结论，也不打开 confirmation。

## 1. 先给结论

这批结果已经证明了三件事：

1. **事件合同基本能运行。** 五折都有 9 条 stream，延迟为 0/1/4，selected-only feedback 和 pending queue 都能完成；fold0、fold1、fold2、fold3 的加强 verifier 已通过。fold4 的 summary 已生成，但 `verification_v2.json` 仍拒绝，原因是 `probe_independent_residual` 的 `provider_local_update_events` summary metric 与 raw 重算不一致。因此五折不能写成“全部验证通过”。
2. **MIRA 的 family transfer 确实发生，但暂时没有形成稳定收益。** 每条 stream 的 family-transfer events 大约为 14–25；MIRA 相对 Independent 的 action difference 只有每折 0–5 个，paired 正确数差异只有 fold0/1 在 delay 0/1 各多 1 条，其余为 0。这个信号太小，不能证明 family sharing 的效果。
3. **最有信息量的下一步不是立即重复 seed，而是用已有 raw 做 transfer 可识别性诊断。** 当前应先判断：family 更新是否有同族可用 provider、是否真的改变同查询的排序/动作、动作边际是否太大。若大多数更新没有足够的 sibling exposure 或没有跨过决策 margin，重复 seed 只会重复一个不可识别的设置。

## 2. 当前结果的紧凑摘要

以下只列 `n_correct/n` 和 paired 结果；delay 是同一 fold/seed 的反馈时钟条件，不是独立样本。

| fold | n | delay | MIRA | Independent | NoUpdate | MIRA−Independent (rescued/harmed/net) | MIRA−NoUpdate (rescued/harmed/net) |
|---:|---:|---:|---:|---:|---:|---|---|
| 0 | 145 | 0 | 127/145 | 126/145 | 126/145 | 1/0/+1 | 2/1/+1 |
| 0 | 145 | 1 | 127/145 | 126/145 | 126/145 | 1/0/+1 | 2/1/+1 |
| 0 | 145 | 4 | 126/145 | 126/145 | 126/145 | 0/0/0 | 2/2/0 |
| 1 | 145 | 0 | 128/145 | 127/145 | 127/145 | 1/0/+1 | 1/0/+1 |
| 1 | 145 | 1 | 128/145 | 127/145 | 127/145 | 1/0/+1 | 1/0/+1 |
| 1 | 145 | 4 | 128/145 | 127/145 | 127/145 | 1/0/+1 | 1/0/+1 |
| 2 | 144 | 0 | 130/144 | 130/144 | 129/144 | 0/0/0 | 2/1/+1 |
| 2 | 144 | 1 | 130/144 | 130/144 | 129/144 | 0/0/0 | 2/1/+1 |
| 2 | 144 | 4 | 130/144 | 130/144 | 129/144 | 0/0/0 | 2/1/+1 |
| 3 | 144 | 0/1/4 | 131/144 | 131/144 | 129/144 | 0/0/0 | 4/2/+2 |
| 4 | 144 | 0/1/4 | 128/144 | 128/144 | 128/144 | 0/0/0 | 1/1/0 |

fold4 三行来自已生成 summary；在 verifier 修复并重新通过前，只能作为 provisional raw 结果。

按 summary 暂时计算，MIRA 相对 Independent 在 delay 0/1 的跨折净差为 `+2/722`（约 `+0.28 pp`），delay 4 为 `+1/722`（约 `+0.14 pp`，来自 fold1）；fold4 尚未验收，且这些数没有 paired CI。delay 0/1 的 MIRA 相对 NoUpdate 净差为 `+5/722`（约 `+0.69 pp`），delay 4 为 `+4/722`（约 `+0.55 pp`，同样只是开发汇总）。这些数字只能用来确定下一步，不能写进论文主结果。

## 3. 对三份生效验收标准的对应

### 3.1 Benchmark + Baseline 标准

**已经达到或接近的条目**

- **B5 / H1：任务合同。** 三个方法在同一 33-provider menu、相同 frozen feature、selected-only feedback、`release=t+1+D` 和同一 failure 语义下运行；fold0–3 verifier 能从 raw event 重建 selected provider、terminal/feedback、pending/release 和 state hash。
- **B6 / H2：信息隔离。** replay 不是 full-panel 选择；当前事件只暴露 molecule feature、menu 和已经 release 的 selected feedback。该证据仍是 development scope，不等于 confirmation。
- **B7：feedback-adaptive dynamic scope。** `D∈{0,1,4}`、pending queue 和 release 后 state update 已出现。它证明的是 feedback-adaptive，不是 environment-nonstationary；没有 arrival/retire/version shift，不能写成环境动态泛化。
- **B8：matched fairness。** MIRA、Independent、NoUpdate 共享输入、初始化、probe seed、menu、hazard 和预算；Independent 的 family fit state 是按 provider 复制，保证首个 decision 同初始化。对照契约清楚。
- **B11：family 相关性。** 结果按 family-transfer 和 provider-local update 分开记录，没有把 33 个 provider 当成 33 个独立动态实验。
- **B15：raw/recovery。** fold0–3 有 raw JSONL、config、manifest、source/input/checkpoint/state hashes 和 verifier。fold4 目前是恢复链失败，B15 不能给满分。

**目前不满足或只能给开发级分数的条目**

- **B13 / Gate C4：统计。** 现在只有 seed0；三个 delay 复用同一 fold/seed 的顺序和 PRNG，不能当 15 个独立重复。尚未给按 molecule/scaffold 的 paired bootstrap、fold-level 汇总区间或明确的 seed/outer-fold 不确定性。
- **B14 / Gate C1、C3。** E2 主比较和机制读出已跑，E4 只有 feedback delay/failure scope；尚没有把结果映射成完整 RQ 主表，也不能把小幅 development delta 解释成稳定效果。
- **R3：headroom 与主比较。** MIRA 相对 Independent 的正差只在少数 fold/delay 出现，且差异在 `0–1/145`；还没有预登记最小效果和区间的通过证据。相对 NoUpdate 的 5 条净差也没有说明是 family sharing、普通 residual update 还是 probe 直接命中。
- **R4/R6：不确定性和负结果。** 负结果已保留，但 fold4 verifier failure 尚未闭合；区间跨 0 与否尚未计算。当前结论必须是 HOLD。

因此，Benchmark+Baseline 的 Gate C 仍为 **HOLD**。这不是 benchmark 选型失败，而是结果与恢复/统计门尚未闭合。

### 3.2 方法论标准

- **H1–H4：基本通过（development）。** 输入、动作、反馈、延迟、state 和 terminal 可执行；pre-call 看不到未选输出；MIRA/Independent 的残差更新合同不同且可观察。
- **H5 / M5：尚未通过。** family residual 是可实现的机制差异，但 raw 结果还没有证明它相对 provider-local residual 的必要性；必须用同查询 transfer/margin 分析或匹配消融定位。
- **H7 / M7：fold4 阻塞。** 三种方法的状态和 event 大多能复核，但一个 fold 的 independent update receipt 不一致，不能把整套矩阵称为完全可恢复。
- **H9 / M10：只满足 feedback-adaptive。** 当前固定 provider pool 没有 environment-nonstationary 证据；若论文写 dynamic，必须限定为 delayed outcome adaptation。
- **M11、M13、M14：部分满足。** 延迟矩阵和 matched controls 齐全，机制负结果已保留；但跨折 CI、fold4 修复和独立复现闭环未完成。
- **M15：必须收窄当前证据。** 目前可写“在冻结 BBB provider pool 上观察到可重放的在线状态更新”；不能写“family sharing 已提高实时工具选择”，更不能外推到其他 endpoint 或自然漂移。

### 3.3 故事线与创新标准

- **H5/H8：当前证据只支持机制存在，不支持贡献成立。** 方法能改变 state，不能自动等于任务收益；MIRA–Independent 的 integration gain 仍未被证明。
- **S3/S4/S6：应暂按 2 分处理。** 失败→根因→机制链条是清楚的，但“family sharing 带来任务增益”的独立 evidence 和 falsification result 尚未闭合。
- 论文中可以保留 MIRA-Relay 作为候选方法和目标 claim，但结果段必须把 v3 标成 development gate；不要删掉零差异折或把 `+1` 事件写成普遍提升。

## 4. 当前数据真正能支持的陈述

可以支持：

1. 在固定 BBB provider menu、frozen BT+CLS representation 和 selected-only delayed feedback 合同下，三种策略可在 CPU 上完成逐事件 replay；
2. MIRA-Relay 的 family residual 更新和 Independent 的 provider-local 更新都确实发生，且输出中能分开记录 family-transfer 与 provider-local update；
3. 在 fold0–3 已通过 verifier 的 development receipts 中，MIRA 相对 Independent 的 paired 动作/收益差异很小，跨折方向不够稳定；
4. NoUpdate 对照比 Independent 更容易出现动作变化，但 net success 增益仍很小，这说明“更新发生”与“任务收益”是两个不同问题。

不能支持：

- family sharing 已经优于 provider-local residual；
- MIRA-Relay 已经满足预登记的 `+0.5 pp` 且 CI 下界大于 0；
- 延迟越小越好、方法对自然 provider drift 有效、方法可迁移到 hERG/AMES；
- fold4 已通过恢复验证；
- 任何跨 seed、跨 endpoint 或 confirmation 结论。

## 5. 最小高信息量分析：先诊断共享为什么没有转化为收益

这项分析只读现有 v3 raw，不改代码合同、不重跑 replay、不新增 seed。

### 5.1 分析单位

不要把 event 行当独立样本。主表单位是 fold×delay 的 paired molecule；诊断单位是一次 selected feedback，但结果按 fold、delay 和 scaffold 聚合。delay 只是同一事件序列的时钟条件。

### 5.2 每个 feedback 计算四个量

对 MIRA 的每条 `residual_updated=true` feedback：

1. **transfer opportunity**：selected provider 所在 family 在当时 `candidate_menu` 和 `available` 中是否还有未选 sibling；
2. **same-query score movement**：使用 frozen feature、fit-state 和 feedback `state_before/state_after`，重算该条 query 在 release 前后的 sibling score/rank；
3. **action margin**：release 前 exploitation 第一名和第二名的 score gap，以及 sibling 距第一名的 gap；
4. **paired consequence**：同一 query 的 MIRA 与 Independent 是否改变 provider action，若改变，记录 rescued/harmed、family action change 和是否来自 sibling。

现有 summary 已提供 transfer/update 的粗粒度计数：MIRA 每条 stream 大约 14–25 个 family-transfer event，但 MIRA–Independent 实际 action difference 每折只有 0–5 条；这已经提示大量更新没有跨过 decision margin。上述四量能把这个推测变成可证伪分析。

### 5.3 预先写死的解释分支

- **A：opportunity 少。** 大多数 selected family 是 singleton，或 sibling 不在可行 menu；则当前 benchmark 不能识别 family sharing，下一步应先改 provider-family exposure 的任务切片，不能重复 seed 追分。
- **B：opportunity 多但 score movement 小。** family update 数值存在，但 representation/variance/局部更新不足以改变排序；应分析 margin 与 update scale，不能宣称共享无效。
- **C：score/rank 改变但 action 不变。** 机制在计算上生效，当前 selector 的 headroom 不足；应报告“机制信号、任务收益不足”，而不是继续堆 seed。
- **D：action 改变但 rescued/harmed 近似抵消。** 共享更新有行为代价，当前 utility 或 hazard 约束没有转成可靠收益；需要错误类型与 provider lineage 分层。
- **E：MIRA–Independent 的 action 差异不是 sibling transfer 导致。** 则 family-sharing 主张被当前证据反驳，应优先检查 runner 的 action attribution 和 state-to-score 重算。

**最小证伪条件：**在有 sibling 且 score/rank 真的改变的 query 上，MIRA–Independent 仍没有可重复的 action/utility 差异；或者 action 改变的 rescued 与 harmed 在 fold-level 分层后跨零且方向不稳定。两者都意味着当前 family sharing 不能作为主要贡献证据。

## 6. 下一步顺序

1. **先修 fold4 verifier 的 `provider_local_update_events` 统计口径并只读重验。** 不重跑 replay，不覆盖旧 receipt，保留 rejected receipt 和修正版验证记录。
2. **用现有 raw 完成第 5 节四量诊断。** 输出每 fold×delay 的 opportunity、score movement、margin、action consequence；这一步的信息价值高于直接增加 seed。
3. **若诊断显示有充分 transfer opportunity、足够 margin headroom 且动作差异方向稳定，再增加 seed1。** seed1 的目的应是估计不确定性，不是寻找正结果；仍按 scaffold/molecule paired 聚类。
4. **若 opportunity 少或 score 改变不能跨 margin，保留开发负结果，修改方法/任务切片后重新预登记。** 不修改当前验收标准，不打开 A800 confirmation。

在 fold4 修复和第 5 节诊断完成前，Gate C、方法 M5/M13/M15 与故事 S4/S6 都保持 HOLD。

## 2026-10-04 更正：fold4 核验闭合与五折汇总

上一节是在 fold4 的旧 verifier 拒绝时写的中间状态，不能继续作为当前结论。修正后的 verifier 使用 runner 已预先固定的 `1e-12` 状态变化阈值重算 provider/family transfer，同时对未选 provider 保留精确不变性检查；它对同一 fold4 raw JSONL 通过，未重跑回放。旧拒绝收据仍保留在 fold4 目录。

当前五折状态是 5/5 verified，9 streams/fold，合计 722 evaluation molecules 和 19,494 raw events。五折汇总见 `matched_effect_matrix_v3_crossfold_aggregate_v1.md/json`：

- MIRA vs Independent 的 fold net 在 delay0/1 为 `[1,1,0,0,0]`，delay4 为 `[0,1,0,0,0]`；fold-level t4 区间都跨 0。
- MIRA vs NoUpdate 的 fold net 在 delay0/1 为 `[1,1,1,2,0]`，delay4 为 `[0,1,1,2,0]`；delay4 的 fold-level区间也跨 0。
- scaffold-cluster bootstrap 只给出很小的描述性区间，不能替代 confirmation 或多 seed；delay0/1/4 不是独立重复。

因此 Gate C 仍然 **HOLD**，原因从“fold4 receipt未闭合”更新为“跨折 paired signal 小且不稳定，主张和机制必要性尚未被证明”。这不是 benchmark 选型失败，也不改变目标 claim；它改变的是下一步的实验顺序。

## 2026-10-04 transfer diagnostic v1

按 `matched_effect_matrix_v3_transfer_diagnostic_card.md` 复用五折 raw，得到 1,369 条 MIRA update records。sibling opportunity 在各 fold/delay 约为 0.117–0.338，但 MIRA 与 Independent 的 action difference 只有每 stream 0–4 条；selected family 的 residual 变化大多可检测，却很少跨过 action margin。当前最可能的可检验解释是：family sharing 在 fixed menu 中的可用 sibling 暴露不足，且 score margin 通常较大。

这一步没有新增 replay、没有调参、没有修改主指标。下一步先做一个小的、预登记的机制修复/敏感性分析：固定同一 raw cohort，只改变能否把 family update 传到 sibling 的计算尺度或 benchmark slice；如果不能增加 action-level consequence，就不申请 seed1/A800。

### 对标准的更新

- Benchmark + Baseline：B5–B8、B11、B15 的合同和恢复证据已达到 development 闭合；B13/B14 以及 Gate C3/C4 仍未达到投稿线。
- 方法论：事件级 online 合同和 selected-only 边界已通过；M5（family sharing 的必要性）、M13/M15（统计和可迁移主张）仍 HOLD。
- 故事线：S4/S6 仍按 2 分处理；可以保留 MIRA 作为目标方法，但不能把 family transfer 写成已证实收益。

## 2026-10-04 HRA fold0 机制敏感性更新

为检验“family-only 更新没有跨过 provider action margin”的解释，按预登记运行了 hierarchical residual candidate：在 family residual 外增加 selected-provider local residual，其他输入、菜单、反馈时钟和 probe 合同不变。独立 verifier 对 3 个 delay、145 个 evaluation molecules、1,305 条 raw events 通过；证据见 `hierarchical_residual_sensitivity_v1_report.md` 及 `experiments/logs/task49_mira_hierarchical_sensitivity_20261004/fold0_seed0/`。

fold0 配对结果：相对 MIRA，delay 0/1 各改变 3 个 action 但 `0/0` rescued/harmed，delay 4 改变 4 个 action，`0/1`，净 -1；相对 provider-independent residual，delay 0/1 为 `1/0`，delay 4 为 `0/1`。这说明 local residual 确实改变了动作，但没有形成稳定收益，不能关闭 M5、M13、M15 或故事 S4/S6。


### HRA 停止规则更正

上一段的“可继续 folds1–4”判断被更正。预登记的停止条件是析取条件：相对两个 matched controls 均为 `0 rescued/0 harmed`，**或**收益只来自 single molecule。HRA 相对 MIRA 没有 rescued 且 delay4 harmed 1 条；相对 Independent 的唯一 rescued 是同一 `z070852`（delay0/1 重复），因此触发停止。HRA 通过实现核验但不进入 folds1–4，也不申请 seed1/A800。Gate C、M5、M13、M15、S4、S6 继续 HOLD。另将 family update 与 non-singleton sibling transfer 分开统计；fold0 后者为 20/20/18（delay0/1/4）。

分层诊断进一步更正归因：MIRA 的 1,369 次 update 中 1,095 次（80.0%）来自 singleton family，只有 274 次有 sibling opportunity；HRA 相对 MIRA 的 changed actions 在 delay 0/1 全部是 `admet_ai` size-2，delay 4 则包含 1 个 `admetlab` singleton、1 个 `admet_ai` 和 2 个 `b3clf`。因此 delay4 的动作变化不能全部写成 family transfer。详见 `margin_family_diagnostic_v1.md/json`。

## 2026-10-04 Provider-Graph Relay 先验审查结论

为解决 singleton family 的覆盖瓶颈，已提出 Provider-Graph Relay 候选，但在实现前完成了一轮一手 prior-art 核验，记录见 `research/provider_graph_relay_prior_art_20261004.md`。feedback-graph bandits、GOB.Lin/CoLin、correlated-arm bandits、label propagation 和 online algorithm portfolios 已覆盖图反馈、相关臂迁移、标签传播和动态算法选择的通用部分。因此，“构造图并向邻居传播更新”不能单独算作创新。

候选暂不改变当前主方法和目标 claim。只有在 fit-derived provider transfer graph、selected-only delayed feedback、未选结果不可见、degree-matched graph controls、同更新预算以及跨分子/跨 delay 的 paired action consequence 全部成立时，才进入 CPU gate；否则降为 baseline/control。当前不申请 seed1、A800 或 confirmation。

## 2026-10-04 Provider-Graph Relay 零机时合同检查

候选卡的第一轮零机时检查已完成，收据见 `experiments/logs/task49_provider_graph_relay_zero_cost_20261004/`。五个 fold 的 fit index 与 sealed fit state 对齐；独立 NumPy reference 与 graph builder 的 overlap、相似度和邻居一致；real/random/identity graph 在更新前不改变冷启动 logits；合成更新只写 selected 与可写邻居，fallback exact invariant，4 个单测通过。

这只关闭了实现前的结构阻塞。图上静态 provider coverage 为 1.000，但它不是 selected replay opportunity，也不是效果证据。数学审查已确认无条件 correctness Pearson 不足以保证 recipient residual alignment，候选目前只能按迁移启发式表述，不能按 Bayesian/PSD posterior 表述。Gate C、M5、M13、M15、S4、S6 仍 HOLD；下一步先预登记 held-out alignment diagnostic，再决定是否进入 fold0 CPU replay。

## 2026-10-04 Provider-Graph Relay held-out alignment 闸门

在不改策略、不重放的前提下，按 `provider_graph_alignment_v1_preregistration.md` 完成了 validation residual-alignment 诊断。五个 fold 的 graph mean $\lVert x\rVert^2\delta_p\delta_q$ 均为正，且均高于保留 source out-degree 与 target in-degree 多重集的随机图；完整收据在 `experiments/logs/task49_provider_graph_alignment_20261004/`。

这只说明 fit-only graph 有值得继续验证的 transfer signal，达到 fold0 replay 资源闸门；不能关闭 Gate C、M5、M13、M15 或 S4/S6。下一步仍须在统一 delayed selected-only replay 中观察 paired action/rescued/harmed，并保留随机图和 MIRA/Independent 对照。

### alignment control 收据更正

原 `provider_graph_alignment_20261004` 收据发现随机 control 生成器的 index/ID 混用，某 source 出现重复 target，因此已降为无效工程现场；没有把它当效果证据。修复 commit `70e5455b` 后，authoritative 结果位于 `experiments/logs/task49_provider_graph_alignment_20261004/corrected_v2/`，随机 control 保留 source writable out-degree 与 target in-degree 多重集，5/5 fold 仍通过预登记资源闸门。该更正不改变 Gate C、M5、M13、M15、S4、S6 的 HOLD 状态。

## 2026-10-04 Provider-Graph Relay fold0 gate（后续候选，不改变 v3 主矩阵）

为解决 singleton family 的覆盖问题，按 `provider_graph_relay_fold0_v1_preregistration.md` 运行了一次 fold0/seed0 CPU gate。该候选的 12 streams、5,220 events 已通过独立 `verification_v2.json`；事件哈希和完整收据见 `experiments/logs/task49_provider_graph_relay_fold0_20261004/receipt_v3/`，人读报告见 `provider_graph_relay_fold0_v1_report.md`。

结果按 molecule 配对报告：Graph Relay 相对 MIRA 的 rescued/harmed/net 为 delay0 `1/3/-2`、delay1 `1/3/-2`、delay4 `4/4/0`；相对 degree-matched random graph 为 `0/1/-1`、`0/1/-1`、`1/2/-1`。动作变化是真实的，但没有一个 delay 取得正净收益，并触发预注册停止条件。因此不扩展 fold1--4、seed1、A800 或 confirmation，不把图 relay写成主方法效果。

这条负机制结果补充了 v3 的根因判断：静态 fit-only correctness graph 能提高关系覆盖和动作变化，却不能保证 delayed release 时 recipient residual 与 selected residual 同向，也不能保证变化跨过决策 margin 后带来正确选择。Gate C、M5、M13、M15、S4、S6 继续 HOLD；主线回到统一 E1--E5 矩阵的合同、公平、统计和跨 endpoint 缺口。
