# Confidence-bin residual screen v1：结果

日期：2026-09-27

## 结果

5 folds × 2 seeds 的 fit-only 区间校准结果：

| 区间数 | Frozen calibrated | bin residual | delta | 正 delta 流数 |
| ---: | ---: | ---: | ---: | ---: |
| 2 | 0.88310 | 0.87966 | -0.00344 | 2/10 |
| 3 | 0.88310 | 0.88172 | -0.00138 | 2/10 |
| 4 | 0.88310 | 0.88240 | -0.00069 | 4/10 |
| 5 | 0.88310 | 0.87619 | -0.00691 | 2/10 |
| 8 | 0.88310 | 0.87963 | -0.00347 | 4/10 |

机器结果：`experiments/logs/task49_confidence_bin_screen_20260927/results_v1.json`。

这里的“正 delta 流数”按严格的 `delta > 0` 统计；`delta = 0` 不算正。结果文件由正式脚本生成，并用相同输入重复运行后逐字节比较通过。

## 裁定

不实现 score-bin residual 的在线版本。fit-only 静态校准没有形成正信号，任何在线版本都只能再引入 selected-only 稀疏和延迟，不能把它当作下一条主候选。
