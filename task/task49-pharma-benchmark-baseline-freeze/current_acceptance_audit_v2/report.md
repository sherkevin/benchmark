# Task49 当前验收审计 v2

日期：2026-09-29  
状态：开发证据审计；三门仍 HOLD；不改变已生效 benchmark、方法或决议。

## 结论

当前已经有一条可复核的 BBB development 证据链，但还没有达到最终论文验收标准。最关键的变化是：Greedy LinUCB 已完成同表示、同反馈合同的全量 CPU replay，baseline 主表可以进入 development 阶段；最关键的未完成项是多端点资格、provider lineage、动态事件 scope、GraphRouter/Laya-online fidelity、完整 E1–E5 矩阵和一次冻结 confirmation。

不能因为当前有很多 negative result 就把 Gate B/C 判为通过。负结果提升了可解释性，但没有替代缺失的 comparator、event stress 或 confirmation。

## 三门状态

| 闸门 | 当前状态 | 已经有的证据 | 仍缺的硬条件 | 结论 |
|---|---|---|---|---|
| Gate A：Benchmark | 部分收口 | TDC BBB truth 审计；P-C1 冲突组整体排除决议；authoritative manifest；722 行 development 分母；157 条 scaffold-disjoint confirmation 候选 | hERG/AMES 的真值与 endpoint equivalence；provider training membership；confirmation 与旧 panel overlap 的最终口径；model-fresh provider lineage | **HOLD** |
| Gate B：Baseline | development 主表已形成 | Best Fixed、Oracle、Random、Frozen、Online/Replay SGD、PILOT、BayesianRouter、Greedy，以及 task-native MLP/BT+CLS static router 均有 development 证据和相应恢复入口；Matched/BayesianRouter v2 raw receipt 已完成 60/60 streams fail-closed replay；Random/Greedy 也有独立 verifier | Random/PILOT/MIRA 仍缺部分统一状态或信息边界字段；Laya/JEV selected-only online 适配或正式 audit-only 范围；GraphRouter 输入语义不匹配，已保留为 audit-only；全 baseline 的 strict prior、成本和 confirmation 仍未统一 | **HOLD** |
| Gate C：矩阵与结果 | 主表可审计但不完整 | delay `{0,1,4}`、failure 分母、selected-only release、状态 hash、负结果、成本边界已记录；MIRA veto 机制消融已完成并独立核验 | E1–E5 全覆盖；动态 event（arrival/retire/version shift）协议和 recovery；paired uncertainty 的正式聚合；跨端点或 confirmation；统一成本/延迟表 | **HOLD** |

## 当前能写进论文的事实

1. 具体 provider 之间存在可复核的 correctness headroom：Best Fixed `0.9155`，Oracle `0.9972`；固定 provider 的错误中大部分有其它 provider 可救。
2. selected-only 普通在线更新的事件合同已经可以复核，但当前严格 Matched Online-SGD
   receipt 的 pooled accuracy 是 `0.892198`，低于同一 receipt 的 Frozen `0.901662`，也低于
   Best Fixed `0.9155`；旧 receipt 的 `0.8991` 不再作为当前主表数字。
3. Greedy/PILOT/BayesianRouter 和 provider-transfer/context-memory 等适配没有形成稳定的 direct dynamic gain；这些结果可以用来排除“只接一个标准在线头就够了”。
4. provider 返回后的 numeric confidence 有独立的 verification signal；固定阈值 escalation 的开发 accuracy `0.923823`、平均调用 `1.206`，但这是 post-call verifier 结果，不能回填成 pre-call selector 结果。

## 不能写进论文的事实

- 不能写“动态 JEV 已经超过 state of the art”；当前没有通过 Best Fixed/Matched/现代近邻和 confirmation 的统一正结果。
- 不能写“环境非平稳工具适应”；现有主流回放是 fixed pool，round 差异审计也没有提供自然 drift 证据。
- 不能写“跨 provider 传播解决了 selected-only bias”；provider transfer 只有信息筛选信号，正式 replay 没有超过 independent baseline。
- 不能写“Laya 是动态 JEV”；当前 Laya 只有静态表示/forward 证据，没有 selected-only optimizer 或 online checkpoint 回写合同。

## 进入下一阶段的最小闭环

### Gate A

- 固化 hERG/AMES 的实验真值、阈值、冲突裁决、覆盖数和 provider lineage；
- 对旧 development panel 与官方 test/confirmation 候选做 molecule/scaffold overlap 清单；
- 给每个进入 confirmation 的 provider 绑定训练来源、checkpoint hash 和版本事件口径。

### Gate B

- 完成 Random、一个同任务静态强 router（当前为 MLP BCE/BT+CLS）和统一 raw-run/recovery；第一版只读字段审计见 `baseline_raw_run_schema_audit_v1/report.md`，下一步仍需实现 writer/replayer 并迁移关键方法；GraphRouter 的输入/依赖阻塞单独归档为 audit-only，不以改写后的模型充数；
- 对所有方法明确 common feature、fit-only warm start、selected-only feedback、failure/unknown 分母和计算预算；
- 若 Laya-online 无法在同一合同中实现，正式把它降为静态表示审计，不在主表冒充在线 baseline。

### Gate C

