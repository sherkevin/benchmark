# Confidence-gated second-tool verification v1：结果

日期：2026-09-28

## 任务

每个 outer fold 先在 fit 区选出第一 provider 和第二 provider。第一 provider 调用后返回 numeric probability，若 confidence 低于 fit-only 的 0.8 coverage 阈值，就追加第二 provider，并采用第二 provider 的输出；否则直接采用第一 provider。两次调用都计入成本。

这个回放检验的是“选择后是否需要验证”的 agent 流程。它不把第二次调用计作一次 direct selection，也没有在线更新。

## 结果

| 指标 | Best Fixed | confidence escalation | 随机追加调用 |
| --- | ---: | ---: | ---: |
| pooled accuracy | 0.915512 | **0.923823** | 0.904432 |
| 相对 Best Fixed | — | **+0.008310** | -0.011080 |
| 平均调用次数 | 1.000 | **1.206** | 1.206 |
| fallback rate | 0 | **0.206** | 0.206 |
| 不低于 Best Fixed 的 folds | — | **4/5** | — |
| 高于随机追加的 folds | — | **4/5** | — |

两工具 oracle 为 `0.966759`，只作上界。第二 provider 单独使用的 pooled accuracy 为 `0.889197`，说明收益来自 confidence 决定何时使用第二工具，不是第二工具本身更强。

预登记闸门是：相对 Best Fixed 提升至少 `0.005`、平均调用次数不超过 `1.25`、至少 `4/5` folds 不下降；相对同 fallback 数量的随机追加至少 `4/5` folds 更高且 pooled 提升至少 `0.003`。结果满足闸门。

固定数量随机 null 的 500 个 seed 中，random pooled accuracy 均值为 `0.911191`，标准差 `0.005553`，只有 `2.2%` 不低于 confidence escalation。完整 null 在 `experiments/logs/task49_confidence_escalation_20260928/run_v1/random_null_sensitivity.json`。

## 解释边界

- 没有新 provider 调用；所有输出来自冻结 BBB cache；confirmation 关闭。
- 这是一个调用后置信度驱动的追加验证结果，不是 selected-only 在线学习结果。它说明 agent 可以用一次调用的结果决定是否付出第二次调用成本。
- 运行前的正式协议固定使用上一项 verifier 已冻结的 0.8 coverage 阈值。此前曾做过未登记的 `.5/.8/.9` 快速屏幕，结果保存在 `experiments/logs/task49_confidence_escalation_20260928/run_v1/exploratory_screen.json`，不把该屏幕当作确认性结果。
- 下一步才是把阈值、第二 provider 可靠性和延迟反馈做成 selected-only 动态状态；必须先写清状态更新和 CPU 停止条件。
