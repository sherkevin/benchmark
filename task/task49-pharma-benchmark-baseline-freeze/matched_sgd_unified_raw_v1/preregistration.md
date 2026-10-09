# Matched Online-SGD unified raw receipt v1

状态：开发阶段预注册，2026-09-30。

## 目的

把与 MIRA 使用同一分子、provider menu、冻结表示、fold、seed 和延迟协议的
Matched Online-SGD baseline，重新记录为统一的 decision/terminal/feedback 原始
事件合同。该任务先解决可审计性，不改变算法、超参或已有主表数字。

## 预先登记的比较

- `frozen_direct`：使用同一 fit-only direct head，反馈只推进协议版本，不改变权重。
- `matched_online_sgd`：同一 direct head；每次 selected feedback 按预先固定的
  logistic SGD 更新被选 provider 的权重，失败输出的系统 reward 为 0。
- feature：已有 frozen BT+CLS encoder 的 64 维输出；不使用未选 provider 输出。
- cohort：722 个 development molecules，5 个 scaffold-disjoint outer folds，2 个
  fit seeds，delay `{0,1,4}`。
- source：固定的 `fingerprint_diagnostic_v1/inputs.json`、恢复的 BT+CLS checkpoints
  和已有 fit snapshots；不打开 confirmation，不产生新工具预测。

## 事件合同

每个 stream 必须逐事件保存 decision、terminal、feedback。必须能独立核对：
候选 menu、selected provider、selected cache output、selected-only feedback、
due/release tick、version、state hash、input/source hash 和 provider identity。
decision 与 terminal 不得看到未选输出；feedback 不保存 evaluator-only chemical
correctness，独立 verifier 从冻结输入重算它。

## 反证条件

以下任一情况出现，receipt 记为失败并不得进入主表：事件不守恒；selected output
与冻结 cache 不同；feedback 提前释放或跨 provider；版本/state hash 链不一致；
独立 replay 的 action 或 weight 更新不一致；输入、checkpoint 或 source hash 不匹配。

## 边界

该 receipt 不是新的方法效果结果，也不证明 Matched Online-SGD 优于 MIRA。它只
把已有强基线放进同一可审计合同，为后续 paired effect table 提供可信原始证据。
