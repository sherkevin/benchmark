# Information screen v1

日期：2026-09-27

## 目的

在申请新的 Nebula 任务前，检查当前 BBB development panel 是否包含两种可以支持动态 JEV 的额外信息：

1. 已选 provider 的反馈能否预测未选 provider 的残差；
2. 已反馈的相似分子残差能否预测后续分子的残差。

如果两者都没有 held-out 信号，selected-only 动态方法没有可靠的信息来源，继续调 Online-SGD、replay 或普通后验没有意义。

## 数据边界

只读冻结的 `fingerprint_diagnostic_v1/inputs.json`：722 个 BBB development 分子、33 个 provider、5 个 scaffold-disjoint outer folds。所有 provider 输出已在输入清单中存在，但 evaluation 阶段的筛选指标只用于离线诊断，不作为在线策略可见信息。

不调用新工具、不训练模型、不读取 confirmation 集。所有 fit 统计只使用当前 outer fold 的 fit 行；validation/evaluation 只用于一次性筛选验证。

## 筛选 A：provider transfer

每个 provider 的 fit-only 基线是其 fit 区正确率 $p_a^0$。残差为 $e_{ia}=y_{ia}-p_a^0$。用 fit 残差的 Pearson 相关矩阵选择正相关 provider 对；在 evaluation 中，用已选 provider $a$ 的残差预测 provider $b$ 是否高于其 fit 基线，报告 ROC-AUC 和残差相关方向的一致性。只统计 evaluation 中目标有正负两类的有序 pair。

同时报告全部有序 pair、fit 正相关 pair、以及置乱 fit pair 的结果。不能把 evaluation 未选输出写入在线状态；这里的全 panel 只用于判断“是否存在可转移的信息”。

## 筛选 B：local residual memory

对每个 fold/provider，用 fit 行构造 Morgan bit-set memory，残差仍为 $e_{ia}$。对 evaluation 分子按 Jaccard 相似度找 top-1 和 top-5 fit 邻居，用邻居残差均值预测 evaluation 残差；报告残差符号命中率、Pearson 相关和 ROC-AUC。另用固定种子的随机 fit 邻居作为噪声对照。

这个筛选只回答 Morgan 表示是否有局部可迁移误差，不等于正式 online memory 效果；延迟、容量和策略选择留到方法合同后再定。

## 解释规则

结果只用于三选一：

- provider transfer 有稳定 held-out 正相关：优先审查低秩/非对角后验；
- local memory 有稳定 held-out 正相关：再审查有界 context residual memory；
- 两者都弱：停止这两条路线，回到新的可观测信息或收束为 benchmark/负结果论文。

本卡不预注册“必须赢过 Best Fixed”的效果结论，也不允许根据结果调阈值后重复筛选。
