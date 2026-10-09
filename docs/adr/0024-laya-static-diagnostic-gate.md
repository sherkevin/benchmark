# ADR-0024：Laya 静态表示诊断不进入主方法

状态：Accepted

日期：2026-09-27

## 背景

当前主 benchmark 是 722 个带 BBB 参考标签的分子、33 个具体 provider 的直接选择任务。Morgan 表示上的开发诊断没有超过 train-only Best Fixed，因此需要一次预登记的 Laya candidate-marker 诊断，判断表示替换是否能解决静态选择瓶颈。

Laya 诊断使用相同的外层 scaffold 划分、同一 provider 顺序、相同训练预算和四类 head。继续条件是 MLP 或 BT+CLS 相对 fixed 提升至少 0.02，且两个 seed 各自至少 4/5 个 fold 为正。

## 决定

Laya candidate-marker **不作为当前静态 selector 的主表示继续调参**。保留已导出的 Laya 表示，只有在预先登记的在线反馈实验中把它作为可选输入，并与同一反馈合同下的现代 baseline 共同比较时，才继续使用。

## 依据

A800 任务 `01a0debb7da647dcee0b42564fcc8ced` 已完成并通过独立恢复。具体 provider 的 Best Fixed 为 0.915512，Oracle 为 0.997230；Laya 的最佳结果是 MLP seed 0 的 0.885042，低于 fixed 0.030471，只有 1/5 个 fold 为正。其余方法和 seed 也都低于 fixed。40 个 checkpoint、5,776 条评价和所有 Laya shard 均已用独立脚本和 NumPy 前向核对。

## 后果

- 不能把 Laya 诊断写成动态 JEV 或在线学习效果。
- 下一步优先补齐 PILOT、BayesianRouter、Frozen/Online-SGD/Replay-SGD 的离线和在线适配，并维持相同的分子、provider、延迟反馈和评价口径。
- 继续使用 Laya 必须回答一个不同的问题：在线反馈是否能修正其静态表示的不足；这个问题需要单独的 claim、最近邻和否定条件。
- 最终 confirmation 集保持关闭。
