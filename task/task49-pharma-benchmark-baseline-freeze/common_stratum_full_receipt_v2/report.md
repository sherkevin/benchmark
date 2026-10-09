# Common-stratum full receipt v2

状态：`verified`，但只作为协议、实现和数据绑定证据；不把这份 receipt 直接写成方法优越性结果。

## 运行与验收

- 输入：封存的 BBB development split，5 个 scaffold outer folds、2 个 seed slots、33 个 provider、722 个分子。
- 矩阵：5 方法 × 5 folds × 2 seeds × 3 delays = 150 streams；每个分子只调用一个 provider，feedback 在 `decision_tick + 1 + delay` 释放。
- 规模：21,660 decisions，64,980 events；真实 cache failure 149 条，失败保留在分母；confirmation 未打开。
- 代码提交：`c16e05e009cec102f3aa840f8ba5a40efbe80549`。
- 原始回执：`experiments/logs/task49_common_stratum_full_receipt_20260930/full_v2/`。
- 独立验收：`verification_v2.json`，重算了 manifest/checkpoint/prior/source tree、cohort/event roster、selected cache output、action/score、delayed state transition、state/parameter hash、feedback clock 和 summary conservation。

## 描述性汇总

| 方法 | streams | correct / decisions | accuracy | failed returns |
|---|---:|---:|---:|---:|
| frozen_direct | 30 | 3,906 / 4,332 | 0.901662 | 33 |
| matched_online_sgd | 30 | 3,865 / 4,332 | 0.892198 | 35 |
| greedy_online | 30 | 3,844 / 4,332 | 0.887350 | 23 |
| bayesian_online | 30 | 3,749 / 4,332 | 0.865420 | 25 |
| mira_relative_reliability | 30 | 3,911 / 4,332 | 0.902816 | 33 |

这些 pooled 数字不能当显著性结论：同一批 722 个分子在方法、seed、delay 间重复出现，统计单位必须是 molecule/scaffold 的 paired contrast。正式分析还要报告 runtime failure、valid-return conditional correctness、provider-family macro 和预先固定的 cluster/bootstrap 或 fold-level randomization。

## 当前解释边界

1. `greedy_online` 已修正为 `Σ0=0.02I` 对应的 precision `50.0`；v1 的 `regularization=1.0` 已判为工程失败。
2. `direct_prior` 是 BCE/logit head 权重。Greedy/Bayesian 将它直接放进 Gaussian reward surrogate，仍是 adapted/out-of-stratum comparator；这表示 likelihood/scale 没有在同一合同中对齐，不等于 Gaussian 线性均值超出 `[0,1]` 就非法。
3. Frozen 是确定性控制，Greedy/Bayesian 同时带探索和更新；当前 receipt 不能单独识别“更新收益”。还需要 no-update exploration-matched controls，以及 reward-scale prior 或统一 logistic posterior。
4. MIRA 的 runtime failure 与 chemical correctness 分开更新，其他动态基线把二者都压成 scalar reward=0/1；正文需要同时报告 joint success、valid-return rate 和 conditional chemical correctness。

## 下一步

- 封存并实现 Gaussian reward-scale prior（或统一 logistic posterior）后重跑严格 baseline stratum。
- 加入 Frozen-exploration matched controls，保持同一 cohort/order/delay。
- 对 full_v2 做 molecule/scaffold paired aggregation、family macro 和 failure decomposition；未完成前不更新 Gate B/C 为通过。
