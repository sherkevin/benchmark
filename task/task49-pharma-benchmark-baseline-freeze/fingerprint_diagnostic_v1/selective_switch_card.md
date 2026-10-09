# 指纹诊断后的 calibrated switch 分析卡

日期：2026-09-26。状态：已完成的 development post-hoc 分析；不打开确认集，不训练新模型。

## 要回答的问题

MLP 选择器低于 Best Fixed，是因为 Morgan 表示没有可用的 provider 信号，还是因为原始 argmax 过于激进，不能判断何时值得离开固定 provider？

## 候选机制

每个 outer fold 先用 fit 区选出 Best Fixed provider $a_0$。对已有 MLP checkpoint 的每个分子得到各 provider 的 sigmoid score $p_a(x)$，令 $a^*(x)$ 为除 $a_0$ 外 score 最高的 provider。只有

$$p_{a^*}(x)-p_{a_0}(x)>\tau$$

时才切换到 $a^*(x)$，否则继续调用 $a_0$。阈值 $\tau$ 只在该 fold 的 validation 区按 system-success accuracy 选择；同分取更大的阈值，减少不必要切换。outer evaluation 只使用固定好的阈值，不能反向调参。

这里的 calibrated switch 是解释性对照，不是 Proposed。它不使用未选 provider 的 evaluation 输出；validation 的 full-panel 标签只用于开发期阈值选择，与本轮已有静态 head 的训练权限一致。

## 比较和停止条件

- 近邻：同 checkpoint 的原始 argmax MLP、train-only Best Fixed、Oracle。
- 主指标：outer molecule accuracy、相对 fixed 的救回/损害数量、switch rate；仍按 molecule 统计。
- 如果 calibrated switch 达到或超过 Best Fixed，说明当前主要瓶颈是决策校准/保守切换，不能把普通 MLP 的失败包装成 backbone 失败。
- 如果 calibrated switch 仍低于 Best Fixed，说明在当前指纹上仅靠静态校准不足；下一步才值得比较 Laya 表示或新的在线状态。
- 无论结果如何，不把这批 development 分析写成最终泛化证据，也不据此打开多端点确认集。

## 证据要求

脚本只读取已核验的 checkpoint、冻结 inputs 和 outer split；不调用模型 API、不重新训练、不生成新的 provider 预测。保存每个 fold/seed 的阈值、validation 选择过程、outer 每条决策和 source/input hash。

## 结果

MLP seed 0 的 outer accuracy 为 `0.8947`，seed 1 为 `0.9100`；Best Fixed 为 `0.9155`。两种 seed 的 10 个 outer fold 中没有一个 calibrated switch 超过 fixed。switch rate 分别为 `0.6994` 和 `0.3366`。因此“原始 argmax 过于激进”不能解释全部损失，当前 Morgan 表示仍不足以稳定识别值得切换的分子。

结果摘要和逐条决策在 `experiments/logs/task49_fingerprint_diagnostic_20260926/selective_switch_mlp_v1/summary.json` 与 `outer_decisions.jsonl`；执行脚本为 `src/probe_headless/analyze_pharma_selective_switch.py`。
