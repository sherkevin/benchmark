# Development baseline master table v1

日期：2026-09-29；2026-10-01 收据校正  
状态：开发期证据汇总；confirmation 关闭；不作为最终主表或录用承诺。

> **当前收据入口已校正。** 旧 Matched Online-SGD 数字来自历史 receipt，曾写成比自身
> Frozen 高约 `+0.78 pp`。本报告现在以通过 v2 fail-closed verifier 的
> `experiments/logs/task49_matched_sgd_unified_raw_20261001/full_v6/` 为权威入口；旧数字
> 保留在历史报告中，不再用于当前主表或 claim。

> **Prior lineage 也已显式分开（2026-10-02）。** 当前 strict 行名为
> `Matched Online-SGD (common-direct-prior)`：它读取 `full_v6/config.json` 中的
> `direct_prior`。历史 Matched/Replay receipt 使用的是旧 BT 训练的 `score.weight`。
> 两者不是同一初始化；独立审查测得 33×64 provider prior 的最大绝对差为 `0.2243737`、
> 相对 L2 差约 `0.9938`（见 `matched_sgd_unified_raw_v1/independent_review.md`）。
> 因此当前 Online 的 `-0.946 pp` 与历史 Replay 的 `+0.50 pp` 不能拼成一组 paired
> superiority 证据。若要直接比较 Replay，必须在同一 common-direct-prior 合同下重新登记
> 回放；本次只修正账本和标签，没有启动新任务。

## 目的

把已经完成的 BBB development replay 放在同一张审计表里，先回答一个问题：在相同的
33-provider、722-molecule、scaffold-disjoint 任务上，普通静态选择器、selected-only
在线更新和近期先验 router 是否已经能稳定超过 fit-only Best Fixed。这里不把不同方法的
专属 Frozen control 混成一个算法排名；每个 online 数字优先和自己的 Frozen 配对，跨方法
只作诊断。

## 共同任务范围

- 722 个 BBB development molecules、33 个 provider、5 个 scaffold-disjoint outer folds。
- 反馈只包含已选 provider 的 `system_success`，延迟为 `{0,1,4}`；未选 provider 的当前结果和未来反馈不可见。
- Best Fixed 只用每个 fold 的 fit 区 full panel 选 provider；Oracle 只作 evaluator 上界。
- 当前 Greedy 使用 P-C1 authoritative manifest；它与旧 fingerprint input 的 `providers` 和 `rows` 内容逐项相同，新增的是冲突收据与 manifest lineage。
- `seed=0/1` 是确定性 protocol slot，不是独立随机重复；表中不生成 seed 置信区间。

## 主表

### 固定参照

| 方法 | accuracy | 角色 |
|---|---:|---|
| Best Fixed (fit-only) | **0.9155** | 训练区选出的 `admetlab_3_0`，全 evaluation 固定 |
| Oracle | 0.9972 | evaluator-side 完整 panel + truth 上界 |
| Random (uniform menu) | 0.8450 | 同合同下界；不更新模型状态 |

### Online 与各自 Frozen 的配对结果

| 方法 | delay 0 | delay 1 | delay 4 | 与自身 Frozen 的模式 |
|---|---:|---:|---:|---|
| Matched Online-SGD (common-direct-prior) | 0.8913 | 0.8885 | 0.8968 | pooled `-0.95 pp` vs current Frozen `0.9017`；仍低于 Best Fixed |
| Matched Replay-SGD (historical BT score.weight) | 0.8947 | 0.8982 | 0.8961 | historical `+0.50 pp` control；与当前 direct-prior Online 不作 paired superiority 比较 |
| PILOT-adapted | 0.8823 | 0.8802 | 0.8850 | 相对自身 Frozen `-0.07/-0.28/+0.21` pp |
| BayesianRouter-adapted | 0.8886 | 0.8893 | 0.8907 | 相对自身 Frozen `-0.28/-0.21/-0.07` pp |
| Greedy LinUCB (fit-prior) | 0.8511 | 0.8449 | 0.8629 | 相对自身 Frozen `-0.55/-1.18/+0.62` pp |

### Strict Gaussian reward stratum（单独分表）

