# Post-call verifier threshold replay v1

日期：2026-09-27

## 问题

在调用前固定训练折 Best Fixed provider，调用后根据该 provider 返回的 numeric confidence 决定接受或拒答，能否在 scaffold-heldout evaluation 上提高 accepted accuracy？这一步只验证静态 verifier，不更新 JEV。

## 固定协议

- 每个 outer fold 的调用 provider 由 fit 区 `system_success` 均值选择；当前应是具体 provider `admetlab_3_0`，但脚本从 fit 重新计算，不硬编码结果。
- confidence 为调用后 `2*abs(raw_probability-0.5)`。目标 fit coverage 固定为 `0.8`：按 fit confidence 从高到低取前 `floor(0.8*n_fit)`，阈值为最后一个被接受样本的 confidence；evaluation 只执行 `confidence >= threshold`，不读取 evaluation 标签调阈值。
- 记录 accepted coverage、accepted accuracy、rejected error rate 和全量 always-accept accuracy。随机拒答对照使用同一 evaluation 数量、同一目标 coverage、seed `20260927`，不看 confidence。
- 工具返回失败时直接拒答并单独记账；当前 Best Fixed provider 的缓存状态仍需由脚本核对。

## 进入标准

confidence verifier 相对随机拒答在至少 4/5 folds 的 accepted accuracy 更高，且 pooled accepted accuracy 至少高 `0.02`，才进入下一步 selected-only delayed online calibration。否则停止 verification confidence 主线，不申请 A800。

## 边界

拒答不是 direct selection 的正确性成功；coverage/accuracy 分开报告。没有新 provider 调用，没有 confirmation，没有在线标签更新。所有阈值和随机种子在运行前固定。
