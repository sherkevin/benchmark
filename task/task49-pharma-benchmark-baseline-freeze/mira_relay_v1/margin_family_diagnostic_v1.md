# Family size / margin diagnostic v1

- 状态：只读分析；没有新增回放、参数或标签。
- 目的：判断 family-sharing 的可识别机会是否集中在少数 non-singleton family，以及 HRA fold0 的动作变化是否来自真正的 sibling family。

## 输入与定义

- MIRA v3 transfer records：1369 条 selected feedback；hash `f048eac75e2594f987399c4314f1c3daba3bcfa59099e0d93d416a7973ee3068`。
- opportunity 沿用 transfer diagnostic：同族另一个 provider 同时在 candidate menu 且 available。action difference、rescued、harmed 沿用 paired 定义。
- fold/delay 是同一 cohort 的时钟复本，不当作独立重复。

## Family size 分层（15 个 fold×delay stream 合并）

| family size | updates | opportunity | opportunity rate | nonzero family delta | action diff vs Independent | rescued | harmed |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 1 | 1095 | 0 | 0.000 | 1080 | 3 | 3 | 0 |
| 2 | 110 | 110 | 1.000 | 110 | 14 | 0 | 0 |
| 3 | 83 | 83 | 1.000 | 83 | 0 | 0 | 0 |
| 24 | 81 | 81 | 1.000 | 81 | 0 | 0 | 0 |

## HRA fold0 vs MIRA

| delay | action changes | changed family sizes | rescued | harmed |
|---:|---:|---|---:|---:|
| 0 | 3 | {2: 3} | 0 | 0 |
| 1 | 3 | {2: 3} | 0 | 0 |
| 4 | 4 | {1: 1, 24: 2, 2: 1} | 0 | 1 |

## 结论与边界

- 1,095/1,369（80.0%）MIRA updates 发生在 singleton family，天然没有 sibling opportunity；当前 fixed provider pool 对 family-sharing 的可识别样本只有 274 条 non-singleton updates。
- non-singleton opportunity 为 100%，但相对 Independent 的 action difference 只有 14 条（跨 delay 计数），rescued 为 0；singleton action difference 的 3 条不能归因于 family transfer。
- HRA 相对 MIRA 的变化全部来自 non-singleton family：delay 0/1 为 3 条 admet_ai（size 2），delay 4 为 3 条 admet_ai 加 1 条 b3clf（size 24）。这排除了“变化来自 singleton”的解释，但没有证明 local residual 带来收益。
- 该诊断支持下一步先改 family opportunity/graph construction 或收窄 family-sharing claim；不支持增加 seed、申请 A800 或把 HRA 写成主方法。
