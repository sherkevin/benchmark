# ADR-0046：延迟动态 verifier 风险门停止在 CPU 闸门

- 状态：Accepted
- 日期：2026-09-28
- 取代：无

## 背景

静态 confidence escalation 在同一 BBB development 集上优于相同 fallback 数量的随机追加。为了检验它是否能实时适应，提出了一个最小动态版本：第一、第二 provider 分开维护 confidence-bin Beta 状态；selected-only feedback 按 0、1、4 个 tick 延迟释放；每 32 个决策做一次风险门判断。

## 决议

停止 `dynamic_verifier_v1`，不提交 Nebula A800。它不进入论文主方法，也不继续调 $α$、$δ$、最小支持、batch 或 confidence 分位数。

## 依据

- 动态 pooled final accuracy 在三个 delay 都是 `0.900277`，静态 fit-only q=.8 escalation 是 `0.923823`。
- 动态平均调用次数是 `1.490305`，超过预注册上限 `1.25`。
- 三个 delay 都有 `0/5` 个批次通过风险门；每个 fold 的安全覆盖条件为 `4/5`，但不能抵消准确率和成本失败。
- 独立 recovery 重新计算每条决策、延迟队列、状态计数和 pooled 指标；重复运行逐字节一致。

## 影响

- 这次结果支持“当前样本和目标风险下，严格 confidence 风险保证会过度回退”的结论，不支持把它写成实时学习效果。
- 失败的调用仍保留在分母；无 numeric confidence 的失败只触发追加，不伪造 confidence-bin 观测。
- 主线保留已经通过 CPU 闸门的静态 post-call verifier/escalation；direct selected-only 动态 JEV 继续遵守 ADR-0045 的范围边界。
- 只有找到新的独立可观测反馈，才重新设计动态风险状态；在此之前不申请新的 GPU 资源。
