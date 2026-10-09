# Random baseline contract smoke v1

日期：2026-09-29  
状态：合同收据通过；不产生效果结论，也不打开 confirmation。

## 目的

补齐 baseline 合同中的 C0 Random 控制。它只检查无学习策略是否能和 C4–C8 使用同一套分子菜单、selected-only 反馈、终止与延迟释放协议，避免把“没有学习”误当成“没有合法反馈”。

## 实现

- `RandomPolicy` 使用固定 seed `20260929`，在当前菜单上均匀随机选择；propensity 明确记录为 $1/|A_t|$。
- 每次释放反馈都会推进 protocol version，但不修改零状态；重复反馈、错 provider、非法 reward 仍被拒绝。
- runner scope 为 `C0+C4-C8 contract-only`。C1 Best Fixed、C2 Frozen、C3 Oracle 仍是单独的 fit/evaluator controls，不在这次 smoke 中冒充已完成。

## 真实缓存回放

| 项目 | 设置 |
|---|---|
| cohort | 固定 64 个 BBB development molecules（含 6 个已知失败 ID） |
| menu | 33 个 provider |
| methods | C0 Random + C4 Matched Online SGD + C5 Replay SGD + C6 Greedy LinUCB + C7 PILOT-adapted + C8 BayesianRouter-adapted |
| delays | 0、1、4 |
| streams | 18 |
| 每条 stream | 64 decision、64 terminal、64 feedback |
| 总事件 | 每类 1,152；三类合计 3,456 |
| GPU / 新预测 | 未使用；0 |
| effectiveness estimate | `null` |
| confirmation | closed |

原始收据：`experiments/logs/task49_baseline_contract_smoke_20260929/random_v2/`。  
独立核验：`verification.json`，状态 `verified`。

## 反篡改检查

复制收据后把第一条 Random decision 的 propensity 改为 `0`，独立 verifier 以退出码 1 拒绝，现场保留在：
`experiments/logs/task49_baseline_contract_smoke_20260929/random_tamper_v2/expected_rejection.log`。

本次收据哈希：

- events：`23ee17c9116004cea64a3207e41eed4cef5919d4c997ee119431fab6d05629a8`
- config：`dc033f02c3d9cd9fa1504336fe2dc4d641511c73d171832931e6a174628b3638`
- inputs：`e9b311f404d502d08bda8db859b3e8a61d8ebeaf5ca3e11a141925c4c8ef58ab`

## Gate 影响与边界

这项工作补齐 Gate B 的 C0 合同收据和统一事件格式，支持 B9/B10 的证据；Gate B 仍未通过。静态强 router fidelity、GraphRouter/Laya 的资格处理、C1–C3 控制的统一主表以及后续 E1–E5 仍缺。由于这是 contract-only smoke，不能报告 Random 准确率，也不能据此声称任何方法有效。

下一步回到唯一 MIRA 候选 **MIRA-SafeSwitch**：先完成 no-event 与 controlled provider-version event 的 CPU replay，检验其是否能减少当前“离开已正确 incumbent 后切错”的问题；未通过 CPU 闸门前不提交新的效果型 A800。
