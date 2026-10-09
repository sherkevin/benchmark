# 动态 JEV 数学建模 v4：可实现的最小问题

**状态：** 当前可实现性审查版。  
**核心判断：** Task36 的环境和回放可以直接承载这个模型；完整 Bayes-optimal 策略可以作为参考定义；动态 JEV 的在线算法还需要补上反馈队列和在线 head。  
**原则：** 只把任务一开始就存在的对象当作原始量。所有状态、标签、策略和价值都从原始量计算得到。

## 1. 我们只研究一个最小任务

给定一个分子和一个性质，系统可以依次调用具体 provider。每次只看到被调用 provider 的返回。系统最后提交一个已经看到的预测，或者拒答。实验真值在之后到达。

第一版固定以下边界：

- 二分类 BBB；
- 每个 episode 最多调用两次；
- 同一个 provider 不重复调用；
- 不融合多个 provider 的结果；
- 先固定终止规则，再研究动态 JEV 的选择和复核。

这样第一组实验只回答两个问题：

1. 只观察已调用 provider 的反馈，在线更新能不能改善下一次选择；
2. 已经看到一个结果后，再调用第二个 provider 有没有可识别的收益。

## 2. 原始量和真实 case

原始量只有：

| 符号 | 含义 | z002905 的实例 |
|---|---|---|
| $x$ | 分子和目标性质 | z002905 的 canonical SMILES，性质 BBB_prediction |
| $\mathcal P$ | 允许调用的具体 provider 集合 | DeepPurpose CNN、DeepPurpose Morgan 等 |
| $a_t$ | 第 $t$ 步动作 | CALL(CNN)、CALL(Morgan)、ANSWER($\hat y$)、ABSTAIN |
| $o_t$ | 被调用 provider 的即时返回 | status=ok，value=0.0030845485161989927 |
| $y$ | 实验真值 | 1 |
| $B$ | 最大调用次数 | 2 |
| $\Delta$ | 真值反馈的回放延迟 | 终止后按预设事件步到达 |

entity_id 只用于日志和对账，不进入策略特征。

### Case 0

$$
x=
\left(
\text{canonical SMILES of z002905},
\text{BBB\_prediction}
\right),
\qquad
y=1,
\qquad
B=2.
$$

**Case 0：** 实际 SMILES 是：

$$
\text{NCC[C@H](O)C(=O)N[C@@H]1C[C@H](N)[C@@H](O[C@H]2O[C@H](CN)[C@@H](O)[C@H](O)[C@H]2O)[C@H](O)[C@H]1O[C@H]1O[C@H](CO)[C@@H](O)[C@H](N)[C@H]1O}.
$$

真值 $y=1$ 来自 BBB screening ledger；CNN 和 Morgan 的实际返回来自 Task36 缓存。

## 3. 历史、剩余预算和动作

历史是已经发生的动作与即时返回：

$$
h_t
=
\left(
x,\mathcal P,
(a_1,o_1),\ldots,(a_{t-1},o_{t-1})
\right).
$$

**Case 1：** 第一步调用 CNN 后：

$$
h_2
=
\left(
x,\mathcal P,
\left(
\mathrm{CALL}(\mathrm{CNN}),
o_1
\right)
\right),
\qquad
o_1=
(\mathrm{ok},0.0030845485161989927).
$$

剩余预算由历史计算：

$$
B_t
=
B-
\sum_{j<t}
\mathbf 1[a_j=\mathrm{CALL}(p)].
$$

**Case 1a：** z002905 预设 $B=2$，调用一次 CNN 后 $B_2=1$。

动作集合为：

$$
\mathcal A(h_t)
=
\left\{
\mathrm{CALL}(p):
p\in\mathcal P\text{、尚未调用且 }B_t>0
\right\}
\cup
\left\{
\mathrm{ANSWER}(\hat y),
\mathrm{ABSTAIN}
\right\}.
$$

**Case 1b：** 在 $h_2$ 中，合法动作是继续调用 Morgan、提交 CNN 的标签 0，或拒答。不能再次调用 CNN。

## 4. 工具返回、答案和正确性

工具返回包含状态、值和返回类型：

$$
o_t=(s_t,v_t,k_t).
$$

Task36 回放中：

$$
o_1=(\mathrm{ok},0.0030845485161989927),
\qquad
o_2=(\mathrm{ok},0.977143406867981).
$$

