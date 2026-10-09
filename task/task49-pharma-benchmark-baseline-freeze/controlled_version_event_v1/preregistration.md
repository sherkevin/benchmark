# Controlled provider-version event positive control v1

日期：2026-09-29

状态：development positive control；不进入主 benchmark、confirmation 或 A800。

## 目的

当前 BBB 回放中的 provider、菜单、模型版本和分子分布都静态，在线方法没有明确的变化对象。本正控在真实冻结输出上只注入一个预先登记的 provider version degradation，检查动态状态在确实发生变化时能否从 selected-only failure feedback 中离开退化 provider。

## 固定协议

- 基础输入：`fingerprint_diagnostic_v1/inputs.json`，不生成新的工具输出，不读取 confirmation。
- fold/seed/delay：5 个 scaffold outer folds、2 个 seed、delay `{0,1,4}`。
- 每个 fold 的退化 provider：只用该 fold fit 区选出的 Best Fixed provider；不看 evaluation reward。
- 事件时刻：evaluation 顺序中点之后的第一个 tick，`event_tick=floor(n/2)`。事件前保留原始冻结 panel；事件后，只有目标 provider 的 selected terminal 被改为 `failure`，其余 provider 和输出不变。
- 反馈：目标 provider 的 post-event failure 在原有 due tick 释放，policy 只能看到 selected provider 的 failure；不接收 version flag、truth、未选 provider 或完整 panel。
- 方法：Frozen direct head、fit-curvature scalar residual、matched direct Online-SGD。方法共享同一 fit-only warm start、菜单、顺序、delay 和事件。

## 主指标

1. 事件后窗口的 system-success accuracy；
2. 事件后目标 provider selection rate；
3. 从第一次 post-event failure 到目标 provider selection rate 降低的 tick 数；
4. 总体 accuracy、valid-return accuracy、provider coverage 和更新耗时作为辅助指标。

## 解释规则

- Frozen 是静态退化对照，事件后仍可能继续选择目标 provider；它不能被当作动态方法的公平上界。
- 若 fit-curvature 或 matched Online-SGD 在多个 delay 下更快离开目标 provider，同时事件前没有明显损失，说明当前在线状态在“有真实变化”时有可利用反馈。
- 若在线方法仍不离开，说明 selected-only failure 信号或更新速度不足；不再从静态回放分数推断动态有效。
- 这是控制变量正控，不证明真实 provider 会按此方式退化，也不改变主 benchmark 的数据合同。

## 停止条件

事件注入、selected-only、failure payload、due tick 或 reward 重算任一不符合合同，整批作废。不得根据结果修改事件时刻、目标 provider或方法超参。
