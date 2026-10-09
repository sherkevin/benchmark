# BayesianRouter controlled-drift stress v2

日期：2026-10-06
状态：执行前冻结；这是对已完成 controlled correctness-drift receipt 的同协议 baseline extension。

## 目的

自然 fresh-menu confirmation 已包含 BayesianRouter。受控漂移 stress 的第一版只报告 Frozen、Matched Online-SGD、Greedy LinUCB 和 MIRA-Context，因此审稿人可能无法判断现代 adapted Bayesian comparator 在同一受控事件下的行为。本小实验只补齐这个比较，不改变主方法、候选集、标签、分母、阈值、漂移规则或已有结果。

## 固定合同

- 数据：与 `provider_fresh_drift_stress_20261006/v2_statehash_run2` 完全相同的四个 checkpoint-bound provider、157 个 scaffold-disjoint BBB candidates、5 个 fit folds、2 个 seeds、delay `{0,1,4}`。
- 事件：每条 stream 只调用一个 provider；selected correctness 反馈按延迟释放；在第 78 个 decision tick 起，仅当 selected provider 是该 fold fit-best provider 时翻转 evaluator 返回的 selected label。selector 看不到 truth、drift flag 或未选输出。
- 新增方法：`bayesianrouter`，沿用 fresh confirmation 已冻结的 Gaussian posterior prior mean、prior variance `0.02`、observation noise variance `1.0` 和 stream seed。每次只用释放的 selected correctness 做 rank-one posterior update。
- 对照：同一输出中保留 Frozen、Matched Online-SGD、Greedy LinUCB、MIRA-Context，便于直接配对审计；不替换或重算原 stress receipt。

## 分析

主要量是 BayesianRouter 相对同 stream Frozen 的 selected correctness 差值。报告 30 条 Bayesian streams、事件合同/回放验证、rescued/harmed action counts、calls、update p95、以及按 Bemis--Murcko scaffold cluster bootstrap 的 95% interval。结果不作为自然漂移泛化，也不据此调整 MIRA 超参或删除任何负结果。

## 停止和保留

若运行失败、字段缺失或 replay 不一致，保留失败收据，不补造结果。若成功，v2 receipt 单独归档；论文是否把它放入主 stress 表，只在读取 raw receipt、verification 和聚合后决定，既有 v1 表不被静默改写。
