# 实验与数据的负结果根因分析 v1

日期：2026-09-29  
范围：只读分析已完成的 BBB development 实验；没有重新训练、没有读取 confirmation、没有提交 Nebula 任务。

证据入口：

- structured CPU 全量回放：`task/task49-pharma-benchmark-baseline-freeze/structured_cpu_effect_v1/report.md`
- joint-success residual：`task/task49-pharma-benchmark-baseline-freeze/joint_success_cpu_effect_v0/report.md`
- 静态选择诊断：`task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/report.md`
- 本次独立重算：`experiments/logs/task49_composition_20260929/root_cause_analysis_v1b/analysis.json`
- 重算脚本：`src/probe_headless/analyze_effect_root_causes.py`

## 1. 结论先说

这次没有达到预期，主要不是某个数字或某个优化器没调好，而是四件事叠在一起：

1. **静态起点本身没有超过强固定策略。** Frozen structured 在 722 个 evaluation 分子上的准确率约为 `0.8982`，fit-only Best Fixed 为 `0.9155`。动态方法从一个已经落后 1.73 个百分点的起点开始。
2. **环境没有真正发生变化。** provider、模型版本、菜单和分子分布在回放中都固定，只有反馈到达时间改变。对一个近似平稳流，在线更新没有明确的变化对象，更新更容易增加方差而不是带来收益。
3. **provider correctness head 的更新过猛。** correctness-only 几乎复现了 structured 的重新排序和损失，truth-only 基本保持 Frozen。这把主要问题定位到 provider head，而不是共享性质 head。
4. **selected-only 的支持太稀疏。** 一个选中事件用一个 64 维向量更新某个 provider 的完整 head；每条流只有约 145 个 evaluation，更新却发生在 33 个 provider 上，而且没有探索。少量被选样本不足以重新估计完整 provider head。

因此，这轮实验真正暴露的是：**当前 benchmark 没有提供足够的动态信号，当前更新规则却足以破坏已有的静态排序。**

## 2. 先看预期和实际差距

### 2.1 预期一：在线反馈应该改善选择

structured 与 Frozen 的 primary accuracy：

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen structured | 0.8982 | 0.8982 | 0.8982 |
| Structured | 0.8857 | 0.8927 | 0.8961 |
| Correctness-only | 0.8878 | 0.8940 | 0.8996 |
| Truth-only | 0.8975 | 0.8989 | 0.8975 |

structured 相对 Frozen 的差值是 `-0.0125/-0.0055/-0.0021`。delay 越大，损失越小；这不是“延迟越大越难学”的表现，反而像是反馈到得慢以后，破坏排序的更新变少了。

joint-success residual 的结果也一样：相对 Frozen 的差值为 `-0.0049/-0.0055/+0.0034`，所有区间都跨 0。它比全向量 structured 更稳定，但没有证明在当前流上存在可利用的在线增益。

### 2.2 预期二：有足够的 provider 选择空间

这个判断只对了一半。

- Best Fixed 为 `661/722=0.9155`。
- evaluator Oracle 为 `720/722=0.9972`。
- Best Fixed 错的 61 个分子中，59 个至少还有另一个 provider 正确，说明可救空间真实存在。

但这 59 个样本是少数错误中的少数。evaluation 中每个分子平均有 `27.83/33` 个 provider 正确，中位数为 32 个；722 个 evaluation 分子里有 490 个至少有 30 个 provider 正确。也就是说，任务的主要难点不是“找一个完全不同的工具”，而是在大量相同正确答案中识别少量 provider 分歧。

这会产生不对称代价：一次错误切换会损害原本已经正确的固定策略；一次正确切换只救回一个少数错误样本。静态诊断也观察到 MLP 选择器相对 Best Fixed 是 `20/34` 或 `24/42` 的救回/损害组合，净变化为 `-14/-18`。

## 3. 直接根因：provider head 在短流上漂移

本次独立分析逐条对齐了同一 fold、seed、delay 下的 Frozen 和动态策略。

### 3.1 correctness 更新造成大规模重排

相对 Frozen 的 provider 选择变化率：