- 先在 development 上完成 E1 任务有效性、E2 主比较、E3 机制消融、E4 delay/failure/menu/event stress、E5 错误与成本；
- 动态 scope 只能写成 `feedback-adaptive`，除非新增预登记的 arrival/retire/version-shift 并有冻结对照；
- 所有主结果按 molecule/scaffold 成对聚合，保存 raw JSONL、recovery、失败和 unknown 分母；
- 方法和代码冻结后，只开一次 confirmation，不回流调参。

## 当前执行决定

在上述硬条件完成前，不提交新的 A800 效果任务，不继续调 Greedy/PILOT/BayesianRouter/context-memory 的学习率、先验或阈值。新增实验必须先指出新的可观测反馈来源和可证伪条件；否则只做文档、审计和复现材料整理。

## 收据

- Development baseline master table：`task/task49-pharma-benchmark-baseline-freeze/development_master_table_v1/report.md`。
- Greedy verifier：`experiments/logs/task49_greedy_linucb_development_20260929/full_v3_fix/verification.json`。
- BBB authoritative manifest：`experiments/logs/task49_gate_a_20260929/authoritative_bbb_manifest_v1/`。
- 生效规则：`docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md`。

## 2026-10-01 FamilyGuard 后续审计

### 已关闭或明显补强的证据

1. **E4 受控压力协议已可复核。** FamilyGuard 的 family-outage full receipt 有 300 条 stream、43,320 个 decision、130,260 条 raw event，独立 verifier `300/300 verified`。它检查了事件前 prefix、selected-only delayed feedback、persistent exactly-once 注入、自然 failure 对照、unexposed 等价和 state hash。
2. **E5 分母和资源账本已补强。** common-stratum receipt 有 6 方法、180 条 stream、77,976 条事件，159 条真实 selected runtime failure 保留；同时报告 task success、valid-return conditional correctness、runtime failure、family macro、selection/update p95 和 state bytes。
3. **机制定位得到可证伪支持。** 在 `S=23/30` 的 persistent family outage 上，FamilyGuard 相对 current relative 的 release-after 改善为 `+7.60/+12.24/+22.47 pp`，三个五折 paired bootstrap 区间都在 0 以上；transient 三个 delay 均为 0。这只支持“family-level repeated-trial failure 有一个可执行修复”，不支持自然漂移或全局 superiority。

### 为什么 Gate C 仍然是 HOLD

common-stratum 主分母的 FamilyGuard−Frozen 和 FamilyGuard−MIRA-relative 配对区间分别为 `[-0.6204,+1.5988] pp` 和 `[-0.6210,+1.1644] pp`，均跨 0；所以普通 BBB development replay 还没有稳定 superiority 证据。Family-outage 是 activation-gated synthetic stress，不能代替普通主分母，也不能被写成自然环境事件。

按照生效标准，Gate C 仍为 `10/16 HOLD`，原因没有被这两条收据完全消除：

- **C1：** `E1–E5` 还没有一张统一冻结的主表；FamilyGuard 补了 E4/E5 的重要部分，但 unknown/refusal 分母和完整机制消融仍缺。
- **C2：** common-stratum 已覆盖六个方法，但 Laya/GraphRouter 的范围、Gaussian reward-scale 先验语义、全部 baseline 的统一预算/成本合同仍未闭合。
- **C3：** 结果方向和控制关系现在可解释，但普通主分母的 paired 区间跨 0，不能给 3/4 的稳定效果判断。
- **C4：** raw receipt 和独立 recovery 已通过，但主矩阵还缺统一 failure/unknown/refusal 聚合，以及一次冻结 confirmation。

### 当前可用 claim 边界

可以写：在固定已部署 BBB provider 菜单和 `feedback-adaptive` selected-only 协议下，FamilyGuard 复现并修复了一个预登记的 persistent family repeated-trial failure mode，并满足候选的延迟、暴露率和计算成本约束。

不能写：FamilyGuard 已在普通 development 任务上稳定超过 Frozen/Matched、已经证明自然 provider drift 适应、已经证明 model-fresh 泛化，或已经完成最终确认。

### 下一步

先按 `task/task49-pharma-benchmark-baseline-freeze/unknown_refusal_gap_v1/report.md` 封存 unknown censoring、provider failure 和 verifier refusal 的事件语义，再把 FamilyGuard、streak-only、relative、matched 和 Frozen 的共同分母及机制消融收进同一 E1–E5 表，之后重新按 C1–C4 评分。没有新的科学问题前不申请 A800；confirmation 继续关闭。

## 2026-10-01 E5-Semantic-Closure 执行后更新

已完成最小真实 CPU semantic smoke：scored `z000282`、cached failure `z001303`、unresolved `z002970` 和低 confidence post-call refusal，4 条主 stream、20 条事件。独立 verifier 通过 source-tree hash、selected identity、delay、state/version 和 stop-batch unknown 语义；4 个重新绑定 event hash 的篡改用例全部被拒绝。报告与收据：`task/task49-pharma-benchmark-baseline-freeze/unknown_refusal_semantic_smoke_v1/report.md`、`experiments/logs/task49_unknown_refusal_semantic_smoke_20261001/v2/`。

这只关闭了 E5 的 accounting/recovery 入口：unknown 不被填成 0/1，runtime failure 的 chemical correctness 保持 null，MIRA/FamilyGuard 的 failure ledger 按合同更新，主动 refusal 不计为 failure 或 wrong answer。它没有关闭 refusal utility、五折 fit-only verifier full table，也没有改变 Gate C=`10/16 HOLD`。

