# Laya 表示静态选择诊断

这是 Laya v3 无标签表示之后的下一小步。它回答一个窄问题：在同一批 BBB development 分子、同一 33-provider 菜单、同一 scaffold 划分和同一 0/1 反馈下，Laya 的 candidate-marker 表示是否比已经失败的 Morgan 表示更容易学出 provider 选择信号。

这不是最终方法，也不是在线实验。它只决定表示路线是否值得继续。

## 固定输入

- 标签和五折划分复用 `../fingerprint_diagnostic_v1/inputs.json`，并核对其 SHA-256。
- Laya 输出来自任务 `01a0de81ce4269ebebc6000c40ce9ae2` 的 OSS 前缀：`oss://ai4sci-develop-storage/jingwu/benchmark/task49/laya_features_v3/`。
- 每个分子有 33 个 candidate marker，每个 marker 1024 维；只保留 722 个有发展标签的分子，另外 8 个请求只用于检查输出完整性。

## 模型和对照

对每个 candidate marker 使用共享的 1024→64→1 head；`linear_bce` 使用 1024→1。这样 provider 身份只来自 Laya marker 本身，head 不另加 provider embedding。

- Linear BCE
- MLP BCE
- BT+CLS：同一 shared score 做 winner/loser pair loss，另加 0.2 BCE 辅助项
- Direct property：33 个 marker 的均值进入 1024→64→1，作为标签预测控制
- train-only Best Fixed 和 evaluator-only Oracle

优化器、200 epoch、AdamW、学习率、权重衰减、seed 0/1、validation 选 checkpoint 与 Morgan 诊断完全一致；不调参，不看确认集。

## 继续条件

如果 MLP 或 BT+CLS 相对 train-only Best Fixed 的 OOF accuracy 至少提高 0.02，且每个 seed 至少 4/5 个 outer fold 为正，才进入下一轮在线反馈诊断。否则停止在 Laya 表示上继续堆 head，转向在线状态或 provider 结构。

这个阈值只是开发资源闸门，不是显著性检验，也不构成最终论文结论。
