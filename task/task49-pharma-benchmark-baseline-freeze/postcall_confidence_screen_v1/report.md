# Post-call confidence screen v1：结果

日期：2026-09-27

## 结果

在 5 个 scaffold-disjoint outer evaluation folds 上，读取调用后 provider 返回的 numeric probability，定义无标签 confidence 为 $c=2|p-0.5|$，用它预测该次调用的 `system_success`。

- 160 个 provider-fold 组合有可计算的 held-out AUC；AUC 中位数 `0.760306`，均值 `0.758333`。
- 对固定 `admetlab_3_0`，全量 evaluation accuracy 为 `0.915575`。
- 按 evaluation 内 confidence 从高到低保留前 50%/80%/90%，accepted accuracy 分别为 `0.961111/0.956747/0.950590`，5/5 folds 都高于对应全量 accuracy。

机器结果：`experiments/logs/task49_postcall_confidence_screen_20260927/results_v1.json`；重复运行逐字节 `cmp` 通过。没有新 provider/API 调用，confirmation 保持关闭。

## 解释边界

这是一个信息筛选，不是在线效果实验。confidence 只在 provider 已经被调用后出现，不能用于当前 direct pre-call provider selection。evaluation 内按 coverage 排序也不能直接作为部署阈值；下一步必须用 fit 区固定阈值或校准器，再在 evaluation 流中按 selected-only delayed feedback 运行。

它说明 verification/refusal 扩展有真实的可观测信号：调用结果自身的置信度与独立参考的正确性有关。若 fit-only 阈值回放仍成立，动态模块可以维护“接受/拒答/追加复核”的状态；这条路线与 direct selection 分开报告。
