# AAMAS 2027 当前验收核对表（2026-09-30）

性质：开发阶段内部审查，不是 AAMAS 官方评分，也不保证录用。
适用标准：[Benchmark 与 Baseline 评价标准 v005](../docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md)。

## 当前结论

| 项目 | 当前分数 | 通过线 | 状态 |
|---|---:|---:|---|
| 故事线与创新 | 47.5/100 | ≥85 且硬门槛全过 | FAIL |
| 方法论 | 37/60 | ≥49 且硬门槛全过 | HOLD/FAIL |
| Benchmark + Baseline 总分 | 41/60（临时重评） | ≥49 且 H1–H10 全 PASS | HOLD |
| Gate A：Benchmark 选型 | 11/16 | ≥12，A1/A3/A4 各≥3 | HOLD |
| Gate B：Baseline 选型 | 11/16（临时重评） | ≥12，四项各≥3 | HOLD |
| Gate C：实验矩阵与结果 | 10/16 | ≥12，四项各≥3 | HOLD |

三门仍未通过，confirmation 继续关闭。临时重评只吸收了 2026-09-30 已完成的 Random 全量收据和 static-router 角色审查，不替代双人最终评分。

## 2026-09-30 端点资格补充

新增 hERG 语义核验后，Gate A 仍保持 `11/16 HOLD`：来源论文身份已可追溯，但 assay/cutoff equivalence、FP-ADMET 9,204 与 NCATS 8,993 的训练谱系差异仍未闭合。因此 hERG 不进入主 benchmark 或 confirmation。 GraphB3 已增加论文支持的 split provenance，但具体 checkpoint-to-seed/member binding 仍缺，不改变 Gate A。

## 2026-09-30 LGBM 资格补充

LGBM-BBB 的 B3DB/Mordred/notebook 处理链已完成一次只读重建，但版本差异导致 7,296 行与文档目标 7,298 行不一致，发布包缺少实际训练中间 CSV；因此仍不能声称 checkpoint-specific 独立测试，Gate A 保持 `11/16 HOLD`。

## 2026-09-30 provider-fresh menu 资格补充

当前生效的是 `provider_fresh_menu_v2`：provider-train 845、provider-validation 180，train/validation 按非手性 Bemis--Murcko scaffold 完全隔离，candidate/confirmation scaffold overlap 为 0，1,624 条源行完整分区。v1 的 14 条重复源行缺口已保留为历史失败，不再作为当前证据。

v2 独立 verifier `verified`，并绑定 RDKit `2025.09.2` 的 canonical/scaffold 重算；candidate 与 development 仍有 118 个 scaffold overlap，不能声称对 development 完全 scaffold-disjoint。

随后完成四个候选 provider 的 CPU 训练/验证，4 个 checkpoint 和 720 条 validation 预测通过独立重算（最大概率误差 `<5.0e-11`）。这只补充 provider lineage 的可执行性和收据完整性，不改变 Gate A 分数，也没有打开 confirmation；完整环境锁定、最终菜单与 selector 公平合同仍缺。

Gate B 已完成 common-stratum full receipt v2：5 方法 × 5 folds × 2 seeds × 3 delays = 150 streams、21,660 decisions、64,980 events，统一 hidden-64 representation、cohort、delay 和 selected-only ledger；修正后的独立 action/state/evaluator verifier `verified`。v1 因 Greedy `regularization=1.0` 与合同 `Σ0=0.02I` 不一致而作为工程失败保留。full_v2 的 Greedy/Bayesian 仍只能标 adapted/out-of-stratum：`direct_prior` 是 BCE/logit 权重，未证明是 Gaussian 0/1 reward-scale prior。

