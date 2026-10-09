# Role-qualified main table and exact control join v1

日期：2026-10-06  
状态：**verified aggregation；development only；不读取 confirmation；不重跑策略**

## 目的

把现有已经独立核验的 CPU 收据整理成一张能进入论文审计流程的主表。这里解决的是三个具体缺口：

1. 每条结果明确自己的比较角色、数据 scope、attempt 分母、runtime failure、unknown/refusal 和恢复状态；
2. Best Fixed、Oracle 和 Random 使用同一份 `fingerprint_diagnostic_v1/inputs.json` 的 fold evaluation 行，与 common-stratum 每个方法、seed、delay 的 stream 做精确分母连接；
3. ordinary `no_event`、`controlled_event`、`family_outage` 保持独立，不能跨 scope 排名。

这不是新的效果实验，也不把不同 scope 或不同 baseline role 的数字合并排名。

## 输入和可复核性

| 输入 | 用途 |
|---|---|
| `fingerprint_diagnostic_v1/inputs.json` | 722 个 BBB development 分子、33 provider、5 个 scaffold-disjoint folds；控制项只读 fit/evaluation 行 |
| `jev_selector_h0_20261005/v1/summary.json` | 记录每个 fold 的 fit-only Best Fixed provider，作为控制口径交叉核对 |
| `random_development_20260930/run_v1/{summary,verification}.json` | 30 条同 menu、同 fold/seed/delay 的 verified Random development streams |
| `mira_unified_raw_20260930/full_v1/summary.json` | Frozen、Matched Online-SGD、MIRA streak-only、MIRA relative；普通 no-event 与 controlled-event 分开保留 |
| `common_stratum_family_guard_20261001/full_v1/summary.json` | Frozen、Matched、Greedy、BayesianRouter、MIRA relative、FamilyGuard 的受控 family-outage stratum |

机器结果：[`result.json`](result.json)；E1 控制连接：[`control_join.json`](control_join.json)。生成器：[`build_role_qualified_main_table_v1.py`](../../../src/probe_headless/build_role_qualified_main_table_v1.py)。`result.json` 保存六个输入的 SHA-256，避免当前工作树变化后误读收据。

## 角色边界

| 角色 | 方法 | 允许的比较 |
|---|---|---|
| strict main | Frozen、Matched Online-SGD | 同一 direct prior、feature、selected-only feedback、delay；可做主 paired comparison |
| proposed candidate | MIRA relative、MIRA FamilyGuard | 只能在各自声明的 scope 与 strict controls 比较；不能把 family-outage 当普通任务收益 |
| candidate ablation | MIRA streak-only | 机制对照，不是独立主方法 |
| adapted stratum | Greedy LinUCB、BayesianRouter | native likelihood/prior 语义不同，单独报告，禁止跨 stratum raw ranking |
| anchor | Random、Best Fixed、Oracle | Random 是无学习下界；Best Fixed 是 fit-only incumbent；Oracle 是 evaluator-only 上界，不具有 policy timing |

## 已闭合的精确控制连接

对每个 fold，脚本从 fit 行选出 Best Fixed，再在同一个 evaluation 行上计算 Best Fixed 与 Oracle。每个 control row 都与 common-stratum 中同 fold 的 `2 scenarios × 4 methods × 2 seeds × 3 delays = 48` 条 stream，以及 Random 的 `2 seeds × 3 delays = 6` 条 stream，通过 evaluation attempt 数连接；5 个 fold、两类 control 共 10 个检查全部 `PASS`。

控制结果（722 个 evaluation attempts）：

| control | accuracy | 说明 |
|---|---:|---|
| Best Fixed | 0.915512 | fit-only provider，不能读取 evaluation labels 选 provider |
| Oracle | 0.997230 | evaluator-only 上界，不能作为在线策略 |
| Random | 0.844875 | 30 条 verified development streams；propensity/事件收据存在，当前聚合未带 timing p95 |

这把 E1 的“存在 headroom”从跨文件拼接变成了同一输入 manifest 下的 exact join。Random 现在已有同 schedule 的 verified development receipt，并进入 exact join；它的 selection/update timing 字段仍缺，不能用作成本比较。

## 普通开发 scope 的聚合（只作定位，不作显著性检验）

| scope | Frozen | Matched | MIRA relative | MIRA streak-only | Random |
|---|---:|---:|---:|---:|---:|
| no-event | 0.901662 | 0.892198 | 0.902816 | 0.901662 | 0.844875 |
| controlled-event | 0.869806 | 0.890120 | 0.892428 | 0.890351 | — |

这些是 pooled molecule attempts 的描述性数字；最终论文表仍需使用已有 scaffold-paired uncertainty。该聚合器不生成 CI，也不把 controlled-event 解释成自然 drift。

Family-outage stratum 的描述性 accuracy 为 Frozen `0.901662`、Matched `0.892198`、MIRA relative `0.902816`、FamilyGuard `0.907664`、Greedy `0.887350`、BayesianRouter `0.865420`。它是独立的 controlled stress scope，不能并入 no-event 主结果。

## E5 分母和失败语义

- `n_attempts` 是该 stream 实际作出的 provider decisions；`n_terminal` 和 `n_feedback` 只在源收据有相应字段时填入。
- `runtime_failures = attempts - valid_returns`（当源收据提供 `valid_returns`）；runtime error 不被转成 chemical wrong。
- `unknown=0` 和 `refused=0` 原样保留，并标记为“observed zero in receipt; no refusal efficacy claim”。这说明收据没有静默丢字段，不能说明拒答有效。
- unknown/refusal utility、Random timing p95、跨方法 paired uncertainty、统一 state bytes 仍显式为缺口，未用估算填充。
- Family-outage、controlled-event、no-event 三种 scope 不共用统计分母。

## 对验收闸门的影响

这次工作实际关闭了：

- E1 的 Best Fixed/Oracle/Random exact control join；
- E2/E5 的 role-qualified schema、scope 和 denominator pointer；
- 主表中“不同 stratum 不能直接排名”的机器约束。

它没有关闭：

- 全方法统一 unknown/refusal/cost/state 字段；
- E3 之外的完整机制和 family sensitivity 表；
- E4 的自然 lifecycle 或 drift（当前仍只声称 `feedback-adaptive`）；
- scaffold-paired uncertainty 与 E6 confirmation。

因此本 artifact 关闭了 E1 的主要控制缺口，但不把三门自动改成通过。下一步应在这张表固定的角色合同上统一 E5 cost/refusal 分母，再按三份标准重新评分。
