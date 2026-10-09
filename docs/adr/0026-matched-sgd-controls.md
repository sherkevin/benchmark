# ADR-0026：先用匹配的 Online-SGD / Replay-SGD 隔离普通更新收益

- 状态：Accepted
- 日期：2026-09-27
- 依据：ADR-0021、ADR-0025、Task49 matched controls review

## 背景

当前 33-provider BBB development 任务中，Morgan BT+CLS 表示上的 selected-only BayesianRouter 在线后验更新在 delay 0、1、4 都没有通过预登记闸门。此时直接提出新的动态 JEV 会把表示、更新器和反馈协议混在一起，无法说明收益来自哪里。

## 决策

先实现并比较三个共享同一 validation-best BT checkpoint 的 head policy：

1. Frozen：只用初始 provider logits 选择，反馈只消费不更新；
2. Online-SGD：每条释放的 selected binary reward 对被选 provider 行做一次 BCE-with-logits 更新；
3. Replay-SGD：用固定 FIFO buffer，从已释放的 selected 样本构成预登记 batch，做一次平均 BCE 更新。

第一轮固定 Online/Replay 学习率 `0.05`，不加 anchor、momentum 或梯度裁剪；Replay 固定容量 `64`、batch 上限 `8`、seed `20260927`。三者共享冻结 encoder、evaluation 顺序、反馈延迟和输入信息边界。它们是 baseline，不称为本文创新。

初始化使用 `bt_cls` checkpoint 的 `score.weight`，与前一轮 BayesianRouter 对照一致；该 checkpoint 另有 BCE `auxiliary.weight`，本轮不使用。因此 Online/Replay 是“BT warm start + selected BCE update”，不是 BT 目标的精确后验更新。auxiliary 初始化若要加入，必须作为事先登记的独立消融。

## 理由

这组对照只改变“是否更新”和“是否回放”，保留 provider 表示、冷启动权重和 selected-only 延迟反馈。它能先检验普通 head 更新是否已经解释实时收益，也能避免把标准 SGD 或 replay 重新命名成新方法。

## 后果

- 需要为三条轨迹分别保存逐事件日志、更新次数、状态版本和 buffer 信息；
- Replay 的额外计算必须单独计时，不能与 Online-SGD 直接宣称同成本；
- 若三者均不超过 Best Fixed，当前表示/任务信号不足，应停止堆叠普通更新；
- 只有在这些近邻无法解释现象后，才继续设计动态 JEV 的新机制。

## 证据入口

- 适配合同：`task/task49-pharma-benchmark-baseline-freeze/matched_sgd_contract_v1.md`
- 只读审查：`task/task49-pharma-benchmark-baseline-freeze/matched_controls_review_20260927.md`
- 实现：`src/probe_headless/pharma_matched_sgd.py`
- 真实缓存 smoke：`src/probe_headless/run_pharma_matched_sgd_smoke.py`
