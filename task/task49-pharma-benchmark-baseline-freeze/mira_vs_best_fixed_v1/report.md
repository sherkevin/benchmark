# MIRA-FamilyGuard 与 fit-only Best Fixed 的逐样本差额分析

日期：2026-10-02
状态：development 只读 post-hoc 分解；没有重跑策略，没有读取 confirmation，不是新的效果实验。

## 这份分析要回答什么

common-stratum 主表显示 MIRA-FamilyGuard 的 task success 为 0.907664，低于 fit-only
Best Fixed 的 0.915512。只看两个均值无法判断损失来自哪里。本分析把同一条封存
FamilyGuard 回执中的每个 (fold, seed, delay, molecule) 与该 fold 的 fit-only 固定赢家
逐条对齐，区分：

- **harmed**：Best Fixed 正确、FamilyGuard 错误；
- **rescued**：Best Fixed 错误、FamilyGuard 正确；
- **switch-away**：FamilyGuard 选择的 provider 不等于 fit-only incumbent；
- **runtime failure**：FamilyGuard terminal 的 valid_return=0。

Best Fixed 的计算严格复用 run_structured_cpu_effect_v1._fit_best_provider：每个 fold
只在 fit 分子上按 system_success 均值取 argmax。五个 fold 的 incumbent 都是
admetlab_3_0。因此它是 development fit-only anchor，不是 evaluator Oracle。

机器结果：[mira_vs_best_fixed_v1.json](../../../experiments/logs/task49_common_stratum_family_guard_20261001/full_v1/mira_vs_best_fixed_v1.json)
分析配置：[mira_vs_best_fixed_v1_config.json](../../../experiments/logs/task49_common_stratum_family_guard_20261001/full_v1/mira_vs_best_fixed_v1_config.json)
分析脚本：[analyze_mira_vs_best_fixed_v1.py](../../../src/probe_headless/analyze_mira_vs_best_fixed_v1.py)

## 总体结果

| 指标 | 数值 |
|---|---:|
| paired cells | 4,332 |
| MIRA-FamilyGuard correct | 3,932 (0.907664) |
| Best Fixed correct | 3,966 (0.915512) |
| MIRA − Best Fixed | −34 cells (−0.7849 pp) |
| both correct | 3,744 |
| both wrong | 178 |
| harmed：固定正确、MIRA 错 | 222 |
| rescued：固定错误、MIRA 对 | 188 |
| switch-away | 3,619 (83.54%) |
| incumbent selected | 713 |

点差的守恒关系为 188 − 222 = −34。因此当前损失不是“没有可替代的工具”：确实
有 188 次救回了固定赢家做错的分子；净负差来自错误切换次数略多于救回次数。

按 molecule 先在六个 seed/delay cell 上取均值，再按非手性 Bemis--Murcko scaffold
cluster bootstrap（5,000 次，seed=20261002），MIRA−Best Fixed 的 95% 区间为
[-2.7932,+1.5659] pp，跨 0。这个区间只表达当前 development cohort 的不确定性，
不能把点估计写成显著劣化，也不能把跨 0 写成没有问题。

## 损失来自什么

### 1. 主要是错误切换后的错误预测，不是运行故障

MIRA 的 400 次错误由 390 次 valid-return wrong label 和 10 次 runtime failure
组成。与 Best Fixed 的 222 次 harmed 中：

- 212 次是选到一个可正常返回但标签错误的 provider；
- 10 次是选到运行失败的 provider；
- 222 次 harmed 全部发生在 switch-away 后；选择 incumbent 本身不会制造相对
  incumbent 的新错误。

这说明第一修复对象是 **incumbent-safe switching**：在没有足够证据表明替代工具更可靠
时，不应因为短期 score 或 family 状态把 admetlab_3_0 换掉。单纯增加普通 SGD 更新、
回放或 Gaussian posterior，不能直接解决这个错误切换问题。

### 2. 误伤集中在少数 provider/family

| 被选 family | harmed | 其中 wrong label | 其中 runtime failure |
|---|---:|---:|---:|
| b3clf | 167 | 157 | 10 |
| lgbm_bbb | 52 | 52 | 0 |
| graphb3 | 2 | 2 | 0 |
| fpadmet | 1 | 1 | 0 |

provider 级别上，主要误伤来自 b3clf_xgb_classic_adasyn（73 次，66 个错误标签、
7 个运行失败）、b3clf_xgb_borderline_smote（66 次，63 个错误标签、3 个运行失败）
和 lgbm_bbb（52 次错误标签）。这些 provider 不是“永远不能用”：它们同时贡献了
rescue，说明问题是 **何时切换**，不是简单删除某个 family。

### 3. 救回也有结构，不能只看负例

188 次 rescue 中，lgbm_bbb 贡献 63 次，b3clf family 贡献 120 次，graphb3 贡献
5 次。故下一版不能把 incumbent 固定成永远不可切换的规则；需要一个带置信度/安全边界
的切换机制，让有足够证据的替代工具承担收益，同时把 222 次误伤压下去。

## 按 fold 和 delay 的观察

fold 级 delta 为：fold0 −1.72/−1.72/−2.07 pp、fold1 +1.38/+1.72/+1.03 pp、
fold2 −3.13/−3.13/−3.13 pp、fold3 −1.39/−1.39/−1.39 pp、fold4
+1.04/+1.04/+1.04 pp（delay 0/1/4）。fold 间方向不一致，fold2 的负差最大。
该分析没有证明 delay 本身造成差异；它只说明当前 FamilyGuard receipt 中的延迟变化没有
消除 incumbent-switch 问题。

## 对下一轮方法的约束

下一轮候选应以“安全切换”作为可证伪机制，而不是再增加一个无约束的在线更新器：

1. **保留 incumbent**：先验动作默认为 fit-only Best Fixed，替代 provider 只有在
   后验优势超过切换 margin 且证据量达到门槛时才可切换；
2. **分开 correctness 与 runtime hazard**：valid-return wrong label 和 runtime
   failure 不能共用一个 reliability count；
3. **按 provider/family 记录证据**：family gate 用于抑制重复试错，但不能覆盖 provider
   的历史正确性；
4. **把 harmed/rescued 作为主要诊断指标**：除 task success 外，固定赢家被误伤次数、
   rescue 次数、switch rate、runtime failure 和 scaffold-cluster CI 必须预登记；
5. **下一实验的否定条件**：如果安全切换在同一 common-direct-prior 合同下不能把
   harmed 降到当前 222 次以下，同时不让 rescued 明显下降，则停止把该机制写成主
   方法收益，保留为失败诊断。

这些结论只来自已封存 development 回执，不能外推到 hERG/AMES、model-fresh provider、
自然版本漂移或 confirmation。
