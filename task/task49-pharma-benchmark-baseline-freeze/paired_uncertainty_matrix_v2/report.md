# Paired uncertainty matrix v2

日期：2026-10-06  
状态：development-only verified aggregation。

在 v1 已核验区间基础上加入 Random–Frozen 的同分子、同 scaffold、同 fold/seed/delay 区间。Random 原始 30 条 stream 和 common-stratum Frozen 原始 30 条 stream 均通过独立 verifier；本次只读重算，没有读取 confirmation，也没有新增模型预测。

| 对比 | 点估计 | 95% scaffold-cluster CI |
|---|---:|---:|
| Random − Frozen | `-5.676 pp` | `[-7.587,-3.822] pp` |

完整 16 行机器表在 `result.json`。它保留 v1 的 Matched、Greedy、Bayesian、MIRA 和受控 family-outage 区间，并明确 ordinary-task、controlled-event 和 post-call scope 不能混合。

仍缺：Replay/PILOT/static audit-only 的成对区间、unknown/refusal efficacy、以及冻结后的 E6 confirmation。该文件补齐了 Random 的不确定性入口，不改变任何最终 claim。
