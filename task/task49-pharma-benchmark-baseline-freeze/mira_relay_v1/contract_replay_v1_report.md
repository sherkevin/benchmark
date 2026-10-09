# MIRA-Relay contract replay v1 报告

日期：2026-10-03  
收据：`experiments/logs/task49_mira_relay_contract_replay_20261003/v4/`（v1–v3 保留为历史版本）  
代码提交：`c4d2bf638f86b7315a6283ccd551c46578fa5a3f`
当前独立 verifier 源码在后续提交中增加了 feedback-provider/reward 绑定检查；回放本身未重跑。

## 结果

- 9 条 stream 完成：3 个方法 × 3 个 delay `{0,1,4}`。
- 每条 stream 32 个 decision、32 个 terminal、32 个 feedback；总事件 864。
- 独立 verifier 通过：`v4/verification_v2.json` 标记 `verified`，所有 stream 的 decision/terminal/
  feedback ID 集合一致，`due_tick = decision_tick + 1 + delay`，每条反馈只 release 一次。
- verifier 还逐条绑定了 feedback provider 与 selected provider，并核对 scored reward 与 terminal
  chemical correctness；未评分状态不允许携带 reward/label。`pending_feedback=0`，selected provider 与
  terminal provider 一致；decision 的读取集合只有
  molecule feature、candidate menu 和已释放的 selected feedback。
- state snapshot 的前后 hash 和跨 event 状态链全部通过独立重算。

## 观察到的状态变化

旧版 `action_changes_after_feedback`（只统计 release 后下一次 decision 的 action 是否变化）如下：

| method | delay 0 | delay 1 | delay 4 |
|---|---:|---:|---:|
| ProbeMIRA-Relay | 24 | 23 | 22 |
| ProbeNoUpdate | 24 | 23 | 22 |
| NoProbeShield | 22 | 21 | 20 |

这些数不能作为反馈导致动作变化的因果指标：相邻 decision 属于不同 molecule，且 probe action
被记成 `probe_forced`。`ProbeNoUpdate` 也会因 runtime ledger 更新而发生 action 变化。正确的
同一 query 前后比较见 `same_query_sensitivity_v2.md`；本回放没有准确率、收益或 MIRA-Relay
优越性结果。

## 边界与下一步

1. 本回放使用已封存的 Task36 cache/judge，但只取 fold0 evaluation 前 32 条；不是完整 CPU
   效果矩阵，也不触及 confirmation 或 Nebula。
2. 现有 candidate archive 只有 `theta`。runner 在同一 fit cells 上重算 Hessian 对角线作为
   precision，并把该假设写入 `config.json`；它不是完整持久 fit-state 的替代品。
3. v4 已使用独立封存的 fit-state v2：`experiments/logs/task49_mira_relay_fit_state_20261003/v7/`。
   该状态包和 verifier 均通过，包含 precision、valid/failure counts、provider/family order、
   candidate artifact 副本和 canonical state hash；回放不再临时重算 fit state。
   state hash 为 `fb05a340a5ba71286592241b9c2b283f84bed41dc6faac6e872a9e0395f622c7`，
   state archive SHA-256 为 `c8b5828427701ee8647e8653a5ee67750e565cf50d09f8855248db6a8d63f787`。
4. 当前结果满足实现合同门，但没有触发 preregistration 的效果门。下一步才是讨论是否把
   trust-exact 作为生效 fit optimizer，并进入同 schedule full CPU replay；这仍需更新生效合同，
   不把本次 32 条回放写成效果结果。
