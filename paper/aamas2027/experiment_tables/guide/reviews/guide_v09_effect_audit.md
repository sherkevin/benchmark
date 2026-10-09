# Guide v09 效果矩阵审查

日期：2026-10-09  
审查角色：只读效果与一致性审查  
审查对象：`paper/aamas2027/experiment_tables/guide/versions/v09_20261008/guide.tex`、`guide/README.md` 及当前 Task49 development/fresh receipts。  
本报告不修改 Guide，不把 development 数字升级为论文结果。

## 结论

**判定：HOLD（目标关系大体合理，但还不足以作为“做完即可关闭”的效果合同）。**

v09 已经修掉了四个明显漏洞：唯一主比较、逐 delay 判定、E1 最小 headroom、E6 暴露收据。它对“候选在哪些地方可以赢、哪些地方允许输”的方向也比 v08 清楚。然而当前矩阵仍有三个会影响验收的核心问题：

1. **候选方法和实验 scope 没有完全统一。** Guide 的主候选是 `mira_context_reliability`，已有普通 development 主表主要是 `mira_relative_reliability`/`MIRA-FamilyGuard`，fresh 结果才是 `mira_context_reliability`。不同候选、菜单、压力事件和成本口径不能拼成一条效果链。
2. **E2/E3/E4 的主比较结果尚未按 Guide 的唯一估计量齐全。** 现在有 pooled 数字、相对 Frozen 的 paired CI 和若干受控压力 CI，但缺少“同一 candidate、同一 intervention、同一 delay 下的 candidate−matched Online-SGD”和“full−primary-ablation”的完整主 CI。
3. **自然场景的安全边界和 harmful switch 没有成为硬门。** 候选即使相对弱的 Online-SGD 不差，也可能明显伤害 Frozen；仅报告 rescue/harm 而不设 guardrail，会留下挑分布或挑分层的空间。

因此，v09 可以作为**修订中的目标矩阵**，不能据它当前的文字直接宣布 Benchmark+Baseline 已关闭。最小修复是统一候选身份和 scope，然后只生成缺失的配对向量/统计收据；不需要重新寻找更多 baseline，也不应先打开 confirmation。

## 1. 先固定“合理结果”应是什么

下表是对当前故事线的可辩护关系。它不是预填数字，而是验收时必须逐项核对的方向。

| 场景 | 候选应优于 | 候选可以持平或逊于 | 不能接受的解释 |
|---|---|---|---|
| E1 任务有效性 | 只需证明 fit-only Best Fixed 明显优于 Random，证明存在 headroom | 不涉及候选优越性；Oracle 只作上界 | 用 candidate 结果倒推 benchmark 有信号，或把 Oracle 当可部署 baseline |
| 自然、stationary、无事件 | 不能要求普遍 superiority；应至少不明显伤害强 Frozen/incumbent | 可因更新开销、无可利用漂移而小幅输给 Frozen；也可与 matched SGD 接近 | 只因 controlled stress 赢，就宣称自然环境 generic gain |
| 受控 persistent event / correctness drift | 应优于 Frozen，并应相对 matched Online-SGD 有明确增益；这是动态机制的主证据 | 可以在 transient event 不显著改善，甚至选择保持 Frozen | 用不同压力强度、不同 candidate 或不同 delay 选择最有利的一组 |
| transient failure | 目标是避免永久错误切换；应接近 Frozen 或不造成显著额外损害 | 不要求正收益 | 把一次偶发故障后的短期波动写成恢复优势 |
| all-wrong 分层 | 各方法都没有可救的 provider 时不应有明显差异 | 可以共同失败 | 把共同 all-wrong 错误计为 routing gain |
| E3 机制消融 | full 应在**同一 scope**下优于去掉 context-relative residual 的 primary ablation | 若没有可利用 context，二者可以相等；但机制 claim 应随之收窄 | 只胜旧 baseline、不胜自己的 ablation |
| E5 成本 | 候选可用更多状态/更新换取受控压力收益，但必须在预先冻结的 utility/cap 内 | 自然 null 时成本更高是合理 trade-off，但不能无限高 | 只报 accuracy，隐藏 update、state、retry、refusal 或 provider cost |
| E6 final sealed | 自然场景应满足预注册 non-inferiority；controlled claim 才要求正 CI | final natural null 是允许的 | 看过 confirmation 后调阈值/epoch，再把它叫 fresh |

