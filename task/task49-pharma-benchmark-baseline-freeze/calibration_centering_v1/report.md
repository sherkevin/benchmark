# 分数修正 × 在线中心化：六格闭环诊断结果

日期：2026-10-07。**状态：真实 v1 已完成并通过独立 verification，0 issues。** 独立核验覆盖 180 条流、25,992 次决策、77,976 条事件与 90 对因果前缀；验证回执见文末。

## 科学问题与结论

本轮检验：上一轮发现的 fit 分数基准偏移，是否真的限制在线更新？先修正 class-balanced logistic 的分数尺度，再将按平均正确率中心化（M）与按调用时冻结分数中心化（Q）做闭环对照；每种分数各配自己的 Frozen（F）。这次使用各策略实际生成的 selected-feedback 轨迹，区别于前两轮只改动作的固定历史 shadow。

本轮结论：类别权重逆变换改善了当前 development OOF 的 Brier 和 log loss，但 Q 的在线增量很小。corrected Q 相对 corrected M 在自然/受控流分别为 **+0.51/+0.37 pp**，两个环境的五个留一折效果均为正，却没有任何一个环境达到预冻结的 **1 pp pooled** 门槛，因此**不升级 Q 为下一候选，不认定全面替代原方法**。标准类别权重修正不是新算法贡献。

本次固定 180 条流、25,992 次决策、722 个 development 分子、三种延迟；实际耗时 8.895 秒，0 provider forward、0 API、0 GPU。每个表格 cell×环境的分母为 2,166（722 分子×3 delay）；pooled 在环境内按分子数加权合并折，再对 delay 等权，natural 与 stress 不合成总分。

## 六格绝对效果与匹配 Frozen

accuracy 为百分比；ΔF 为相对同分数版本 Frozen 的百分点差。F/M/Q 分别是无更新、平均正确率中心化、调用时冻结分数中心化。

| cell | 自然 accuracy | 自然 ΔF (pp) | stress accuracy | stress ΔF (pp) | stress 后半 accuracy |
|---|---:|---:|---:|---:|---:|
| raw / F | 79.36% | +0.00 | 69.11% | +0.00 | 56.63% |
| raw / M | 79.18% | -0.18 | 77.29% | +8.17 | 73.11% |
| raw / Q | 78.49% | -0.88 | 75.95% | +6.83 | 70.63% |
| corrected / F | 80.19% | +0.00 | 63.30% | +0.00 | 45.58% |
| corrected / M | 79.82% | -0.37 | 78.30% | +15.00 | 75.51% |
| corrected / Q | 80.33% | +0.14 | 78.67% | +15.37 | 75.97% |

raw M 的自然收益为 −0.18 pp，raw Q 为 −0.88 pp；query 中心化本身并没有解决自然流的有害切换。corrected Q 在自然流仅比对应 Frozen 多答对 **3/2166**，即 +0.14 pp；不能把 corrected Q 比 raw Q 的跨分数差全部解释为在线机制提升。

corrected Frozen 相对 raw Frozen 的自然增量为 +0.83 pp，说明部分变化来自静态打分；同一变换在受控流的 Frozen 上反而为 −5.82 pp。原因边界需要结合下一表的实际目标暴露，不能只比较两种分数版本各自很不相同的 Frozen 基线。

## stress 暴露与在线恢复

每折 target 只由 fit 正确率 argmax 固定，后半段选择 target 时才翻转返回 label。全程选择计数的分母为 2,166，后半计数的分母为 1,086；stress 后半 target 计数等于实际被翻转的调用数。

| cell | 自然 target 全程/后半 | stress target 全程/后半 | stress 后半暴露率 |
|---|---:|---:|---:|
| raw / F | 666 / 378 | 666 / 378 | 34.81% |
| raw / M | 335 / 177 | 237 / 79 | 7.27% |
| raw / Q | 396 / 248 | 226 / 78 | 7.18% |
| corrected / F | 1170 / 618 | 1170 / 618 | 56.91% |
| corrected / M | 543 / 248 | 380 / 85 | 7.83% |
| corrected / Q | 610 / 332 | 378 / 100 | 9.21% |

corrected Frozen 的后半 target 暴露为 618/1086，raw Frozen 为 378/1086；所以 corrected Q 的 +15.37 pp 匹配 Frozen 增量不能单独证明跨分数版本的在线恢复更强。绝对 stress accuracy 为 corrected M 78.30%、corrected Q 78.67%，对应后半为 75.51% 与 75.97%；Q 对 M 的小增量伴随 target 后半选择 100 次而非 85 次，并非仅靠更少触碰 target 得到。这里仍是固定 evaluator 翻转 stress，不是自然漂移证据。

## 同分数 Q−M 与预冻结资源线

| 分数 | 自然 Q−M | stress Q−M | 自然五个 LOO 范围 | stress 五个 LOO 范围 |
|---|---:|---:|---:|---:|
| raw | -0.693 pp | -1.339 pp | [-1.098, -0.346] pp | [-2.080, -0.809] pp |
| corrected | +0.508 pp | +0.369 pp | [+0.347, +0.635] pp | [+0.115, +0.750] pp |

