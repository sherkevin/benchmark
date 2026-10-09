# Laya 静态表示诊断：预登记

日期：2026-09-27。阶段：实验卡和读取器准备中，尚未占用 GPU。

## 问题

Morgan 指纹的直接 provider selector 没有超过 train-only Best Fixed。这里固定所有训练条件，只替换输入表示：每个请求的 33 个 Laya candidate marker 各自进入一个共享 head，观察表示本身是否携带可泛化的 provider 选择信号。

最近参照是 `fingerprint_diagnostic_v1` 中的 Linear BCE、MLP BCE、BT+CLS、Best Fixed 和 Oracle。它们使用同一 722 分子、33 provider、五个 scaffold-disjoint outer fold、每 fold 内 20% scaffold-disjoint validation、两个 seed。Laya 诊断不把旧 Morgan 数字重新算成新结果。

## 信息权限

训练只接触 fit 分子的 33 个工具成功/失败标签；validation 只用于选 epoch；evaluation 只在最后打分。Laya 表示本身在无标签任务中先前已经固定导出，不能读取参考真值、工具输出或正确性标签。8 个没有 BBB 参考标签的请求只用于验证 Laya 输出完整，不进入任何 loss 或 score。

## 模型

令 $h_{i,a}\in\mathbb{R}^{1024}$ 为分子 $i$ 和 candidate $a$ 的 Laya marker。Linear/MLP/BT+CLS 都计算一个共享的 $s(h_{i,a})$，得到 33 个 candidate score；不加入额外 provider embedding，不使用 Laya 原始 choice logits 作为特征。BT+CLS 的 pair loss 和 0.2 BCE 辅助项沿用 Morgan 诊断。Direct property 只把 $\frac{1}{33}\sum_a h_{i,a}$ 输入一个 1024→64→1 控制头。

所有优化器设置固定为：AdamW，lr=0.001，weight decay=0.01，full-batch，200 epochs，seed 0/1，validation loss 最小的最早 epoch。没有搜索维度、学习率、温度、gate 或特征归一化。

## 继续和否定

继续条件：MLP 或 BT+CLS 相对 Best Fixed 的 OOF accuracy 至少 +0.02，且两个 seed 各自至少 4/5 个 outer fold 为正。否则停止把问题归因于 backbone，转而检查在线状态、provider 结构或标签资格。无论正负，都保留完整逐 epoch、逐分子 evaluation 和 checkpoint 证据。

这批不是在线更新实验，也不打开 frozen confirmation。它只能决定下一步是否值得在 Laya 表示上做 selected-only feedback 诊断。
