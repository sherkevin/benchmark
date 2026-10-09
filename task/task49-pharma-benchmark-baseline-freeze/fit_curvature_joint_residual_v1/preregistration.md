# Fit-curvature joint residual CPU diagnostic v1

日期：2026-09-29

状态：CPU development diagnostic；不进入 confirmation 或 A800。

## 问题

已有 joint-success residual 使用常数初始精度 `1.5`。候选方法的数学描述则要求用
fit-only baseline 的 Fisher 曲率作为初始精度。本诊断只检验这一个实现差异：它是否
能减少 selected-only residual 对冻结排序的破坏。

## 固定协议

- 输入：`fingerprint_diagnostic_v1/inputs.json`，722 BBB development rows、33 providers、5 个 scaffold-disjoint outer folds。
- 每个 fold/seed：同一 1024→64 frozen encoder、同一 fit/validation direct system-success head；evaluation 只暴露 selected provider 的 status/label，correctness 只在 release tick 进入更新。
- seed：`0,1`；delay：`0,1,4`；顺序、菜单和 failure/unknown 语义与 `joint_success_cpu_effect_v0` 完全相同。
- 方法：Frozen direct head；常数初始精度 `1.5` 的 scalar residual；fit-curvature scalar residual；fixed-step scalar residual；matched direct Online-SGD。
- fit-curvature 初始化对 provider $a$ 固定为 $I_{a,0}=1+sum_{iin fit}p_{i,a}(1-p_{i,a})$，其中 $p_{i,a}$ 是冻结 direct head 在 fit molecule 上的 probability。这个值只来自 fit 区，不看 evaluation reward，也不调参。
- 在线递推保持原式：$I^+=I+p(1-p)$，$c^+=c+(b-p)/I^+$；只更新被选 provider，失败目标为 0，unknown 不更新。

## 主比较与停止条件

主比较是 fit-curvature residual 相对 Frozen direct head 和 matched direct Online-SGD 的按 fold/seed 配对 system-success accuracy。另报 valid-return accuracy、provider coverage、更新次数、决策/更新耗时和 state drift。

若 fit-curvature 仍不能在至少两个 delay 上超过 Frozen，或只是在一个 delay 上偶然变好，则不把 Fisher 初始化写成方法收益，也不申请 A800。所有负结果保留。

## 可证伪性

- 若 fit-curvature 与常数精度版本几乎一致，说明初始精度不是当前负结果的主要原因。
- 若 fit-curvature 明显恢复 Frozen 且在多个 delay 上保留 matched baseline 的优势，才值得继续审查其动态事件和跨 endpoint 泛化。
- 本诊断不能证明静态回放中存在动态收益；它只检查 warm-start curvature 的机制影响。
