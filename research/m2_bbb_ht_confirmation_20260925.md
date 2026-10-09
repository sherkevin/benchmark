# BBB 本地 HT 确认

**日期：** 2026-09-25  
**定位：** 冻结回放上的实现和 estimand 检查，不是方法效果实验。

> **状态更新（2026-09-25）：历史 exploratory 记录，不能作为方法确认。** 后续审查发现旧 M2 的 mixture propensity、Freedman 反解和随机 `V/B` 使用均有问题；本文件中的 coverage、MAE 和 gate 数字不再支持安全或性能结论。请以 ADR-0017 和 v5 代码为准。

## 设置

使用 `data/tool_results` 的 730 个冻结分子，排除已登记的 8 个 unresolved reference，得到 722 个可评分分子。首 provider 固定为 CNN，候选为 Morgan 和 MPNN，反馈延迟 1，严格 overlap 的随机 probe，重复 10 个 seed。counterfactual oracle 只在 evaluator 侧从冻结缓存计算，runner 没有读取它。

脚本：[run_m2_bbb_ht_confirmation.py](/Users/jingwu/work/benchmark/src/probe_headless/run_m2_bbb_ht_confirmation.py)。主要日志：

- [30-case v2](/Users/jingwu/work/benchmark/experiments/logs/m2_bbb_ht_confirmation_20260925_v2)
- [722-case、探索概率 0.5 v4](/Users/jingwu/work/benchmark/experiments/logs/m2_bbb_ht_confirmation_20260925_v4)
- [722-case、探索概率 0.8 v5](/Users/jingwu/work/benchmark/experiments/logs/m2_bbb_ht_confirmation_20260925_v5)

## 结果

| split | exploration | Morgan oracle mean | MPNN oracle mean | Morgan MAE | MPNN MAE | gate open |
|---:|---:|---:|---:|---:|---:|---:|
| 30 | 0.5 | 0.200 | 0.100 | 0.120 | 0.093 | 0 / 300 |
| 722 | 0.5 | 0.274 | 0.173 | 0.047 | 0.043 | 0 / 7220 |
| 722 | 0.8 | 0.274 | 0.173 | 0.016 | 0.023 | 0 / 7220 |

三组运行都满足：队列 flush 为空、所有分析 episode 可评分、候选 inclusion propensity 为正。HT 的点估计在 722 分子上已经接近冻结 oracle；但当前 alpha-spent martingale gate 在这三个配置都没有打开 exploitation，原因是严格 anytime 半径在单个分子流上仍然太宽。这不是成功结果，说明“可识别”和“能做出有用决策”是两道不同的闸门。

## 结论

这一步支持把 HT 更新、延迟释放和 propensity 记录作为可运行底座；不支持声称当前 gate 已经能在 BBB 上节省调用。下一步必须在理论不退化的前提下处理置信序列效率（例如预注册批次边界的 time-uniform Freedman 或固定 horizon 的一次性决策），并重新做本地对照。没有解决这个问题之前，不提交 Nebula 性能实验；否则远程结果只会重复“全程探索”。
