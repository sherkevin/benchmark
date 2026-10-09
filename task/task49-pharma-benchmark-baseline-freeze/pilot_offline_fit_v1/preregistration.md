# PILOT offline prior fit v1

日期：2026-09-27

## 目的

把已有 `PilotOnlineKernel` 的随机 fixture prior 替换为只由 outer-fold fit 区得到的医药任务先验，使 PILOT 可以进入统一 baseline runner。这个任务只验证离线适配和状态接口，不比较 development 效果，不读取 confirmation。

## 适配边界

原论文的 preference data 是 query、LLM pair 和人类偏好。本任务没有外部偏好数据，因此把 `system_success` 面板转换成明确的 correctness preference：同一分子上正确 provider 胜过错误 provider；多个正确 provider 之间不造胜负，完全错误行跳过。provider 是 33 个具体工具，query 是 Morgan radius=2、1024-bit 分子指纹。

## 固定两阶段 fit

1. **Query projection**：用 fit 区构造 provider-conditional triplets。anchor 和 positive 都是同一 provider 正确的分子，negative 是该 provider 错误的分子；每个 provider 每个 anchor 用循环索引选一个 positive 和一个 negative。优化 64 维线性 projection 的 cosine triplet loss，margin=0.2，Adam，lr=0.001，weight decay=0.01，100 个 full-batch epochs，seed=20260927。
2. **Provider preference prior**：冻结 projection，初始化 33 个 64 维 provider vectors 为零；对每个 fit 分子的所有 correct–wrong provider pairs 优化 `softplus(cos(query, wrong)-cos(query, correct))`，加 `0.01` L2，Adam，lr=0.001，100 个 full-batch epochs，同一 seed。

query projection 输出在送入 online kernel 前做单位化；provider prior 保留原向量，online kernel 的 cosine 读取负责归一化。每个 provider 的 ridge precision 使用 fit 区 accuracy 的倒数，accuracy=0 时固定为 `1e6`，并记录具体值。

## 不能声称的内容

这不是原论文官方代码的逐行复现，也没有使用原论文的文本 preference 或 cost policy。它是按论文两阶段思想对二元科学工具正确性任务的适配；正式结果中必须单独标为 `PILOT-adapted`，并与 zero-prior LinUCB、BayesianRouter-style、matched SGD 和 Frozen 同表。

## 失败条件

如果 fit 输入含 evaluation 标签、triplet 构造在 seed 间不确定、projection/prior 出现非有限值、或 online kernel 接口不能重放同一 prior，则停止，不跑远程任务。
