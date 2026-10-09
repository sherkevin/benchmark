# MIRA-Relay transfer diagnostic card v1

状态：下一步只读分析，2026-10-04；不新增回放、不改 v3 指标、不申请 seed1/A800。

## 目的

五折 v3 已完成同合同回放，但 MIRA 与 provider-independent residual 的动作差异只有 6–8 条 / 722 个 evaluation molecules。这个分析要回答一个具体问题：**family-shared update 是否真的有机会改变同族 provider 的排序，并且跨过 action margin；如果没有，重复 seed 不能提供有效信息。**

## 要检验的主张

- 候选主张：selected feedback 更新 family residual 后，可以把信息迁移给未被选择的同族 provider，并改善后续选择。
- 最近邻：同一初始化、同一 probe、同一 delayed feedback 的 provider-independent residual。
- 当前可证伪条件：在存在同族 sibling 且 sibling 可用的 feedback 上，若 release 前后 family score/rank 不变，或 rank 改变但 MIRA 与 Independent 的 action/utility 不变，则当前 replay 不能证明 family sharing 是收益来源。

## 输入与边界

只读以下已封存 receipt：fold0/seed0 r2、fold1/2/3/4 seed0 的 raw `events.jsonl`、`config.json`、`summary.json` 和 `verification_latest.json`。使用已有 event 的 `candidate_menu`、`available`、`selected_provider`、`state_before/after`、`scores`、`logit_means`、`logit_variances`、`feedback_status` 和 paired reward。不得读取 evaluation truth 或未选 provider 的输出来改变动作；truth 只用于已有 terminal reward 的误差标签。

分析单位是 selected feedback 和 fold×delay 的 paired molecule；不把 raw event 行、三个 delay 或 33 个 provider 当独立样本。

## 固定指标

对每条 MIRA `residual_updated=true` feedback 记录：

1. **Sibling opportunity**：selected provider 的 family 中，是否存在另一个 provider 同时出现在 `candidate_menu`、`available=true` 且不是 selected provider。输出 opportunity rate，并按 family 汇总。
2. **Transfer magnitude**：该 feedback 前后 family residual 的 $L_2$ 变化，以及同族 sibling 的 family contribution 变化。只使用 state snapshot；若维度或 family roster 不一致则拒绝该记录。
3. **Rank/margin consequence**：在同一 query 的 release 前决策中，记录 selected provider、第一/第二 exploit score gap，以及 sibling 相对 exploit 第一名的 gap。若 event schema 没有可独立重算的 post-release score，则只报告 pre-release margin，不凭空补 post score。
4. **Action consequence**：MIRA 与 Independent 是否在同一 entity 改变 provider action；同时记录 rescued/harmed 和是否为 sibling family change。
5. **Delayed sensitivity**：上述指标分别按 $D\in\{0,1,4\}$ 报告；delay 是同一 fold/seed 的时钟复本。

## 解释分支

- **A opportunity 少**：family sibling 可用率低于 20%，当前菜单无法识别迁移；不重复 seed，先改任务切片或收窄 claim。
- **B opportunity 足够但 transfer 小**：sibling opportunity ≥20%，但 family score 的中位 $L_2$ 变化低于预登记的数值门，说明更新被表示/后验尺度压小；下一步只能改更新机制并另立协议。
- **C transfer/rank 变化但 action 不变**：机制状态确实变化，但 action margin 没被跨过；报告机制信号与任务收益分离，不把 state update 写成效果。
- **D action 变化但 rescued/harmed 抵消**：共享更新改变了行为但没有可靠 utility；按 family、failure 和 probe/exploit 分层，必要时修改安全门而不是追 seed。
- **E action 变化不是 sibling transfer**：family-sharing 不能承担当前收益解释，回到 provider bias、fallback/hazard 或 runner 归因检查。

## 输出与停止条件

输出一个 JSON（逐 feedback）和一个 markdown 汇总，至少包含 fold、delay、family、entity、opportunity、transfer magnitude、margin、action consequence、reward consequence 及原始 event id。任何 raw/schema/hash 不一致立即停止。

这一步完成后才决定：

- 若 opportunity 足够、transfer 可观测且 action consequence 在 fold 间同向，写新的 seed1 预注册卡；
- 若 opportunity 足够但 action/utility 不变，先提出机制修复，不申请昂贵实验；
- 若 opportunity 不足或归因失败，保留 v3 的负机制证据，重审 benchmark slice 与 claim scope。

它不改变三份生效主线文档、验收分数或双方确认的目标主张。
