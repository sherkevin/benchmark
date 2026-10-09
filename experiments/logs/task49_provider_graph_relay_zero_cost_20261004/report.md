# Provider-Graph Relay v1：零机时合同检查报告

日期：2026-10-04  
状态：**结构合同通过；候选仍未运行 replay，未产生效果结论。**

## 目的

这一步只回答三个实现前问题：fit-only 图能否由独立公式复算，图是否改变冷启动，稀疏 relay 是否越过 fallback 和更新预算边界。它不回答 Graph Relay 是否改善工具选择。

## 固定输入与配置

- 输入：`fingerprint_diagnostic_v1/inputs.json`，SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 五个 scaffold-disjoint fold；只把 `fold.fit` 传给 graph builder。
- `m_min=128`、`lambda=32`、`k=2`、`tie_tol=1e-12`、`rho=0.5`。
- 方向：selected provider `q` 是 source，邻居 `p` 是 writable target；fallback 从 target 集合排除。
- random control 只匹配每个 source 的 out-degree、边数和 relay mass；不声称保持 in-degree。
- 代码和测试配置见同目录 `config.json`，原始结构化收据见 `result.json`。

## 结果

| fold | fit rows | graph edges | provider coverage* | fallback | fit-state index check | initial logits | reference graph | synthetic relay |
|---:|---:|---:|---:|---|---|---|---|---|
| 0 | 444 | 66 | 1.000 | `admetlab_3_0` | pass | exact | pass | pass |
| 1 | 483 | 66 | 1.000 | `admetlab_3_0` | pass | exact | pass | pass |
| 2 | 492 | 66 | 1.000 | `admetlab_3_0` | pass | exact | pass | pass |
| 3 | 483 | 66 | 1.000 | `admetlab_3_0` | pass | exact | pass | pass |
| 4 | 480 | 66 | 1.000 | `admetlab_3_0` | pass | exact | pass | pass |

﻿* `provider coverage` 是图上至少有一个正邻居的 provider 比例，只是静态图覆盖率；它不是 replay 中 selected provider 的 available-neighbor opportunity，也不是效果指标。

另外，`src/probe_headless/tests/test_provider_graph_relay_zero_cost.py` **4 passed**。测试覆盖：独立 NumPy 图复算、fit/evaluation 边界、固定 tie 规则、fallback exact invariance 和 relay mass。

## 结论与边界

这一步关闭了候选进入 fold0 CPU gate 前的结构阻塞：图公式、fit index 与 sealed fit state 对齐；独立参考实现与 builder 的邻居/重叠/相似度一致（浮点容差 `1e-12`）；冷启动 logits 不受 real/random/identity graph 影响；合成更新只写 selected 和可写邻居，fallback 保持不变。

它没有证明以下任何一点：

- graph edge 在 held-out query 上能预测 residual alignment；
- Graph Relay 会改变 action 或提高 rescued/net utility；
- delay `{0,1,4}` 下不会受损；
- 该图机制比 MIRA、Independent 或随机图更好。

因此当前仍不申请 Nebula，不写论文效果表。下一步必须先把“selected feedback 对邻居 residual 是否同向”作为预登记的 held-out diagnostic，且保留 out-degree-matched random graph；只有该诊断和 fold0 replay contract 都通过，才讨论扩大五折。
