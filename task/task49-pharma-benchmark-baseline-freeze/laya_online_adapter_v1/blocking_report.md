# Laya / JEV 在线适配审查 v1

日期：2026-09-29  
状态：**在线版本未实现；静态 Laya 诊断已完成并保留为负结果。**

## 1. 先把 Laya 本身说清楚

本地固定快照的 Laya 是一个 typed-decision inference runtime。它的 `Router` 只在语言或 workflow 层选择 checkpoint，不能直接把 33 个药物 provider 当成它的候选模型。药物实验使用的是 candidate-marker 表示和外接选择 head，这是一个明确的任务适配层。

源码中 `Agent.system_one` 带 `@torch.no_grad()`，每次加载后执行 `self.model.eval()`；仓库没有 optimizer、loss、`update()` 或在线 checkpoint 写回接口。`Router` 也只做 checkpoint dispatch，不接收题后 correctness feedback。

## 2. 这对“动态 JEV”意味着什么

当前已有的 Laya forward 和静态诊断证明的是：固定 checkpoint 可以导出表示，但静态 head 没有超过 Best Fixed。它们不能证明 Laya 自己会实时学习。

若把 Laya 接成 selected-only 在线 baseline，至少有三种不同定义，不能混叫一个 Laya baseline：

1. **冻结 Laya + 外接在线 head**：encoder 和 candidate-marker 固定，只更新 provider correctness head。这是可运行、成本最低的适配，但科学上应命名为 `Laya-representation + online head`，不能写成原生 Laya 在线训练。
2. **只更新 scorer/adapter 层**：需要改 runtime，定义 selected-only reward 到 scorer 参数的梯度、学习率、梯度裁剪和 checkpoint 状态。这是自定义 online fine-tuning，必须与 matched Online-SGD/Replay-SGD 共享预算。
3. **更新 Laya encoder 或 LoRA**：计算、显存和遗忘风险都显著增加，而且源码没有现成训练入口；这应作为新的方法实现，不是 baseline 适配的第一步。

## 3. 当前建议

在方法冻结前，不再把“在线 Laya/JEV baseline”当作必须立刻跑的实验。保留：

- Laya 静态 candidate-marker 负诊断，作为 representation control；
- `Laya-representation + online head` 作为候选适配，只有在我们明确要比较“表示是否能被反馈救回”时才预登记；
- 真正的动态 JEV 主方法与 Laya runtime 解耦，避免把 checkpoint dispatch、静态表示和在线 provider 选择混成一个名称。

若继续做候选适配，合同必须固定：冻结 encoder/checkpoint hash、外接 head 参数量、fit-only warm start、selected-only delayed release、失败语义、更新 p50/p95、模型状态 hash 和与 Morgan/BT matched controls 的同预算对照。否则一个“实时 fine-tune Laya”的数字无法说明究竟是表示、head 还是训练预算带来的变化。

## 4. 停止条件

没有明确的外接 head 定义、参数预算和 update 事件合同前，不安装新训练栈、不把 Laya 重新 fine-tune 成另一个模型，也不提交新的 A800 效果任务。当前可复用的结论是：Laya 是已验证的静态表示来源和候选冷启动 backbone，不是现成的在线 JEV 实现。
