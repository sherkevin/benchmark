# Context residual memory v2：结果与裁定

## 结果

使用已恢复的 Task49 BBB development fit snapshots（5 folds、2 seeds、delay `0/1/4`），没有训练、没有新工具调用。每个 provider 的 BT score 只在 outer-fold fit 区做固定 affine logistic calibration；evaluation 只释放已选 provider 的 `system_success`。

| 方法 | delay 0 | delay 1 | delay 4 |
| --- | ---: | ---: | ---: |
| Frozen calibrated | 0.8831 | 0.8831 | 0.8831 |
| Independent residual bias | 0.8769 | 0.8769 | 0.8713 |
| Context memory | 0.8755 | 0.8727 | 0.8700 |
| Random memory | 0.8727 | 0.8769 | 0.8699 |

Context memory 相比 independent 的差值依次为 `-0.0014/-0.0042/-0.0014`；三个 delay 都没有超过 independent，delay 1 也低于 random memory。预登记闸门失败。

## 裁定

这次结果不支持把 context residual memory 写成主方法，也不支持继续调容量、邻居数或 kernel。v1 的正向数字不能再作为效果证据，因为它使用了未校准排序分数。该构造停止，保留代码、协议和负结果；confirmation 继续关闭。

下一步回到方法设计：在已经校准的 correctness score 和 selected-only delayed feedback 合同上，寻找有明确统计假设的更新机制；在新机制没有通过小规模 CPU falsification 前，不占用 A800。

证据：`experiments/logs/task49_context_memory_calibrated_replay_20260927/results_v1.json`、`summary_v1.json`；代码：`src/probe_headless/replay_pharma_context_memory_calibrated.py`。