## 2026-10-01 Gate-C unified denominator and coverage audit

按照生效标准 v005，新增只读的 E1--E6 coverage matrix：
`task/task49-pharma-benchmark-baseline-freeze/gate_c_unified_matrix_v1/matrix.json`，说明见同目录 `report.md`。
它把 common-stratum full、controlled family-outage 和 semantic smoke 分成三个 scope，
显式保留 `PARTIAL`、`MISSING` 和 `NOT_APPLICABLE`，没有跨 scope 填充数字。

common-stratum 六方法各有 4,332 attempts、4,332 terminal 和 4,332 feedback；
`unknown=0`、`refused=0`，runtime failures 为 Frozen/Matched/Greedy/Bayesian/MIRA-relative/
FamilyGuard=`33/35/23/25/33/10`。这证明主收据没有静默丢掉 failure 字段，但也证明它
没有提供 refusal efficacy 或 unknown censoring 的效果分母。

因此 C1/C2/C4 的缺口被机器表定位而没有被“补齐”：common-stratum 的 E1--E5 仍是
`PARTIAL`，E6 是 `MISSING`；family-outage 只在受控 dynamic stress 的 E3/E4 为
`PASS`；semantic smoke 只在 E5 语义入口为 `PARTIAL`。Gate A=`11/16`、Gate B=`11/16`、
Gate C=`10/16` 不变，confirmation 继续关闭。下一步先补统一 baseline 公平合同和 refusal
utility full table，再重新评分；不申请新的效果型 A800。

## 2026-10-01 baseline role roster and prior-scale correction

新增 `task/task49-pharma-benchmark-baseline-freeze/baseline_role_roster_v1/roster.json`，
把当前方法分为 strict main controls（Frozen、Matched Online-SGD）、task anchors（Random、
Best Fixed、Oracle）、MIRA candidates、adapted modern comparators（Greedy、BayesianRouter、
PILOT）、static separate control，以及 GraphRouter/Laya audit-only。该分层只规定比较权限，
没有通过排除 adapted 方法来提高 Gate B；Gate B 仍 `11/16 HOLD`。

同时修正了 prior-scale 诊断的数学表述：旧 sigmoid-RMSE 和“Gaussian 均值越界即非法”
均不再作为判据。Matched Online-SGD 是当前合法的二元 logistic 强对照；direct-logit→
Gaussian 的 Greedy/Bayesian 旧收据继续标为 adapted，直到 likelihood、prior、noise 和
更新规则在新合同中一起固定。修正记录见 `common_prior_scale_diagnostic_v2/`。

随后完成 reward-ridge Gaussian semantic smoke：`reward_ridge_gaussian_smoke_v1/report.md`。
它用 fit-only binary reward ridge（`lambda_fit=1.0`）和独立 Gaussian precision `50.0`
运行 2 streams、20 decisions，并由 immutable-tree verifier 通过。该结果只关闭了
“拟合正则与 posterior precision 混淆”的接口问题；full 60-stream effect receipt、paired
uncertainty 和成本表仍缺，因此 Gate B/C 不变。

随后按同一合同完成 full receipt：60 streams、8,664 decisions，独立 verifier `verified`。
reward-ridge Online 相对自身 Frozen 的 molecule-paired delta 为 `-0.1154 pp`，scaffold
cluster bootstrap 95% CI=`[-1.1242,+0.9707] pp`；因此它是合法但没有稳定在线收益的
Gaussian comparator。selection/update p95 已记为 Frozen=`163,227/1,459 ns`、Online=
`161,083/26,583 ns`。完整报告：`reward_ridge_gaussian_full_v1/report.md`。

这补齐了一个 strict Gaussian comparator 的 development evidence，但不能和 strict
logistic stratum 或 adapted direct-logit Gaussian receipt 混成总排名；Gate B/C 仍保持
`11/16`、`10/16 HOLD`。

## 2026-10-01 baseline cost/budget fairness audit

新增 `baseline_cost_budget_v1/table.json` 与对应报告，逐方法记录 input/menu、hidden feature、fit-only warm start、likelihood、update rule、selected-only feedback、provider-call budget、feedback count、selection/update p95、state bytes 和 unknown/refusal 分母。

结果把当前可直接比较的 strict logistic rows 与 strict Gaussian、adapted modern、anchor、static rows 分开。common-stratum 的 Frozen/Matched/MIRA-relative/FamilyGuard 已有完整的 cost ledger；adapted unified receipts 的 state bytes、Best Fixed/Oracle 的 event timing 和 static router 的 online feedback budget 仍显式为 `MISSING`；Random 的 selection/update p95 已从事件账本补入。这一步补强 B11/B12/B13/E5，但不能通过“把缺失字段写进表里”冒充公平性闭合。

