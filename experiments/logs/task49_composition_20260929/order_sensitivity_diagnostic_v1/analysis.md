# Online policy order-sensitivity diagnostic v1

## 核验

本次固定 fold 0 / seed 0，20 个顺序、3 个方法、2 个 delay，共 120 条 stream。独立脚本从原始 terminal 事件和冻结输入重算每条 selected reward；输入 hash、decision/terminal 对齐和报告 accuracy 均一致。

## 结果

| delay | 方法 | 原始顺序 accuracy | 19 个随机顺序均值 | 原始减均值 | 原始相对随机顺序分位 |
|---:|---|---:|---:|---:|---:|
| 0 | frozen_structured | 0.8690 | 0.8690 | -0.0000 | 1.00 |
| 0 | correctness_only | 0.8345 | 0.8599 | -0.0254 | 0.16 |
| 0 | structured | 0.8276 | 0.8581 | -0.0305 | 0.11 |
| 4 | frozen_structured | 0.8690 | 0.8690 | -0.0000 | 1.00 |
| 4 | correctness_only | 0.8966 | 0.8704 | +0.0261 | 0.95 |
| 4 | structured | 0.8828 | 0.8675 | +0.0152 | 0.79 |

## 判断

delay=0 时，Structured 相对 Frozen 的原始差值为 -0.0414，19 个随机顺序的均值为 -0.0109，原始差值落在随机分布的 0.11 分位；Correctness-only 也从原始 -0.0345 变为随机均值 -0.0091。这说明原始顺序确实放大了 provider head 的负更新。

delay=4 时，Structured 原始差值为 +0.0138，随机均值为 -0.0015，没有同样的低尾异常。因此顺序敏感性是 delay=0 下的局部放大因素，不是所有延迟条件下的统一根因。

## 范围

该结果只覆盖一个 outer fold、一个 seed 和 20 个顺序；它不能证明方法效果，也不能替代主实验。它把后续修复目标收窄为：控制 selected-only provider head 的高方差更新，并在主实验中报告顺序敏感性。
