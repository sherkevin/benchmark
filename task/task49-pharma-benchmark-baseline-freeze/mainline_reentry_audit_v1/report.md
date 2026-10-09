# 主线回归审查 v1

日期：2026-09-29  
状态：主线工作记录；不修改生效 benchmark、baseline 或方法决议。  
目的：在暂停题目和摘要打磨后，重新检查当前工作是否仍围绕 AAMAS 主线推进。

## 1. 当前主线只回答四个问题

1. 公共 truth 和 provider routing protocol 是否足够独立、可复核，能构成合格 benchmark。
2. baseline 是否覆盖强固定策略、现代在线方法、匹配控制和必要的上界，并且在同一信息合同下运行。
3. proposed method 是否真的有可执行机制，而不是把普通在线更新换一个名字。
4. 实验结果是否能区分静态选择、selected-only 延迟适应、受控版本变化和调用后验证。

题目和摘要只在这四个问题有结果后更新 headline；当前不把文字工作当作主线进展。

## 2. Gate 快照

| 闸门 | 当前分数 | 状态 | 主要缺口 |
|---|---:|---|---|
| Gate A：Benchmark 选型 | 11/16 | HOLD | BBB canonical conflict policy 尚未共同确认；confirmation 与 provider lineage 仍未完全闭合 |
| Gate B：Baseline 选型 | 9/16 | HOLD | Greedy LinUCB 尚未正式效果回放；GraphRouter/Laya-online 仍是 fidelity 阻塞；完整主表未闭合 |
| Gate C：实验矩阵与结果 | 10/16 | HOLD | 当前主要是开发诊断、负结果和受控正控；尚无冻结方法后的完整 E1--E5 主表 |

三门不能互相抵消。Gate A/B/C 全部达到内部阈值前，不打开 confirmation，也不提交新的效果型 A800 任务。

## 3. Benchmark 的当前真实状态

主 benchmark 已收敛为“公共 TDC truth + Task36 provider routing protocol”。这个收敛是有效的 scope 决策，但不等于 benchmark 已通过验收。

- BBB 有 10 个 canonical conflict groups（train/validation 8，test 2），现有公开文件不能说明哪一行具有更高 assay 权威性。
- 旧 722 条 development panel 与官方 test 有 canonical/scaffold overlap；旧 development 结果不能直接充当 untouched confirmation。
- 已生成 157 条 scaffold-disjoint confirmation 候选，但它只是候选清单，尚未打开 confirmation。
- provider lineage 已分为 G1/G2/G3，当前可支持 lineage-aware operational routing；不能外推为 model-fresh 泛化。

目前唯一可直接执行的冲突政策候选是 P-C1：排除冲突组、保留原始收据、报告清洗前后分母，并把 hERG/AMES 单独审计。P-C1 尚未写入生效文档，不能在代码中静默采用。

## 4. Baseline 的当前真实状态

统一合同已经有真实缓存 smoke：15 streams、960 decision/terminal/feedback，v5 逐事件字段和独立 verifier 通过。这证明接口和信息边界能跑通，但不产生效果排名。

已有正式开发证据：

- Best Fixed、Frozen、Online-SGD、Replay-SGD：有统一开发回放或静态诊断。
- BayesianRouter-adapted：5 folds × 2 seeds × 3 delays 的 selected-only BBB replay 已完成，三个 delay 均未超过 Frozen，保留为正式负结果。
- PILOT-adapted：有 fit-only prior 和适配 smoke，完整统一效果仍未闭合。

尚未进入正式主表：

