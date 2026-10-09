# Baseline fairness contract v3

日期：2026-10-06  
状态：development evidence；不打开 confirmation。

## 目的

把当前已经有 raw receipt 和独立 recovery 的方法按比较权限固定下来，避免把不同 likelihood、warm-start 或 scope 的数字混成一个榜单。这个文件是协议收口，不是新的效果实验。

## 主比较协议

主协议固定使用同一份输入、33-provider menu、64 维 common feature、fit-only warm start、selected-only feedback、`decision_tick + 1 + delay` 释放时钟和 delay `{0,1,4}`。六个主方法都有完整事件收据、独立 recovery、选择/更新耗时和最终状态字节：

| 方法 | 角色 | 允许的比较 |
|---|---|---|
| Frozen | strict control | 主表基线 |
| Matched Online-SGD | strongest legal logistic control | 与 Frozen、MIRA 成对比较 |
| Greedy LinUCB | modern Gaussian adapted comparator | 同协议报告；不与 logistic likelihood 混成无条件总榜 |
| BayesianRouter online | modern Gaussian adapted comparator | 同协议报告；不与 logistic likelihood 混成无条件总榜 |
| MIRA relative | candidate method | 与 controls 成对比较 |
| MIRA FamilyGuard | candidate method | 普通任务和 family-outage 分 scope 报告 |

Greedy/Bayesian 的状态字节来自已核验的 common-stratum family receipt，而不是从矩阵维度推算：两者所有 stream 的 `final_state_bytes=1,098,504`。选择和更新 p95 来自对应成本收据。适配方法的 Gaussian likelihood 与 Matched 的 logistic likelihood 仍是不同模型 stratum；因此协议公平不等于允许跨 likelihood 的单一排名。

## 锚点和分开报告的方法

- Random 是同菜单、同反馈时钟的 protocol anchor；新增了 Random–Frozen 的 scaffold 成对区间，点估计 `-5.676 pp`，95% CI `[-7.587,-3.822] pp`。
- Best Fixed 是 fit-only incumbent，Oracle 是 evaluator-only upper bound；两者不是可部署 online baseline，online latency、feedback 和 learner state 对它们标为 N/A。
- Reward-ridge、Replay-SGD、PILOT 和 static router 保留为各自 stratum 的开发证据；GraphRouter/Laya 仍 audit-only，不用缺失字段填充主表。

## 仍未通过的部分

普通任务的 unknown/refusal 仍为零观察，不能据此声称拒答有效；provider model-fresh exposure 仍是 benchmark 资格边界；E6 confirmation 尚未执行。这个契约因此把 Gate B/C 的比较权限和证据路径固定了，但不把 development 结果写成最终结果。

机器结果：`contract.json`。来源 hash 和每个方法的 raw/recovery 路径都在该文件中。