raw Q 未满足自然相对 Frozen 不负，且 Q−M 在两个环境均为负。corrected Q 满足自然 ΔF≥0、stress ΔF>0、两个环境 Q−M 非负、两组五个 LOO 均不反号；**失败项是两个环境的 Q−M pooled 都小于 1 pp**。不以某一个 delay 的正数绕过该门槛。

corrected Q−M 的各折与各延迟如下；stress 有负折和负 delay，不能写成所有条件一致获益。完整 raw 分层、所有 rescued/harmed、实际 selected Brier 与绝对前后半结果均保存在 paired_comparisons.json。

| 分组 | 自然 corrected Q−M (pp) | stress corrected Q−M (pp) |
|---|---:|---:|
| fold0 | +0.000 | +1.149 |
| fold1 | +1.149 | +0.920 |
| fold2 | +0.463 | -1.157 |
| fold3 | +0.000 | -0.463 |
| fold4 | +0.926 | +1.389 |
| D=0 | +0.139 | -0.277 |
| D=1 | +0.831 | +1.385 |
| D=4 | +0.554 | +0.000 |

## OOF 分数审计

以下只统计自然 OOF 全面板 722×4=2,888 个 provider–molecule 格，不是 selected-feedback 在线得分，也没有把未选标签送给 updater。

| 分数 | 平均分数 | 真实正确率 | 分数−正确率 | Brier | log loss |
|---|---:|---:|---:|---:|---:|
| raw | 0.530717 | 0.771814 | -0.241098 | 0.234888 | 0.661950 |
| corrected | 0.783432 | 0.771814 | +0.011617 | 0.173292 | 0.533768 |

五折的 Brier 与 log loss 均下降，支持在这个 development 面板上使用更合理的分数尺度。修正后仍有折间均值偏移（例如 fold0 约 +0.1035，fold3 约 −0.0907）；均值差缩小和 proper score 改善不等于条件校准已被证明，也不保证 greedy selected-history 的 query 残差无偏。

## 下一步与可复用证据

保留全部六格和负结果；不继续扫 prior count、学习率、stress 位置或按正 delay 重新选结果。本轮不替换已报告主方法或 157 分子 confirmation。下一步回到**可预测 headroom 与 greedy 反馈缺失**：哪些可救错误能由合法 pre-choice 特征识别，哪些 provider 因长期未被选择而缺乏可校验反馈？若要实测，先给出可证伪主张和新的冻结卡，而不是将本轮小正数升级成新算法收益。

可以直接复用已封存 OOF 分数、native 概率与五折 priors，以及本次 77,976 条事件中的完整 state/queue/q/counter/timer。这些资产已落盘，后续研究不需要重复 provider forward。当前数据经过多轮 development 使用、分子跨 delay 重复、fit folds 重叠；不作显著性、独立样本 CI 或未知 provider 泛化结论。

## 执行与验证状态

- [固定卡](experiment_card.md)，SHA-256 `2d64fc3b997d92eb4542044df559675e7dc3029a837fcb86061366f2e40af3d5`。
- 原 raw F/M natural 共 30 条流由 runner 复现，最大分数误差 2.22e-16；90 对 natural/stress 在首次受影响反馈释放前通过 runner 的状态/动作/分数相等检查。随后独立 verifier 从封存缓存和事件重新核对原 30 条 legacy 行为、完整 counter/pending/state/hash、队列时钟及全部聚合，结果 verified、0 issues。
- 先前 6 项合成单元测试通过，准备记录位于 `experiments/logs/task49_calibration_centering_20261007/prepare_v1/`；科学 run 由 root 启动，本报告阶段没有新运行。
- 正式 run：`experiments/logs/task49_calibration_centering_20261007/v1/`；命令 `/Users/jingwu/work/benchmark/.srcprobe/beir_venv/bin/python -B -m src.probe_headless.run_calibration_centering_v1 --execute`。
- `events.jsonl` 保存完整逐事件；`stream_summaries.json`、`paired_comparisons.json`、`oof_score_audit.json`、`behavior_checks.json` 保存结果。policy_operation timer 不含 evaluator/queue/snapshot/hash/编码/I/O；state bytes 指 canonical JSON，不是进程内存。
- 本报告读取的配对、OOF、stream 和 config 文件 SHA-256 均与 summary.artifact_sha256 相符。
- summary SHA-256：`791a4cd25a0fe5215914aa9f7105b0bef68d7155da59767336edfa9461b8d7ae`。
- paired_comparisons SHA-256：`b1a0ed97085cd26f9c2c63d3864fc1b050a84a8871b1e65c1a1706ad7be2d966`。
- oof_score_audit SHA-256：`f365ceedef563b7af8f40da50731d671295b91f9f849c4e2f5d99198a06bc8f3`。

- 独立验证：`experiments/logs/task49_calibration_centering_20261007/v1/independent_verification.json`，SHA-256 `ed43f414806f21258f5e035e032f2339d8a310f7def710cb4f5326a28e8afd44`。该验证证明收据与封存缓存一致，不额外证明模型训练来源或条件校准；原 summary 的 pending 字段保留为运行结束时状态，后续状态以此独立回执为准。

原始事件另存为可逐字节还原的gzip，原始文件仍保留本地；归档和恢复说明见 `experiments/logs/task49_calibration_centering_20261007/README.md`。独立checker的40项准备测试和原始版本链均保留。