- Greedy LinUCB：runner 和 verifier 已有，但需在冲突政策、warm-start、seed 和 lineage 合同冻结后跑一次 CPU development replay。当前候选使用 Morgan-1024，而已有 Bayesian/PILOT/Matched-SGD 主证据使用 fit-only BT+CLS hidden-64；若不另设 common-64 版本或明确分层，只能作为表示分层适配，不能与 hidden-64 方法直接做算法排名。seed 0/1 是确定性复制槽位，不是独立随机重复，不能生成 seed 置信区间。输入 manifest 还需显式绑定 conflict receipt 和 source/input 生成收据。更直接的阻塞是：现 runner 的逐事件日志还没有 baseline contract v5 要求的 `method_id/run_id/episode_id/released_feedback_ids/decision_reads/provider_event_ids/state_hash_before/after/update_ms` 等 provenance 字段；在补齐 v5 schema 或定义经过独立审查的兼容合同前，不能把它作为 Gate B 正式 raw run。
- 另外，当前 C6 预登记是 fit-prior 适配，不是原论文的 zero-prior 版本。若主表要声称原始 Greedy LinUCB，必须另有 canonical zero-prior 对照；若只运行 fit-prior，名称必须明确写成 `Greedy LinUCB (fit-prior adaptation)`，不能让一个初始化合同覆盖两种方法。
- 还有一个执行层约束：Morgan-1024 的 disjoint LinUCB 为 33 个 arm 保存约 33×1024² 的协方差状态，60 streams 的 replay 可能达到约 $3\times10^{11}$ 级矩阵运算，且 online/frozen 状态约占 0.5 GB 以上。若保留 1024-D，必须先做小规模 smoke 和 p50/p95 测量；common hidden-64 不仅有公平性价值，也能把计算成本降到可测范围。
- GraphRouter：缺 provider description/edge contract 与 `torch_geometric`，不能直接重写后称为同一 baseline。
- Laya/JEV online：Laya runtime 没有 optimizer、loss 或在线写回接口；只能把冻结表示外接 head/adapter 作为明确的新适配定义。

## 5. 摘要承诺与可执行方法的对应关系

当前摘要承诺了三个机制：

| 摘要承诺 | 当前可执行证据 | 结论 |
|---|---|---|
| calibrated frozen selector 作为 cold-start prior | Best Fixed/Frozen 已有；概率校准与最终 prior 接口仍未在生效方法卡中冻结 | 目标明确，尚不能写成已验证方法 |
| 只在 selected feedback release 后更新 | online adapter contract、pending queue、delay `{0,1,4}` 和独立事件 verifier 已通过 | 信息时序已可执行；性能增益尚未成立 |
| uncertainty gate 通过后才切换 | DG-SafeJEV 只有候选方法卡；现有 confidence gate 是 post-call verifier，不能直接替代 pre-call selector gate | 必须先分清两个 gate，再决定是否进入主方法 |

因此，摘要现在可以作为投稿候选，但不能被当成方法已冻结的证据。若最终 CPU 主方法没有通过预登记闸门，摘要中的“可靠适应”必须收窄到实际验证的 scope，不能靠文字保留未验证承诺。

## 6. 下一步顺序

### 必须先共同确认的事项

P-C1 是否作为主 BBB benchmark 的生效 conflict policy。确认内容只包括：

1. 冲突 canonical group 整体排除，不修标签；
2. 主结果使用排除后的 molecule-level 分母，同时报告原始分母和排除收据；
3. 157 条 scaffold-disjoint 候选只在方法、菜单、epoch、阈值和 baseline 冻结后打开；
4. hERG/AMES 不能未经各自审计直接套用 BBB 规则。

### 确认后立即执行

1. 生成带 conflict receipt 的 authoritative BBB manifest。
2. 补齐 feature strata 合同：优先增加与 hidden-64 主证据同表示的 common-64 Greedy；Morgan-1024 只能单独报告，不能混排名。明确 fit-only full-panel 权限、P=I 适配差异、确定性 seed 槽位和 input generation receipt。
3. 在同一 manifest、同一 fit-only warm-start、同一 seed 槽位下运行一次 Greedy LinUCB CPU development replay。
4. 依据 Greedy、PILOT、BayesianRouter、matched SGD 和 Frozen 的同表结果，决定是否需要 DG-SafeJEV 这一最后的 pre-call 安全切换诊断。
5. 如果直接选择仍没有超过 Best Fixed，停止堆叠 selector 变体，把主方法候选转为调用后动态 verifier/refusal，并单独报告 direct-selection 的负结果。
6. 只有 Gate A/B/C 全部通过并且方法卡可独立恢复，才提交一条 A800 任务；不为探索性候选并行开多任务。

## 7. 主线停止条件

