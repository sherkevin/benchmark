# Provider-Graph Relay v1：fold0 CPU replay验收报告

- 日期：2026-10-04
- 目的：检验 fit-only provider graph 的 selected-only residual relay 是否在延迟反馈下产生可复核的 action-level utility。
- 范围：fold0 / seed0，145 个 evaluation molecules，33 个 provider，delay `{0,1,4}`。
- 运行性质：development mechanism gate；不打开 seed1、A800、其他 endpoint 或 confirmation。

## 1. 证据状态

第三次正式回放使用代码提交 `70e5455b7b3bdab333b9156dbcf8f19a4dcf365a`，生成 12 条 stream、5,220 条 raw events（每条 stream 145 decision + 145 terminal + 145 feedback）。输入、fit state、checkpoint、graph manifest、源码哈希和事件哈希均在 receipt 中封存。

- 原始事件：`experiments/logs/task49_provider_graph_relay_fold0_20261004/receipt_v3/events.jsonl`
- 紧凑收据：`experiments/logs/task49_provider_graph_relay_fold0_20261004/receipt_v3/`
- 独立验收：`verification_v2.json`，状态 `verified`
- 旧验收器拒绝：`verification_rejected_v1.json`。拒绝原因是验收器错误要求每个声明的 relay target 都必须产生非零浮点变化；修正后只允许变化发生在目标集合内，并要求未标记更新时状态完全不变。
- 原始文件清单和 SHA-256：`raw_manifest_v1.json`

验收器从冻结 provider cell 独立重算 selected output、valid return、reward、due/release tick、state hash chain、fallback 不变量和 paired metrics。没有重跑回放，也没有用修正版验收器改动结果。

## 2. 配对结果

下表是 Graph Relay 相对各对照的 molecule-level paired 结果。三个 delay 使用同一 fold/seed 的不同反馈时钟，不能当作独立重复。

| delay | 对照 | action changes | rescued | harmed | net |
|---:|---|---:|---:|---:|---:|
| 0 | MIRA family relay | 28 | 1 | 3 | -2 |
| 0 | Independent residual | 29 | 2 | 3 | -1 |
| 0 | degree-matched random graph | 6 | 0 | 1 | -1 |
| 1 | MIRA family relay | 26 | 1 | 3 | -2 |
| 1 | Independent residual | 27 | 2 | 3 | -1 |
| 1 | degree-matched random graph | 7 | 0 | 1 | -1 |
| 4 | MIRA family relay | 32 | 4 | 4 | 0 |
| 4 | Independent residual | 32 | 4 | 4 | 0 |
| 4 | degree-matched random graph | 10 | 1 | 2 | -1 |

Graph Relay accuracy 为 delay0/1/4 的 `125/145`、`125/145`、`126/145`；MIRA 分别为 `127/145`、`127/145`、`126/145`。因此图结构确实改变了动作，但没有产生净收益；delay0/1 反而相对 MIRA 为负，delay4 只达到持平。

## 3. 预注册停止条件

本批触发以下停止条件：

1. 相对 MIRA 在 delay0 和 delay1 均为负净差；
2. 相对 degree-matched random graph 三个 delay 均无正净差；
3. delay4 相对 MIRA 净差为 0，未形成跨 delay 的稳定收益。

因此 Provider-Graph Relay v1 停止扩展 fold1--4、seed1 和 Nebula。保留该候选作为**已核验的负机制诊断/对照实现**，不升级为主方法，不写成论文的正向效果。

## 4. 对项目主线的结论

这次结果不能证明“图关系没有任何价值”。它证明的是：在当前 33-provider BBB 菜单、frozen encoder、fit-only correctness graph、固定 relay mass 和 selected-only delayed feedback 合同下，静态相似图带来的动作变化不能稳定转化成正确选择。held-out alignment 的正信号没有通过 action-level utility gate。

最有用的结果是把失败定位得更具体：

- 关系图覆盖了更多 provider，但相似关系本身不足以保证 release 时 residual 的方向和决策排序一致；
- 允许图 relay 改变动作会引入 harmed selections，尤其在 delay0/1；
- 仅增加邻居和更新规模不是当前问题的可靠修复。

本结果不改变 benchmark、baseline 或目标 claim；它关闭的是这条 graph-relay 候选的继续投入。Gate C、方法 M5/M13/M15、故事 S4/S6 继续保持 HOLD。

## 5. 下一步

下一步回到主线验收标准：不再为图 relay 增加算力，转而完成统一 E1--E5 矩阵中的缺口，尤其是 baseline fairness、paired uncertainty、failure/unknown/refusal 分母、cross-endpoint qualification 和 frozen confirmation 前的完整主表。任何新的机制候选都必须先解释为什么能避开本次已观测的“action change 但 net utility 不增”失败模式，并在预登记后再运行。