| 方法 | task success | 与自身 Frozen 的 paired delta | 95% scaffold-cluster CI | 角色 |
|---|---:|---:|---:|---|
| Reward-ridge Gaussian Online | 0.855494 | `-0.1154 pp` | `[-1.1242,+0.9707] pp` | 语义自洽的 Gaussian baseline；不与 logistic stratum 混排 |
| Reward-ridge Gaussian Frozen | 0.856648 | — | — | 同一 fit-only reward prior 的静态控制 |

该分表来自 60 streams、8,664 decisions 的独立 verified receipt。它补足了 direct-logit→
Gaussian adapted 对照之外的一个合法 Gaussian likelihood/scale 合同，但 Online 没有稳定
超过自己的 Frozen；selection/update p95 分别为 Online `161,083/26,583 ns`、Frozen
`163,227/1,459 ns`。完整 failure、family macro 和 hash 见
`task/task49-pharma-benchmark-baseline-freeze/reward_ridge_gaussian_full_v1/report.md`。

对应的旧 adapted Frozen accuracy 分别为：PILOT `0.8830`、BayesianRouter `0.8914`、Greedy
`0.8566`；这些 Frozen 是各自初始化和表示的控制，不是可以互换的同一策略。当前
Matched/Frozen strict common receipt 的 pooled accuracies 是 `0.892198/0.901662`，不再使用
旧表中的 `0.8913` Matched Frozen 数字。

### 静态 full-panel router（单独分表）

| 方法 | seed 0 | seed 1 | 信息角色 |
|---|---:|---:|---|
| MLP BCE | 0.8961 | 0.8906 | fit 区 full-panel 训练，evaluation 冻结 |
| BT + CLS | 0.8878 | 0.8947 | fit 区 full-panel pair objective，evaluation 冻结 |

这两行不是 online 结果，不能和上表按 delay 排名。它们用于检验静态表示/训练是否已经解释主任务，均低于 Best Fixed `0.9155`。

## 证据和恢复状态

| 方法 | 规模 | 独立恢复 | 结果入口 |
|---|---|---|---|
| Matched Online-SGD | 60 streams，8,664 decisions/terminals/feedback | v2 NumPy recovery，fail-closed | `task/task49-pharma-benchmark-baseline-freeze/baseline_verifier_p0_repair_v2/report.md`、`experiments/logs/task49_matched_sgd_unified_raw_20261001/full_v6/` |
| Replay-SGD（历史 BT score.weight 独立控制） | 90 streams，12,996 decisions；其中 Replay 30 streams | 通过 NumPy recovery | `experiments/logs/task49_matched_sgd_development_20260927/report.md`、`baseline_cost_budget_v1/table.json` |
| PILOT-adapted | 60 streams，5 folds × 2 slots × 3 delays | 通过 | `experiments/logs/task49_pilot_adapted_development_20260927/run_v1/recovered_v1/summary.json` |
| BayesianRouter-adapted（native BT unified receipt） | 60 streams，8,664 decisions/terminals/feedback | 通过；native BT prior/checkpoint lineage 和 NumPy action/state recovery | `task/task49-pharma-benchmark-baseline-freeze/bayesianrouter_native_bt_raw_v1/report.md` |
| Greedy LinUCB | 60 streams，8,664 events | 通过；状态 hash 逐事件核验 | `experiments/logs/task49_greedy_linucb_development_20260929/full_v3_fix/verification.json` |
| Reward-ridge Gaussian | 60 streams，8,664 decisions | 通过；fit prior、`0.02I` covariance、UCB、队列和 state hash 独立核验 | `task/task49-pharma-benchmark-baseline-freeze/reward_ridge_gaussian_full_v1/receipt_binding.json` |
| Random | 30 streams，4,332 events | 通过；独立重算 accuracy、propensity 和 release clock | `experiments/logs/task49_random_development_20260930/run_v1/verification.json` |
| MLP/BT+CLS static | 40 fits，5,776 evaluations | 通过；独立 checkpoint/NumPy recovery | `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/report.md` |

Structured JEV CPU 预筛、GraphRouter source audit 和 Laya 静态表示诊断保留为辅助/范围证据，没有塞进上表：它们的协议角色
或输入表示不同，直接混排会把诊断当成方法排名。

