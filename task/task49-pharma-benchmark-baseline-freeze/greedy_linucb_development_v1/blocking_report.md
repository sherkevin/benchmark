# Greedy LinUCB 开发回放阻塞说明

日期：2026-09-29

## 当前结论

代码和预登记卡已完成静态检查，但没有运行 60 条流，也没有生成准确率结果。当前任务只能作为开发适配候选，不能进入主表或 confirmation。

## 阻塞原因

1. Gate A 的 BBB 冲突/重叠政策仍是候选，`gate_a_conflict_policy_v1` 尚未由双方确认；旧 722 条面板是已检查过的 development 输入，不能在政策未冻结时重新命名为正式主结果。
2. fit ridge 均值加 $P=I$ 是允许共同 fit 标签后的 warm-start 适配，不是原 Greedy LinUCB 的零先验原算法。它必须和其他 baseline 共享同样的 fit-only 权限，并在统一 baseline contract 中单独列出。
3. 本实现的两个 seed 槽位对确定性 LinUCB 是相同的；它们不提供两个独立随机重复。若最终统计需要 seed 方差，必须改成预登记的随机策略或另写“重复执行而非独立 seed”的统计口径。
4. 现有 runner 已能读取真实缓存，但统一主矩阵的公平合同、provider lineage 和冲突清洗尚未全部冻结，因此本轮效果回放的资源收益不足。

## 已完成的可复核工作

- `GreedyLinUCB` 增加可选 `prior_mean`，默认路径仍是零先验；原有 21 个数值/反馈测试通过。
- fit-only ridge prior 计算通过预检：5 个 fold，形状均为 `33×1024`，只读 fit 行。
- CPU runner 和独立 numpy verifier 已写好；verifier 不导入 runner 或 policy 实现。
- 未使用 evaluation 标签调参，未启动 Nebula/A800，未调用外部 API。

## 解阻塞条件

1. 确认 BBB conflict exclusion 及 confirmation manifest 版本；
2. 在统一 baseline 合同中确认 fit-prior 适配与 zero-prior canonical control 的角色；
3. 确认 seed 槽位只作为协议重复，或为所有方法预登记同样的随机顺序；
4. 完成上述确认后，才运行候选 CPU 回放并让 verifier 生成 `verified`/`blocked` 收据。

