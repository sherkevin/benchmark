# ADR-0043：调用后置信度阈值复核通过 CPU 闸门

状态：Accepted

日期：2026-09-27

## 背景

调用前 direct provider selection 的多条 selected-only 动态更新候选都没有超过强固定策略。此前信息筛选发现 provider 返回的 numeric probability 在调用后能预测 `system_success`，但 evaluation 内排序本身不能当作部署阈值。

## 决定

保留 post-call confidence verifier/refusal 作为下一条方法支线。每个 outer fold 只用 fit 区拟合 $c=2|p-0.5|$ 的固定 coverage 0.8 分位阈值，evaluation 按阈值接受或拒答；随机拒答是同 coverage 对照。该支线与 direct pre-call selection 分开报告。

## 证据

五折 scaffold-heldout 回放中，verifier 的 pooled accepted accuracy 为 `0.961606`，随机拒答为 `0.915078`，差值 `+0.046528`，五个 fold 全部更高。独立恢复器重新计算 provider、阈值、接受数和 accuracy，结果一致。证据和报告见 `task/task49-pharma-benchmark-baseline-freeze/postcall_verifier_threshold_v1/` 与 `experiments/logs/task49_postcall_verifier_threshold_20260927/run_v1/`。

考虑置信度并列导致 verifier 实际 coverage 与目标 coverage 不完全相同，按 verifier 每折实际接受数匹配的随机敏感性对照 pooled accepted accuracy 为 `0.916230`，结论不变。

## 边界和后果

这只证明调用后返回值带有复核信息，不证明调用前选择收益，也不证明在线更新收益。拒答不计作 direct-selection 正确答案。下一步可以在 CPU 上测试 selected-only delayed feedback 下的在线校准/风险状态；只有 CPU 预登记闸门通过后才占用一条 Nebula A800 任务。confirmation 集保持关闭。
