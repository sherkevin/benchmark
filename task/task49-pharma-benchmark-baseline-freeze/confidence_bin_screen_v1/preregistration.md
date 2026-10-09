# Confidence-bin residual screen v1

日期：2026-09-27

## 问题

校准后的冻结 provider score 是否还保留可利用的非线性 correctness residual？如果有，按 score 区间保存少量状态可能比全局 provider bias 更适合作为实时 JEV；如果 fit-only 的区间校准在 scaffold-heldout 上都没有信息，就不做在线版本。

## 固定做法

对每个 outer fold/seed，用 fit 区完整 provider 面板和已恢复 BT snapshot 得到校准概率 $q_a^0(x)$。分别固定等宽区间数 $B\in\{2,3,4,5,8\}$，计算 fit 区 provider×score-bin 的平均 residual $r-q^0$，直接加回 evaluation 的对应 score 后选择最高 provider。没有读取 evaluation 标签来选 $B$，不做在线更新和探索。

## 判据

如果任一 $B$ 在至少 4/5 outer folds 且两个 seed 上有稳定正的 paired delta，才值得实现 selected-only online calibration；否则把 score-bin residual 记为没有信息优势，不占 A800。