这个关系与 v09 §9 的自然 null、all-wrong、压力分层和成本 trade-off 方向一致；缺的是把自然 incumbent safety、harm budget 和候选身份写成机器硬门。

## 2. 与当前真实证据的逐项对照

### E1：方向可信，但主 CI 仍缺

当前 exact join 的描述性控制为 Best Fixed `0.915512`、Random `0.844875`， pooled 差值约 `+7.06 pp`，明显超过 v09 的 `+2 pp` 目标。`paired_uncertainty_matrix_v2` 已给出 Random−Frozen `-5.676 pp`、95% CI `[-7.587,-3.822] pp`，但没有给出 Guide 要求的 Best Fixed−Random 主 paired CI 和五个 outer-fold effect 的机器收据。

**判断：方向 PASS，验收证据 HOLD。** 不能用两个 pooled accuracy 相减替代 E1 的 scaffold-clustered molecule-pair estimand。三个 delay 的 Random 行是同一 molecule 的重复事件，不能被当成额外独立样本。

**最小修复：**从 immutable E1 input/role-qualified control join 生成一次 `e1_primary_effect.json`：每个 outer fold 只定义一次预登记 molecule/scaffold 配对，报告等权 fold effect、scaffold bootstrap CI、失败/unknown 分母；不需新模型或新 provider 调用。

### E2：自然边界与 controlled superiority 没有统一到同一候选

当前 fresh natural receipt 的 `mira_context_reliability` 与 Frozen 都为 `89.81%`，paired delta `0.00 pp`，95% CI `[-1.24,+1.40] pp`。这与“自然场景可以持平”一致，但若 E2 的 `-1.0 pp` non-inferiority 被用于该 fresh scope，CI 下界 `-1.24 pp` **未达 v09 门槛**。33-provider common-stratum 的 MIRA-relative vs Frozen 是 `+0.115 pp`，CI `[-0.373,+0.806] pp`，但这不是同一个 candidate/scope。

controlled fresh stress 中 MIRA-Context 为 `87.56%`，matched Online-SGD 为 `84.03%`，描述性差约 `+3.52 pp`；已有 CI 是 MIRA−Frozen `+14.18 pp [9.61,19.03]`，并非 Guide 唯一要求的 MIRA−matched-SGD CI。family-outage common support 中旧 relative-reliability 相对 matched-SGD 的差值为 `-4.04/-5.18/-0.96 pp`（delay 0/1/4），说明不能把这组旧候选结果回填为新 Context 的 controlled gain。

**判断：自然 E2 HOLD；controlled E2 方向有信号但主 CI HOLD。**

**最小修复：**在 preregistration 中一次性锁定 candidate method/source hash、scope、pressure intervention 和 comparator；从同一 raw event trajectory 生成 MIRA−matched-SGD 的 pooled 及逐 delay scaffold-clustered CI。自然 E2 另加 Frozen safety contrast；若保留 `-1.0 pp`，必须明确它只适用于指定 cohort，不能在 33-provider 和 fresh cohort 间择优。

### E3：机制增量当前没有满足主门

v09 将 `mira_relative_reliability` 固定为 primary ablation，这是正确的可识别方向。现有 common-stratum 的 FamilyGuard−MIRA-relative 为 `+0.483 pp`，95% CI `[-0.621,+1.164] pp`；family-outage 中 FamilyGuard−relative 的 delay-specific 点差 `+7.60/+12.24/+22.47 pp`，但那是另一个 controlled support/scenario。fresh natural Context 与 Relative 不能用当前日志简单承担 full−ablation 的机制 CI。

