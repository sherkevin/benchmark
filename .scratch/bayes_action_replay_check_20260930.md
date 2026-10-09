# BayesianRouter action replay check（只读旁路）

日期：2026-09-30  
用途：审查 `recovered_v2` 是否能从现有 checkpoint、输入与日志唯一重建 action；不修改正式结果，不替代官方 recovery。

## 输入

- raw events: `experiments/logs/task49_fingerprint_online_20260927/recovered_v2/events.jsonl`
- frozen input: `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json`
- BT+CLS checkpoints: `experiments/logs/task49_fingerprint_diagnostic_20260926/recovered_v4/checkpoints/fold{0..4}_seed{0,1}_bt_cls.pt`
- kernel: `src/probe_headless/pharma_online_baselines.py::BayesianRouterOnlineKernel`
- policy seed: `20260926 + 100*fold + 10*seed + delay`（与 online runner 源码一致）

## 做法

1. CPU 加载每个 fold/seed 的 BT+CLS state dict，重算 722 个 hidden-64 query features 和 33-provider prior vectors。
2. Frozen stream 使用 prior 的 deterministic argmax。
3. BayesianRouter stream 按原始事件顺序先处理 feedback，再处理 decision；每次 update 使用 raw feedback 的 selected provider/reward，Thompson draw 使用固定 policy seed。
4. 用 `(fold, seed, delay, tick, entity_id)` 构造 decision ID，并逐条比较重算 selected provider 与 raw decision 的 `provider_id`。

## 结果

- 60 streams（5 folds × 2 seeds × 3 delays × 2 methods）。
- 8,664 decisions 全部重放。
- action mismatches：**0**。
- 说明现有 checkpoint、输入、源码和 seed 足以唯一重建 Frozen/Bayesian action 序列；正式 `recover_pharma_fingerprint_online_diagnostic.py` 尚未包含这一步，但缺少 action replay 不是当前结果必须重跑的理由。

## 边界

这项旁路只证明 action/state update 的数值路径可重建；它不改变 raw JSONL 仍缺 `decision_reads`、released feedback IDs、state hashes 以及 terminal 原始快照的事实。旧日志应保留原样；若纳入正式收据，应将本检查脚本、命令和环境一起固化。

附加核对：online `recovered_v2` 的 2,000 条 BT+CLS epoch 事件与 static `recovered_v4/raw.jsonl` 按 `(fold, seed, epoch)` 逐项比较，`train_loss`、`validation_loss`、`best_epoch` 均 2,000/2,000 exact match（max absolute diff = 0）。这进一步绑定了 online run 与已恢复 checkpoint 的训练路径。
