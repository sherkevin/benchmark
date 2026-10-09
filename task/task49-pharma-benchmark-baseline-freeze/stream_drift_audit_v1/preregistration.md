# Task49 stream drift audit v1

日期：2026-09-27

## 目的

在提出 change-point 或 regime-adaptive JEV 前，先检查当前 BBB development evaluation 流是否存在真实可观测的 provider correctness 漂移。这里使用已有缓存和完整 evaluation panel 做诊断，不训练、不调用新 provider、不打开 confirmation。

## 输入

`fingerprint_diagnostic_v1/inputs.json`，SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`；5 个 scaffold-disjoint outer evaluation 流，33 个 provider。

## 诊断

对每个 fold 的原始 evaluation 到达顺序按 32 个样本分 block，计算每个 provider 的 block accuracy、block oracle、global fit-only Best Fixed，以及相邻 block 的最佳 provider 是否改变。再报告前半/后半 provider accuracy、最佳 provider margin 和 block 间 rank correlation。

这些统计只回答“顺序里是否有可以被在线反馈利用的 regime 信号”，不把全 panel 结果当作 policy 可见信息，也不宣称构成效果实验。

## 裁定

- 如果最佳 provider 在 block 间很少改变且 block margin 小，动态选择缺乏任务内信息来源；先收窄故事或改 benchmark 定义。
- 如果最佳 provider 稳定切换且切换超过 noise floor，下一方法优先研究 change-point / shared-error state；仍需和 Frozen、Best Fixed、Online-SGD、Replay-SGD 比较。