三门状态不变：Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16 HOLD`。仍缺全方法统一 raw contract、refusal utility full table、完整 E1–E5 冻结矩阵和 confirmation；不申请新的效果型 A800。

## 2026-10-01 post-call utility audit

按 `postcall_utility_audit_v1/analysis_plan.md` 先完成零机时分母审计，再做一次 CPU development replay。五折 evaluation 共 722 个有真值分子；第一 provider 在 fit/evaluation 均为 numeric/ok；第二 provider 的 6 次 cached failure 被单独记账，没有把 `system_success=0` 直接当成 wrong。

这次回放保存了两条分开的 receipt：policy receipt 不含 truth、`answer_correct`、`tool_feedback` 或 `grade`；evaluator-only 文件单独保存终止结果和反事实错误分解。独立 recovery `recovered_v3.json` 通过，policy/evaluator 共 4,332+4,332 行。

结果是：always-accept `wrong=61`；confidence refusal `wrong=22、refusal=149`；confidence escalation `wrong=52、final failure=3、calls=871`。拒答点估计 break-even `rho=0.2617`，追加调用点估计 break-even `kappa=0.0403`；scaffold-cluster paired bootstrap 的两个固定审计点区间都跨 0。完整表和边界见 `task/task49-pharma-benchmark-baseline-freeze/postcall_utility_audit_v1/report.md`。

这补上了 E5 的 refusal/utility 子项，但没有补 provider latency、update time、memory、unknown censoring effect 或 confirmation。因此 Gate C 仍为 `10/16 HOLD`，不把 post-call utility 写成 MIRA direct-selection/online-learning 效果，也不申请新的 A800。

## 2026-10-01 unified baseline receipt P0 repair

独立审查发现旧 Matched/BayesianRouter raw verifier 对菜单、read-set、事件顺序和根 summary 存在 fail-open。已保留旧失败现场，并用独立 v2 NumPy verifier 修复：它绑定 canonical input/cache hashes 和声明 git tree，逐事件重放 5 folds × 2 seeds × 3 delays，拒绝未知字段和 summary mutation。

新的 Matched 收据 `experiments/logs/task49_matched_sgd_unified_raw_20261001/full_v6/` 与 Bayesian 收据 `experiments/logs/task49_bayesianrouter_unified_raw_20261001/provenance_repaired_v1/` 均通过 60 streams、8,664 triples 的独立验证。Bayesian 收据只做了 provenance/summary 字段补全，原始 inputs、snapshots、events 和 action 未改；不完整回放和旧收据均不纳入结果。

这一步只修复 baseline 证据合同，不改变任何效果数字。Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16 HOLD` 维持不变；native prior lineage、统一成本矩阵、MIRA 主方法效果和 held-out confirmation 仍未闭合。

## 2026-10-01 development master receipt correction

三份标准联审时发现 `development_master_table_v1/report.md` 的 Matched Online-SGD 行仍引用
旧 receipt 的 `0.9024/0.8975/0.8975` 和 `+0.78 pp`。逐 stream 复核当前 v2
`full_v6` 后，已改为 `0.891274/0.888504/0.896814`，pooled=`0.892198`；同一 Frozen
为 `0.901662`。旧数字保留为历史记录，不再进入当前主表或 claim。该修正没有读取
confirmation、没有重跑策略，也没有改变三门状态或任何原始收据。

## 2026-10-01 baseline cost table receipt-pointer repair

按三份生效标准做了机器表到 raw receipt 的一致性复核。原表的成本数值没有错误，但
Frozen/Matched 行只指向 common metrics，Bayesian 行只指向旧 native-BT 目录，复核者无法从
成本表直接找到本轮通过 v2 verifier 的严格 raw contract。现已由生成器重建
`task/task49-pharma-benchmark-baseline-freeze/baseline_cost_budget_v1/table.json`，版本为
`baseline_cost_budget_contract_v2`：

- Frozen/Matched 追加 `baseline_verifier_p0_repair_v2/report.md` 和 Matched
  `full_v6/verification_v2.json`；
- Bayesian native-BT 行保留 native-prior receipt，并明确追加的
  `provenance_repaired_v1/verification_v2.json` 仅是 direct-prior raw-contract 审计，不能
  被当作 native prior 结果；
- 运行时没有读取 confirmation，没有重跑策略，没有修改任何 outcome、成本、分层或比较权限。

