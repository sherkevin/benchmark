# Affine 诊断 retry_v2 只读审计

日期：2026-09-28
状态：不完整，不能用于 go/no-go 或方法结论。

## 检查结果

- 当前没有残余进程。只读执行 `ps`，没有匹配 `run_affine_structured`、`affine_structured` 或本轮任务目录的进程。
- `retry_v2/run/config.json` 记录了输入 SHA、两个源文件 SHA、冻结 encoder 路径和启动时间 `2026-09-28T11:17:40.296525+00:00`。
- `retry_v2/run/events.jsonl` 是合法 JSONL，大小 1,274,533 bytes，SHA-256 为 `a6f65f6686530408b4e09e78fac272b9999e62571fdaa6b7bce6576f4d3b5479`。
- 事件只有 `fit_complete=2` 和 `evaluation=290`：`fold0_seed0`、`fold0_seed1` 各完成 145 条评价。没有 fold 1–4 的拟合或评价。
- 预注册设置要求 5 folds × 2 seeds，即应有 10 个 fit 完成事件和 1,444 条评价；因此实际只完成一小部分。
- 两个 stdout 文件均为 0 bytes；没有保存 traceback、终止信号或退出码，也没有 `summary.json`、独立验证结果或最终汇总。

## 结论与收据

这不是一个失败结果，也不是一个通过结果；它是一个**不完整的执行尝试**。不能从现有文件判断终止原因，更不能把它解释为 affine 方案的科学负结果。部分事件、配置和哈希必须原样保留，不能从该目录续跑或补写成完整结果。

为避免后续误读，已在同一 attempt 目录新增 [`abort_receipt.json`](../experiments/logs/task49_composition_20260929/affine_structured_diagnostic_v1/retry_v2/abort_receipt.json)，记录观测到的进程状态、事件计数、期望计数和“退出原因未知”的边界。完整逐事件证据仍在 `run/events.jsonl`。

后续如果还要做这个诊断，应新建 attempt 目录，先修复“每个 fit 完成后才 flush、stdout 不落盘、无 finally 收据”的可观测性问题，并在启动前写入 PID/命令，在正常结束、异常和信号退出时都写终止收据。没有这些证据，不应提交新的 GPU 任务或把 partial run 写进论文。
