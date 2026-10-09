# Task32 estimand / oracle 更正

本页澄清数学对象，不修改 Task31 已有计分。

## 1. 不能混用两种 advantage

冻结 reward matrix 的逐行差是 `d_i = r_i(challenger) - r_i(incumbent)`。Task31 precision、recall、harm 和逐行 oracle `max(r_i(0), r_i(1))` 都基于这个已实现差值。

随机环境中的条件均值差则是 `tau(x) = E[Y(1)|x] - E[Y(0)|x]`。两者仅在奖励给定完整 context 后确定时相同。一次随机输出的正负号不等于真实均值差的正负号。

因此 benchmark 的 `beneficial-adoption precision` 不得在理论部分悄悄改解释为 `P(tau(X)>0 | adopt)`。主实验仍使用原矩阵口径；理论如使用条件均值，必须另写对象与假设。

## 2. 一个能排除错误论证的反例

同一 context 下，考虑两种联合分布：

| 世界 | (Y(0),Y(1)) 的分布 | 两个 action 的均值 | P(Y(1)>Y(0)) | E[max(Y(0),Y(1))] |
|---|---|---|---|---|
| A | (0,0)、(1,1) 各一半 | 都为 0.5 | 0 | 0.5 |
| B | (0,1)、(1,0) 各一半 | 都为 0.5 | 0.5 | 1 |

只看被选 action 的 reward，两个世界具有相同的可观测边缘分布。即使均值完全已知，任何只依赖 context/过去记录的策略在 B 中平均 reward 仍为 0.5，而逐行 hindsight oracle 为 1。

所以：正的 realized-oracle headroom 不自动证明它能被 context policy 学到；不能从 Task30 的负 IOU 推出所有现有算法或整个方法族不可能有效。需要区分统计噪声、表示不足、冷启动探索成本与模型失配。

该反例是本项目的数学自检，不作为新理论贡献。

## 3. proxy residual 的误差来自哪里

实际 learner 只能用 `W = Y(1) - m0(x)`；其中 m0 是 history-only incumbent value prediction，不是当前样本的 incumbent 真实结果。

因此：

$$E[W\mid x] = \tau(x) + (\mu_0(x)-m_0(x)).$$

在同一个同时成立的事件上，若

$$|\widehat w(x)-E[W\mid x]|\leq\epsilon_c(x),\qquad |m_0(x)-\mu_0(x)|\leq\epsilon_0(x),$$

则仅由三角不等式得到

$$\tau(x)\geq\widehat w(x)-\epsilon_c(x)-\epsilon_0(x).$$

这不是 Ours，也不是 novelty；只是说明基于 proxy residual 的安全结论不能忽略 baseline estimation error。平均校准误差小不自动给出逐 context 的 epsilon0 保证，不能把两者混用。

## 4. 对当前证据的正确读法

Task32 r2 对历史信息隔离作修复，不会因为 RFF/P-LinUCB/ACTS 的某些配置失败就宣称各理论方法族已被排除。H1 的线性 rank-8 构造缺少稳定收益，只否定这次具体构造足以解决任务的假设；不否定所有 residual representation。

H2 offline ranking 在 BBB 小预算存在正增益例外，不能丢掉。它仍不是 passive-stream 策略，也没有给出跨划分稳健性保证。下一项结构诊断应在既有 benchmark 内验证可学习的 baseline-relative value，而不是换数据集或直接构造一个保证赢家的 Ours。

完整实测及限制见同目录 `history_isolation_audit.md`；原始结果见 `data/tool_results/divergence/t32_history_isolation_r2_serial.json`。