答案由性质和返回类型对应的归一化器把返回值映射成标签：

$$
\hat y_t=g_f(o_t).
$$

对 BBB 概率 provider，$g_{\mathrm{BBB}}(o_t)=\mathbf 1[v_t\geq 0.5]$；对标签 provider，$g_{\mathrm{BBB}}$ 使用其词表映射。

**Case 2：**

$$
\hat y_{\mathrm{CNN}}=0,
\qquad
\hat y_{\mathrm{Morgan}}=1.
$$

工具正确性是等真值到达后才计算的派生量：

$$
C_p\in\{0,1,\bot\},
\qquad
C_p=\mathbf 1[g_f(o_p)=y]
\quad\text{仅当 }s_p=\mathrm{ok}\text{ 且 }y\text{ 可判分}.
$$

另记执行成功标签 $z_p=\mathbf 1[s_p=\mathrm{ok}]$。失败返回更新 $z_p$，不把缺失的正确性标签当成 $C_p=0$。

**Case 2a：** z002905 的反馈为：

$$
C_{\mathrm{CNN}}=0,
\qquad
C_{\mathrm{Morgan}}=1.
$$

未调用的 provider 没有 $C_p$ 标签。

## 5. 延迟反馈和在线状态

一个 episode 终止后产生一条反馈事件：

$$
F_i
=
\left(
i,\,
y_i,\,
\{(p,C_{i,p}):p\text{ 在 episode }i\text{ 中被调用}\},\,
\tau_i,\,r_i
\right),
$$

其中 $\tau_i$ 是反馈到达的事件步，$r_i$ 是到达序号。实现层还要保存 episode id、反馈组 id 和 terminal action，才能支持乱序和 exactly-once 更新。

**Case 3：** 如果 z002905 先调用 CNN，再调用 Morgan，反馈事件包含：

$$
F_i
=
\left(
i,\,
1,\,
\{(\mathrm{CNN},0),(\mathrm{Morgan},1)\},
\tau_i,\,r_i
\right).
$$

如果只调用 CNN 后拒答，反馈中只有 CNN：

$$
F_i
=
\left(
i,\,
1,\,
\{(\mathrm{CNN},0)\},
\tau_i,\,r_i
\right).
$$

第 $n$ 个 episode 开始前，在线参数只能使用已经到达的反馈：

$$
\theta_n
=
U
\left(
\theta_{n-1},
\{F_i:\tau_i<n\}
\right).
$$

**Case 3a：** 如果 z002905 的反馈在下一题之后才到达，那么下一题仍使用 $\theta_n$；反馈到达后才得到 $\theta_{n+1}$。Task36 当前可以用预登记的 $\Delta\in\{0,1,4\}$模拟这个队列，但这属于 delayed-feedback replay，不是现实生产环境的墙钟实时学习。

同一个实验真值产生的多个 $C_{i,p}$ 共享一个反馈组，更新时按 episode 批量处理，置信区间也按分子/episode 聚类。

## 6. 动态 JEV 和主 Agent 的关系

动态 JEV 只输出结构化建议，不直接执行动作：

$$
u_{n,t}
=
J_{\theta_n}(h_{n,t}).
$$

**Case 4：** 对 z002905，$J_{\theta_n}$ 可以根据已看到的 CNN 返回输出“建议继续调用 Morgan、风险较高”等结构化结果；它不能看到真值，也不能直接读取缓存里的 Morgan 返回。

主 Agent 决定真实动作：

$$
a_{n,t}
\sim
\pi_{\phi}
\left(
\cdot\mid h_{n,t},u_{n,t}
\right).
$$

**Case 4a：** 模块实验固定 $\pi_\phi$，只替换 $J_{\theta_n}$；Agent 实验固定 JEV，只替换 $\pi_\phi$。日志同时记录 JEV 建议和 Agent 实际动作。

## 7. 最优参考策略

价值计算还需要两项预先固定的协议常数：每次调用成本 $c_{\mathrm{call}}(p)$ 和拒答成本 $c_{\mathrm{abstain}}$。Task36 当前没有真实价格或 latency 字段，因此第一组实验只能使用公开登记的单位成本；不能把它们解释成真实线上成本。

当前状态下直接作答或拒答的最小损失为：