生成器校验为 `row_count=16`、所有行使用同一 input SHA、缺失字段显式保留、禁止跨 stratum
raw ranking。该修复只提高 B15/B12 的证据可定位性，不改变三门状态：Benchmark+Baseline
`41/60 HOLD`，Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16 HOLD`。

## 2026-10-02 三项审计口径修正

### Family registry

FamilyGuard common-stratum 的运行配置声明的是
`task/task38-p01-provider-pool/external_qualification.json`，但旧 `metrics_v1.json`
实际记录了 `data/bbb_census_v3/tool_registry.json`。后者没有 `fpadmet` 和
`admetlab_3_0`，使旧 family macro 把它们算成 `UNKNOWN`。现已在同一封存
`events.jsonl` 上只读重聚合 `metrics_v2_external_registry.json`，并用
`metrics_v2_external_registry_audit.json` 记录源 events/config/registry/analyzer hash。
科学输入、task success、failure、cost 和 confirmation 状态均未改变；当前 family macro
读取 v2 口径，旧 v1 数字只保留为历史失败现场。

### Replay cost scope

成本生成器原先把 90 条 source streams 的 Frozen、Online-SGD、Replay-SGD 事件一起算 p95，
却把 decisions 写成 Replay 的 4,332。现已按 `replay_sgd_matched` 过滤 30 条 Replay
streams，当前 Replay-only selection/update p95 为 `50,713.9/149,029.45 ns`，并在机器
表记录 source scope `30/90`。旧 pooled 数字不再用于验收。

### Prior lineage

当前 Matched Online-SGD 是 `common-direct-prior`；历史 Matched/Replay receipt 使用旧 BT
`score.weight`。fold0/seed0 的 prior 最大绝对差是 `0.2243737`；跨 10 个 fold/seed 的最大值为
`0.3328`。因此当前 Online
`-0.946 pp` 与历史 Replay `+0.50 pp` 不构成 paired superiority。要直接比较 Replay，必须
登记同一 direct-prior 合同的新回放；本次不申请任务。

三份验收分数不变：Benchmark+Baseline=`41/60 HOLD`，Gate A/B/C=`11/16`、`11/16`、
`10/16 HOLD`。下一步仍是闭合统一 baseline fidelity/unknown/refusal/成本合同和冻结
confirmation，而不是把这三项审计修正误写成方法收益。

## 2026-10-02 MIRA-FamilyGuard 损失分解

为决定下一轮方法是否值得继续，新增一项只读 post-hoc 分析：将 common-stratum 的
4,332 条 FamilyGuard action 与每个 fold 的 fit-only Best Fixed 对齐。五个 fold 的固定
incumbent 都是 `admetlab_3_0`。FamilyGuard/Best Fixed 分别为 `0.907664/0.915512`，
配对差 `-0.7849 pp`；`rescued=188`、`harmed=222`、`switch-away=3,619`。

222 次 harmed 中 212 次是替代 provider 的错误标签，10 次是 runtime failure，说明当前
主要问题是 incumbent-safe switching，而不是运行时成本或没有可替代 provider。替代工具
仍救回 188 次固定赢家错误，所以不能把方法改成永久固定。scaffold-cluster molecule-level
bootstrap 95% CI 为 `[-2.7932,+1.5659] pp`，跨 0；该分析只定位机制，不提高 Gate C。
完整分解见 `task/task49-pharma-benchmark-baseline-freeze/mira_vs_best_fixed_v1/report.md`。

## 2026-10-04 Replay-SGD provenance 收敛

按 baseline 资格矩阵的下一步，对历史 Replay-SGD receipt 做了只读 provenance 审计，没有重跑。
旧收据的 10 个 fold/seed prior 都精确对应保存的 BT checkpoint `score.weight`；722×64
feature 与当前 strict snapshot 的最大绝对差为 `3.51×10^-7`。但历史 BT prior 与当前
common-direct-prior 并不相同，最大绝对差为 `0.3328`；旧事件也没有
`state_bytes_before/after`。

因此 Replay-SGD 现在被机器矩阵明确归为 `separate_control`，可以保留为历史
Frozen/Online/Replay 的 paired control，但不能进入 strict 主表或与当前 Matched
Online-SGD 做 superiority 比较。原始结果、独立 recovery 和缺失字段均保留在
`task/task49-pharma-benchmark-baseline-freeze/replay_sgd_provenance_audit_v1/`。
这次只收敛了 baseline 角色，不改变三门分数：Benchmark+Baseline=`41/60 HOLD`，Gate
A/B/C=`11/16`、`11/16`、`10/16 HOLD`。

## 2026-10-04 endpoint qualification matrix

新增只读端点资格矩阵 `endpoint_qualification_matrix_v1/`，把已有数据审计、AMES 来源和
hERG 语义审计收敛到同一张表。当前角色固定为：BBB=`development_primary`，AMES=
`appendix_candidate`，hERG=`blocked_semantics`。AMES 的 129 个可连接分子不能直接升级为
第二主 benchmark；hERG 的 TDC Wang、FP-ADMET liability/blocker 与 ADMETlab cutoff 仍不能
证明同一端点。最终多端点 confirmation 继续关闭，Gate A 仍 `11/16 HOLD`。

## 2026-10-02 ContextShield 停止测试

依据上一节定位的问题，先实现并独立审查了 ContextShield 候选：固定 fit-only fallback、context-conditioned logit surrogate、selected-only delayed feedback，以及独立 runtime hazard ledger。局部 Bernoulli MAP/Laplace 更新、事件时钟、状态 hash 和 failure 语义均可由独立 verifier 重算；单元测试 4/4，历史 v4 smoke 的 192 个事件通过恢复。

在进入 replay 前按预登记做静态 gate-open 检查，不读取 evaluation truth。fold0/seed0 的 145 个 evaluation 分子中，预登记的 `delta=.05`、`delta_h=.05`、`eta=.02` 打开的 challenger 为 `0/145`（0 个 challenger cell）；无半径诊断为 `119/145`，不能用于调参。因此候选停止，不进入 full receipt 或 confirmation，也不把实现性 smoke 写成动态收益。

这一步关闭的是“数学不可执行/事件边界不清”的风险，不能提高 Gate C。当前分数仍为 Benchmark+Baseline=`41/60 HOLD`，Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`。若继续动态路线，下一候选必须先固定结果无关的 probe schedule，并报告 coverage、probe 成本、harmed/rescued 和 release 后真正改变 action/state 的次数；不能继续调 ContextShield 阈值追求 gate-open。材料见 `task/task49-pharma-benchmark-baseline-freeze/mira_context_shield_v1/` 和 `experiments/logs/task49_mira_context_shield_gate_diagnostic_20261002/v1.json`。

## 2026-10-02 ContextProbe full receipt