| 方法 | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Structured | 80.3% | 82.1% | 77.9% |
| Correctness-only | 80.1% | 82.8% | 78.1% |
| Truth-only | 3.9% | 3.3% | 3.7% |

这组对照很关键：

- 只更新 q 的 truth-only 几乎不改变选择；
- 只更新 h 的 correctness-only 已经造成和 structured 接近的变化；
- 所以当前主要损失来自 provider correctness head 的在线更新，而不是 q 的恢复标签。

在 changed decisions 上，structured 相对 Frozen 的结果对为：

| delay | Frozen 错 → structured 对 | Frozen 对 → structured 错 | changed-event 净差 |
|---:|---:|---:|---:|
| 0 | 28 | 46 | -18 / 1160 |
| 1 | 26 | 34 | -8 / 1186 |
| 4 | 22 | 25 | -3 / 1125 |

### 3.2 分数变化已经超过原始 margin

Frozen 的 top-1 与 top-2 分数差中位数只有约 `0.0016`。动态更新后，structured 相对 Frozen 的 score vector：

- delay 0：平均 L2 差约 `0.249`，平均最大坐标差约 `0.172`；
- delay 1：平均 L2 差约 `0.234`，平均最大坐标差约 `0.176`；
- delay 4：平均 L2 差约 `0.199`，平均最大坐标差约 `0.162`。

这说明在线更新不是轻微校准，而是在短流里把 provider 排序改写了。一个 provider 被选中后，更新的是该 provider 的完整 `64+1` 维参数；一次反馈就可以改变所有特征方向。当前没有 curvature、正则锚点或信赖域约束。

### 3.3 支持量不足

structured 的每条流只有约 145 个 evaluation，但平均会使用约 8–9 个 provider；同一个 provider 的 selected-only 样本很不均衡。把所有 fold、seed、delay 合并看，structured 被选中的 provider 有的只有 1 个样本，有的超过 900 个样本。这种数据量不能支持对每个 provider 重新估计 65 个参数。

同时，配置明确记录 `exploration_epsilon=0.0`。未被选中的 provider 没有反馈，也没有 propensity 记录。当前更新不是在一个覆盖充分的 bandit 流上做校准，而是在 policy 自己挑出的样本上改写被选 provider。

## 4. 数据为什么不容易产生动态收益

### 4.1 provider 之间高度相关，33 个名字不是 33 个独立信息源

在 722 个 evaluation 分子上，provider 正确性两两相关系数的：

- 均值约 `0.447`；
- 中位数约 `0.449`；
- 90 分位约 `0.679`；
- 最大值约 `0.969`。

尤其是 B3clf 的多个变体来自同一数据和模型家族。它们可以作为不同工具实现，但不能在统计上当作 33 个独立 provider。一次 selected feedback 很难告诉模型“未选 provider 现在是否更好”，因为大量 provider 在同一个分子上给出相同结果。

### 4.2 反馈本身的边际信息很小

当前 selected correctness 的正例率接近 0.9，system failure 只有每个结构化 stream 约 `12/1444`。在这种高基准正确率下，单个 provider 的反馈大多是正例；如果没有明确的漂移、版本变化或探索，在线状态会主要累积“继续相信当前 provider”的信号，难以识别稀有分歧。

### 4.3 evaluation 顺序不是当前主因

五个 outer fold 的 evaluation entity 都按 entity id 排序，但顺序审计已经完成。对 block size 16、32、64，原始顺序的 provider winner switching 分别为每 fold 平均 `7.20、4.00、1.60`，500 次 within-fold permutation null 分别为 `7.48、3.63、1.73`；winner margin 也没有系统高于 null。

因此当前顺序与随机 null 的差异可以解释为有限样本 winner noise，不能支持 order-only change point 或 regime-switching 的动态假设。顺序仍应作为最终实验的控制变量记录，但不把它解释成数据层面的主要根因。

