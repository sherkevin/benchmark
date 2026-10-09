# 组合式实时 JEV 候选方案：selected correctness 驱动的真值恢复与结构化更新

- 日期：2026-09-29
- 状态：候选方法卡，不是已冻结方法，不是 confirmation 结果
- 目标：把已有 selected-only 延迟反馈、轻量在线头和 provider 路由整合成一个能运行、能恢复、能被消融的整体机制。
- 详细数学审查：`task/task49-pharma-benchmark-baseline-freeze/composition_20260928/math_review.md`
- 先验/近邻审查：`research/task49_composition_20260928/prior_review.md`

## 1. 一句话

在二元性质任务中，被选工具的二元输出 $z_t$ 和释放后的正确性 $r_t$ 可以恢复当前样本的性质标签 $\tilde y_t$；我们用这个共同标签同时更新共享性质状态和被选 provider 的条件正确性状态，再用全概率公式计算下一次调用前的 provider 分数。

这不是把两个模型并排放在一起：同一个恢复标签、同一个 release event 和同一个决策分数把两个状态连接起来。是否构成论文贡献，要由 matched ablation、integration gain 和独立确认决定。

## 2. 信息合同

每轮选择前可见：分子 $x_t$、端点 $e_t$、菜单 $A_t$ 和已经释放的历史 $H_t$。不可见：本轮真值、未选 provider 输出、本轮 correctness 和未来 release。

选中 provider 成功返回二元结果 $z_t$，并在终止后按 delay 释放 $r_t$。只有满足以下条件才恢复标签：

- provider 输出有效且为二元值；
- correctness 与同一个冻结真值定义绑定；
- 事件已终止且反馈已释放；
- truth 不是 unknown，事件不是 system failure。

失败、unknown、冲突 canonical 和未释放事件不进入恢复损失。当前 runner 必须把 selected output label 绑定到 pending event，release 时传给 learner；不能从 evaluator full panel 回填。

## 3. 候选模型

共享性质状态：

$$
q_\theta(x,e)=P(Y=1\mid x,e).
$$

provider 条件正确性状态：

$$
h_{a,\phi_a}(x,e,y)=P(R_a=1\mid x,e,Y=y).
$$

调用前对候选 provider 的分数：

$$
s_a(x,e)=q_\theta(x,e)h_{a,\phi_a}(x,e,1)+(1-q_\theta(x,e))h_{a,\phi_a}(x,e,0).
$$

这里没有把 provider 彼此独立作为前提。$h_a$ 是每个 provider 的边际条件正确性；共享 $q$ 表示共同的性质状态。只有要建模多个 provider 的联合出错事件时，才需要增加相关性状态。

事件释放后的恢复规则：

$$
\tilde y_t=\begin{cases}z_t,&r_t=1,\\1-z_t,&r_t=0.\end{cases}
$$

候选联合损失：

$$
\ell_t=\operatorname{BCE}(q_\theta(x_t,e_t),\tilde y_t)+\lambda\operatorname{BCE}(h_{a_t,\phi_{a_t}}(x_t,e_t,\tilde y_t),r_t).
$$

每个 release 只更新共享状态和 $a_t$ 对应的 provider 状态。固定编码器、线性头、维度 $d$、菜单大小 $K$ 时，选择成本为 $O(Kd)$，单事件更新为 $O(d)$，状态空间为 $O((K+1)d)$；pending queue 和 snapshot 成本按未释放事件数增加。

**具体 case：** 若 `b3clf_dtree_common` 返回 $z_t=0$，延迟到期后收到 $r_t=0$，则 $\tilde y_t=1$；该 release 更新共享 $q$ 和 `b3clf_dtree_common` 的 $h_a$，不更新当轮未选的 `admetlab_3_0`。

## 4. 整体贡献如何量化

候选方法的 contribution_type 暂定为：`mechanism synthesis + interface/protocol + empirical evidence`。integration_gain 必须在实验卡中选择并冻结：

- 主要目标：未来 selected correctness / cumulative error；
- 次要目标：调用成本、更新 p50/p95、恢复一致性、低反馈 provider 的覆盖；
- 可选扩展：provider/version 事件后的恢复速度。

定义与匹配控制的整体增益：

$$
\Delta_{\mathrm{int}}=U(\mathrm{TRS})-\max\{U(\mathrm{BestFixed}),U(\mathrm{MatchedOnlineSGD}),U(\mathrm{TruthOnly}),U(\mathrm{CorrectnessOnly})\}.
$$

如果准确率不增加，但成本、延迟、覆盖或可恢复性按预登记目标改善，仍可形成整体贡献；必须明确该收益和代价。若所有整体收益均为负，保留负结果，不靠重命名或调参改变结论。

## 5. 必须对照的版本

- Best Fixed：fit-only 选择固定 provider；
- Matched Online SGD：同一编码器、参数量、反馈、delay 和探索权限，只用 selected reward；
- Truth-only：只更新共享 $q$；
- Correctness-only：只更新 selected provider 的 $h_a$；
- No-recovery：不把 $(z,r)$ 变成 $\tilde y$；
- No-integration：分别预测 q/h，但不用 $s_a$ 的概率积分；
- Frozen selector、PILOT/BayesianRouter-adapted：最近邻和现代在线控制；
- Oracle：仅作 full-panel evaluator 上界，不参加可部署排名。

## 6. CPU 证伪卡

第一轮只用现有 development replay，不提交 A800，不读取 confirmation：5 个 scaffold folds、2 个 seeds、delay $0/1/4$，固定编码器和超参，跑 A0/A1/A2/A3/A5。

每条事件记录：选择前可见历史 hash、provider/menu、selected $z$、释放 $r$、恢复标签摘要、release tick、更新前后 state hash、更新耗时和失败语义。独立恢复器不读取 truth store，只从事件恢复 state 和指标。

通过 CPU 预筛的必要条件：

1. 有效事件的恢复公式与 evaluator truth 100% 一致；
2. 无未选输出/未来标签读取；
3. 至少在两个 delay 上优于 Matched Online SGD，或在预登记的成本/恢复指标上有明确优势；
4. A5 能区分完整积分和独立排名；
5. 失败、unknown、冲突和 coverage 的边界报告齐全。

停止条件：恢复 mismatch、A5 不优于 A1 且无其他 integration gain、q 指标上升但 selection 不升、或结果只对一个 seed/delay 有利。

## 7. 现有证据和未决问题

已完成的只读日志审计扫描了 10,776 条 terminal events；其中 10,621 条具备二元 selected output、correctness 和 recovered label，恢复公式 mismatch 为 0；155 条因字段缺失或非二元状态被排除，没有被补标签。收据：`experiments/logs/task49_composition_20260928/label_recovery_audit_v1/result.json`。

这只证明现有事件字段满足恢复关系，不证明 TRS 的选择效果。未决问题包括：selected policy 对共享 $q$ 的样本偏差、低覆盖 provider 的条件头如何初始化、provider family 相关错误、冲突 truth、provider version 变化和冻结表示是否足够。

## 8. 不改变的边界

- 仍然是 direct tool selection 主线；post-call verifier 不回填 pre-call selector；
- 反馈仍是 selected-only delayed correctness；不看未选当前输出；
- `feedback-adaptive` 是主线动态范围；`environment-nonstationary` 只有在出现 provider/version/menu/分布事件并明确声明时才加入；
- 这是候选整体机制，不是已经证明的创新，也不是 confirmation 结论。