随后完成 exploration-matched controls：`greedy_no_update` 与 `bayesian_no_update` 共 60 streams、25,992 events，独立 verifier `verified`。molecule-paired 描述性分析显示 Matched/Greedy/Bayesian 相对 Frozen 的平均 stream delta 为 `-0.00946/-0.01430/-0.03623`，MIRA 为 `+0.00115`；固定 scaffold-cluster bootstrap 后，MIRA 区间为 `[-0.00373, 0.00806]`，跨过 0。证据入口：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_full_receipt_v2/report.md`、`common_stratum_exploration_controls_v1.md`、`common_stratum_paired_analysis_v1.md`、`common_stratum_bootstrap_v1.md`。Gate B/C 仍 HOLD。

## 本轮新增证据

1. Random：同一 BBB development 分母、33-provider menu、五个 scaffold folds、2 个 protocol slots、delay `{0,1,4}`，30 条 stream，独立 verifier 通过；accuracy=`0.844971`。
2. Static router：MLP BCE/BT+CLS 的 fit-only full-panel 诊断已有 40 fits、5,776 evaluations、checkpoint/NumPy recovery；MLP=`0.8961/0.8906`，BT+CLS=`0.8878/0.8947`，均低于 Best Fixed=`0.9155`。
3. GraphRouter：继续保留论文/源码审计，不执行缺 provider-description/edge semantics 的改写版本；这不是删除强对照，而是避免把不同输入语义冒称原方法。
4. Raw-run schema：对旧的 Matched Online-SGD、PILOT、BayesianRouter、Greedy、MIRA 和 Random 原始 JSONL 完成只读字段审计；另外新增 Matched 的 common-direct-prior receipt、BayesianRouter 的 native BT-prior receipt、Greedy 与 PILOT 的当前输入 unified receipt，以及 MIRA/Matched 的 240-stream unified receipt，各自独立重放通过。native BT-prior、Greedy、PILOT receipt 均通过基于声明 commit 不可变 git tree 的逐事件 verifier（各 60 streams、8,664 triples）；PILOT 的独立回放明确检查 normalized query、cosine+UCB、selected-only delayed update 和 state hash。旧日志的字段缺口仍保留为历史证据；跨方法 feature、warm-start、预算和成本公平合同仍未闭合，因此 Gate B 暂不能判为通过。
5. MIRA veto ablation：300 条 CPU streams 的 Frozen/streak-only/beta-only/full/matched SGD 机制消融已完成；full 与 v2 逐事件一致，streak-only 保留主要 controlled-event 信号，beta-only 低于 Frozen，matched SGD 仍高于 full。该结果补强 E3 机制诊断，但不关闭 E2/E4/E6。
6. Failure replay：先完成 fit-only target 的 240 条 CPU replay，发现 transient target support 不足；随后按预注册的 method-conditioned incumbent 重跑 action-supported 240 条 CPU streams，schema/evaluator verifier 240/240、relative action/state replayer 60/60 通过。进一步对 sealed receipt 做 v2 metric-only 审计，拆分 injected-terminal loss、feedback blind window 与 release 后 recovery；persistent relative 在 release 后高于 streak-only、仍低于 matched。该只读审计不替代新的完整 effect receipt，不关闭 action-supported dynamic stress、统一 paired uncertainty、成本表或 confirmation。
7. Cross-method fairness：Greedy 与 PILOT 的 unified full receipt 已补齐，五条 receipt 的 input/menu/fold/seed/delay/selected-only/feedback clock 一致；common-stratum v2 又增加了 source-tree/action/state replay 和 no-update exploration controls。审计确认 feature strata、prior/warm-start、初始 covariance、reward scale 和 update semantics 仍不同。当前主表只允许方法内 online−frozen 与 online−no-update paired effect，不能把异质 raw accuracy 混成总榜；cached replay 也不支持 cost-aware superiority。报告：`task/task49-pharma-benchmark-baseline-freeze/cross_method_fairness_audit_v1/report.md`、`common_prior_scale_diagnostic_v1.md`。

## Gate B 临时重评理由

| 子项 | 分数 | 依据 | 仍缺 |
|---|---:|---|---|
| 角色覆盖 | 3/4 | Random、Fixed、Oracle、Frozen、matched Online/Replay、现代 online nearest、task-native static router 已有角色证据 | Laya/GraphRouter 仍为范围审计 |
| 任务匹配 | 3/4 | Random 全量、common-stratum v2 和 exploration controls 均记录 menu、selected-only feedback、delay、failure、release clock | valid-return conditional correctness、provider-family macro 和最终成本表未完成 |
| 实现保真 | 3/4 | 现代 kernel 数值测试、版本/hash、source-tree/action/state 独立 recovery；common-stratum v2 150 streams 与 controls 60 streams 可重放 | Gaussian 分支的 logit/reward likelihood 尚未在同一 strict contract 中对齐；Laya runtime 没有 online update API；PILOT/BayesianRouter 是 adapted |
| 公平合同 | 2/4 | 共同分母、delay、selected-only、paired molecule 聚合和 no-update controls 已有收据 | 全部主 baseline 的 feature、warm-start、预算、成本、family macro 和统计 uncertainty 尚未统一收口 |

## 仍然阻止投稿确认的事项

- Gate A：hERG endpoint equivalence、provider training membership、旧 panel/confirmation overlap 与 model-fresh lineage；AMES 公共来源已审，但 provider 资格仍未闭合。
- Gate B：common-stratum 的 Gaussian reward-scale/统一 posterior、feature/warm-start/update-budget/cost 合同、Laya 的正式 audit-only 范围和 provider lineage 记录。
- Gate C：E1–E5 的统一主表、dynamic event stress、paired uncertainty、成本表和一次冻结 confirmation。

当前最稳妥的论文事实仍是：固定已部署 BBB 菜单上的 delayed selected-only development replay 已可复核；MIRA-SafeSwitch v2 在预先注入的 provider-version event 上有机制信号，但没有超过 Best Fixed 的最终证据。目标 claim 不下调，下一轮工作必须继续补齐方法和证据，而不是用改写文字替代缺口。

## 2026-10-01 FamilyGuard 修复回执

上一轮 family-outage 收据把缺口定位为 provider-local availability ledger 的 family-level repeated trial。按预登记的 `MIRA-FamilyGuard v1` 候选完成了独立 no-event control、150-entry target manifest 和 300-stream CPU event receipt；未读取 confirmation，未使用 A800。

- 事件回执为 300 streams、43,320 decisions、130,260 raw rows；274 exposed、26 unexposed、3,605 次注入。独立 replay verifier `300/300 verified`，并检查 prefix、unexposed 等价、selected-only feedback clock、自然 failure 对照和 state hash。
- 在旧四方法封存的 `S=23/30` 上，FamilyGuard 相对 current relative 的 persistent release-after delta 改善为 `+7.60/+12.24/+22.47 pp`（delay 0/1/4），五-fold paired bootstrap 区间分别为 `[+3.74,+13.70]`、`[+5.87,+19.82]`、`[+13.93,+29.76]` pp。target-family exposure 下降 `9.00/14.00/24.32 pp`；transient 三个 delay 均为 `0 pp`。
- 相对 relative 的 selection/update p95 比值为 `1.42x/1.69x`，数值状态数组比值 `1.012x`；满足候选预登记的成本上限。candidate common support 只有 `18/30`，因此主表仍以运行前封存的旧四方法 `S` 为主，并单独报告 candidate-support sensitivity。

这轮通过了 FamilyGuard 候选自己的预登记条件，说明上一轮定位的 family-level repeated-trial failure mode 有一个可执行修复。它补强了 E4 dynamic stress 和 E5 recovery/cost 的证据，但不改变 Gate C=`10/16 HOLD`：matched/common-stratum 主实验合同、机制消融、全部 baseline 的统一 cost/failure/unknown 分解和 frozen confirmation 仍未闭合。机器报告：`task/task49-pharma-benchmark-baseline-freeze/mira_event_replay_v1/family_guard_v1_report.md`；收据摘要：`experiments/logs/task49_mira_family_guard_cpu_20261001/event_full_v1/family_guard_v1_summary.json`。

## 2026-10-01 FamilyGuard common-stratum 接入

FamilyGuard 已在同一 common-stratum manifest 上和原有五个方法一起跑完 180 streams（6 methods × 5 folds × 2 seeds × 3 delays），并通过独立 action/state/evaluator recovery。159 条真实 selected runtime failure 保留在分母。机器指标显示 all-provider task success：Frozen=`0.901662`、MIRA-relative=`0.902816`、FamilyGuard=`0.907664`；FamilyGuard 的 runtime failure rate=`0.002308`，低于 Frozen/relative 的 `0.007618`。

按 molecule 配对、scaffold-cluster bootstrap，FamilyGuard−Frozen 为 `+0.5977 pp [-0.6204,+1.5988]`，FamilyGuard−MIRA-relative 为 `+0.4828 pp [-0.6210,+1.1644]`；两个区间都跨 0，因此这里只能记录为方向一致的 development evidence，不能写成稳定 superiority。FamilyGuard selection/update/state p95=`118.69 µs/91.08 µs/17,945 bytes`，relative 为 `85.02 µs/47.75 µs/17,721 bytes`。

这轮补齐了 B11/B12/E5 的 FamilyGuard 端分解和公平主分母，但 Gate C 仍 `10/16 HOLD`：机制消融、全部 baseline 的统一 unknown/refusal 分母与成本合同、以及 confirmation 尚未完成。报告：`task/task49-pharma-benchmark-baseline-freeze/mira_event_replay_v1/family_guard_common_stratum_report.md`；机器结果：`experiments/logs/task49_common_stratum_family_guard_20261001/full_v1/`。

## 2026-10-01 common-stratum 成本账本补充

对 common-stratum 的成本账本做了只读一致性复核后，6 个方法已经都有 selection/update/state p95：Frozen=`36.44/2.00 µs/16,896 B`，Matched=`35.33/13.21 µs/16,896 B`，Greedy=`178.39/37.83 µs/1,098,504 B`，Bayesian=`165.93/27.31 µs/1,098,504 B`，MIRA-relative=`85.02/47.75 µs/17,721 B`，FamilyGuard=`118.69/91.08 µs/17,945 B`。这些数值在历史 `metrics_v1.json` 与当前 external-registry 重聚合中保持不变；这关闭了该 receipt 内的空成本列，也暴露出 Greedy/Bayesian 的状态开销不能被忽略。

该补充不改变 Gate B/C：PILOT、旧适配 receipt、调用后 verifier/refusal 以及全部 baseline 的统一成本/预算合同仍未收口。当前数字仍是 Python/cache replay 的策略计算开销，不是线上 provider 金钱或 token 成本；confirmation 继续关闭。

## 2026-10-01 E5-Semantic-Closure 补充

新增的 semantic smoke 使用真实 scored、runtime failure、unresolved truth 和低 confidence provider 输出，4 条主 stream、20 条事件；独立 verifier 与 4 个重新绑定 event hash 的 mutation tests 均通过。它证明了当前 stop-batch unknown、MIRA/FamilyGuard runtime failure ledger 和独立 verifier refusal 的事件记账可以被重放并 fail-closed。

这不是 refusal efficacy 实验：主动 refusal 只在一个真实 cached probability fixture 上检查动作和分母，没有五折 fit-only threshold、同 accepted/refusal rate random、always-accept/zero-coverage controls。因此 Gate C 仍 `10/16 HOLD`，unknown/refusal 的统一效果表和 utility 仍缺，confirmation 不打开。

## 2026-10-01 Gate-C unified coverage audit

新增只读机器矩阵 `task/task49-pharma-benchmark-baseline-freeze/gate_c_unified_matrix_v1/matrix.json`，
把 common-stratum、controlled family-outage 和 semantic smoke 三个 scope 的 E1--E6 覆盖状态
放在同一张表中；缺少的估计量保留为 `MISSING`，不跨实验补数字。

主 common-stratum receipt 的六个方法各有 4,332 attempts/terminal/feedback，unknown 和
refused 均为 0；runtime failure 为 `33/35/23/25/33/10`（Frozen、Matched、Greedy、
Bayesian、MIRA-relative、FamilyGuard）。这一步证明 failure 没有被主表静默删除，但不能
替代 refusal utility 或 unknown censoring 效果实验。

矩阵结论：普通 common-stratum 的 E1--E5 仍为 `PARTIAL`，E6 为 `MISSING`；family-outage
仅在受控 E3/E4 为 `PASS`；semantic smoke 仅在 E5 语义入口为 `PARTIAL`。因此 Gate A=`11/16`、
Gate B=`11/16`、Gate C=`10/16` 不变，confirmation 继续关闭。下一步仍是统一 baseline
公平合同和 refusal utility full table，而不是新增无明确问题的效果跑批。

## 2026-10-01 prior-scale diagnostic correction

复核发现旧 `common_prior_scale_diagnostic_v1` 把 probability-scale ridge score 再过
sigmoid 后计算 RMSE；同时把 Gaussian 线性均值越过 `[0,1]` 当成硬非法条件。这两点都
不能支撑“Gaussian prior 非法”的结论。已保留 v1 作为工程失败，并新增同一输入/manifest
的更正 v2：`task/task49-pharma-benchmark-baseline-freeze/common_prior_scale_diagnostic_v2/`。

更正后的结论是 **logit 与 Gaussian reward likelihood 的语义尚未对齐**，而不是 Gaussian
线性均值本身非法。Matched Online-SGD 已经是二元 correctness 的合法 logistic 强对照；
现有 direct-logit→Gaussian Greedy/Bayesian receipt 继续标为 adapted/out-of-stratum，
不追溯改写旧结果。Gate B/C 分数不变；若后续需要严格 Gaussian 近邻，另登记 fit-only
reward-ridge（把拟合正则与 posterior precision 分开）及其小流 recovery，再决定是否重跑。

## 2026-09-30 E5/B11/B12 分解补充

对 full_v2 的 64,980 条原始事件做了只读的 failure、valid-return、provider-family 和成本分解，机器结果：`experiments/logs/task49_common_stratum_metrics_20260930/full_v1/metrics.json`，说明：`task/task49-pharma-benchmark-baseline-freeze/common_stratum_metrics_v1/report.md`。

- Frozen / Matched / Greedy / Bayesian / MIRA 的 all-provider task success 分别为 `0.901662 / 0.892198 / 0.887350 / 0.865420 / 0.902816`；MIRA 相对 Frozen 的 `+0.001154` 仍落在既有 paired bootstrap 跨 0 的区间内，不能写成稳定提升。
- valid-return conditional correctness、runtime-failure rate 和 family-macro 已有固定分母；family 暴露差异较大，不能只用 family macro 排名。
- 记录了 selection/update p50/p95 和 state bytes。MIRA 的 selector/update p95 高于 Frozen；Greedy/Bayesian 的状态约 1.1 MB。实时主张必须把这些代价写进主表。

这一步补强 B11、B12、B13 和 E5 的证据入口，但不关闭 Gate B/C。下一项唯一的科学实验是 method-conditioned incumbent 的 transient/persistent dynamic event recovery；先按 release 前盲区、release 后恢复、target exposure 和切换延迟建立可证伪主表，再决定是否值得申请下一条 Nebula 任务。

证据入口：[current_acceptance_audit_v2](../task/task49-pharma-benchmark-baseline-freeze/current_acceptance_audit_v2/report.md)、[development master table](../task/task49-pharma-benchmark-baseline-freeze/development_master_table_v1/report.md)、[Random report](../task/task49-pharma-benchmark-baseline-freeze/random_development_v1/report.md)、[static-router audit](../task/task49-pharma-benchmark-baseline-freeze/static_router_sufficiency_audit_v1/report.md)。

## 2026-10-01 动态事件 v2 复核

按登记的 `dynamic_scope=feedback-adaptive` 完成了 method-conditioned incumbent 的 transient/persistent failure replay。该范围只表示 selected-only 延迟反馈释放后改变状态和后续动作，不包含 provider arrival/retire、version drift 或自然环境非平稳。

- 240 条 stream、34,656 个 decision、104,208 条 raw event；`delay={0,1,4}`，包含 transient/persistent 两种受控事件。
- schema/evaluator verifier 为 `240/240 verified`，relative action/state verifier 为 `60/60 verified`；v2 metric analyzer 的 240 个 paired event/control 均通过新增的事件前 provider、version、state hash、released feedback identity、menu 和 control-valid-terminal 检查。
- persistent recovery 中 relative-reliability 相对 streak-only 的 release 后差值为 `+0.56/+0.28/+0.59 pp`（delay 0/1/4 的点估计），但仍低于 matched SGD；transient 的 relative 差值为 `0 pp`。这支持“反馈自适应机制能运行”的局部判断，不支持跨方法优越性。
- target 不是跨方法共同干预：120 个 target 中 106 个来自 `b3clf`、10 个 `lgbm_bbb`、4 个 `admetlab`；30 个 fold/seed/delay 坐标没有四方法共同 target。因此不能把不同 target 的 raw post accuracy 混成方法排名。
- 独立 target verifier、manifest self-hash、input/snapshot/source commit 已与 full receipt 做 post-hoc binding；原始 receipt 的 run id 复用了旧字符串，已记录为 provenance warning，后续 runner 已改为强制独立 verification receipt、`dynamic_scope` 和唯一 `run_id`。

Gate C 仍为 `HOLD`，Gate A/B 分数不变。provider-family 分层已经完成，但它只提供方法内覆盖率和恢复分解；下一步仍需决定是否做 common Frozen-incumbent secondary，并冻结 baseline fairness contract。在此之前不打开 confirmation，也不把这轮机制诊断升级为论文效果 claim。

## 2026-10-01 common Frozen-incumbent family outage

按封存的 family-outage 协议完成了唯一一条 240-stream CPU full receipt：5 folds × 2 seeds × 3 delays × 4 methods × 2 scenarios。独立 cross-run verifier 通过 `240/240`，并确认 226 exposed、14 unexposed、3,388 次 target-family 注入和 exactly-once delayed feedback。主分析器已按 ITT 和运行前封存的 `S=23/30` common support 计算 release-after delta、family exposure、leave-family latency 和成本。

- persistent 的 `relative-reliability − streak-only` 在 S 上为 `-0.22/-1.26/-0.22 pp`（delay 0/1/4）；五-fold paired bootstrap 区间分别为 `[-4.41,3.25]`、`[-5.24,1.88]`、`[-7.63,4.98] pp`，因此 delay1 的点估计没有形成稳定反证。
- relative 相对 matched-SGD 的 persistent release-after delta 为 `-4.04/-5.18/-0.96 pp`，且 family exposure 高 `+5.26/+5.72/+3.93 pp`；当前结果不支持 relative 超过 matched。
- transient 的 relative release-after delta 为 0；unexposed 全部和 no-event control 语义一致。这关闭了协议可执行性风险，但没有关闭方法效果风险。

问题定位为 provider-local availability ledger：同一 family 的多个 provider 会被逐个尝试，family-level isolation 太慢。下一步候选修复是 selected-only 的 family hazard ledger，加 posterior/streak、短期 family gate 和 cooldown/hysteresis；修复前不打开 confirmation。机器入口：`task/task49-pharma-benchmark-baseline-freeze/mira_event_replay_v1/family_outage_v1_report.md`、`experiments/logs/task49_mira_event_replay_cpu_20261001/family_outage_v1_full_v1/metrics_v1.json`。Gate A/B/C 仍保持 HOLD。

## 2026-10-01 reward-ridge Gaussian smoke

按更正后的 prior 合同完成了小流 semantic smoke：2 streams、20 decisions，fit-only
binary reward ridge `lambda_fit=1.0`，Gaussian posterior precision `50.0`，delay=0。
独立 immutable-tree verifier `verified`，并检查了 prior、`0.02I` covariance、UCB、
selected-only feedback、state hash 和事件守恒。初次收据因复用旧 run id 被保留为
provenance engineering failure，修复后 v2 使用唯一 run id 重跑通过。

该 smoke 只证明严格 Gaussian 对照可以运行，不提供效果或 superiority 结论；60-stream
full receipt、paired uncertainty 和成本表仍缺，Gate B/C 不变。报告：
`task/task49-pharma-benchmark-baseline-freeze/reward_ridge_gaussian_smoke_v1/report.md`。

## 2026-10-01 reward-ridge Gaussian full receipt

按同一合同完成 60-stream full development receipt：8,664 decisions，独立 verifier
`verified`。reward-ridge Online 相对自身 Frozen 的 molecule-paired delta=`-0.1154 pp`，
scaffold-cluster bootstrap 95% CI=`[-1.1242,+0.9707] pp`；因此该 Gaussian comparator
没有稳定在线收益。selection/update p95 为 Frozen=`163,227/1,459 ns`、Online=
`161,083/26,583 ns`，并保留 failure、family macro 和 unknown/refusal 分母。

这补齐了一个语义自洽的 Gaussian development comparator，但不改变 strict logistic、
adapted Gaussian 和 MIRA candidate 的角色边界，也不关闭 Gate B/C 或 confirmation。报告：
`task/task49-pharma-benchmark-baseline-freeze/reward_ridge_gaussian_full_v1/report.md`。

## 2026-10-01 baseline cost/budget fairness audit

新增机器表 `task/task49-pharma-benchmark-baseline-freeze/baseline_cost_budget_v1/table.json`，以及只读生成器和报告。它把 baseline 的输入、菜单、表示、warm start、似然、更新规则、调用/反馈预算、运行 p95、状态大小、failure、unknown 和 refusal 字段统一到一张 schema 中，并把 strict logistic、strict Gaussian、adapted modern、task anchor、static control 分层。

该表明确保留 `MISSING`：adapted unified receipt 没有 state bytes，anchor 没有 event timing，static router 没有 online feedback budget。它补强了 benchmark/baseline 的可审查性和 E5 成本入口，但没有凭空完成跨方法公平性。因此分数保持不变：Benchmark+Baseline=`41/60 HOLD`，Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16`，confirmation 继续关闭。