随后做的直接 stateful-policy 重放给出更窄的结论：在 fold 0 / seed 0 的 145 个 evaluation 上，delay=0 时原始顺序的 Structured−Frozen 为 `-0.0414`，19 个随机顺序均值为 `-0.0109`；Correctness-only 也从 `-0.0345` 变为 `-0.0091`。delay=4 没有同样的低尾异常。因此顺序会在某些反馈时钟下放大 provider head 的高方差更新，但它不是跨 delay 稳定的动态事件，也不能单独解释 full CPU effect。证据：`task/task49-pharma-benchmark-baseline-freeze/order_sensitivity_diagnostic_v1/report.md`。

## 5. 哪些假设已经被排除

### 不是简单的 feedback delay 问题

delay 0、1、4 都没有稳定增益，且 delay 4 的 structured 损失反而更小。单纯减少延迟不能解决问题。

### 不是 q 更新单独导致的

truth-only 的选择变化率只有约 3–4%，准确率基本保持 Frozen。主要问题在 h/provider head。

### 不是“再加一个普通静态头”就能解决

Morgan 线性头、MLP、BT+CLS 都低于 Best Fixed；校准 switch 也没有超过 Best Fixed。仅仅更换普通选择头或给 argmax 加阈值不够。

### 也不能说“只要换成更稳的 residual 就会有效”

joint-success residual 的分数漂移更小，delay 4 有正点差，但所有区间跨 0，且 delay 0/1 仍低于 Frozen。它说明“保守更新”值得验证，不说明已经找到方法。

## 6. 下一步应该验证什么

下一轮不应该继续调学习率或换 backbone，而应验证下面三个因果假设：

### G1：去掉漂移是否能恢复 Frozen

固定当前表示和 fit-only 状态，只允许一个 provider 标量 residual 更新，并把 residual 锚在 fit 信息量上。若准确率恢复到 Frozen 附近，说明主要问题确实是高维更新方差。

这项诊断已经完成：将 joint-success residual 的初始精度改为 fit-only Fisher 曲率后，delay 0/1/4 相对 Frozen 的点差为 `-0.00002/-0.00277/-0.00209`，没有稳定超过 Frozen。它说明先验强度确实会放大或压低漂移，但只靠更合理的曲率初始化不能产生动态收益。

### G2：在真实动态事件下，在线更新是否有目标

在不改变 molecule truth 的前提下，预先登记 provider version change、provider retirement 或受控分布 shift。没有这些事件，在线学习只能在静态流上校准，无法证明“动态适应”。

已完成一个受控正控：在真实冻结 BBB 输出上，按 fold 的 fit-only Best Fixed provider 注入中点后的 failure degradation。fit-curvature residual 在 delay 0/1/4 的事件后 accuracy 分别比 Frozen 高 `0.0357/0.0302/0.0371`，并把目标 provider 的事件后选择率降低约 `0.04`。这只证明模块在明确退化事件下有可利用反馈，不证明自然 provider 漂移已经存在；证据：`task/task49-pharma-benchmark-baseline-freeze/controlled_version_event_v1/report.md`。

### G3：没有探索时，selected-only 学习是否可识别

对所有方法统一加入极小、预登记的 exploration，记录 propensity；同时保留 Exploration-Frozen 对照。若没有探索时方法只是在少数选中 provider 上自我强化，则不能把结果解释成 provider 选择能力。

## 7. 当前方法决策

当前证据支持保留的不是某个已经成功的候选，而是一个更窄的设计方向：

> 冻结一个已经验证过的静态选择函数，在线只更新低维、带先验锚定的 provider 校准状态；在有明确 provider/version/分布事件时，才要求它产生收益。

下一候选必须先通过 CPU 证伪卡，至少同时满足：

- 相对 Frozen 不发生系统性下降；
- 相对 matched direct baseline 在至少两个 delay 上有预登记的正差；
- provider coverage、顺序敏感性、失败语义和更新成本全部报告；
- 仍然不能把静态回放上的正点差写成动态 JEV 已成立。

如果加入真实动态事件后仍没有收益，就应停止继续堆叠在线更新，把论文切口改成 selected-only 反馈下的安全校准/拒答机制，或者重新定义任务，而不是继续调 backbone。
