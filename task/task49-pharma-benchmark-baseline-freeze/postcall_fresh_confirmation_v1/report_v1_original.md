# Post-call verification/refusal on fresh BBB confirmation v1

日期：2026-10-08  
状态：**完成；fresh confirmation 的 verification/refusal utility 通过预登记效果门，但仍是扩展结果，不替代主稿的一次调用主结果。**

## 运行范围

- 157 个与 development panel 按 canonical molecule/scaffold 分离的 fresh BBB confirmation 分子。
- 五个 development scaffold folds；每折只用 fit rows 选 provider 和拟合置信度阈值，confirmation truth 只在 evaluator 侧使用。
- 四个 checkpoint-bound provider：`ecfp_logreg`、`ecfp_mlp`、`ecfp_random_forest`、`graph_message_passing`。
- 四个 policy：always-accept、fit-only confidence-refuse、同 coverage random、always-refuse。每次先调用 fit-selected provider；本轮不做第二 provider escalation，也不做 online update。
- confidence 为 $c=2|p-0.5|$；fit 目标覆盖率为 0.8。fresh coverage 是固定阈值在新数据上的实际覆盖率，不能写成 0.8。

## 结果

| policy | coverage | accepted accuracy | wrong | refusal | calls | $U(0.25,0.01)$ |
|---|---:|---:|---:|---:|---:|---:|
| always-accept | 100.00% | 88.15% | 93 | 0 | 785 | -0.1285 |
| confidence-refuse | 84.46% | **94.57%** | **36** | 122 | 785 | **-0.0947** |
| same-coverage random | 84.46% | 88.69% | 75 | 122 | 785 | -0.1444 |
| always-refuse | 0.00% | — | 0 | 785 | 785 | -0.2600 |

`n=785` 是 5 folds × 157 confirmation rows；它不是 785 个独立分子。confidence-refuse 相对 same-coverage random 的 pooled accepted-accuracy 提升为 **5.88 pp**，五折均胜出；相对 always-accept 提升 **6.42 pp**。按 scaffold 聚类 bootstrap（10,000 draws，seed 20261008），accepted-accuracy 差值的 95% CI 为：confidence-refuse − random `[+3.34, +9.72]` pp，confidence-refuse − always-accept `[+3.34, +9.72]` pp。这里 pooled 点差与 bootstrap mean 的差异来自 scaffold 重采样，不能混写。

在固定的 $(\rho,\kappa)=(0.25,0.01)$ 下，$U=-(W+\rho R+\kappa C)/N$：confidence-refuse 相对 always-accept 改善 `+0.0338`，相对 same-coverage random 改善 `+0.0497`。效用网格完整保存在 `experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/analysis.json`，没有从网格中挑选一个有利点作为唯一结果。

## 证据完整性

- smoke：16 个 confirmation、5 folds、320 policy rows；独立核验通过。
- full：实际为 **3,140 policy rows**（5 × 4 × 157），对应 3,140 evaluator rows。
- 独立核验 `verification_v2.json`：`status=verified`，逐行重算 3,140 条 action/provider/threshold/probability 绑定，并用 sealed manifest truth 重算 evaluator correctness/terminal。
- policy receipt 不含 `truth`、`answer_correct`、`terminal`、`scaffold` 等 evaluator 字段；manifest truth 只被 verifier/evaluator 读取。
- action mutation（把第一条 always-accept 改为 refuse）被 verifier 拒绝；evaluator correctness mutation 也被 verifier 拒绝。失败现场保存在 smoke/full 目录中。
- 没有新 API、LLM 或 GPU 调用；全部输入来自已保存的 provider checkpoint forward cache。原始 JSONL、配置、输入 hash、结果和 verifier 均保留。

## 判定与主稿边界

这轮通过了预登记的 fresh verification/refusal 效果门：pooled accepted-accuracy gain 大于 2 pp，且 5/5 folds 高于同 coverage random；固定 utility 点也优于 always-accept。它支持“调用后风险门可以拒答高风险返回”的扩展性结论。

它**不**证明：MIRA-Relay 在 fresh confirmation 上有动态收益；自然生命周期漂移；第二 provider escalation 的净收益；hERG/AMES 多端点；或端到端 LLM agent 的最终答案提升。主稿的一次调用 direct-selection 结论和已有 MIRA 结果不因本轮而重算。若把本轮纳入正文，应以短扩展段落/附录表格呈现，并明确它是 post-call verifier/refusal，不把它写成 pre-call selector 或 online learner。

## 文件

- 执行前协议：本目录 `experiment_card.md`。
- policy receipt：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/policy_receipt.jsonl`。
- evaluator ledger：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/evaluator_ledger.jsonl`。
- 聚合结果：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/result.json`。
- utility 网格：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/analysis.json`。
- 独立核验：`experiments/logs/task49_postcall_fresh_confirmation_20261008/full_v1/verification_v2.json`。