**判断：E3 HOLD。** 目前可以说“有一个受控 family-event 方向信号”，不能说 v09 的 full−primary-ablation `+1 pp` 且 CI 下界不低于 0 已被证明。尤其不能把 family-outage 的大效应代替 ordinary/no-event 的机制归因。

**最小修复：**用同一 candidate implementation、同一 input/menu/prior/seed/budget/release clock，分别固定 `natural`、`controlled correctness drift` 和（若要声称）`family outage` 三个 scope；主表只指定一个 E3 estimand，并给出 full−relative 的独立 paired vector/CI。若只有压力场景通过，claim 只能写成 pressure-specific mechanism。

### E4：逐 delay 门槛是对的，但 intervention 和候选身份仍可被挑选

逐 `D={0,1,4}` 检查并用 Holm–Bonferroni，方向是正确的。问题是 v09 同时允许 `transient/persistent failure` 和 `controlled correctness flip`，却没有在表中指定唯一的 intervention/support set。现有收据有两套不同压力：四-provider fresh correctness-drift（MIRA-Context vs Frozen）和 23/30 common-support family outage（MIRA-relative/FamilyGuard）。它们不能混称为同一 E4。

**判断：E4 统计形式 PASS；科学合同 HOLD。** family-outage 中旧 relative 在每个 delay 都不如 matched-SGD，正好说明候选变体和压力选择会改变结论。

**最小修复：**冻结一个 intervention registry：包含事件类型、触发规则、支持集、暴露/未暴露坐标和 ITT 分母；所有方法同一 registry、同一三 delay 运行。保留 unexposed 流，不得事后只分析 exposed。E4 主 contrast 固定为同一 candidate−matched SGD；Frozen、ablation 和 Bayesian 只作预注册 secondary。

### E5：当前候选成本与硬 cap 有直接冲突

v09 的 policy p95 `≤2× Frozen` 且 `≤100 μs` 能防止用小幅 accuracy 换取无限成本，方向合理。但当前 common-stratum 数字已经触发风险：MIRA-relative selection/update p95 `85.02/47.75 μs` 对 Frozen `36.44/2.00 μs`，FamilyGuard `118.69/91.08 μs`，因此分别约为 `2.33×/23.9×` 和 `3.26×/45.5×`。fresh four-provider Context stress 的 selection p95 `32.47 μs`、Frozen `17.88 μs`（约 `1.82×`）可能符合 cap，但这是不同 menu、不同 implementation scope。

**判断：E5 对当前 33-provider candidate HOLD；fresh Context 只能算有条件通过。** 不能从四-provider cost receipt 证明 33-provider 主 benchmark 的 cost gate。

**最小修复：**每次 decision 写入单独的 `policy_select_plus_update_ns`，不能把两个 p95 相加冒充联合 p95；锁定硬件、Python/runtime、warm/cold、batch=1、重复次数和 timer boundary。若论文主张 33-provider，必须用该 menu 的 candidate-specific cost；否则把 fresh four-provider 明确作为独立 scope。

### E6：效果方向合理，exposure 仍是关闭阻断

fresh natural 的 CI `[-1.24,+1.40] pp` 满足 v09 的自然 `-1.5 pp` 边界；controlled stress CI `[+9.61,+19.03] pp` 满足 controlled lower bound `>0`。但 Guide v09 自己已正确规定：157 条只有在 `exposure_receipt.json` 证明 truth、aggregate 和决策没有回流前才可承担 E6。当前 Stage-B 报告是效果/回放 receipt，不等于这份 exposure receipt。

**判断：E6 数值方向 PASS（按当前 fresh receipt），验收状态 HOLD。** 如果 157 条在 candidate freeze 前被查看、用于选择 Context 版本或调整 claim，则必须另建 sealed manifest；不能用“scaffold-disjoint”替代“未暴露”。

