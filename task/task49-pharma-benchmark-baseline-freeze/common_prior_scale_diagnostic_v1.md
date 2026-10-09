# Direct prior scale diagnostic

状态：development diagnostic，不是方法效果实验。

## 设置

对 5 个 scaffold folds × 2 个 seed slots 的封存 `direct_prior` 做审计。它是 BCE/logit head 的权重矩阵。对每个 fold/seed，另做一个固定 ridge `λ=1.0` 的投影：目标是 fit split 上原 logit head 产生的 model-implied probability，不能读取 evaluation truth。输出只比较 score scale 和模型概率的偏差。

原始证据：`experiments/logs/task49_common_prior_scale_diagnostic_20260930/v1.json`。

## 结果

| split | BCE/logit score 落在 `[0,1]` 外的比例 | ridge reward prior 落在 `[0,1]` 外的比例 | ridge score 经 sigmoid 后与原概率 RMSE |
|---|---:|---:|---:|
| fit | 0.812 | 0.133 | 0.151 |
| validation | 0.877 | 0.180 | 0.157 |
| evaluation | 0.824 | 0.146 | 0.150 |

## 结论

直接把 BCE/logit 权重当 Gaussian 0/1 reward mean 没有成立，问题不是一两个异常分子。固定 ridge 投影能把 out-of-range 比例从约 82% 降到约 15%，但仍不能把它当成已校准的 reward prior，也会改变原模型的概率函数（evaluation sigmoid RMSE 约 0.15）。

因此 full_v2 中 Greedy/Bayesian 只能作为 adapted/out-of-stratum comparator。严格 baseline 需要二选一：

1. 采用 logistic posterior/UCB/TS，使 `direct_prior` 的 logit 语义和在线 likelihood 一致；或
2. 预先固定一个 reward-scale warm start 的拟合规则，再单独封存、验证 calibration 和 out-of-range gate，不能把简单 ridge 投影直接包装成共同先验。

下一步优先实现并做小流对照：`logistic` posterior 与 no-update exploration controls；在该诊断通过前，不更新 Gate B/C。

## 2026-10-01 更正

上面的 RMSE 计算把 `reward_score` 再过了一次 sigmoid；这不是 ridge 输出与目标
probability 的同量纲误差。并且 Gaussian 线性 surrogate 的预测均值超出 `[0,1]`
本身不是非法状态。因此 v1 只能保留为“发现 logit 与 reward surrogate 的语义需要
显式对齐”的工程诊断，不能用来断言 Gaussian prior 非法或用 v1 RMSE 判定 calibration。

按更正合同重新计算的 v2 见
`task/task49-pharma-benchmark-baseline-freeze/common_prior_scale_diagnostic_v2/report.md`。
现有 full receipt 仍不追溯改写；它使用的 direct-logit→Gaussian 组合继续标为
adapted/out-of-stratum，直到 likelihood、prior、noise 和更新规则被一份新合同同时固定。
