# Laya 表示提取 v3：完成记录

日期：2026-09-27

## 结果

任务 `01a0de81ce4269ebebc6000c40ce9ae2` 在 Nebula `damo_genomic_gene_a800` 的一张 NVIDIA A800-SXM4-80GB 上成功完成。执行源码固定为 `e9d394377862a2c873735f4cd29d5d72670d9d0a`，模型权重和 tokenizer 的 hash 与 `asset_manifest.json` 一致。

- 730 个 BBB 开发请求；每个请求 33 个 provider。
- 1024 维 candidate-marker 表示，92 个 shard（91 个 8 行，最后 2 行）。
- `events.jsonl`、`raw.jsonl`、`summary.json` 均已写入 OSS。
- Nebula worker 记录 92 次 `batch_complete` 和 1 次 `complete`，退出成功。
- 模型运行时间 45.07 秒；峰值 allocated 2,603,155,456 bytes。
- warm-up 之后单请求 forward p50 约 19.44 ms，total p50 约 24.88 ms。这只是本次批处理的量级记录，不是服务端 SLA。

## 完整性核对

OSS 前缀 `oss://ai4sci-develop-storage/jingwu/benchmark/task49/laya_features_v3/` 列出 96 个对象，其中 1 个目录标记，95 个内容对象：92 个 shard 加上 3 个 JSONL/JSON 文件。用 ossutil 对全部内容对象读取 CRC64，命令输出和完整 Nebula 日志均保存在 `experiments/logs/task49_laya_representation_20260926/remote_v3/`。

完整 worker/scheduler 日志已下载并脱敏。模型 forward 中把 marker 重新送入原 scorer；若 logits 不一致，入口会直接失败，本次没有触发该失败。输出脚本还检查了有限值、行数、provider 维度和逐 shard SHA-256，并把这些值写进 `summary.json`。

## 结论边界

这一步只证明固定 Laya checkpoint 能在未截断的真实输入上稳定导出表示。它没有使用真值或正确性标签，没有训练选择器，也没有比较任何 baseline。因此不能据此声称 Laya 选择效果、在线学习效果或最终方法成立。

下一步是在相同 scaffold 划分和信息权限下，用这些表示做冻结 head 与在线更新的开发诊断；先和 train-only Best Fixed、Morgan head、PILOT/BayesianRouter 适配器对齐，再决定是否值得进入确认集。

证据入口：

- `experiments/logs/task49_laya_representation_20260926/remote_v3/download_manifest.json`
- `experiments/logs/task49_laya_representation_20260926/remote_v3/full_logs/`
- `experiments/logs/task49_laya_representation_20260926/remote_v3/oss_verify/remote_crc64_all.log`
