# Corrected common prior scale diagnostic v2

日期：2026-10-01  
状态：已完成；这是 v1 指标实现更正，不是 selector 效果实验。

## 先纠正 v1

v1 的 ridge prior 是用 fit 区的 model-implied probability
$p=\operatorname{sigmoid}(XW_{\mathrm{BCE}})$ 拟合的，但计算 RMSE 时又用了
$\operatorname{sigmoid}(XW_{\mathrm{ridge}})$。这不是 ridge 输出与目标概率之间的误差，
因此 v1 的 RMSE 不能用于判断 reward-scale prior 是否接近原模型。v1 的代码、日志和旧
报告保留为工程失败；本报告不覆盖它们。

## v2 合同

- 输入仍是 `fingerprint_diagnostic_v1/inputs.json` 和 common-stratum manifest，hash 未变；
- 5 folds × 2 seeds，冻结 encoder 和 `direct_prior` 未变；
- ridge 正则固定为 `lambda=1.0`，不看 evaluation truth，不训练 selector；
- 正确比较是 $s=XW_r$ 与 $p=\operatorname{sigmoid}(XW_{\mathrm{BCE}})$，其中
  $W_r=\operatorname{ridge}(X_{fit},p_{fit})$。

机器结果：`experiments/logs/task49_common_prior_scale_diagnostic_20261001/v2.json`；
执行脚本：`src/probe_headless/audit_common_prior_scale_v2.py`。结果包含 10 个
fold/seed records 和每个 split 的范围、区间外比例、RMSE、MAE。

## 结果

| split | reward score 落在 `[0,1]` 外的比例 | $s$ 对 $p$ 的 RMSE | $s$ 对 $p$ 的 MAE |
|---|---:|---:|---:|
| fit | 0.1326 | 0.1360 | 0.1054 |
| validation | 0.1798 | 0.1450 | 0.1157 |
| evaluation | 0.1464 | 0.1605 | 0.1260 |

表中是 10 个 fold/seed 的宏平均；逐条结果在机器 JSON。reward score 的区间外比例
只是线性 Gaussian surrogate 的诊断，不把 `[0,1]` 当成它必须满足的硬约束。

## 对 baseline 合同的影响

1. 不能再说“所有 Gaussian 对照直接使用 BCE/logit 权重，因此必然语义非法”；这个
   说法由 v1 的错误 RMSE 支撑，已撤回。
2. 也不能把 v2 的 ridge projection 直接叫作共同 prior 已经成立：evaluation 上
   $s$ 与原模型概率仍有约 0.16 RMSE，且 reward likelihood、prior precision、noise
   和 update 公式尚未作为同一合同重跑验证。
3. 旧 common-stratum full receipt 仍然是既有的 adapted/out-of-stratum receipt，v2
   不追溯改写它；要声称严格 common-prior 比较，需要另立新 receipt。
4. 下一步最小技术选择是：先固定 reward-scale warm start 的生成规则并做小流 verifier，
   或改用与 Bernoulli correctness 同尺度的 logistic posterior/UCB/TS。选择前不跑新的
   效果型 A800。

## 验收结论

这次更正提高了审计真实性，消除了一个错误诊断，但没有让 Gate B/C 自动加分：
统一 baseline 的初始化、likelihood、预算、成本和 recovery 仍需一个新的可复核合同。