- 不把 contract smoke 当效果结果。
- 不把 controlled version event 正控当自然环境漂移证据。
- 不把调用后 confidence/verifier 的收益写成调用前 selector 的收益。
- 不把普通 ridge、Bayesian posterior、cosine memory 或安全回退规则单独命名为新算法，除非同一任务合同下有独立机制收益和可证伪证据。
- 任何新的方法变体必须先写 claim、最近邻、预算、信息权限和 falsification condition；没有这些内容就不跑实验。

## 8. 2026-09-29 回归更新

小方向不再停在等待确认：P-C1 已按 owner 授权落为生效决议 `docs/决议/0005-BBB冲突样本采用P-C1整体排除.md`，并生成 authoritative manifest。Greedy LinUCB 也已在同一 P-C1 manifest、BT/CLS hidden-64、fit-only warm start 和统一延迟队列上完成 60 条 development 流，独立 verifier 通过。

Greedy online 在 delay 0/1/4 为 `0.8511/0.8449/0.8629`，Frozen 为 `0.8566`，Best Fixed 为 `0.9155`；因此它保留为现代在线负结果，不进入方法候选。第一次全量回放暴露的最终 drain 状态哈希问题已修复并保留失败证据。当前 Gate A/B/C 仍不能宣称通过：冲突政策已落地，但 provider lineage、动态事件、GraphRouter/Laya-online fidelity、统一主表和 confirmation 仍未闭合。

下一步不再继续调 Greedy；先把已有 matched SGD、PILOT、BayesianRouter、Greedy、Frozen 和 Best Fixed 的结果收成一张可审计 development 主表，再根据共同失败模式决定是否只保留一个带校准先验与安全切换的动态候选。没有跨近邻的可重复优势，不申请新的 A800。

## 9. 证据索引

- 生效 benchmark/baseline：`docs/研究主线/生效/03-Benchmark与Baseline.md`
- 生效方法：`docs/研究主线/生效/02-方法论.md`
- 验收闸门：`docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md`
- 冲突政策候选：`task/task49-pharma-benchmark-baseline-freeze/gate_a_conflict_policy_v1/report.md`
- baseline 合同 smoke：`task/task49-pharma-benchmark-baseline-freeze/baseline_contract_smoke_v1/report.md`
- 直接选择方法审查：`task/task49-pharma-benchmark-baseline-freeze/fresh_method_review_20260928.md`
- 原始实验日志：`experiments/logs/task49_*`

## 10. 2026-10-01 FamilyGuard 后续审查

FamilyGuard 已经从候选设计推进到两条独立证据链：

1. common Frozen-incumbent family-outage：300 streams、独立 verifier `300/300`，persistent S 上相对 current relative 的 release-after 改善为 `+7.60/+12.24/+22.47 pp`（delay 0/1/4），三个 paired bootstrap 区间均在 0 以上；transient 为 0 pp，成本满足预登记上限。
2. common-stratum 主分母：180 streams、6 methods、77,976 events、159 条真实 failure，独立 action/state/evaluator recovery 通过。FamilyGuard pooled task success=`0.907664`，Frozen=`0.901662`，MIRA-relative=`0.902816`；但 scaffold-cluster paired 区间分别为 `[-0.6204,+1.5988]` pp 和 `[-0.6210,+1.1644]` pp，仍跨 0。

因此，FamilyGuard 解决了上一轮 family-level repeated-trial 的具体 failure mode，并补齐了 dynamic stress、failure、family macro、latency 和 memory 的开发证据。它仍不能让 Gate C 自动通过：E3 的机制消融、全部 baseline 的统一 unknown/refusal/cost 合同、cross-endpoint qualification 和 frozen confirmation 仍缺。当前正确的主线判断是“候选修复已具备可复核的正向机制证据，主效果仍需冻结合同后的统一统计复核”。

下一步顺序：

- 将 FamilyGuard、relative、streak-only、matched 和 Frozen 的结果放入一张同口径主表；
- 补齐 unknown/refusal 事件的共同分母和 abstain/verifier 代价；
- 重新按 Gate C 的 C1--C4 逐项打分；
- 在 Gate A/B/C 全部达线前，不打开 confirmation，不申请效果型 A800。
