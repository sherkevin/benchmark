# Post-call verification/refusal on fresh BBB confirmation v1

日期：2026-10-08  
状态：执行前冻结；本卡只验证调用后的风险门，不替代主稿的一次调用选择结果。

## 目的与主张

主张是：在 provider 已经被 fit-only 规则选定并返回一个概率后，基于该返回值的置信度门可以在未看 confirmation truth 的情况下拒答一部分高风险答案，并在 scaffold-disjoint 的 fresh BBB confirmation 上保持更高的 accepted accuracy。它不声称完成在线 MIRA、自然生命周期漂移或端到端 agent 增益。

## 固定输入

- fresh confirmation：157 个分子，来自 `experiments/logs/task49_gate_a_20260929/bbb_confirmation_candidate_manifest_v1/manifest.csv`；其 truth 只进入 evaluator ledger。
- 四个已封存 provider 的 fresh 概率：`experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/provider_predictions.json`。
- 五个 development scaffold folds 与 fit-only provider rates：`experiments/logs/task49_fit_score_reference_20261007/v1/prior_cache.json`。
- development provider 概率：`experiments/logs/task49_fit_score_reference_20261007/v1/development_provider_probabilities.jsonl`。每折只用 `fit_indices` 拟合 provider 和阈值；evaluation/confirmation truth 不进入策略。

运行前写入上述文件 SHA、命令、Python/numpy 版本和 git commit。输入、策略 receipt、evaluator ledger、独立核验结果分文件保存。

## 策略与基线

每折从 fit correctness 最高的 provider 开始；并列按固定 provider 顺序。置信度定义为 $c=2|p-0.5|$。在 fit rows 上取覆盖率目标 $0.8$ 的第 $\lfloor0.8n\rfloor$ 个最高置信度作为固定阈值。

| 策略 | confirmation 动作 |
|---|---|
| always-accept | 调用 fit-selected provider，始终接受其二值输出 |
| confidence-refuse | 置信度低于 fit-only 阈值时主动拒答，否则接受 |
| same-coverage random | 每折接受与 confidence-refuse 相同数量的随机位置，固定 seed |
| always-refuse | 调用后全部拒答，作为零回答覆盖率边界 |

本轮不把第二 provider 的 escalation 混入主结果；它另有独立的成本审计合同，避免用 confirmation 结果挑选追加策略。

## 指标与统计

报告每折和 pooled 的 coverage、accepted accuracy、wrong、refusal、calls、rejected-error-rate，并按 scaffold 做 paired bootstrap（10,000 draws，固定 seed 20261008）。主对比是 confidence-refuse 对 same-coverage random；次对比是 confidence-refuse 对 always-accept。另报告固定效用网格
$U=-(W+\rho R+\kappa C)/N$，$\rho\in\{0,0.1,0.25,0.5,1\}$，$\kappa\in\{0,0.01,0.05,0.1,1\}$。

## 证伪与停止条件

若 pooled accepted-accuracy 相对 random 提升小于 2 个百分点，或五折中少于四折胜出，则不把 fresh confirmation 写成验证收益。若 $(\rho,\kappa)=(0.25,0.01)$ 下 confidence-refuse 不优于 always-accept，utility 主张不成立。任何 policy receipt 出现 truth、未选 provider 输出或 evaluator 字段；缺失/非法概率未 fail-closed；或独立核验不能重算所有动作，整轮只记为失败工程审计，不作科学结论。

## 运行顺序

先运行 16 个 confirmation 的 smoke，完成 receipt/evaluator 分离、泄漏检查、独立重算和 mutation fail-closed。smoke 通过后运行全部 157 个 confirmation；负结果和失败日志保留，不改阈值、不换 split、不删除样本。

## Post-run scope amendment (2026-10-08)

The execution card was frozen before this replay, but the 157 candidates had already been inspected in earlier holdout routing work. The cohort remains scaffold-disjoint from development fit/validation and the policy inputs were frozen before this replay; it is not an untouched prospective confirmation set. The corrected interpretation and audit are recorded in `correction_amendment_20261008.md` and `report.md`. The original card is retained as the pre-run protocol.
