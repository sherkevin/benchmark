# Provider-transfer replay v2

日期：2026-09-27

## 为什么有 v2

v1 的低秩 $W h(x)$ 版本在真实冻结 BT 先验上三个 delay 都低于 Frozen。这个结果保留为负诊断，不再调它。v2 改测一个更小、不同的候选：只维护 provider-level residual bias，用 fit-only provider 残差协方差把 selected feedback 转移到相关 provider。它不学习 context-dependent $W$，因此不能和 v1 混称。

## 固定输入和方法

- 输入：同一 `fingerprint_diagnostic_v1/inputs.json`、同一 5-fold evaluation 流、同一 matched-SGD 恢复的 `(fold, seed)` 冻结 BT prior 和 64 维 hidden features。
- fit-only residual：$e_{ia}=y_{ia}-\sigma(\ell_a^0(x_i))$，中心化后得到 $G=E^\top E/(n-1)$。
- transfer matrix：$A_{ba}=G_{ba}/(G_{aa}+10^{-8})$。
- 状态：$m_t\in\mathbb R^K$，初值零；反馈释放时做
  $m_{t+1}=m_t-0.1\,\operatorname{clip}(p_t-r_t,-0.5,0.5)A_{:a_t}$。
- 决策：$ell_a^0(x_t)+\operatorname{clip}(m_{t,a},-0.5,0.5)$。
- delays：0、1、4；反馈释放规则仍是 decision tick 加 `1+delay`。
- 对照：Frozen、独立 provider bias、真实 $A$、固定种子 provider 行置换 $A$。

除低秩 v1 已有的固定参数外，v2 不用 evaluation 调参、不调用新工具、不打开 confirmation。记录每题选择、反馈、状态版本、更新次数、准确率、状态范数和真实/置换差异。

## 否定条件

若真实 $A$ 在三个 delay 中至少两个低于 Frozen，或真实 $A$ 与置换 $A$ 没有一致方向差异，则删除 provider-transfer 主候选；不再以换学习率、clip 或窗口补救。