$$
V_{\mathrm{stop}}(h_t)
=
\min
\left\{
c_{\mathrm{abstain}},
\min_{\hat y\in\mathrm{Seen}(h_t)}
\Pr(\hat y\neq y\mid h_t)
\right\}.
$$

**Case 5：** 对 z002905 的 $h_2$，CNN 已见标签为 0。如果此时直接作答，条件错误概率由 $y=1$决定；如果拒答，损失是预注册的 $c_{\mathrm{abstain}}$。$V_{\mathrm{stop}}(h_2)$取两者较小值。

调用尚未调用的 provider $p$ 的价值为：

$$
Q_{\mathrm{call}}(h_t,p)
=
c_{\mathrm{call}}(p)
+
\mathbb E_{o\sim
\Pr(\cdot\mid h_t,\mathrm{CALL}(p))}
\left[
V_{\mathrm{next}}
\left(
h_t\oplus(p,o)
\right)
\right].
$$

**Case 5a：** 对 z002905，$p=\mathrm{Morgan}$时，期望值需要考虑 Morgan 可能返回的概率、返回后能否降低最终错误，以及调用成本。实际观察到的返回是 0.977143406867981。

单个 episode 的参考最优递推只保留一条：

$$
V(h_t)
=
\min
\left\{
V_{\mathrm{stop}}(h_t),
\min_{p\in\mathcal P\setminus\mathcal P(h_t)}
Q_{\mathrm{call}}(h_t,p)
\right\}.
$$

**Case 5b：** 在 z002905 的 $h_2$ 中，系统只比较“当前停止”和“继续调用 Morgan”，不再额外引入一套同义的 $Q^{\mathrm{answer}}$、$Q^{\mathrm{abstain}}$ 和 $V_t$ 定义。

复核的价值是：

$$
\mathrm{VOI}(h_t,p)
=
V_{\mathrm{stop}}(h_t)
-
Q_{\mathrm{call}}(h_t,p).
$$

**Case 5c：** Morgan 的全局准确率不能代替 z002905 上的 VOI。这里真正要测的是：在已经看到 CNN 返回之后，Morgan 是否带来了足够大的新增信息。

## 8. 目标函数

一个 episode 的损失为：

$$
L_i
=
\mathbf 1[
\mathrm{ANSWER}(\hat y_i)
\land
\hat y_i\neq y_i
]
+
c_{\mathrm{abstain}}\mathbf 1[\mathrm{ABSTAIN}]
+
c_{\mathrm{call}}N_{\mathrm{call},i}.
$$

**Case 6：** 对 z002905：

- CNN 直接错误作答：$L_i=1+c_{\mathrm{call}}$；
- CNN 后调用 Morgan，Morgan 正确作答：$L_i=2c_{\mathrm{call}}$；
- CNN 后拒答：$L_i=c_{\mathrm{call}}+c_{\mathrm{abstain}}$。

长期在线目标为：

$$
\min_{J,U,\pi}
\limsup_{N\to\infty}
\frac{1}{N}
\sum_{i=1}^{N}
\mathbb E[L_i].
$$

**Case 6a：** 同一组 BBB 分子、同一调用预算和同一延迟协议下，比较不同 JEV 更新器的平均 episode loss。不能只比较 provider 命中率。

同时报告风险和覆盖率：

$$
\mathrm{Risk}
=
\Pr(\hat y\neq y\mid \mathrm{ANSWER}),
\qquad
\mathrm{Coverage}
=
\Pr(\mathrm{ANSWER}).
$$

**Case 6b：** z002905 若 CNN 错误作答，会增加 risk；若拒答，会降低 coverage。两者必须同时报告。

## 9. provider 关系的最小解释

第一次选择 provider 时，分别估计边际正确率：

$$
q_p(h_t)
=
\Pr(C_p=1\mid h_t,\mathrm{CALL}(p)).
$$

**Case 7：** z002905 初始没有工具结果，可以分别估计 CNN 和 Morgan 的 $q_p(x)$。

进入复核后，需要条件返回关系：

$$
\Pr(o_p,y\mid h_t,\mathrm{CALL}(p)).
$$

**Case 7a：** z002905 中，CNN 和 ADMET-AI 都输出阴性且共同错误，Morgan 输出阳性且正确。这个例子说明 provider 可能共享错误，也可能互补；它不单独提供相关系数，相关关系需要跨分子估计。

