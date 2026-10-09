# ADR-0039：confidence-bin residual 没有形成信息优势

- 状态：Accepted
- 日期：2026-09-27
- 依据：`task/task49-pharma-benchmark-baseline-freeze/confidence_bin_screen_v1/report.md`

## 背景

统一 provider residual、低 margin residual 和多种 provider transfer 都没有形成主方法。下一种可能的信息来源是冻结 correctness score 自身的置信区间：如果同一 provider 在不同分数区间有系统性 residual，在线维护 provider×区间状态可能有意义。

## 证据

在 5 folds × 2 seeds 的 fit-only 静态筛选中，区间数 2/3/4/5/8 的 corrected accuracy 分别为 `0.87966/0.88172/0.88240/0.87619/0.87963`，Frozen calibrated 为 `0.88310`。所有粒度的平均 delta 都为负，正 delta 流数最多仅 4/10（严格按 `delta > 0` 统计）。

## 决策

不实现 score-bin residual 的在线版本，不申请 A800。这个结果只说明当前冻结分数的简单区间校准没有可利用的 fit-only 信号；它不否定其它表示或主 Agent 验证/拒答任务。
