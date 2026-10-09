# Task49：匹配的 Online-SGD / Replay-SGD baseline 合同 v1

日期：2026-09-27  
状态：适配器与真实缓存 smoke 已通过；尚未运行 A800 效果实验。

## 目的

BayesianRouter 的 selected-only 更新已经没有通过开发闸门。下一步要先回答一个更窄的问题：在相同的冻结 BT 表示和相同的反馈协议下，普通 head 更新是否已经足以解释所谓的实时收益。

这两个方法是 baseline，不是本文创新。它们只更新已释放反馈对应的 provider 行；encoder、未选 provider 行和 auxiliary head 都冻结。

## 共同输入与初始化

每个 fold/seed 使用同一个 validation-best `bt_cls` checkpoint。对请求 $x_t$，冻结 encoder 输出 $h_t\in\mathbb{R}^{64}$；provider $a$ 的初始权重是 checkpoint 中的 $w_{a,0}$。选择分数为：

$$
s_{t,a}=w_{a,t}^{\top}h_t.
$$

实例：若某请求的冻结表示是 $h=(1,2)$，两个 provider 的初始权重是 $w_{A,0}=(0.4,0.1)$、$w_{B,0}=(-0.2,0.3)$，则分数分别为 $0.6$ 和 $0.4$，全菜单下选择 A。这个数值只是接口算例，正式实验使用对应 fold 的 checkpoint。

作出选择时保存该请求的 $h_t$ 副本。反馈到达时只提供被选 provider $a_t$ 和二值系统反馈 $r_t\in\{0,1\}$；未选输出、真值和完整 provider 面板不进入 policy。

这里的 $w_{a,0}$ 明确取 `score.weight`，因为它与前一轮 BayesianRouter 对照使用的 BT provider logits 相同。`bt_cls` checkpoint 还包含一个单独的 BCE `auxiliary.weight`，本轮不切换到它。因而下面是“BT warm start 后使用 selected BCE 更新”的匹配 baseline，不声称是 BT 目标的精确在线后验；若以后比较 auxiliary warm start，必须把它作为预先登记的独立初始化消融，不能看结果后切换。

## Online-SGD

每条 selected feedback 只做一次 BCE-with-logits 梯度步：

$$
\ell_t=\operatorname{BCEWithLogits}(w_{a_t,t}^{\top}h_t,r_t),
$$

$$
w_{a_t,t+1}=w_{a_t,t}-\eta\left[\sigma(w_{a_t,t}^{\top}h_t)-r_t\right]h_t.
$$

其余 provider 行保持不变。第一轮固定学习率 $\eta=0.05$，不加额外 anchor、momentum 或梯度裁剪。

实例：若当前 $w=(0,0)$、$h=(1,0)$、$r=1$，则 $\sigma(0)=0.5$；$\eta=0.05$ 时更新为 $w'=(0.025,0)$。标签只在反馈释放后使用。

## Replay-SGD

Replay-SGD 使用固定 FIFO buffer。每条反馈释放时，先取当前样本，再从释放前的历史 buffer 均匀抽取最多 $b-1$ 条，组成 batch；所有梯度在更新前的同一组权重上计算，并对 batch 求平均后只做一次参数步：

$$
 g_t=\frac{1}{|B_t|}\sum_{(h,a,r)\in B_t}
 \left[\sigma(w_{a,t}^{\top}h)-r\right]h,
$$

$$
w_{a,t+1}=w_{a,t}-\eta\,g_{t,a}.
$$

本轮预登记 $B=64$、$b=8$、采样 seed `20260927`、$\eta=0.05$。当前样本一定在 batch 中；更新后才写入 FIFO，超过容量丢弃最旧记录。Replay 的额外计算量单独报告，不能把它当作实时机制创新。

## 反馈与失败边界

反馈 release tick 沿用 Task42：决策发生在 tick $t$，delay 为 $d$ 时在 $t+1+d$ 释放。每个 event 只能消费一次，状态版本每次 release 增加 1。工具执行失败保留在分母并进入 $r=0$；未知真值不转成错误标签。

Frozen 对照使用同一初始权重，只消费反馈，不修改权重。三者共享 fold、seed、evaluation 顺序、菜单、checkpoint 和 delay $d\in\{0,1,4\}$；由于选择不同，在线阶段收到的 selected labels 可以不同。

## 验收与停止线

先做 NumPy/本地真实缓存 smoke，再考虑 A800。必须核对：

- decision、terminal、feedback 数量守恒；
- feedback 只在合法 release tick 到达，版本转换恰好一次；
- pending event 和 feature hash 不丢失；
- 初始 checkpoint、source hash、输入 hash 一致；
- 不读取未选矩阵、truth 或 confirmation；
- Replay buffer 永远只含已释放的 selected 样本。

如果 Frozen、Online-SGD、Replay-SGD 都不能超过 Best Fixed，结论应收窄为当前表示或任务信号不足；如果 Replay 只凭更高更新成本取得收益，必须按更高计算预算 baseline 报告，不把收益归因于动态 JEV。

实现：`src/probe_headless/pharma_matched_sgd.py`。  
真实缓存 smoke：`src/probe_headless/run_pharma_matched_sgd_smoke.py`。  
单元测试：`src/probe_headless/tests/test_pharma_matched_sgd.py`。
