# Figure 3 receipt-bound data layer v1

状态：**已用于程序数据图 `../../figure3/v02_data/`**。原始 JSON 与 source
hashes 保留；AI 布局元数据只代表历史来源，不决定当前绘图方式。

绑定脚本是 `build_data_binding_v1.py`，机器可读结果是
`data_binding_v1.json`。脚本只读三份已有 summary/result，不运行实验。

## Panel A — feedback effect

每个数是同一 fold×seed 配对下，MIRA-Relay 与 Frozen 的 selected-success
差值，单位 pp，按 `D` 分组：

| D | natural fresh | controlled label flip |
|---:|---:|---:|
| 0 | -0.1274 | +13.6306 |
| 1 | -0.1274 | +14.5223 |
| 4 | +0.2548 | +14.3949 |

这组数只描述当前论文声明的一次调用合同；controlled label flip 是
evaluator-side stress，不能标成自然 provider drift。

## Panel B — rescued versus harmed

这些是 controlled stress 中相对同一收据 Frozen action 的 paired action
changes，不是 accuracy 分母：

| Method | Rescued | Harmed |
|---|---:|---:|
| Frozen | 0 | 0 |
| SGD | 720 | 218 |
| LinUCB | 414 | 48 |
| Bayes | 492 | 167 |
| MIRA | 780 | 112 |

## Panel C — update cost

每个点是 30 条 controlled-stress stream 的均值。横轴是 policy-operation
update p95（微秒），纵轴是最大序列化 state（KiB）；不代表 provider inference
或端到端服务延迟。

| Method | Update p95 (µs) | State (KiB) |
|---|---:|---:|
| Frozen | 1.4988 | 5.3283 |
| SGD | 20.0209 | 5.3399 |
| LinUCB | 92.3247 | 403.9534 |
| Bayes | 92.5503 | 405.4820 |
| MIRA-Relay | 3.4644 | 5.5946 |

## 绘制约束

- exact marks、图例和坐标数值只能来自 `data_binding_v1.json`；不能从 AI 图
  里读回或手抄预期数值。
- 当前 receipt 没有为每个 `D` 单独提供已核验的 scaffold interval，因而不在
  这一版绑定中擅自添加误差线；如需误差线，必须先生成独立、可审计的聚合收据。
- 当前由 matplotlib 直接绘制精确 marks，并校验曲线、柱宽和散点坐标。
  原 AI v03 空布局不再用作数据底图；程序输出为 7-inch 矢量 PDF。
