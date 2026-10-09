# Provider-fresh BBB confirmation v1

日期：2026-10-06  
状态：执行前冻结；本文件在 confirmation 开始后不得修改。

## 目的

检验一个固定的四 provider、model-fresh menu 上，selected-only feedback 是否能让 MIRA-Relay 的在线可靠性更新超过同一冷启动 prior 的 Frozen selector，并报告强在线近邻。该批次只回答一次调用的工具选择问题；不把它解释成跨端点、自然漂移或 Agent 复核能力。

## 输入与资格

- provider 训练/验证：`experiments/logs/task49_provider_fresh_menu_20260930/manifest_v2/`，只用已核验的 `provider_train.csv` 与 `provider_validation.csv` 训练和选 provider checkpoint。
- provider menu 固定为 `ecfp_logreg`, `ecfp_mlp`, `ecfp_random_forest`, `graph_message_passing`；四者分别属于线性指纹、神经指纹、树模型和图模型四个 family。
- provider checkpoint 固定为 `experiments/logs/task49_provider_fresh_menu_20260930/cpu_v3/checkpoints/`，只做前向，不对 confirmation 分子重新训练或调参。
- selector development 使用 `fingerprint_diagnostic_v1/inputs.json` 的 722 个 BBB development 分子和其五个 scaffold-disjoint folds；这些分子只用于估计 provider correctness prior 与固定最佳工具，不打开 confirmation truth。
- confirmation 使用 `bbb_confirmation_candidate_manifest_v1/manifest.csv` 中 157 个分子；该 manifest 已排除旧 development panel 的 canonical/scaffold overlap，并按 P-C1 排除冲突组。`truth` 列只在 evaluator 侧用于最终评分，决策代码不能读取。

## 共同表示和 prior

- 分子表示是 RDKit Morgan radius=2、64 bits、`useChirality=True`，L2 normalize；再附加一个常数 1 作为固定 intercept，得到 65 维 query。表示不读取 provider 输出、truth 或 feedback。
- 每个 outer fold 只在 development fit rows 上，以每个 fresh provider 的二值 correctness `1[predicted_label == BBB truth]` 拟合一个无额外搜索的 logistic prior（`C=1`, `liblinear`, `class_weight=balanced`, `max_iter=2000`）；单一类别时使用对应常数 logit。validation rows 只用于 fold 结构审计，不用于选择 selector 超参。
- 所有方法在同一个 `(fold, seed)` 读取相同 prior。seed 只控制 Random/Thompson；方法超参不搜索：Gaussian covariance prior precision 50、noise variance 1、LinUCB alpha 0.5、MIRA `delta=.05, n_min=20, tau_fail=.20, runtime_failure_streak=2`、Matched Online-SGD learning rate .05。

## 信息和反馈合同

- 每个 confirmation 分子最多一次 provider call；决策时只见 query、完整四-provider menu 和已释放的 selected-only feedback。
- provider 输出由 frozen checkpoint 前向产生；未选 provider 的当前输出和 confirmation truth 永不进入 selector。
- terminal 后 feedback 在 `release_tick = decision_tick + 1 + delay` 释放；confirmation delay 固定为 0、1、4，seed 固定为 0、1。
- fresh providers 本批次若无 runtime exception，`runtime_failure` 计为 0；不能把化学错误重编码为 runtime failure。每次 selected 输出的 correctness 只在 release 时供更新。

## 方法和指标

主比较：Frozen、Matched Online-SGD、Greedy LinUCB、Bayesian Thompson router、MIRA-Relative-Reliability。控制：Random、fit-only Best Fixed；Oracle 只作 evaluator 上界。所有方法共享 menu、features、folds、seed、delay 和一次调用预算。

主指标是 molecule-level selected correctness（runtime failure 计 0）；同时记录 selected calls、released feedback、选择/更新耗时、最终状态字节、provider-family macro accuracy。主要 estimand 是每个 `(fold, seed, delay)` 的 MIRA−Frozen 配对差异，区间用 scaffold 聚类 bootstrap（10,000 draws，seed 20261006）。

## 否证和停止条件

1. 任一决策读到 confirmation truth、未选 provider 输出或未到期 feedback，整批作废，不作解释性修复。
2. 任一 checkpoint、输入 manifest、源码 hash 或 independent recovery 不匹配，整批作废。
3. 若 MIRA 相对 Frozen 的 pooled difference ≤0 或 95% scaffold bootstrap CI 覆盖 0，不能声称 confirmation 支持主要效果；保留结果并回到方法修订。
4. 若 provider 前向出现异常，按原始 status 记录为 runtime failure；不得删除分子或更换 checkpoint。

## 预期交付

`events.jsonl`（逐事件原始收据）、`config.json`（输入/源码/checkpoint hash）、`summary.json`（不确定性前的聚合）、`verification.json`（独立回放）和人读报告。confirmation 只运行一次；失败也保留完整日志。