## 3. 会诱导 cherry-pick 的缺口

| 风险 | 为什么会被审稿人质疑 | 最小防线 |
|---|---|---|
| candidate 漂移 | Context、Relative、FamilyGuard 被分别放在 natural/stress/common 表 | 一个 candidate ID + source hash；任何变体另列 ablation |
| scope 漂移 | 33-provider common、四-provider fresh、family-outage、correctness-flip 的分母不同 | 结果表第一列固定 `scope/intervention/menu hash`，禁止跨 scope 排名 |
| comparator 漂移 | historical Replay-SGD 与 strict common-direct-prior SGD 不是同一初始化 | E2 只认 `matched_online_sgd`；Replay 独立行，不能用于过门 |
| delay 选择 | pooled 正数可掩盖某个 delay 负数 | v09 已阻止 pooled 覆盖；还需同一 intervention 的逐 delay raw receipt |
| pressure 选择 | 允许 transient、persistent、correctness flip 而不锁唯一主压力 | intervention registry + ITT/unexposed 全量分母 |
| aggregate masking | rescue 可以抵消大量 harmed | 固定 harm/rescue guardrail 和 both-wrong；至少在 natural/no-event 报告 incumbent safety |
| cost masking | 分别报告 select/update p95 可能规避联合 cap | 逐决策联合 timer，附 state、memory、provider time 和 utility |
| refusal zero | `refused=0` 可能是没有覆盖，不是零错误 | 没有 refusal fixture/真实事件时标 `N/A`，不能把 E5 refusal 写 PASS |

## 4. 建议给 v09 的最小硬化（不改变故事线）

1. 在 E2 增加 **natural incumbent-safety contrast**：若论文写“自然场景不伤害强固定策略”，则 MIRA−Frozen 的 CI 下界必须不低于预注册 margin；否则删掉该安全 claim，不能只靠 MIRA−SGD non-inferiority。
2. 把 E2/E3/E4 的 `candidate_id`、`menu_hash`、`intervention_id` 写进唯一主 estimand；不同 candidate/scope 不能共享一条门。
3. 把 E4 的 `transient/persistent/correctness flip` 从并列可选改成“主 intervention + 明确 secondary”；所有预登记坐标按 ITT 入账。
4. 为 E3 增加 scope 分层：ordinary/no-event 的机制差异和 controlled-event 的机制差异分开；只有主 scope 满足 full−ablation 才能写机制 superiority。
5. E5 采用逐 decision 联合 policy timer，并在 cap 之外固定“若无 utility 定义则只能写成本描述，不能写 cost-effective”。
6. 给 rescue/harm 一个预注册安全 guardrail，至少要求 natural/no-event 不出现超过 margin 的 incumbent harm；否则只可写“净准确率结果”，不能写“安全切换”。
7. E1 补齐 Best Fixed−Random 的 scaffold-clustered CI 和 fold-level effect 文件；E6 补齐 exposure receipt。以上均是统计/审计收据，不需要新科学工具调用。

## 最终判定

**效果方向本身是合理的：**自然 null/小幅输、controlled persistent event 赢、transient 不应过度反应、all-wrong 不应制造收益、成本应单独受限。  
**但当前 Guide 还不能作为“完成矩阵即可关 Benchmark+Baseline”的充分合同：**E2/E3/E4 缺同候选同干预的主 CI，E5 与 33-provider candidate 的已有成本冲突，E6 缺 exposure receipt，且 natural incumbent safety 没有硬门。

最小推进顺序：先锁 candidate/scope/intervention registry → 只读重聚合 E1/E2/E3/E4 主 paired vectors → 做 candidate-specific cost/refusal receipt → 做 exposure audit；任何一步不通过都保留 HOLD，不通过更换分母或换候选名称解决。
