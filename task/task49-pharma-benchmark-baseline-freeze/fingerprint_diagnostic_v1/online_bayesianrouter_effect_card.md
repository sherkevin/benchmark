# BT 先验 + selected-only BayesianRouter 诊断卡 v1

日期：2026-09-26。状态：待执行的 development baseline 诊断；不打开 confirmation。

## 唯一主问题

在同一个已核验的 Morgan 共享表示和 BT+CLS 离线先验上，BayesianRouter 式 selected-only Gaussian posterior 更新，是否比完全冻结的同一先验降低后续 provider 选择错误？反馈延迟变化时，收益是否仍存在？

这一步闭合的是现代强 baseline 的在线部分，不是 Proposed，也不声称动态 JEV 有效果。

## 方法和权限

- 使用 `fingerprint_diagnostic_v1/inputs.json` 的 722 个 development 分子、33 个 provider 和五个 scaffold-disjoint outer evaluation 流。
- 每个 fold/seed 只用 fit 区训练 64 维 BT+CLS 共享 encoder；用同一 validation 区选择最佳 epoch。evaluation 只在决策后由 evaluator 释放所选 provider 的 `system_success`。
- `Frozen BT prior` 使用 checkpoint 的 BT provider vectors，完全不更新。
- `BayesianRouter online` 使用相同 BT vectors 作为 Gaussian prior mean，论文适配的 prior variance `0.02`、observation noise variance `1.0`；每次只用已终止且到期的 selected reward 做 rank-one posterior 更新。
- 所有流使用预冻结 evaluation 顺序、相同 fit/validation、相同候选菜单和三种反馈延迟 $d\in\{0,1,4\}$。未选 provider 的当题输出永不传给 policy。
- Best Fixed 和 evaluator Oracle 只作控制与上界；失败保持 system success=0，不删除分母。

## 主要记录和否定条件

每个 fold/seed/delay 记录每步 decision version、选择、到期 feedback、后验版本、累计错误、分段准确率、选择/更新耗时和最终状态。主比较是 online BayesianRouter 与 Frozen BT 的配对 accuracy 和 cumulative error。

如果 online 在三个 delay 点都不超过 Frozen，停止把“实时后验更新”作为当前任务的有效机制；如果只在零延迟有效，主张收窄为低延迟开发现象；只有在至少两个延迟点稳定改善且不由训练/表示差异解释时，才继续做完整 baseline 表。无论结果如何，BBB 仍是 development，不打开最终确认集。

## 复现和边界

入口在 `src/probe_headless/nebula_fingerprint_online_diagnostic.py`，使用一条 git-only A800 任务。实验保存逐事件 JSONL、配置、源文件 hash 和按 stream 的摘要；不调用新的科学工具、LLM 或外部 API。该任务结果只用于确定下一步 baseline/方法顺序。
