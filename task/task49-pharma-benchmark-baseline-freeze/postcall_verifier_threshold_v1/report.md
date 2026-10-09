# Post-call verifier threshold replay v1：结果

日期：2026-09-27

## 这次验证什么

这一步只验证调用后的复核能力。每个 outer fold 先在 fit 区用 `system_success` 选出 Best Fixed provider，再用同一 fit 区返回的 numeric probability 拟合置信度阈值。evaluation 只读取 provider 已经返回的 probability，按固定阈值接受或拒答；evaluation 标签只用于报告结果，不参与阈值选择。

置信度定义为 $c=2|p-0.5|$，fit 目标 coverage 固定为 `0.8`。随机拒答对照使用相同 evaluation 数量和目标 coverage，随机种子为 `20260927`。

## 结果

五个 outer fold 都选择了 `admetlab_3_0`。阈值完全由各自 fit 区确定，evaluation 实际接受率因为置信度并列而不同：`0.6621/0.6759/0.9167/0.8889/0.8264`。

| 指标 | 置信度 verifier | 随机拒答 |
| --- | ---: | ---: |
| mean accepted accuracy | 0.960185 | 0.915127 |
| pooled accepted accuracy | 0.961606 | 0.915078 |
| verifier 高于随机的 folds | 5/5 | — |
| 预登记 pooled gain | +0.046528 | — |

预登记闸门要求至少 4/5 个 fold 的 accepted accuracy 高于随机，且 pooled gain 至少 `0.02`。结果满足两个条件。

为排除置信度并列造成的实际 coverage 不同，独立恢复器又按 verifier 每个 fold 的实际接受数抽取随机对照。这个更严格的对照 pooled accepted accuracy 为 `0.916230`，仍低于 verifier `0.961606`。

调用后拒答也降低了接受结果中的错误率：各 fold 的 rejected error rate 为 `0.1837/0.2553/0.2500/0.3125/0.4000`。这些数只描述被拒绝样本，不把拒答计作 direct-selection 的正确答案。完整逐 fold 结果在 `experiments/logs/task49_postcall_verifier_threshold_20260927/run_v1/results.json`。

## 独立复核与边界

- 独立复核器重新从冻结 inputs 和 `PredictionCache` 计算 provider、阈值、接受数和两种 accepted accuracy，结果逐 fold 与结果文件一致；同时做了实际接受数匹配的随机敏感性对照：`experiments/logs/task49_postcall_verifier_threshold_20260927/run_v1/recovered.json`。
- 输入 SHA 为 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`；没有新 provider 调用，没有 confirmation 数据，没有 Nebula 任务。
- 这是 post-call verification/refusal 的静态 fit-only 证据，不能证明 direct pre-call provider selection 变好，也不能证明动态 JEV 在线更新有效。
- 当前信号足以进入下一条 CPU 候选：在同一 confidence 状态上维护 selected-only delayed feedback 的在线校准或风险状态，并与 Frozen verifier、普通 selected-only SGD、随机拒答做匹配回放。若该候选 CPU 闸门不过，不申请 A800。
