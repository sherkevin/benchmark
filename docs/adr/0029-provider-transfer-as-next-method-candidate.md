# ADR-0029：把 provider 残差转移作为下一方法候选

- 状态：Accepted
- 日期：2026-09-27
- 依据：Task49 information screen v1

## 背景

同一反馈合同下，Frozen、selected-only BayesianRouter、Online-SGD 和 Replay-SGD 都没有超过 train-only Best Fixed。普通更新只改所选 provider 的独立状态，没有利用未选 provider 或相似分子的可迁移信息。

我们在 5 个 scaffold-disjoint outer folds 上做了零训练筛选。每个 fold 只用 fit 区估计 provider 残差相关性，再在 evaluation 上测试相关 provider 的残差转移；没有把 evaluation 的未选输出写进任何在线状态。

## 证据

- fit 正相关 provider pair 的 held-out transfer AUC 五折均为正，均值 `0.7366`；fit/evaluation pair-correlation 均值 `0.8060`。
- Morgan top-5 近邻残差 AUC 均值 `0.6248`，固定种子随机邻居均值 `0.4691`，说明 local memory 也有信号，但其新颖性和 scaffold 泛化风险更高。
- stream drift 的分块 winner 变化与 within-fold permutation null 接近，因此下一方法不以 order-only change point 为前提。

## 决策

1. 下一方法候选优先使用 fit-only provider 残差关系，让一次已选 provider 的反馈在受约束的情况下更新相关 provider。
2. 第一版只实现最小的非对角/低秩状态和 Frozen fallback/置信门；不宣称发明相关臂后验、Kalman 更新或低秩分解本身。
3. local residual memory 先作为轻量对照/消融，不先作为主贡献。
4. 在 transfer replay 通过预先登记的硬否定条件前，不申请新的 A800 完整方法实验；confirmation 集保持关闭。

## 后果

候选方法新增了真实的信息来源，但也引入错误传播风险。方法合同必须说明 provider 关系只来自 fit 区、反馈延迟如何进入状态、协方差或低秩状态的数值稳定条件，以及何时退回 Frozen。最终效果仍需在统一 runner 中与 Best Fixed、Frozen、Online-SGD、Replay-SGD 和 BayesianRouter-style 同表验证。

证据：`task/task49-pharma-benchmark-baseline-freeze/information_screen_v1/report.md`、`experiments/logs/task49_information_screen_20260927/analysis_v1.json`。
