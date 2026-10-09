# Corrected reward-scale prior diagnostic v2

日期：2026-10-01  
性质：对 v1 指标实现的更正；不是新的效果实验。

## 触发原因

v1 用 fit-only ridge 拟合 model-implied probability
$p=\operatorname{sigmoid}(XW_{\mathrm{BCE}})$，但在评估时比较的是
$\operatorname{sigmoid}(XW_{\mathrm{ridge}})$ 与 $p$。因为 ridge 输出本身就是
probability-scale 的线性预测，这个额外 sigmoid 改变了被测对象。v1 的 RMSE 不能继续
解释为 reward-scale prior 对原模型概率的误差；v1 原始输出和报告保留，不覆盖。

## 固定输入

- 同一 `fingerprint_diagnostic_v1/inputs.json`、common-stratum manifest、5 folds × 2 seeds；
- 同一冻结 encoder、同一 `direct_prior` 快照、同一 ridge 正则 `lambda=1.0`；
- 不读取 evaluation truth，不训练 selector，不改 full receipt；
- 仅使用 fit 区的 model-implied probability 拟合 reward-scale prior。

## 正确指标

对每个 split 计算：

1. direct logit：$z=XW_{\mathrm{BCE}}$；
2. model-implied probability：$p=\operatorname{sigmoid}(z)$；
3. reward-scale prior：$W_r=\operatorname{ridge}(X_{fit},p_{fit})$；
4. reward score：$s=XW_r$；
5. 报告 $\operatorname{RMSE}(s,p)$、MAE、score 的区间外比例和范围。

`score` 区间外是诊断信息，不是自动失败条件：Gaussian 0/1 surrogate 的线性均值
可以超出 `[0,1]`；是否采用它必须在 baseline 合同中同时固定 reward likelihood、
prior、noise 和 update 公式。真正的目标是不能把 logit 权重无说明地当成 reward mean。

## 失败条件

- 输入/manifest hash 与 v1 不同；
- 读取 confirmation truth；
- 重新选择 ridge、fold、epoch 或阈值；
- 继续使用错误的 `sigmoid(reward_score)` RMSE 作为 v2 结果。

旧 full receipt 的 adapted/out-of-stratum 标签不因这次诊断自动改变；若要重跑严格
baseline，另立执行合同和新 receipt。