## 2026-10-01 post-call utility audit

完成一轮 CPU development utility audit，专门核对调用后拒答/追加调用的成本是否抵消 accuracy 改善。输入仍是 722 个 BBB development 分子和原五折；第一 provider 的 status/confidence 分母通过零机时审计，第二 provider 的 6 次缓存失败单独记录。

回放结果：always-accept wrong=`61`；confidence refusal wrong=`22`、refusal=`149`；confidence escalation wrong=`52`、final failure=`3`、calls=`871`。拒答相对 always-accept 的点估计成本边界为 `rho*=0.2617`；追加调用相对 always-accept 为 `kappa*=0.0403`。两项 fixed audit point 的 scaffold-cluster paired 95% 区间都跨 0，不能写稳定 utility superiority。

policy receipt 与 evaluator-only outcome 已分离，独立恢复 `verified`。这补强 B12/E5 的拒答 utility 和错误归因，但不补 provider latency/update/memory、不证明 dynamic MIRA、不替代多端点或 confirmation。三门状态仍：Benchmark+Baseline `41/60 HOLD`，Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16 HOLD`。

## 2026-10-01 unified baseline receipt P0 repair

旧 Matched/BayesianRouter verifier 被独立变异测试证明存在 fail-open：伪造菜单、read-set 或 stream summary 仍可通过。已改为 immutable git-tree + canonical input/cache manifest + exact event schema 的 v2 verifier，并完整重放 raw event 顺序、terminal outcome、delayed feedback 和 root summary。最终 Matched v6 与 Bayesian provenance-repaired receipt 均为 60 streams、8,664 triples、`verified`；不完整回放和旧收据只保留作失败现场。

该修复提升的是 Gate B 的证据可审查性，不是方法效果。Benchmark+Baseline=`41/60 HOLD`，Gate A=`11/16`、Gate B=`11/16`、Gate C=`10/16` 维持；common-direct-prior Bayesian receipt 仍不能冒充 native prior，MIRA effect、统一成本和 confirmation 仍缺。

## 2026-10-01 development master receipt correction

三份标准联审时发现 development master 的 Matched Online-SGD 行仍引用旧 receipt 的
`0.9024/0.8975/0.8975`。以 v2 fail-closed `full_v6` 逐 stream 汇总后，当前 strict
common receipt 应为 `0.891274/0.888504/0.896814`，pooled=`0.892198`；同一 Frozen 为
`0.901662`。旧数字保留在历史报告，不再进入当前主表或 claim；development master 和
current acceptance audit 已同步。该修正不读取 confirmation、不重跑策略，也不改变三门分数：
Benchmark+Baseline=`41/60 HOLD`，Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`。

