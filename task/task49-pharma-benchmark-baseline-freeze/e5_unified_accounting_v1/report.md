# E5 统一错误、成本与恢复账本 v1

日期：2026-10-06  
状态：**verified aggregation；development only；不读取 confirmation；不重跑策略**

## 目的

把已有 online receipts、Random receipt 和 post-call utility receipt 放进同一个字段合同，先解决“失败、unknown、拒答、调用次数和恢复状态到底各自怎么记”的问题。它是 accounting closure，不是把不同任务 scope 的 accuracy 拼成总榜。

机器结果：[`result.json`](result.json)；生成器：[`build_e5_unified_accounting_v1.py`](../../../src/probe_headless/build_e5_unified_accounting_v1.py)。五份源收据的 SHA-256 都在 `result.json`。

## 字段合同

| 字段 | online routing | post-call utility |
|---|---|---|
| attempts / calls | 实际 provider decision | 实际 provider calls |
| terminals | provider terminal record | episode terminal |
| feedback | selected feedback release | 不属于该 extension 的 online feedback |
| runtime failure | `attempt - valid_returns`；chemical correctness 保持 null | evaluator failure 单列 |
| unknown | explicit unknown；不填 wrong | 当前 summary 没有统一 unknown 字段，保留 null |
| refused | policy refusal；不算 provider failure | explicit refusal |
| correctness | `chemical_correct / valid_returns` | evaluator `correct / n` |
| p95 / state | 源收据有则填；没有则 null | 当前 extension 不提供 |

## 已纳入的收据

- common-stratum：Frozen、Matched Online-SGD、MIRA streak-only、MIRA relative；8,664 attempts/method，runtime failure 保留；
- family-outage common stratum：Frozen、Matched、Greedy、BayesianRouter、MIRA relative、FamilyGuard；4,332 attempts/method，包含 state/cost 字段在源收据中的部分；
- Random：30 verified development streams、4,332 attempts；
- post-call utility：always accept/refuse、confidence/random refuse、confidence/random escalate，policy/evaluator 分开恢复。

## 关键账本

| scope | method | attempts/calls | runtime failure | unknown | refused |
|---|---|---:|---:|---:|---:|
| common | Frozen | 8,664 | 222 | 0 | 0 |
| common | Matched Online-SGD | 8,664 | 89 | 0 | 0 |
| common | MIRA relative | 8,664 | 106 | 0 | 0 |
| family-outage | FamilyGuard | 4,332 | 10 | 0 | 0 |
| no-event | Random | 4,332 | missing in summary | 0 | 0 |
| post-call | confidence refusal | 722 | 0 | null | 149 |
| post-call | confidence escalation | 871 | 3 | null | 0 |

`unknown=0/refused=0` 的 common online rows是收据中实际观察到的零，不是拒答有效性结果；post-call rows 是调用后扩展，不能回填 pre-call selector。

## 关闭和未关闭的缺口

已关闭：

- online terminal、selected feedback、runtime failure 和 episode denominator 的统一命名；
- post-call refusal/escalation 的 calls、refusal、failure 和 correctness 分开记账；
- scope、role、recovery 状态和空值含义明确。

仍未关闭：

- 所有 baseline 同时具备 p50/p95、update cost、state bytes 和 unknown/refusal reason；
- unknown censoring 与 refusal utility 的完整主实验；
- paired scaffold uncertainty；
- post-call extension 与 online routing 的统一成本模型。

因此 E5 的 accounting 子项已经可复核，但 E5 整体仍 `PARTIAL`，Gate C 不变。下一步不是再加 baseline，而是用这份字段合同生成同一 strict stratum 的 paired CI/cost 表。
