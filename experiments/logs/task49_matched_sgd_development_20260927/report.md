# Task49 matched SGD full development report v1

日期：2026-09-27  
状态：真实 Nebula A800 运行完成，完整日志下载并脱敏，独立 NumPy 回放通过。  
范围：development baseline comparison；confirmation 保持关闭。

## 这次到底跑了什么

- 代码 commit：`2017967daf80e5f58fa8c51ac2f2bb4e09f53c5d`。
- 输入：`fingerprint_diagnostic_v1/inputs.json`，SHA-256：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 5 个 scaffold-disjoint outer fold，2 个 seed，3 个反馈 delay（0、1、4）。
- 共享同一个 validation-best BT checkpoint：`score.weight` 初始化、64 维冻结 hidden、33 个 provider。
- 三个策略：Frozen、selected-only Online-SGD（lr 0.05）、selected-only Replay-SGD（capacity 64，batch 8，lr 0.05）。
- 90 条 stream，12,996 条 decision，12,996 条 feedback，10 个 fit snapshot，2,000 条 epoch 事件。
- A800-SXM4-80GB；worker 正常退出。完整日志 archive SHA-256：`a65b0df8a0de00418267746975208c054004937568df1850fc2aee26456847e8`。

## 结果

整体 pooled accuracy（4332 个 evaluation 分子）：

| policy | accuracy | 与 Frozen 的 paired delta | 与 Best Fixed 的差值 |
|---|---:|---:|---:|
| Frozen | 0.8913 | — | -0.0242 |
| Online-SGD | 0.8991 | 0.0078 | -0.0164 |
| Replay-SGD | 0.8964 | 0.0050 | -0.0192 |
| Best Fixed（fit-only） | 0.9155 | — | — |

Online-SGD 相比 Frozen 的 paired improvement 是 `0.0078`，30 条 paired stream 中正向 13、持平 7、负向 10。Replay-SGD 的 improvement 是 `0.0050`，正向 12、持平 7、负向 11。

按 delay 的 pooled accuracy：

| delay | Frozen | Online-SGD | Replay-SGD |
|---:|---:|---:|---:|
| 0 | 0.8913 | 0.9024 | 0.8947 |
| 1 | 0.8913 | 0.8975 | 0.8982 |
| 4 | 0.8913 | 0.8975 | 0.8961 |

Best Fixed 的 pooled accuracy 是 `0.9155`，Oracle 是 `0.9972`。三个动态 head 都没有超过 Best Fixed：Online-SGD 低 `0.0164`，Replay-SGD 低 `0.0192`。

## 解释边界

这不是 Proposed dynamic JEV 的结果。它只说明：在当前 Morgan BT representation 上，selected-only 的普通 head 更新可以把 Frozen warm start 往上推一点，但还不足以超过 fit-only Best Fixed；Replay 也没有稳定优于 Online-SGD，且平均 selection p95 更高（约 50.4k ns，相比 Online 46.3k ns）。因此不能把“在线更新有效”直接写成论文贡献，也不能继续在这组普通更新上调参。

这次恢复器第一次运行因为残留了 smoke 的“只允许 3 条 stream”检查而失败；修复后没有重跑任务，使用同一份全量日志完成第二次恢复。恢复器独立解码 10 个 fit snapshot，并用 NumPy 重放全部选择、反馈绑定、延迟释放和 Online/Replay 更新。

## 证据入口

- 预登记：`task/task49-pharma-benchmark-baseline-freeze/matched_sgd_development_v1/preregistration.md`
- 运行计划：`task/task49-pharma-benchmark-baseline-freeze/matched_sgd_development_v1/run_plan.md`
- 全量脱敏日志：`full_download_sanitized_v1/`
- 独立恢复：`recovered_v2/summary.json`、`recovered_v2/events.jsonl`
- 可复现分析：`analysis_v1.json`、`src/probe_headless/analyze_pharma_matched_sgd_development.py`

下一步应回到方法问题：要么换能表达“provider correctness”的表示/目标，要么设计有明确信息优势的动态 JEV；确认集仍然关闭。标准 Online-SGD、Replay-SGD 和 Morgan BT head 不再作为创新候选。