## 统一结论

1. **机会空间是真实的**：Best Fixed 为 `0.9155`，Oracle 为 `0.9972`；固定工具错的 61 个分子中 59 个至少有另一个 provider 正确。
2. **普通在线更新可以运行，但没有解决主问题**：当前严格 Matched Online-SGD（common-direct-prior）相对自己的
   Frozen 为 `-0.946 pp`（scaffold/family 的 paired uncertainty 仍需统一主表），比 Best
   Fixed `0.9155` 低约 `2.33 pp`；历史 Replay receipt 不能覆盖当前 strict receipt。
3. **近期 router 适配没有形成一致优势**：PILOT、BayesianRouter 和 Greedy 在多数 delay 上不如自己的 Frozen，且都明显低于 Best Fixed；Random 的 0.8450 提供了统一下界。BayesianRouter 现在同时保留 common-direct-prior control 和 native-BT unified receipt，二者不静默合并。历史 Replay 的 +0.50 pp 也只作为 BT-prior 线上的独立控制，不与当前 direct-prior Online 合并。
4. **当前瓶颈不是“有没有一个在线公式”**，而是强 incumbent 下的安全切换：模型必须知道什么时候离开 `admetlab_3_0`，还要承受 selected-only、延迟、失败和 provider/version 事件。
5. **Gaussian 分支的负结果现在可解释**：在 reward-scale prior 和 posterior precision 语义自洽后，普通在线更新仍未超过自身 Frozen；这排除了“只要修正尺度就会自动产生动态收益”的解释，但不否定 MIRA 的结构化事件适应目标。

## 对主方法的约束

这张表不把组合重新命名成创新，也不提前宣称任何新方法。它给下一候选设了硬门槛：

- 必须在同一 feedback 合同下超过 Matched Online-SGD 和最强合法近邻，或者给出可量化的错误/调用/恢复成本优势；
- 必须围绕 incumbent-safe switch、校准先验、延迟信用分配或动态 provider event 给出明确机制，而不是再接一个普通 ridge、SGD 或 Gaussian posterior；
- 固定池上的微小正点差不够支撑 dynamic claim。若没有 event 后 recovery 的优势，论文只能写 fixed-pool delayed adaptation。

因此，下一步先不申请新的 A800，也不继续调 Greedy/PILOT/BayesianRouter 的超参。应把已有
负结果和 matched SGD 的严格负结果写入方法审查，再只保留一个可证伪的安全动态候选。

## 收据索引

- Greedy 原始事件 SHA-256：`7d96d466f5ee5ae6098e19154a343077110860f1882e8e7d5551cb967f55ce1c`。
- Greedy verifier 状态：60 streams、8,664 events、`verified`。
- Greedy 汇总：`experiments/logs/task49_greedy_linucb_development_20260929/full_v3_fix/summary.json`。
- Matched SGD 报告：`experiments/logs/task49_matched_sgd_development_20260927/report.md`。
- 当前 Matched strict raw receipt：`experiments/logs/task49_matched_sgd_unified_raw_20261001/full_v6/`；独立校验：`experiments/logs/task49_matched_sgd_unified_raw_20261001/full_v6/verification_v2.json`。
- PILOT 报告：`task/task49-pharma-benchmark-baseline-freeze/pilot_adapted_development_v1/report.md`。
- BayesianRouter 报告：`task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/online_bayesianrouter_effect_result.md`。

## 2026-10-05 H=0 双标签 JEV 诊断补充

正式 label contract 下新增一条只做 cold-start 的静态诊断：correctness head 只在
`r_chem` 非 null 时训练，runtime head 使用全量 `r_run`，决策分数为两头概率乘积。
它没有 bandit、delay 或 online update，因此不与上面的 selected-only rows 混排。

| 方法 | OOF accuracy | 相对同折 Best Fixed | 角色 |
|---|---:|---:|---|
| H=0 双标签 JEV selector | 0.887883 | −2.769 pp | 新 label contract 的 cold-start 诊断，独立 recovery 通过 |