## 2026-10-01 baseline cost table receipt-pointer repair

按三份生效标准复核 baseline 成本表与严格 raw receipt 的指针一致性。成本数字和分层没有
改变；机器表已重建为 `baseline_cost_budget_contract_v2`。Frozen/Matched 行现在直接绑定
`baseline_verifier_p0_repair_v2/report.md` 与 Matched `full_v6/verification_v2.json`；Bayesian
native-BT 行仍绑定 native-prior 收据，同时标出
`provenance_repaired_v1/verification_v2.json` 只能作为 direct-prior raw-contract 审计。
这一步没有读取 confirmation、没有重跑策略、没有修改 outcome。生成器校验为 16 行、统一
input SHA、显式 MISSING、禁止跨 stratum raw ranking。Gate A/B/C 和 Benchmark+Baseline
分数维持 `11/16`、`11/16`、`10/16 HOLD`、`41/60 HOLD`。

## 2026-10-02 三项只读审计修正

### Family registry 指针

FamilyGuard common-stratum 的 `config.json` 声明使用 external qualification registry，但旧
`metrics_v1.json` 实际使用了旧 `data/bbb_census_v3/tool_registry.json`；因此
`fpadmet`/`admetlab_3_0` 在旧 family macro 中被归为 `UNKNOWN`。同一封存 events 已用
`external_qualification.json` 只读重聚合为 `metrics_v2_external_registry.json`，审计绑定见
`metrics_v2_external_registry_audit.json`。新的 family macro 为 Frozen=`0.873464`、
Matched=`0.756552`、Greedy=`0.886374`、Bayesian=`0.873853`、MIRA-relative=`0.888708`、
FamilyGuard=`0.883267`；task success、failure、成本和 raw event 未变。family macro 只作
family exposure 分解，不能据此跨方法排名。

