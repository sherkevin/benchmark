# E3 机制消融覆盖矩阵 v2

日期：2026-10-06  
状态：**verified aggregation；development only；不读取 confirmation；不申请 A800**

## 这次补了什么

v1 只整理了 provider-local veto ablation。v2 把它和已经通过独立核验的 common-stratum FamilyGuard receipt 连接起来，但保留两个 scope，不做跨 scope 排名：

1. veto ablation：Frozen、streak-only、beta-only、full MIRA、Matched Online-SGD；300 条 stream，43,320 decisions/terminals/feedback；
2. common stratum：MIRA-relative 与 MIRA-FamilyGuard 的 60 条 stream，包含 state hash、state bytes、runtime failure 和 task success。

机器结果：[`result.json`](result.json)；生成器：[`build_mechanism_ablation_matrix_v2.py`](../../../src/probe_headless/build_mechanism_ablation_matrix_v2.py)。输入文件和 source hash 均保存在结果中。

## 每个新增机制的去除对照

| 新增机制 | 去除/对照 | 证据 | 判定 |
|---|---|---|---|
| selected correctness Beta gate | streak-only | 同一 veto schedule，runtime failure ledger 保留 | 已覆盖 |
| selected runtime-failure ledger | beta-only | 同一 veto schedule，Beta gate 保留 | 已覆盖 |
| 所有在线状态 | Frozen | 同一 veto schedule，无更新 | 已覆盖 |
| provider-local online residual | Matched Online-SGD | strict selected-only online control | 已覆盖为最近邻控制，不宣称二者数学等价 |
| family-level hazard state | MIRA-relative | common-stratum 同一 menu/feature/delay，FamilyGuard 多了 family state | 已覆盖 |
| interface-null | Frozen/no-update 语义 | Frozen 在同一 runner 中保持 state/version 不变 | 已覆盖语义；没有另建空接口 runner |
| family label permutation | permutation stress | 不是当前主机制必要条件 | **MISSING；作为可选 adversarial sensitivity** |

因此当前主方法的新增状态（correctness ledger、runtime ledger、family hazard state）都有对应去除或最近邻控制。`family_permutation` 仍没有收据，不能把 family label 的敏感性写成已验证的鲁棒性。

## 关键结果边界

veto ablation 的 controlled-event 结果与 v1 一致：streak-only 恢复了 full MIRA 的主要事件收益；beta-only 不能处理 runtime failure 且带来 no-event 损失；Matched Online-SGD 三个 delay 的 post accuracy 都高于 full MIRA。common-stratum 中 FamilyGuard 的 pooled task success `0.907664`，MIRA-relative `0.902816`，但已有 scaffold-paired 区间跨 0，不能写成普通任务 superiority。

这些数字回答“新增状态是否在相同接口下改变了行为”，不回答“整个方法已经超过强 online baseline”。controlled family-outage 的 release-after 改善另有收据，不能和 ordinary no-event 主结果混用。

## E3 结论和剩余缺口

按当前评价标准，E3 的“去掉每个新增状态/更新”证据已具备合同、原始事件和独立复核。剩余缺口是：

- family permutation 的 adversarial sensitivity；
- 统一主表中的 paired scaffold CI 入口；
- E1–E5 其他项（Random exact stream、unknown/refusal utility、全 baseline cost/state 字段和 E6 confirmation）。

这项矩阵可以把 E3 从“缺少完整机制消融”提升为“机制消融已有、仍有边界敏感性待补”的状态，但不能单独让 Gate C 通过。下一步重新按三份标准评分，若只剩可选 permutation 和跨 E1–E5 的统一 CI 问题，再决定是否需要一次最小 CPU sensitivity run。
