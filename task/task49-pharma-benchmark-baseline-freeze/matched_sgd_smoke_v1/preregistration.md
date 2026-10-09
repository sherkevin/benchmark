# Task49 matched SGD A800 smoke v1

日期：2026-09-27  
代码：`a83e7d7c511b0c8c533bc51ed2d87c446da8c838`  
状态：提交前冻结；只验证远端入口和反馈合同，不估计方法效果。

## 目的

确认同一 validation-best BT checkpoint 下的 Frozen、Online-SGD、Replay-SGD 三条策略可以在 Nebula A800 入口中完成训练、选择、终止、延迟反馈和完整事件输出。

## 固定配置

- 输入：`fingerprint_diagnostic_v1/inputs.json`，SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- fold：0；seed：0；feedback delay：0；BT 训练 epoch：2。
- 选择表示：训练得到的 `bt_cls` encoder hidden；初始 provider 行使用 `score.weight`。
- Online-SGD：learning rate 0.05，selected BCE-with-logits，一条释放反馈一次更新。
- Replay-SGD：learning rate 0.05，FIFO 64，batch 上限 8，seed 20260927；当前样本强制入 batch，释放后入队。
- Frozen：同一初始权重，只消费反馈，不更新。
- confirmation：关闭；不新增工具预测；只使用已经冻结的 BBB development panel。

## 通过条件

必须出现 1 个 `config`、2 个 `epoch`、3 个 `stream_complete` 和 1 个 `complete` 事件；每条流决策数等于 fold 0 evaluation 行数，feedback 数相同，状态不出现非有限值。三条流的初始权重和源码 hash 必须一致，Frozen 更新数为 0，Online/Replay 更新数等于释放的 selected feedback 数。

## 失败处置

入口导入、CUDA、输入 hash、事件守恒或状态复核失败，整条任务只记为基础设施/实现失败，不产生任何 accuracy 结论；保留完整脱敏日志，不重跑直到修正并换新的 run ID。

这条 smoke 的 `fixed_accuracy` 和 `oracle_accuracy` 仅用于核对 evaluator 接线，不能写成方法效果，也不能据此选择学习率、Replay 大小或最终方法。