### Replay cost 与 prior lineage

Replay-SGD 的成本 p95 现在只统计 30 条 `replay_sgd_matched` stream，selection/update
为 `50,713.9/149,029.45 ns`；旧 90-stream pooled 数字保留为历史失败现场。当前 Matched
Online-SGD 读取 `common-direct-prior`，历史 Matched/Replay 读取旧 BT `score.weight`；
prior 最大绝对差 `0.2243737`、相对 L2 约 `0.9938`。因此当前 Online `-0.946 pp` 与历史
Replay `+0.50 pp` 不能组成 paired superiority。三门仍为 Benchmark+Baseline=`41/60 HOLD`、
Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`，没有新 A800 任务。

## 2026-10-02 MIRA-FamilyGuard vs Best Fixed 逐样本审计

对 4,332 条 common-stratum FamilyGuard action 重新做了只读 paired 分解。五个 fold 的
fit-only Best Fixed 都选择 `admetlab_3_0`。FamilyGuard 正确 `3,932`，Best Fixed 正确
`3,966`，净差 `-34`（`-0.7849 pp`）；`rescued=188`、`harmed=222`，switch-away
`3,619/4,332`。harmed 中 `212` 次是替代 provider 的错误标签，`10` 次是 runtime failure，
误伤全部发生在离开 incumbent 后；同时替代工具救回了 188 次固定赢家错误。

该结果把下一轮候选的科学问题收窄为 incumbent-safe switching，并要求把 correctness 与
runtime hazard 分开计数。molecule-level scaffold bootstrap 95% CI 为
`[-2.7932,+1.5659] pp`，跨 0，所以它是机制定位，不是 superiority claim。报告与机器
结果：`task/task49-pharma-benchmark-baseline-freeze/mira_vs_best_fixed_v1/`。三门评分仍不变。

## 2026-10-02 ContextShield 候选停止

ContextShield 的实现和独立回放合同已经闭合：固定 fit-only fallback、context logit surrogate、selected-only delayed update、独立 runtime hazard ledger；局部 MAP/Laplace 更新与事件边界可复核。预登记静态 gate-open 检查在 fold0/seed0 的 145 个 evaluation 分子上为 `0/145`，所以候选停止，不做 full receipt，不申请 A800，也不把 smoke 结果升级为效果 claim。下一张候选卡固定 `epsilon=.20` 的结果无关 probe schedule，先在 CPU 上验证 coverage 和 release 后 action/state change。

这轮只补方法可执行性与停止证据，不改变验收分数：Benchmark+Baseline=`41/60 HOLD`，Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`，confirmation 继续关闭。

