# Exploration-matched controls

状态：`verified`，用于解释 full_v2 的动态差异，不是独立方法优越性结论。

## 设置

沿用 full_v2 的 5 folds × 2 seeds × 3 delays、33 providers、selected-only feedback 和同一随机种子公式。只增加两个不消费模型更新的控制：

- `greedy_no_update`：与 Greedy 相同的 `Σ0=0.02I`、`alpha=0.5` 和 action rule，feedback 只释放和记账，`learn=False`。
- `bayesian_no_update`：与 Bayesian 相同的 prior、noise、按 stream 区分的 Thompson 随机数，`learn=False`。

原始回执：`experiments/logs/task49_common_stratum_exploration_controls_20260930/full_v1/`。
独立验收：`full_v1/verification.json`，60 streams、25,992 events、4,332 decisions，verified。

## 描述性结果

| 方法 | correct / decisions | accuracy | failed returns |
|---|---:|---:|---:|
| greedy_no_update | 3,906 / 4,332 | 0.901662 | 33 |
| bayesian_no_update | 3,829 / 4,332 | 0.883887 | 27 |
| full_v2 greedy_online | 3,844 / 4,332 | 0.887350 | 23 |
| full_v2 bayesian_online | 3,749 / 4,332 | 0.865420 | 25 |

这组数字提示：在当前封存先验和回放顺序下，在线更新没有带来 pooled accuracy 提升；Greedy 的差异不能简单归因于探索，Bayesian 的 online 版本低于相同探索规则的 no-update control。它仍不是统计结论，因为分子在方法/seed/delay 间重复，必须做 molecule/scaffold paired aggregation。

## 对方法设计的影响

1. “动态更新有效”不能靠 Frozen vs online 单一对照证明；no-update exploration controls 已成为主实验矩阵的一部分。
2. 当前 Gaussian 组还存在 logit prior 与 reward-scale 的语义问题；该控制只隔离 update，不修复 scale mismatch。
3. 下一步先做统一 logistic posterior 或封存 reward-scale prior，再重复同一 control 矩阵；同时报告 valid-return conditional correctness、runtime failure 和 provider-family macro。