ContextProbe 在 ContextShield 的静态 gate-open 停止后补上了固定结果无关的 probe schedule，并完成 5 folds × 2 seeds × 3 delays 的 CPU candidate receipt。独立 verifier 通过 60 streams、25,992 events；coverage 诊断显示 32 个非 fallback provider 均被 probe 至少一次。

机制分解显示，ContextProbe 的 4,332 个 task 正确 `3,905/4,332=0.901431`，Frozen direct 为 `3,906/4,332=0.901662`。ContextProbe 有 895 次 probe、3,437 次 shield action；probe 正确率 `757/895=0.845810`，shield 正确率 `3,148/3,437=0.915915`。相对 fit-only fallback，harmed=`99`、rescued=`38`，所有离开 fallback 的动作都来自 forced probe，shield branch 没有额外 provider switch；释放反馈虽更新 `4,332/4,332`，但没有转化为可观察的 exploitation switch。现有 decision payload 未保存 shield 可行集合和预测分数，不能进一步归因 gate 为什么没有切换。

因此 ContextProbe v1 作为候选停止。它补上了 coverage 和实时状态可回放证据，却没有解决“探索结果如何进入非 probe 决策”的核心问题，不提高 Gate C，不申请 A800，不打开 confirmation。下一候选必须先提出新的可识别 gate/目标并通过 CPU 静态 stop test；不能把 forced probe 的动作变化写成动态收益。报告：`task/task49-pharma-benchmark-baseline-freeze/mira_context_probe_v1/report.md`。

独立审查补充：32/32 非 fallback provider 是 30 条 Probe stream 的 union coverage，每条 stream
仍有未覆盖 provider；现有 decision payload 没有保存 shield 可行集合和预测分数，不能把“无可行
challenger”和“可行但未超过 fallback”混为一个原因。审查记录：
`task/task49-pharma-benchmark-baseline-freeze/mira_context_probe_v1/independent_audit.md`。
新增的 MIRA-Relay 只是候选设计，先完成预登记的 state→action sensitivity、同 schedule 对照
和独立 verifier；Benchmark+Baseline=`41/60 HOLD`，Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`
不变。

## 2026-10-04 Provider-Graph Relay 候选 gate 更新

为检验 singleton-family 覆盖瓶颈，按预登记完成 Provider-Graph Relay fold0/seed0 CPU gate。12 streams、5,220 raw events 已通过独立 verifier；Graph Relay 相对 MIRA 的 net reward 为 delay0 `-2`、delay1 `-2`、delay4 `0`，相对 degree-matched random graph 三个 delay 均为 `-1`。动作变化是真实的，但没有形成正净效用，候选按停止条件降为负机制诊断/对照，不扩展 fold1--4、seed1 或 A800。

这条结果不改变三门状态：Benchmark+Baseline=`41/60 HOLD`，Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`。它补充了 M5/S4 的反证材料：静态 fit-only correctness graph 能改变动作，却不能保证 delayed release 下的 utility gain。主线回到统一 baseline 公平合同、完整 E1--E5 主表、unknown/refusal 分母、跨 endpoint qualification 和 frozen confirmation；详见增量核对表 `research/AAMAS2027_current_evaluation_scorecard_20261004.md`。

## 2026-10-04 provider lineage 与 E1--E5 覆盖审计

新增两项只读机器审计，未重跑 provider、未启动 GPU、未读取 confirmation：

1. `provider_lineage_audit_v1/` 将 BBB 固定菜单的 33 个具体 provider 逐一绑定到来源、运行
   资产、回放行数和训练成员缺口。结果为 G3=1（GraphB3）、G2=30（ADMET-AI×2、
   DeepPurpose×3、LGBM×1、B3clf×24）、G1=2（FP-ADMET、ADMETlab）；没有 provider 因此
   获得 model-fresh confirmation 资格。该表明确区分 source overlap 与 checkpoint
   membership，24 个 B3clf 和 3 个 DeepPurpose 不被当作独立训练来源。
2. `e1_e5_coverage_audit_v1/` 将当前生效评价标准、Gate C 收据、baseline eligibility/cost
   和 endpoint qualification 重新逐项对齐。E1、E2、E3、E4、E5 全部为 `partial`，E6
   为 `missing`；具体缺口是统一 E1 控制收据、分层后的 E2 主表、普通任务的完整机制消融、
   lifecycle 事件协议，以及统一 unknown/refusal/cost 主表。该审计没有用其他 scope 的数字
   填空，也没有把 controlled family outage 写成自然 drift。

两项审计都只提高了缺口定位性，不改变三门分数：Benchmark+Baseline=`41/60 HOLD`，Gate
A/B/C=`11/16`、`11/16`、`10/16 HOLD`。下一步顺序固定为 E5 schema/denominator → E2
role-qualified main table → E1 exact control join → E3 matched mechanism table → E4
scope-bound stress；E1--E5 不再是 partial 前不打开 E6 confirmation。

## 2026-10-04 论文构建修复

并发写 `paper/aamas2027/build/main.pdf` 造成的损坏现场已保留并关闭。当前源码在隔离目录
`paper/aamas2027/build/verify_current_20261004/` 单进程构建成功，主文 7 页；源码/PDF/hash、
预览和警告见 `paper_format_review_v1/build_provenance.json`。这只关闭交付完整性问题，不改变
科学结果或三门评分；主表仍是 pre-results 占位，图表与 E2/E4 分层缺口仍待修复。