该 selector 在 10 个 fold/seed 槽位全部低于 Best Fixed，也低于已有同容量 direct
`r_sys` MLP 的两个 seed 均值约 0.89335。它否定的是“未经校准的双头概率乘积可以直接
argmax”这一最小候选，不否定两个 head 作为 incumbent-relative evidence 或 runtime
hazard 输入。正式收据和否证处置见
`task/task49-pharma-benchmark-baseline-freeze/jev_selector_h0_v1/report.md`。

## 2026-10-01 FamilyGuard 结果补充

本节追加两类不能合并的证据。前一类沿用 common-stratum 主分母，回答“在普通 BBB development replay 上，新增 family channel 是否带来稳定的总体收益”；后一类注入预登记的 persistent/transient family outage，回答“它是否修复了上一轮已经定位的同族重复试错”。两类实验使用相同的 molecule/provider 菜单和 selected-only 延迟反馈，但事件压力不是自然环境漂移，不能替代普通主分母。

### Common-stratum 主分母

本表的 family macro 已于 2026-10-02 改用配置中声明的 external qualification registry。
旧 `metrics_v1.json` 把 fpadmet/admetlab 归为 UNKNOWN；当前读取
`metrics_v2_external_registry.json`。其余指标和配对区间不变，具体更正与 hash 见
`mira_event_replay_v1/family_guard_common_stratum_report.md`。

FamilyGuard 已接入与 Frozen、Matched Online-SGD、Greedy、Bayesian 和 MIRA-relative 相同的 hidden-64、fit-only prior、fold/seed/delay 和 selected-only 合同。180 条 stream、25,992 个 decision、77,976 条原始事件通过独立 action/state/evaluator recovery；159 条真实 selected runtime failure 保留在分母。

| method | task success | valid-return correctness | runtime failure | family macro | selection/update/state p95 |
|---|---:|---:|---:|---:|---|
| Frozen direct | 0.901662 | 0.908583 | 0.007618 | 0.873464 | 36.44 / 2.00 µs / 16,896 B |
| Matched Online-SGD | 0.892198 | 0.899465 | 0.008079 | 0.756552 | 35.33 / 13.21 µs / 16,896 B |
| Greedy | 0.887350 | 0.892086 | 0.005309 | 0.886374 | 178.39 / 37.83 µs / 1,098,504 B |
| Bayesian | 0.865420 | 0.870443 | 0.005771 | 0.873853 | 165.93 / 27.31 µs / 1,098,504 B |
| MIRA-relative | 0.902816 | 0.909746 | 0.007618 | 0.888708 | 85.02 / 47.75 µs / 17,721 B |
| **MIRA-FamilyGuard** | **0.907664** | **0.909764** | **0.002308** | **0.883267** | **118.69 / 91.08 µs / 17,945 B** |

按 molecule 配对、再按 scaffold cluster bootstrap，FamilyGuard 相对 Frozen 为 `+0.5977 pp [-0.6204,+1.5988]`，相对 MIRA-relative 为 `+0.4828 pp [-0.6210,+1.1644]`；两个区间都跨 0。因此这轮只能作为方向一致的 development evidence，不能写成普通任务上的稳定 superiority。完整结果和机器文件见 `task/task49-pharma-benchmark-baseline-freeze/mira_event_replay_v1/family_guard_common_stratum_report.md` 与 `experiments/logs/task49_common_stratum_family_guard_20261001/full_v1/`。

### 受控 family-outage 压力

在旧四方法运行前封存的共同支持集 `S=23/30` 上，FamilyGuard、current relative、streak-only、matched 和 Frozen 共同回放 transient/persistent family outage。300 条 stream、43,320 个 decision、130,260 条 raw event、3,605 次注入失败通过独立 replay verifier。persistent 场景中，FamilyGuard 相对 current relative 的 release-after delta 为 `+7.60/+12.24/+22.47 pp`（delay `0/1/4`），五折 paired bootstrap 区间均在 0 以上；target-family exposure 同时下降 `9.00/14.00/24.32 pp`。transient 场景三个 delay 均为 `0 pp`。

这条压力结果支持一个更窄的机制判断：selected-only family hazard gate 能减少已经定义的 persistent family repeated-trial failure。它不证明自然 provider drift、model-fresh 泛化或普通 development accuracy 的稳定提升。相对 current relative 的 selection/update p95 比值为 `1.42x/1.69x`，数值状态数组比值为 `1.012x`，成本未超过候选预登记上限。详见 `task/task49-pharma-benchmark-baseline-freeze/mira_event_replay_v1/family_guard_v1_report.md`。

