# ADR-0042：post-call numeric confidence 作为 verification/refusal 候选信号

- 状态：Accepted
- 日期：2026-09-27
- 依据：`task/task49-pharma-benchmark-baseline-freeze/postcall_confidence_screen_v1/report.md`

## 背景

当前 direct pre-call selection 的冻结模型和多种 selected-only 更新都没有稳定超过 Best Fixed。工具调用完成后，缓存返回中还包含 provider 自己的 numeric probability；这个字段在决策前不可见，但可能帮助主 agent 判断结果是否可信。

## 证据

用 `c=2|p-0.5|` 作为无标签 confidence，在 5-fold scaffold-heldout evaluation 上得到 160 个 provider-fold AUC，median `0.760306`。固定 `admetlab_3_0` 的全量准确率为 `0.915575`；evaluation 诊断中保留 confidence 前 80% 的 accepted accuracy 为 `0.956747`，5/5 folds 高于全量。

## 决策

保留 post-call confidence verifier/refusal 作为下一条 CPU 候选。它不回填 direct provider selection，也不能把 evaluation 排序诊断写成在线效果。下一版必须在 fit 区固定阈值或校准器，沿 selected-only delayed feedback 合同回放接受/拒答，拒绝时的覆盖率和错误率同时报告。

## 后果

如果 fit-only 阈值在 evaluation 仍有信号，论文故事可以把“选择工具”和“验证工具结果”分成两个可审查模块；如果信号消失，就停止这条扩展，不用它包装 direct selection 的负结果。
