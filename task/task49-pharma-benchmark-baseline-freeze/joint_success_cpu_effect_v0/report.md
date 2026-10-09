# Joint-success residual CPU effect v0

日期：2026-09-28  
状态：开发期预筛；不构成 confirmation 或 A800 结果。

## 协议

使用冻结 BBB development 输入、5 个 scaffold fold、2 个 seed、3 个 delay
`{0,1,4}`，共 120 条流、51,984 条 decision/terminal/feedback 事件。每个 fold 的
direct system-success head 只在 fit/validation 上训练；evaluation 只把 selected
provider 的 terminal status/label 送给 policy，correctness 在 release 后才进入更新。

方法包括 Frozen joint-success、曲率锚定 joint residual、fixed-step scalar residual
和 matched direct Online-SGD。没有读取 confirmation，也没有使用 GPU。

证据目录：

`experiments/logs/task49_composition_20260929/joint_success_cpu_effect_v0/run_v2/`

独立合同检查：`verification_v3.json`，状态 `verified`；首条 feedback 提前一个 tick
的篡改样本被 `tamper_rejection.json` 拒绝。聚合脚本不导入 runner，实现了按 fold
配对的 bootstrap；结果在 `aggregate.json`。

## 点结果与保守区间

下面的点差是 `joint_success_residual - comparator`，区间沿用 5 个 scaffold fold
的 paired cluster bootstrap，并保持同一 fold 内的两个 seed 配对：

| comparator | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| Frozen joint-success | -0.004861 [-0.012462, 0.004804] | -0.005527 [-0.018664, 0.005527] | +0.003434 [-0.009722, 0.013103] |
| fixed-step scalar | -0.005541 [-0.013156, 0.002074] | -0.001370 [-0.011034, 0.005536] | +0.003439 [-0.009722, 0.015872] |
| matched direct SGD | +0.005498 [-0.005556, 0.017237] | +0.007620 [-0.004138, 0.019377] | +0.008300 [-0.003506, 0.016652] |

所有区间都跨 0。残差方法没有在两个 delay 上超过 Frozen，也没有达到进入
confirmation 的预设门槛。它在本次 warm start 下相对 matched direct SGD 的正点差，
不能单独证明方法收益，因为 direct SGD 的点结果低于 Frozen，且当前数据没有显示
稳定的环境漂移。

## 决定与下一步

本轮只支持三个事实：

1. joint-success residual 的 selected-only、延迟释放和状态更新可以在冻结 BBB replay
   上完整运行；
2. 它不需要因子化 availability 假设，数学边界比之前的 PAR 候选干净；
3. 在当前 frozen representation、warm start 和静态 evaluation 顺序上，没有超过
   Frozen 的证据。

因此不打开 confirmation，不提交新的 A800 效果任务，不把这个结果写成“动态 JEV
有效”。保留完整事件、验证失败样本和负结果。下一步只有在引入预先定义的 provider
version/menu 事件或新的独立 endpoint 后，才重新审查动态性；否则应把该候选降为
低成本校准对照，继续完善 baseline fidelity 和 benchmark qualification。