## 2026-10-05 H=0 label-contract gate 更新

正式 `r_chem`/`r_run` receipt 已接入 H=0 双标签 JEV selector。10 个 fold/seed 槽位、
1,442 条 evaluation 预测和 10 个 checkpoint 经独立 NumPy verifier 核对通过；OOF
`0.887883`，同折 Best Fixed `0.915575`，差 `-2.769 pp`。已有同容量 direct `r_sys`
MLP 的两个 seed 均值约 `0.89335`，双头乘积也未超过它。

因此 label interface gate 通过，但 H=0 直接乘积 argmax 的 effect gate 失败。它没有改变
Benchmark、baseline 名单或最终 MIRA-Relay 目标；它只把下一候选收窄为 incumbent-safe
switch：双头作为相对证据与 runtime hazard 输入，不能直接替换强固定 provider。后续仍需
在同一信息合同下验证 harmed/rescued、延迟释放和成本，再判断是否进入 H=1 evidence planner。

## 2026-10-06 role-qualified main table 与 E1 exact join

按 E5 → E2 → E1 的顺序完成一次只读证据收口：
`role_qualified_main_table_v1/` 从已核验的 common-stratum、family-outage 和 H=0
控制收据生成机器主表，显式记录 role、scope、attempt/terminal/feedback 分母、runtime
failure、unknown/refusal、成本字段、state 字段和 recovery 状态。普通 `no_event`、
`controlled_event`、`family_outage` 不再混合聚合。

E1 的 Best Fixed/Oracle 使用同一份 `fingerprint_diagnostic_v1/inputs.json` 的 fold
evaluation 行；每个 control row 与同 fold 的 common-stratum
`2 scenarios × 4 methods × 2 seeds × 3 delays = 48` 条 stream 通过 attempt 数连接，
5 folds × 2 controls 的 10 个检查全部 PASS。结果为 Best Fixed `0.915512`、Oracle
`0.997230`。这把原来跨文件引用的 headroom 控制变成可机器复核的 exact join。