所以第一版不需要给每一对 provider 训练复杂联合网络。先做 selected-only 的边际在线 head；只有随机化复核实验能够识别验证收益后，再增加条件依赖模型。

## 10. 可实现性审查结论

### 已经可以直接跑的部分

Task36 已经提供：

- 冻结分子输入；
- 具体 provider/version 候选集合；
- 显式 provider allowlist；
- 只返回被调用 provider 的回放结果；
- 预算和重复调用检查；
- 终止后的真值判分；
- 未调用 provider 和真值不进入 selector observation。

实际验证结果：

- 88 条 Task36 replay 测试通过；
- 47,450 个 snapshot cells 与原始 CSV 核对；
- 288 个 failure cells 被单独检查；
- z002905 的 CNN/Morgan case 可以重放；
- 真值只在终止后由 evaluator 使用。

### 还不能直接声称的部分

1. Task36 当前是进程内 replay，不是 OS sandbox；
2. 当前延迟是预登记事件延迟，不是生产环境墙钟延迟；
3. Task36 缓存缺少真实工具 latency 字段，不能自行编造调用价格和时间；
4. Bellman 中的条件分布 $\Pr(o_p\mid h_t)$ 尚未由模型估计；
5. AnyJev 的 observe 是周期性重拟合，不能直接当逐条实时更新；
6. 动态 JEV 的 $\theta_n$、pending queue、propensity 和 exactly-once wrapper 仍需实现。

## 10.1 实时性和准确率的可检验边界

如果第一版只更新 selected-only 的对角 logistic/RLS 或 Beta head，每条反馈的更新开销可以控制在 $O(d)$，状态量约为 $O(|\mathcal P|d)$；Task36 的 provider call 本身只是内存查表，预期瓶颈在日志和策略包装层。AnyJev 的 `observe` 是累积历史后的全量重拟合，复杂度随历史增长，必须单独测 update latency，不能和逐条在线 head 混称。

准确率不能从数学定义推出。第一组结果必须同时报告 prequential loss、risk、coverage、unsafe-answer rate、calls/episode、Brier/NLL/ECE 和反馈样本效率，并包含 always-abstain、always-call、fixed incumbent 等边界。如果工具错误高度相关，单独学习 $q_p$ 可能只会学到“都不可靠”，这时应保留负结果，不能把准确率提升写成默认结论。

## 11. 第一组真正可执行的实验

先做三步，不同时学习三个能力：

### M0：单次调用

- $B=1$；
- 固定终止策略；
- 只比较 selected-only 在线 provider head；
- 比较 static JEV、random、fixed incumbent、LinUCB/LinearTS。
- 当前合同实现：`src/probe_headless/m0_dynamic_jev.py`；它提供 delayed selected-only feedback、`q_exec/q_corr`、pending queue、exactly-once 和 seeded propensity。运行命令与原始 JSONL 记录见 `experiments/logs/m0_dynamic_jev_contract_recheck_20260925_*`。

### M1：一次随机复核

- $B=2$；
- 禁止重复调用；
- 在预先定义的 gray-zone 状态随机决定是否调用第二个 provider；
- 固定作答/拒答规则；
- 估计真实 verification gain。

### M2：动态复核

- 在 M1 的随机探针数据上学习 VOI；
- 再比较 dynamic JEV 与 never-verify、always-verify、random-verify。

如果直接同时学习 provider 选择、VOI、拒答和 Agent 行为，三种收益会互相改变，无法归因。

## 12. 最终判定

这个方案分三层：

- **环境模型：可实现。** Task36 已经能承载原始输入、动作、观察、预算和终止反馈。
- **理论参考策略：可定义。** 它是有限时域的预算约束主动信息获取问题。
- **动态 JEV 算法：尚未完成。** M0 合同 runner、M1 固定首 provider + 随机二次 probe runner，以及 M2 HT 置信 gain-gate runner 已实现；它们分别只验证 provider-marginal feedback、delayed verification-gain 状态和 gain/STOP propensity 合同。冻结 Laya/AnyJev 表示、context-conditioned JEV、VOI 和最终多步策略仍需实现。

所以当前不能说“动态 JEV 已经能实时学习”，只能说：

> 我们已经有了一个真实可回放的任务环境，以及一个可以在这个环境上实现和验证的最小数学问题。
