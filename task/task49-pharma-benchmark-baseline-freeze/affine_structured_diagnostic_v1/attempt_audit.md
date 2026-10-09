# Affine structured diagnostic v1：中止尝试收据

日期：2026-09-28
状态：不完整；不作为效果结果，不进入 confirmation。

## 发生了什么

- 首次启动因导入路径错误退出，未写入训练事件。
- 修正导入路径后，retry_v2 运行并写入 `run/config.json` 与 `run/events.jsonl`。
- 进程后来退出，但没有生成 `summary.json`，说明主流程没有完成。

## 已保存的证据

- `run/config.json`：配置与源文件 hash。
- `run/events.jsonl`：292 行，约 1.27 MB。
- 事件包含 2 个 `fit_complete`（fold0 的两个 seed）和 290 条 evaluation，预期是 10 个 fit manifest、1,444 条 evaluation。
- 没有 summary、退出码、traceback 或独立验证；因此没有可用于汇总的完整结果，也不能回答“加入 affine intercept 是否改善选择”。
- 独立审计收据：`experiments/logs/task49_composition_20260929/affine_structured_diagnostic_v1/retry_v2/abort_receipt.json`。

## 处理结论

这次尝试标记为 `incomplete_abort`。保留原始文件，不补跑、不从部分事件推断效果。若以后重做，必须先把每个 fit 的权重/状态和 checkpoint flush 到独立文件，再进入全量运行，避免长时间无输出且无法恢复。重做前还需要先讨论是否值得承担新增截距、随机初始化变化和计算成本带来的混杂。
