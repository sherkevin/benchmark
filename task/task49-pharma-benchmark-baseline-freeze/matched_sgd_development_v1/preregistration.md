# Task49 matched SGD full development v1

日期：2026-09-27
状态：预登记；confirmation 保持关闭。

## 要回答的问题

在同一个 scaffold-disjoint BBB development cohort、同一个冻结 BT representation/head 和同一个 selected-only delayed-feedback 合同下，普通的 Online-SGD 或 bounded Replay-SGD 是否已经能稳定改变 direct provider selection。这个实验是 baseline qualification，不是 Proposed dynamic JEV 的效果证明。

## 数据与划分

- 输入：`fingerprint_diagnostic_v1/inputs.json`，SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 722 个已解析的 BBB development 分子，33 个 provider；8 个 unresolved 不进入输入。
- 每个 outer fold 的 fit/validation/evaluation 为 scaffold-disjoint；evaluation 总数为 722。
- fit/validation 可以使用完整 provider panel；在线 evaluation 只允许读取被选 provider 的 `system_success`，失败保留为 0，未知真值不转换成标签。

## 策略

1. `frozen_matched_sgd`：共享同一个 validation-best BT checkpoint，只选择，不更新。
2. `online_sgd_matched`：每个释放的 selected feedback 做一次 BCE logistic gradient step，`lr=0.05`，无 anchor/momentum/clip。
3. `replay_sgd_matched`：当前 selected sample 加释放前 FIFO 中最多 7 个历史样本，batch=8，capacity=64，均值梯度一步，seed=`20260927`，`lr=0.05`。

三者都从 `bt_cls.score.weight` 初始化，encoder 和未选 provider 行冻结；这不是精确 BayesianRouter 后验。Replay 的额外计算单独报告。

## 轨迹与预算

- folds=`0..4`，seeds=`0,1`，delays=`0,1,4`；每个 fold/seed 训练 200 epochs，validation loss 选择 checkpoint。
- 5×2×3×3=90 条 stream；决策与反馈各 12,996 条；fit snapshot 10 份；epoch 2,000 条。
- 一次只提交一条 Nebula A800-SXM4-80GB 任务；确认集不在代码、输入、命令或 OSS 中。

## 主要报告

按 fold/seed/delay 报告 selected accuracy、cumulative error、前半/后半 accuracy、与同一 stream Frozen 的 paired delta；同时报告 selection/update p50/p95。先汇总每个 seed 的 5-fold OOF，再报告跨 seed 的均值和区间，不用 evaluation 结果选超参。

## 事先写下的裁定

- 如果 Online/Replay 没有稳定超过 Frozen/Best Fixed：当前 Morgan BT representation 下普通 selected-only adaptation 不足，不能继续把它包装成动态 JEV 贡献。
- 如果只有 Replay 改善且 update 成本明显更高：按更高计算预算 baseline 报告，不归因于实时机制。
- 只有通过完整日志下载、source/input hash、fit snapshot 和独立 NumPy replay 后，才允许把结果写进 development report；confirmation 仍关闭。