### 主表使用规则更新

1. Common-stratum 表进入 development 主表，但配对区间跨 0，不能升级为 superiority 结果。
2. Family-outage 表作为 E4 受控压力和 E3 机制证据单独呈现，不与普通 task success 混成一条排名。
3. FamilyGuard 当前满足候选自身的压力预登记条件，但尚未满足 Gate C：unknown/refusal 共同分母、全部 baseline 的统一成本合同、完整机制消融和冻结 confirmation 仍未闭合。
4. 在这些条件闭合前，不申请效果型 A800，不把 development 结果写成最终确认结果。

## 2026-10-01 baseline cost/budget contract

新增只读机器表 `baseline_cost_budget_v1/table.json`，把每个方法的 input/menu、feature、fit-only warm start、likelihood、selected-only feedback、调用预算、selection/update p95、state bytes、unknown/refusal 和比较权限放在同一 schema 中。没有在 receipt 中记录的字段保留为 `MISSING`，不从参数维度推测。

这张表把 strict logistic、strict Gaussian、adapted modern、task anchor 和 static control 分开。它关闭了成本字段分散、角色混排的审计缺口，但没有把 adapted 方法升格为 strict baseline，也没有改变 Gate B=`11/16 HOLD`。详见 `task/task49-pharma-benchmark-baseline-freeze/baseline_cost_budget_v1/report.md`。

## 2026-10-01 post-call utility extension

调用后 verifier 的 utility 作为独立 extension 收录，不和 direct selector raw accuracy 混排。`postcall_utility_audit_v1/report.md` 报告 722 个 evaluation 分子、六个策略、policy/evaluator 分离 receipt 和独立 recovery。

| 策略 | wrong | final failure | active refusal | calls | utility 说明 |
|---|---:|---:|---:|---:|---|
| always-accept | 61 | 0 | 0 | 722 | direct post-call control |
| confidence refusal | 22 | 0 | 149 | 722 | fit-only confidence gate |
| confidence escalation | 52 | 3 | 0 | 871 | fit-only second-tool extension |
| matched random controls | 50/60 | 0/1 | 149/0 | 722/868 | evaluator conditional null |

拒答 break-even `rho*=0.2617`，追加调用 break-even `kappa*=0.0403`；两项固定成本点的 scaffold-cluster paired 区间均跨 0。该 extension 补的是 E5 的 utility/错误归因子项，缺少 provider latency、update time、memory、unknown censoring effect 和 confirmation，不改变主方法候选排序或 Gate A/B/C。

## 2026-10-02 MIRA-FamilyGuard 对 Best Fixed 的逐样本分解

对同一 4,332-cell common-stratum FamilyGuard 回执做只读 post-hoc 对齐后，五个 fold 的
fit-only incumbent 都是 `admetlab_3_0`。FamilyGuard 正确 `3,932/4,332=0.907664`，
Best Fixed 正确 `3,966/4,332=0.915512`，净差 `-34=-0.7849 pp`。其中 both-correct=`3,744`、
both-wrong=`178`、rescued=`188`、harmed=`222`；switch-away=`3,619/4,332=83.54%`。

这把问题从“在线更新没有收益”进一步定位为 **incumbent-safe switching**：222 次误伤中
212 次是替代 provider 返回错误标签，10 次是 runtime failure；误伤全部发生在离开
`admetlab_3_0` 后。另一方面，替代 provider 也救回了 188 次固定赢家错误，不能简单禁用
所有切换。按 scaffold cluster 的 molecule-level bootstrap，净差 95% CI 为
`[-2.7932,+1.5659] pp`，跨 0。逐样本机器结果和下一轮安全切换约束见
`task/task49-pharma-benchmark-baseline-freeze/mira_vs_best_fixed_v1/report.md`。

该分解不是新效果主张，也不改变 Gate 分数；它决定下一候选必须显式压低 harmed、保留
rescue，并把 correctness error 与 runtime hazard 分开建模。
