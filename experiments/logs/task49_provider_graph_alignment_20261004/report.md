# Provider-Graph Relay v1：held-out residual alignment 诊断

日期：2026-10-04  
状态：**通过资源闸门；尚未运行 replay，也没有效果结论。**

## 目的

零机时结构检查只能说明图能构造，不能说明图边适合迁移。这里固定 fit-only 图，用每个 fold 未参与建图的 validation 分子检查 selected provider 和 recipient 的 residual 是否同向，并与保留 source out-degree 和 target in-degree 多重集的随机图比较。

## 预登记口径

- graph：`m_min=128`、`lambda=32`、`k=2`、`tie_tol=1e-12`；只读 fit rows。
- state：同一 fold 的 sealed direct prior、family residual 和 provider bias；validation 标签只用于诊断，不写回 state。
- writable target：fallback 从 graph relay 中排除。
- 主量：$g=\lVert x\rVert^2\delta_p\delta_q$，其中 $\delta=c-s$；正值表示 recipient BCE 的一阶 relay 方向有利。
- random control：保留每个 source 的 writable out-degree 和所有 target 的 in-degree 多重集，只打乱 source-target 配对；seed=`20261004 + fold`。
- 继续线：5 个 fold 中至少 4 个 `graph_mean_product>0`，并且至少 4 个 `graph_minus_random>0`。该线只决定是否值得支付 replay 成本，不是论文效果阈值。

## 结果

| fold | validation rows | graph valid pairs | graph mean $g$ | random mean $g$ | graph minus random | graph positive fraction |
|---:|---:|---:|---:|---:|---:|---:|
| 0 | 133 | 8,778 | 2.47045 | 1.68321 | +0.78724 | 0.98485 |
| 1 | 94 | 6,204 | 2.12750 | 1.54536 | +0.58214 | 1.00000 |
| 2 | 86 | 5,676 | 1.55852 | 1.05934 | +0.49918 | 0.96970 |
| 3 | 95 | 6,270 | 2.19913 | 1.63113 | +0.56800 | 1.00000 |
| 4 | 98 | 6,468 | 1.98397 | 1.16141 | +0.82256 | 1.00000 |

5/5 fold 的 graph mean 为正，5/5 fold 高于 degree-matched random，达到预登记资源闸门。逐 edge 原始记录见 `records.jsonl`，配置、哈希和完整 JSON 见同目录 `config.json`、`summary.json`、`full.json`。

## 解释边界

这说明 fit-only graph 在当前 frozen representation 上有 held-out residual-alignment 信号，值得继续做一次 fold0 CPU replay。它不能证明 Graph Relay 会改变 action、提高 accuracy、优于 MIRA/Independent，不能证明 delay4 安全，也不能把图传播写成通用 Bayesian/PSD 更新。

下一步只预登记 fold0/seed0、delay `{0,1,4}` 的统一 replay，加入 MIRA、Independent 和 degree-matched random graph；先核验完整 receipt 和 paired rescued/harmed。若 action-level gate 不通过，停止候选，不靠增加 seed 或 A800 追分。
