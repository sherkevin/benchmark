# 冻结分数基准恢复：development-only

日期：2026-10-07。分析前固定；不改变方法或 confirmation。

## 要解决的问题

前一诊断发现 401/2166 次同历史 shadow 动作差异，但 class-balanced logistic 的分数基准与正确率不一致，不能把全部组成项叫作在线选样偏差。本轮恢复 fit 参考分数均值，区分两者，决定下轮中心化对照是否需要先校准。

该工作对应方法论 E3；最近已发表 comparator 仍是既定 BayesianRouter-adapted，本轮不重跑效果比较。对照参考为原始 Frozen 和原始 MIRA，同一 priors、特征、反馈和计数；不产生新策略准确率。

## 固定输入和重建规则

- 输入仅 `fingerprint_diagnostic_v1/inputs.json` 的 722 个 development 分子与原 5-fold fit/evaluation indices；其 SHA-256 为 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 只读 `experiments/logs/task49_provider_fresh_menu_20260930/cpu_v3/checkpoints/` 的四个原始 checkpoint；源码使用现有 `load_checkpoint_predictions` / `fit_prior` / `selector_feature`，不得调用包含 confirmation 的 `main()`。
- 精确复用原训练参数：C=1、liblinear、class_weight=balanced、max_iter=2000、random_state=20261006。这是原 fit 的确定性重建，不是新超参搜索。
- checkpoint、helper 源码、环境、命令、本卡和原始 development 日志 hash 在任何 inference 前写 config；完整缓存 development 概率、每折 prior、每个 fit/evaluation 的分数与 fit rate，方便下轮直接复用。
- 本地 CPU，一次重建，0 API；预算 180 秒，不申请 Nebula。原效果和原始日志只读，不读取 confirmation manifest 或数据。
- 对齐前一诊断 `experiments/logs/task49_residual_decomposition_20261007/v1/decisions.jsonl`，其 hash 从已封存 summary 核验。

## 数学和输出

令 $m_a$ 为同一 frozen head 在该折 fit 样本上的 sigmoid 分数均值。只读真实 selected 历史，把组成项拆成

$$\frac{\sum_i(q_i-\bar r_a)}{20+n_a}=\frac{\sum_i(q_i-m_a)}{20+n_a}+\frac{n_a(m_a-\bar r_a)}{20+n_a}.$$

实例：fit 均分 0.5、fit 正确率 0.8、20 条 selected 均分 0.5 时，左侧 -0.15；相对 fit 的组成变化为 0，基准差项 -0.15。这是示例，不是假造实验数据。

1. 先核重建 logits 与前一诊断全部 Frozen logits，fit rates 与原 summary，误差阈值 $10^{-8}$；任何不符停止解释，保留失败。
2. 记录 20 个 fold/provider 的 $m_a,\bar r_a$；保存 fit 级与 OOF 级分数，分开命名，不以 fit 校准误差作泛化成绩。
3. 对原 2166 条决策和 8664 个 provider 状态记录新两项；统计与前一诊断相同分母的 signed mean、mean absolute、max absolute、各 fold/delay。恒等式误差要求 $10^{-10}$。
4. 在固定观察历史上分别移除“相对 fit 分数组成变化”或“fit 基准差”后计算 shadow argmax差异；不得计算 shadow accuracy、闭环收益、CI或任一候选排名。

## 否证及下一步

如果原分数或 fit rates 无法重建，本轮仅报告 provenance/reconstruction failure；不拟合新 prior 逼近日志。若 fit 基准差明显参与动作，下一轮把校准与在线中心化分成独立因素；若没有可见作用，则保留当前基准，优先检查样本组成。此处没有预设胜出方案，不据结果回调原确认方法。

本卡只授权这次诊断。之后的校准拟合与闭环对照需要新的冻结卡，所有变体使用匹配的 Frozen；新的效果确认需要未用于选择方案的数据。
