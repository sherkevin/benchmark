# Laya 静态表示诊断报告

状态：**已完成恢复和独立核验；预登记闸门未通过。**

这次实验只回答一个开发问题：把原来的 Morgan 指纹换成 Laya 的 candidate-marker 表示后，能不能在当前 33 个具体 provider 的直接选择任务上学到更好的静态选择器。它不是动态 JEV、不是在线更新实验，也不是最终确认集。

## 实验合同

- 父数据：722 个有 BBB 参考标签的分子，33 个具体 provider；8 个没有参考标签的请求不进入训练或评价。
- 表示：Laya v3 的 1024 维 candidate-marker，730 个无标签请求，92 个 shard。Laya 输出菜单按字典序排列，训练时按父 benchmark 的历史 provider 顺序重排。
- 划分：固定的 5 折 scaffold-disjoint 外层评价；每折再用 fit/validation 选择 epoch，evaluation 只在最后使用。
- 模型：`linear_bce`、`mlp_bce`、`bt_cls`、`direct_property`，每个模型 2 个 seed，共 40 次 fit，每次 200 个 epoch。
- 预登记继续条件：MLP 或 BT+CLS 相对 train-only Best Fixed 至少提升 0.02，且每个 seed 至少 5 折中的 4 折为正增益。

任务输入清单的 SHA-256 是 `ffde2911dbee66c2960a2d31eeab6690160b734b2b216ccb4e8ea0c978210fd2`，父输入 SHA-256 是 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。Nebula 任务使用提交 commit `e453c0647f58b533bcd075245b7ec1d721920f1e`，实际 GPU 为 A800-SXM4-80GB。

## 证据恢复

任务 `01a0debb7da647dcee0b42564fcc8ced` 的完整 worker stdout 已通过 Nebula `logs download --all` 取回。结构化事件数量为：

| 事件 | 数量 |
|---|---:|
| config | 1 |
| split | 5 |
| epoch | 8,000 |
| checkpoint_manifest | 40 |
| checkpoint_chunk | 1,644 |
| evaluation | 5,776 |
| fit_complete | 40 |
| complete | 1 |

恢复器逐个检查了 40 个 gzip/base64 checkpoint 的压缩 hash、原始 hash和 `weights_only` state dict，重算了每个选择的 provider、输出状态、0/1 reward、fixed 和 oracle 记录，并检查了 validation-only 的最佳 epoch。所有检查通过。

为避免只相信 worker 日志，95 个 OSS 内容对象（92 个 shard 加上 3 个 JSON 文件，共 92,857,171 bytes）被重新下载到本地恢复目录。用这些 shard 和 checkpoint 做了独立 NumPy 前向；CPU 与 CUDA float32 GEMM 的最大绝对差为 `1.64032e-4`，在预先写明的 `5e-4` 数值容差内，5,776 条分数全部通过。

恢复入口：

- 代码：[recover_pharma_laya_static_diagnostic.py](../../../src/probe_headless/recover_pharma_laya_static_diagnostic.py)
- 事件计数：[observed_counts.json](../../../experiments/logs/task49_laya_static_diagnostic_20260927/retry_v3/recovered_v5/observed_counts.json)
- 独立核验汇总：[summary.json](../../../experiments/logs/task49_laya_static_diagnostic_20260927/retry_v3/recovered_v5/summary.json)
- OSS 下载清单：[transfer.jsonl](../../../experiments/logs/task49_laya_static_diagnostic_20260927/retry_v3/recovery_features/transfer.jsonl)
- 完整原始日志：`experiments/logs/task49_laya_static_diagnostic_20260927/retry_v3/full_download/`

## 结果

具体 provider 的 train-only Best Fixed 为 **0.915512**，provider Oracle 为 **0.997230**。Laya 表示的外层 OOF 结果如下，括号内为相对 fixed 的差值，最后一列是正增益 fold 数：

| 方法 | seed 0 | seed 1 |
|---|---:|---:|
| Linear BCE | 0.871191 (-0.044321), 0/5 | 0.879501 (-0.036011), 0/5 |
| MLP BCE | 0.885042 (-0.030471), 1/5 | 0.858726 (-0.056787), 0/5 |
| BT+CLS | 0.875346 (-0.040166), 0/5 | 0.868421 (-0.047091), 0/5 |
| Direct property | 0.713296 (-0.202216), 0/5 | 0.713296 (-0.202216), 0/5 |

四个模型、两个 seed 都没有达到预登记继续条件。作为同协议参考，Morgan 诊断中最好的 MLP 也只有 0.896122/0.890582；因此这不是简单换一个 head 就能解决的问题。

## 裁定

这次结果支持两个结论：

1. 当前 33-provider benchmark 有明确的可救空间，但 Laya 的静态 candidate-marker 表示没有把这个空间学出来。
2. 不再在这条静态 Laya head 线上继续调参，也不把这次结果写成动态 JEV 的效果。在线反馈是否能弥补表示的静态瓶颈，仍需在同一反馈合同下和 PILOT、BayesianRouter、Frozen/Online-SGD/Replay-SGD 做单独实验。

当前保留 Laya shard 和恢复证据，作为后续在线适应的可选表示；在方法冻结前不打开最终 confirmation 集。