## 2026-10-02 ContextProbe full receipt

ContextProbe 的 coverage、selected-only delay 和独立 recovery 已在 60-stream CPU candidate receipt 中闭合，但机制结果没有支持当前候选：4,332 个 task 的 ContextProbe/Frozen 正确率为 `0.901431/0.901662`；895 次 probe 的正确率为 `0.845810`，3,437 次 shield action 的正确率为 `0.915915`。相对 fit-only fallback 的 harmed/rescued 为 `99/38`，所有离开 fallback 的动作都来自 forced probe，shield branch 没有额外 switch。

这轮把问题进一步收窄为“探索反馈没有进入 exploitation gate”，所以 ContextProbe v1 停止，不申请 A800，不改变 Benchmark+Baseline=`41/60 HOLD`、Gate A/B/C=`11/16`、`11/16`、`10/16 HOLD`，confirmation 继续关闭。下一步只能先在数学和 CPU stop test 中提出新的可识别 gate/目标。

独立审查进一步确认：32/32 非 fallback provider 只是 30 条 Probe stream 的 union coverage，
每条 stream 的 provider 反馈并不完整；旧 receipt 还缺少 shield feasible set 和 score payload，
不能据此断言 gate 永远关闭。新的候选 `mira_relay_v1` 改用已有 family registry 的共享残差，
并预登记 safe family-uniform probe、ProbeNoUpdate 对照、cluster bootstrap 和完整 decision
payload。它尚未实现或运行，不能提高当前分数。
