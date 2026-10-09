# Gated provider-transfer replay v3

日期：2026-09-27

## 动机

v2 的 scalar provider-transfer 有真实几何信号，但仍低于 independent bias。v3 只加一个有明确语义的安全门，不再试新的表示：selected arm 始终按 independent bias 更新；跨 provider 的 off-diagonal transfer 只有在两件事同时成立时才打开：

1. 当前 selected feedback 足够意外，$|p_t-r_t|\ge0.25$；
2. 决策时最高与第二高分数的 margin 不超过 `0.2`。

阈值、学习率、残差 clip 和 score clip 全部固定，不从 evaluation 调参。

## 固定设置

沿用 v2 的输入、恢复的冻结 BT prior/hidden features、fit-only residual transfer matrix、delay `0,1,4` 和 Frozen/independent/真实矩阵/置换矩阵对照。门打开时只对 off-diagonal provider 方向追加一次 transfer；selected arm 的独立更新始终执行。

## 否定条件

若 gated transfer 在三个 delay 中至少两个低于 independent bias，或真实矩阵与置换矩阵无一致方向差异，则 provider-transfer 主线停止，不再通过学习率、阈值或 clip 搜索补救。