本次没有把 Random 的解析期望当成随机策略收据，也没有为 unknown/refusal 的观测零填入
效用结论。paired scaffold uncertainty、全方法统一 state/cost/refusal 字段、E3 完整
机制表和 E6 confirmation 仍缺。因此 Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16`
继续 HOLD；confirmation 继续关闭。

证据：

- `task/task49-pharma-benchmark-baseline-freeze/role_qualified_main_table_v1/report.md`
- `task/task49-pharma-benchmark-baseline-freeze/role_qualified_main_table_v1/result.json`
- `task/task49-pharma-benchmark-baseline-freeze/role_qualified_main_table_v1/control_join.json`

## 2026-10-06 E3 机制消融矩阵收口

将已通过独立 verifier 的 MIRA veto ablation 收据整理为
`mechanism_ablation_matrix_v1/`：300 条 stream、43,320 decisions/terminals/feedback，
同一 schedule 下同时包含 Frozen、streak-only、beta-only、full MIRA 和 Matched Online-SGD。
机器表明确记录 full、remove-correctness-gate、remove-runtime-channel 和 remove-state 四个
已覆盖子轴，并把 interface-null、independent-residual、family-permutation 和 paired CI
保留为 `MISSING`。

机制结果与原收据一致：controlled-event 下主要恢复来自 runtime-failure streak ledger；
beta-only 不能处理 runtime failure，并带来 no-event 静态损失；full MIRA 在三个 delay 都低于
Matched Online-SGD。该矩阵因此支持 `availability-hard / correctness-soft` 的修复方向，
但不支持 superiority claim，也不把 controlled event 写成自然 drift。

这次关闭了 E3 的一个真实子轴，仍不足以把 Gate C 从 `10/16 HOLD` 改成通过。剩余工作是
补最小的 interface-null、independent-residual、family-permutation 同 schedule CPU 表，
随后再按三份验收标准重算 E1–E5。

证据：

- `task/task49-pharma-benchmark-baseline-freeze/mechanism_ablation_matrix_v1/report.md`
- `task/task49-pharma-benchmark-baseline-freeze/mechanism_ablation_matrix_v1/result.json`

## 2026-10-06 E3 coverage v2：FamilyGuard state removal

继续只读整理已封存 receipt，生成 `mechanism_ablation_matrix_v2/`。它把 v1 的
provider-local veto ablation 与 common-stratum 的 MIRA-relative / MIRA-FamilyGuard
state-hash receipt 放在同一张**角色和 scope 分开的**机器表中，共 90 行：

- correctness Beta gate：full vs streak-only；
- runtime-failure ledger：full vs beta-only；
- all adaptation state：full vs Frozen；
- family hazard state：FamilyGuard vs MIRA-relative；
- strict online residual：Matched Online-SGD 作为最近邻控制。

这覆盖了当前主方法新增状态和更新的去除对照；`family_permutation` 仍明确为可选 sensitivity，
没有被填成通过。common-stratum 的 FamilyGuard `0.907664` 与 relative `0.902816` 仍沿用
已有 paired/bootstrap 边界，区间跨 0，不写成 ordinary-task superiority。

E3 从“没有完整机制去除”收敛为“核心新增状态已有去除/最近邻对照，仍有 permutation sensitivity
边界”。Gate C 仍暂不改分数，因为 E1 Random exact stream、E2/E5 统一字段、paired uncertainty
和 E6 confirmation 仍未闭合。

证据：

- `task/task49-pharma-benchmark-baseline-freeze/mechanism_ablation_matrix_v2/report.md`
- `task/task49-pharma-benchmark-baseline-freeze/mechanism_ablation_matrix_v2/result.json`

## 2026-10-06 E1 Random exact join

`random_development_20260930/run_v1` 是完整的 30-stream development receipt，已有独立
verifier `verified`；本次把它接入 `role_qualified_main_table_v1`。每个 fold 的 Best
Fixed/Oracle control row 现在同时连接 common-stratum 的 48 条 stream 和 Random 的 6 条
stream（2 seeds × 3 delays），共 54 条同 evaluation 分母，5 folds × 2 controls 的 10 个
检查全部 PASS。

Random pooled accuracy 为 `0.844875`，Best Fixed=`0.915512`，Oracle=`0.997230`。这补上
了 E1 的 Random exact control，不把早期 contract-only smoke 或解析期望值混入主表。Random
的 selection/update timing p95 仍缺，因此它只作任务有效性下界，不进入成本比较。

Gate C 仍保持 `10/16 HOLD`：E1 控制已更完整，但 E2/E5 的统一 cost/state/unknown/refusal、
paired scaffold uncertainty 和 E6 confirmation 仍未闭合。

## 2026-10-06 E5 accounting closure

新增 `e5_unified_accounting_v1/`，只读连接 common-stratum、family-outage、Random 和
post-call utility 的已核验收据。统一表有 17 行 scope-method/policy，显式记录 attempts/calls、
terminals、feedback、valid returns、chemical correctness、runtime failure、unknown、refused、
latency/state 字段和 recovery 状态。runtime failure 没有被填成 chemical wrong；post-call
refusal 与 provider failure 分开。

这关闭了 E5 的 accounting 命名和分母入口，但没有把观测到的 `unknown=0/refused=0` 解释成
拒答有效，也没有为缺失的 Random timing、全 baseline state/cost、unknown censoring 或 paired
scaffold CI 填估算。因此 E5 仍是 `PARTIAL`，Gate C=`10/16 HOLD` 不变。

证据：

- `task/task49-pharma-benchmark-baseline-freeze/e5_unified_accounting_v1/report.md`
- `task/task49-pharma-benchmark-baseline-freeze/e5_unified_accounting_v1/result.json`

## 2026-10-06 paired uncertainty matrix

从已核验的 common-stratum bootstrap、FamilyGuard persistent event summary 和 post-call
utility bootstrap 中只读收集预计算区间，生成 `paired_uncertainty_matrix_v1/`。共 15 行，
保留原始 estimand、单位、scaffold/molecule 分母和 bootstrap seed；没有区间的 Random 与
其它缺失 baseline 仍为 `MISSING`。

当前关键边界：Matched−Frozen 的 common-stratum 区间为 `[-2.2906,+0.2189] pp`，
MIRA-relative−Frozen 为 `[-0.3728,+0.8063] pp`，FamilyGuard−Frozen 为
`[-0.6204,+1.5988] pp`；受控 family-outage 的 release-after 改善单独保留，不能写成
ordinary-task superiority。

这补上了 C3/C4 的 paired uncertainty 入口，但没有让所有 baseline 共用同一 CI、cost/state/
unknown/refusal 字段，也没有补 Random CI 或 E6。因此 Gate C 继续 HOLD，下一步需要把该矩阵
与 E5 accounting 和 role-qualified main table 绑定成最终 E1–E5 coverage table。

证据：

- `task/task49-pharma-benchmark-baseline-freeze/paired_uncertainty_matrix_v1/report.md`
- `task/task49-pharma-benchmark-baseline-freeze/paired_uncertainty_matrix_v1/result.json`


## 2026-10-06 Stage-A gate v2：operational routing 范围收口

这次没有新增效果运行，而是把已经封存的证据按生效标准重新接成可审查的比较合同。`baseline_contract_v3/contract.json` 固定六个主方法的同菜单、同 64 维 common feature、fit-only warm start、selected-only feedback、delay `{0,1,4}`、成本和状态字段；Greedy/Bayesian 的 `1,098,504` state bytes 来自已核验 family receipt，不是按矩阵维度推算。`random_paired_uncertainty_20261006/v1/random_vs_frozen.json` 给出 Random–Frozen 的 molecule/scaffold paired delta `-5.676 pp`，95% CI `[-7.587,-3.822] pp`。

`gate_c_unified_matrix_v2/matrix.json` 将 E1–E5 的普通任务、受控 family outage、post-call utility 和 semantic smoke 分开，E3 的边界敏感性和普通 cohort 中 unknown/refusal 零观察仍显式保留。由此得到的 Stage-A 复核分数为：Gate A `13/16`、Gate B `12/16`、Gate C `12/16`，均只适用于“固定已部署 BBB 菜单上的 feedback-adaptive operational routing”范围。该结论记录在 `stage_a_gate_scorecard_v2/scorecard.json`，不等于最终 confirmation，也不支持 model-fresh 或自然 drift claim。

Stage A 现在具备执行一次 E6 的资格；confirmation 仍未打开。下一步是冻结方法、菜单、baseline、epoch、阈值和 evaluator，提交一项 provenance-locked 只读 confirmation。若确认不能复现预登记方向，按停止规则保留负结果并收窄 claim。
