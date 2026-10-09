# Joint observation real-panel gate v1

日期：2026-10-05  
状态：**结构预测 gate 通过；policy gate 未通过/尚未测试。**

## 目的

检查真实 BBB development panel 中是否存在 Independent Evidence 无法表达、而小型 Joint Evidence 可以在 scaffold-disjoint held-out rows 上预测的 provider-pair 结构。该实验不是在线策略实验，不打开 confirmation，也不把结果写成 MIRA 效果。

## 数据与协议

- 输入：`task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json`
- 输入 SHA-256：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`
- 722 BBB development rows、33 providers、5 个已有 scaffold-disjoint folds。
- fit 区拟合；evaluation 区只评估；validation 不调参。
- `ok` 输出使用二元 label；provider `error` 统一编码为第三类观测 `⊥=2`，不使用失败行中可能存在的 label。
- Independent Evidence 是每个 provider 在每个 truth 下的三类边际分布；Joint Evidence 是每个 truth 下固定 2-mode mixture-of-products。两者使用完全相同的 fit rows、provider menu、三类观测合同和 fold。
- 报告 full-panel 指标以及 528 个 provider pair 的平均边际指标。Pair likelihood 由联合模型对对应 provider 子集求边际后计算。

## 结果

### Full panel

| model | conditional NLL | posterior Brier | accuracy | ECE |
|---|---:|---:|---:|---:|
| Independent | 14.0317 | 0.1010 | 0.8990 | 0.1018 |
| Joint | 10.1815 | 0.1195 | 0.8727 | 0.1229 |
| Joint − Independent | −3.8502 | +0.0185 | −0.0263 | +0.0211 |

Joint 的 full-panel likelihood 在 5/5 folds 改善，但 posterior Brier、ECE 和分类 accuracy 变差。这说明更好的联合观测 likelihood 还没有自动变成可直接使用的真值 posterior。

### Provider pair

宏平均 Joint−Independent：

| 指标 | 差值 |
|---|---:|
| conditional NLL | −0.0716 |
| posterior Brier | +0.0006 |
| accuracy | +0.0029 |
| ECE | +0.0136 |

Pair NLL 在 5/5 folds 改善；pair Brier 变化接近 0，且没有通过“校准改善”这一更强的要求。预登记结构 gate 因 NLL 方向和 fold 覆盖通过，但 posterior/calibration 不能视为已解决。

## 判定

1. **支持继续 CPU 研究。** 真实 panel 存在可被联合观测模型压缩的 held-out pair likelihood 信号；这比合成 gate 更接近问题本身。
2. **不支持直接进入 planner。** Full-panel posterior 变差，说明当前 2-mode mixture 可能过度自信、把 provider family 相关性当成真值证据，或者需要重新校准 truth prior。不能直接用当前 posterior 计算追加调用价值。
3. **当前方法 claim 不变。** MIRA-Relay 仍是生效规格；MIRA-Joint 仍是候选。E2a 一次调用仍是 primary，H=2 仍是 extension。

## 下一步 CPU 闸门

- 加入 `family-only` 和同容量 `pairwise conditional` control，检查 Joint 的 NLL 收益是否只是 provider family 或额外参数容量；
- 做 held-out posterior calibration：只在 fit 区校准，不用 evaluation 调阈值；
- 按 provider pair 的 failure pattern 分层报告，避免低共同成功 pair 与同家族高覆盖 pair 混在一个平均数里；
- 只有 pair predictive + calibration 通过后，才实现 exact finite-horizon planner，比较 Independent/Joint/Frozen Joint/fixed cascade 的 net utility。

机器结果：`result.json`。协议：`preregistration.md`。实现：`src/probe_headless/joint_observation_real_panel_gate.py`。
