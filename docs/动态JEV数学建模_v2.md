> **更新说明（2026-09-25）：** 公式层级和 Bayes-optimal 参考仍保留；符号最小化、依赖关系和真实 BBB case 以 [动态JEV数学建模_v3_最小符号案例.md](动态JEV数学建模_v3_最小符号案例.md) 为准。

# 动态 JEV 数学建模：贝叶斯最优参考问题与可实现近似

**版本：** v2  
**状态：** 当前数学建模候选；用于确定问题对象，不等于最终 backbone 或训练算法。  
**适用范围：** 医药具体模型/版本的可靠调用、复核、作答和拒答。

## 1. 先说明“最优”是什么意思

在没有预先规定错误代价、拒答代价、调用预算和反馈权限之前，不能证明某个数学模型是“绝对最优”。不同代价会产生不同最优策略。

本文件采用下面的严格含义：

> 在给定的信息权限、工具输出分布、反馈到达规则、调用成本和安全约束下，定义一个 Bayes-optimal reference problem。实际的动态 JEV、backbone 和在线更新器，都是对这个参考问题的可计算近似。

因此，数学问题可以做到最优定义；算法只能通过实验逼近它。

## 2. 基本对象

一个 episode 对应一个医药问题：

$$
x_i=(m_i,f_i)
$$

其中：

- $m_i$：分子；
- $f_i$：要预测的性质，例如 BBB；
- $P_i$：本题允许调用的具体 provider/version 集合；
- $B_i$：本题最多允许的调用次数；
- $Y_i$：实验真值。

对于每一个候选 provider $p\in P_i$，定义它在本题上的潜在返回：

$$
Z_{i,p}=(R_{i,p},S_{i,p})
$$

其中：

- $R_{i,p}$：模型返回的预测值；
- $S_{i,p}$：运行状态，例如正常返回、超时或格式错误。

定义 provider 的正确性标签：

$$
C_{i,p}=g(R_{i,p},S_{i,p},Y_i)
$$

对于二分类性质：

$$
C_{i,p}
=
\mathbf{1}
\left[
S_{i,p}=\mathrm{ok}
\land
R_{i,p}=Y_i
\right].
$$

对于连续值性质：

$$
C_{i,p}
=
\mathbf{1}
\left[
S_{i,p}=\mathrm{ok}
\land
\left|R_{i,p}-Y_i\right|\leq \tau_f
\right],
$$

其中 $\tau_f$ 是性质 $f$ 的预先登记容差。

所有 provider 的潜在返回可以联合建模为：

$$
\mathbf{Z}_i=(Z_{i,p})_{p\in P_i}
\sim
P_{\Theta}
\left(
\mathbf{Z}_i
\mid x_i,Y_i,P_i
\right).
$$

这里保留联合分布很重要。多个模型可能共享训练数据、结构特征或系统性错误，因此不能把各个 provider 的正确性当成相互独立的 Bernoulli 变量。

这里要区分两个层次。对每个 provider 单独建模的边际概率

$$
q_p(H_t)=P(C_{i,p}=1\mid H_t,p)
$$

足够支持第一次 provider 选择。只有当已经调用了 provider A、需要判断是否继续调用 provider B 时，才需要建模它们的联合关系，因为第二次调用的价值取决于 B 能否提供 A 没有提供的新信息。若 A、B 共享训练数据并且经常一起出错，调用 B 的新增价值会很低；若两者近似独立，B 可能提供明显的新证据。

因此，联合分布是完整 Bayes-optimal 参考问题的一部分，不代表第一版实现必须为每一对 provider 都训练一个复杂模型。第一版可以先用每个 provider 的边际模型完成单步选择，再用随机化复核数据学习必要的依赖或验证增益。

## 3. 观测和反馈必须分开

### 3.1 立即观测

如果第 $t$ 步调用 provider $p_t$，Agent 立即看到：

$$
O_{i,t}=Z_{i,p_t}.
$$

这个结果可以影响同一个 episode 的下一步动作。

### 3.2 延迟反馈

实验真值在延迟 $\Delta_i$ 后到达：

$$
F_i=
\left(
Y_i,
\{C_{i,p}:p\in\mathcal{C}_{i,T_i}\}
\right),
$$

其中 $\mathcal{C}_{i,T_i}$ 是本 episode 实际调用过的 provider 集合。

未调用的 provider 不产生标签：

$$
p\notin\mathcal{C}_{i,T_i}
\quad\Longrightarrow\quad
C_{i,p}\text{ 不可观测}.
$$

同一个实验真值产生的多个 provider 标签必须共享同一个 feedback_group_id，不能被当作相互独立的训练样本。

## 4. Episode 内的状态和动作

第 $t$ 步的历史为：

$$
H_{i,t}
=
\left(
x_i,P_i,B_{i,t},
\mathcal{C}_{i,t},
\{(p,O_{i,p}):p\in\mathcal{C}_{i,t}\}
\right),
$$

其中：

- $B_{i,t}$：剩余调用预算；
- $\mathcal{C}_{i,t}$：已经调用的 provider 集合；
- $O_{i,p}$：已经观察到的工具返回。

可行动作集合为：

$$
\mathcal{A}(H_{i,t})
=
\left\{
\mathrm{CALL}(p):
p\in P_i\setminus\mathcal{C}_{i,t}
\right\}
\cup
\left\{
\mathrm{ANSWER}(\hat y):
\hat y\in\mathcal{Y}(H_{i,t})
\right\}
\cup
\{\mathrm{ABSTAIN}\}.
$$

其中：

- $\mathrm{CALL}(p)$：调用一个尚未调用的 provider；
- $\mathrm{ANSWER}(\hat y)$：提交答案；
- $\mathrm{ABSTAIN}$：拒答。

本项目中的“复核”不是一个额外的原子动作，而是已经调用至少一个 provider 后再次执行 $\mathrm{CALL}(p)$。

为了先得到可辨识的最小问题，第一版可以限制：

$$
\mathcal{Y}(H_{i,t})
=
\{R_{i,p}:p\in\mathcal{C}_{i,t}\}.
$$

也就是说，第一版只允许选择一个已调用模型的结果作答，不允许自由融合多个模型输出。后续再单独研究 aggregation。

## 5. 信念状态：完整问题的充分统计量

令隐藏状态为：

$$
S_i=(Y_i,\mathbf{Z}_i).
$$

严格的 Bayes-optimal 参考问题假设环境分布 $P_{\Theta}$ 已知。如果 $\Theta$ 也未知，则应把它一并放入隐藏状态，使用联合后验 $P(S_i,\Theta\mid H_{i,t})$；跨 episode 的在线更新就是对这个环境参数后验的近似更新。当前实现用 $\Theta_t$ 表示可计算的状态摘要，不能把它误写成真实环境参数。

在已知环境参数 $\Theta$ 时，历史 $H_{i,t}$ 对隐藏状态形成后验信念：

$$
b_{i,t}(s)
=
P_{\Theta}(S_i=s\mid H_{i,t}).
$$

在理论上，$b_{i,t}$ 是决策的充分统计量：

$$
\pi(a_{i,t}\mid H_{i,t})
=
\pi(a_{i,t}\mid b_{i,t},B_{i,t}).
$$

调用 provider $p$ 并观察 $o$ 后，信念按 Bayes 规则更新：

$$
b_{i,t+1}
=
\tau
\left(
b_{i,t},p,o
\right).
$$

实际系统不可能维护完整的 $b_{i,t}$，所以动态 JEV 的任务是学习一个可计算的近似：

$$
\widehat b_{i,t}
=
\operatorname{JEV}_{\theta_t}(H_{i,t}).
$$

这比单独预测每个 provider 的正确率更完整，因为复核价值取决于新观测如何改变对 $Y_i$ 的判断。

## 6. Bayes-optimal 决策问题

设：

- $c_{\mathrm{call}}(p)$：调用 provider $p$ 的成本；
- $c_{\mathrm{abs}}$：拒答成本；
- $\ell_{\mathrm{err}}(\hat y,Y)$：提交答案的错误损失。

终止动作的期望损失为：

$$
Q_t^{\mathrm{answer}}(b,\hat y)
=
\mathbb{E}_{Y\sim b}
\left[
\ell_{\mathrm{err}}(\hat y,Y)
\right],
$$

$$
Q_t^{\mathrm{abstain}}(b)
=
c_{\mathrm{abs}}.
$$

调用 provider $p$ 的动作价值为：

$$
Q_t^{\mathrm{call}}(b,p)
=
c_{\mathrm{call}}(p)
+
\mathbb{E}_{O_p\sim P(\cdot\mid b,p)}
\left[
V_{t+1}
\left(
\tau(b,p,O_p),
B_{t+1}
\right)
\right].
$$

于是有限预算下的最优价值函数满足 Bellman 方程：

$$
V_t(b,B)
=
\min
\left\{
\begin{aligned}
&\min_{\hat y}
Q_t^{\mathrm{answer}}(b,\hat y),\\
&Q_t^{\mathrm{abstain}}(b),\\
&\min_{p\in P\setminus\mathcal{C}}
Q_t^{\mathrm{call}}(b,p)
\end{aligned}
\right\},
$$

边界条件为：

$$
V_{T+1}(b,B)
=
\min
\left\{
\min_{\hat y}
Q_{T+1}^{\mathrm{answer}}(b,\hat y),
Q_{T+1}^{\mathrm{abstain}}(b)
\right\}.
$$

这给出了完整问题的 Bayes-optimal policy：

$$
\pi^\star(a\mid b,B)
=
\mathbf{1}
\left[
a\in
\arg\min_{a'\in\mathcal{A}}
Q_t(b,a')
\right].
$$

因此，当前任务最准确的数学名称是：

> **带延迟反馈的预算约束主动信息获取 POMDP**  
> delayed-feedback budgeted active information acquisition POMDP

Contextual Bandit 只是它的一个限制情况：只允许一次 $\mathrm{CALL}$，没有 episode 内复核、STOP 和拒答。

## 7. 复核价值的严格定义

对尚未调用的 provider $p$，定义它相对于当前最优终止动作的价值：

$$
\operatorname{VOI}_t(p)
=
\min
\left\{
\min_{\hat y}
Q_t^{\mathrm{answer}}(b,\hat y),
Q_t^{\mathrm{abstain}}(b)
\right\}
-
Q_t^{\mathrm{call}}(b,p).
$$

当：

$$
\max_p \operatorname{VOI}_t(p)>0
$$

且预算允许时，继续调用有理论价值；否则应该终止。

这个定义说明了一个关键点：

$$
\operatorname{VOI}_t(p)
\neq
q(p\text{ 正确}\mid H_t).
$$

只知道每个 provider 的独立正确概率，通常不足以计算 VOI。还需要知道：

1. provider 会返回什么；
2. 不同 provider 的错误是否相关；
3. 新返回如何改变对 $Y_i$ 的后验；
4. 调用成本和剩余预算。

所以“JEV 输出 provider 正确率，再用它直接做 VOI”只能是近似，不能当作完整数学定义。

## 8. 跨 episode 的在线学习

每个 episode 的真值可能延迟到达。设第 $n$ 个时刻已经到达的反馈集合为：

$$
\mathcal{F}_{\leq n}^{\mathrm{arrived}}
=
\left\{
F_j:
\operatorname{arrival\_time}(F_j)\leq n
\right\}.
$$

动态 JEV 的跨 episode 状态或参数按：

$$
\Theta_{n+1}
=
U_{\phi}
\left(
\Theta_n,
\mathcal{F}_{\leq n}^{\mathrm{arrived}}
\right)
$$

更新。

更新器必须满足：

$$
\begin{aligned}
&\text{只能读取已经到达的反馈};\\
&\text{只能使用决策时保存的特征和候选版本};\\
&\text{同一 feedback\_id 只能更新一次};\\
&\text{未调用 provider 不产生伪标签}.
\end{aligned}
$$

实时学习的定义因此是：

> 每条合法反馈到达后，下一条 episode 使用更新后的 $\Theta$，而不是积累固定数量样本后再统一重训。

## 9. 约束优化形式

为了防止“全部拒答”成为平凡解，正式评价不能只看错误率。更稳妥的形式是约束优化：

$$
\min_{\pi}
\quad
R_{\mathrm{sel}}(\pi)
=
P(\hat Y\neq Y\mid D=\mathrm{ANSWER})
$$

满足：

$$
\operatorname{Coverage}(\pi)
=
P(D=\mathrm{ANSWER})
\geq \kappa,
$$

$$
\mathbb{E}_{\pi}[N_{\mathrm{call}}]\leq B,
$$

$$
P(D=\mathrm{ANSWER},\hat Y\neq Y)
\leq \delta.
$$

其中：

- $R_{\mathrm{sel}}$：选择性风险；
- Coverage：作答覆盖率；
- $B$：平均调用预算；
- $\delta$：危险作答上限。

实现时也可以使用拉格朗日形式：

$$
\mathcal{L}(\pi)
=
R_{\mathrm{err}}(\pi)
+
\lambda_{\mathrm{abs}}R_{\mathrm{abs}}(\pi)
+
\lambda_{\mathrm{call}}C_{\mathrm{call}}(\pi),
$$

但 $\lambda_{\mathrm{abs}}$、$\lambda_{\mathrm{call}}$ 必须在实验前固定，不能根据测试结果调到最好看。

## 10. 动态 JEV 和 backbone 的关系

数学问题本身不要求某个特定 backbone。backbone 只是用来近似：

$$
b_{i,t}
\quad\text{或}\quad
Q_t(b,a).
$$

实际可实现的近似可以写成：

$$
z_{i,t,p}
=
f_{\psi}
\left(
x_i,
\operatorname{card}(p),
H_{i,t}
\right),
$$

$$
\widehat Q_{\theta_t}(H_{i,t},a)
=
g_{\theta_t}(z_{i,t},a).
$$

其中：

- $f_\psi$：冻结或慢速更新的表示层；
- $g_{\theta_t}$：实时更新的小型决策头。

因此，Laya 或 AnyJev 可以作为候选表示层，但不能直接把它们的 choice distribution 当成 provider 正确率或 Bayes 后验。

第一版最合理的实现顺序是：

1. 固定 $f_\psi$，先学习 selected-only 的 provider 可靠性；
2. 用合法探索收集反馈并记录 propensity；
3. 学习近似 $Q^{\mathrm{call}}$ 或 VOI；
4. 加入安全作答/拒答门；
5. 最后接入主 Agent 的自由语言轨迹。

PPO、GRPO 或解冻整个语言模型不属于这个数学问题的必要组成部分。

## 11. Benchmark-only implementation boundary

本项目的事件、状态、更新器和评价指标只服务于预算约束的 provider acquisition
问题。实现可以复用通用的延迟反馈、exactly-once、乱序重放和版本日志组件，
但策略头、损失函数、标签语义和评价指标必须由本项目单独定义并验收。

## 12. 公式层级和主目标的整理

文档中的公式分成两层，不能混读：

1. 第 5–7 节是完整问题的参考定义：belief state、Bellman 方程和 VOI 给出理论最优策略。
2. 第 8–10 节是可实现近似：冻结表示、在线 head、延迟反馈和约束评价。

主目标只保留一个：在固定调用预算、反馈权限和拒答覆盖率下，最小化最终选择性风险。错误代价、拒答代价和调用成本的加权形式只是实现这个约束目标的拉格朗日写法，不再把它当成第二个独立问题。

这版已经足够作为数学定义，但还不能把它当成最终算法。最终算法需要说明它近似的是 belief、action value，还是两者的低维摘要，并由独立测试决定。

## 13. 关于“最优”的最终判断

当前可以明确说：

1. **问题层面**：最完整、最合适的形式是带延迟反馈的预算约束主动信息获取 POMDP，并带有跨 episode 的在线状态更新。
2. **Contextual Bandit**：只是首个 provider 选择动作的简化模型，不是完整问题。
3. **provider 正确率模型**：可以作为第一阶段的可实现近似，但不能单独代表完整的复核和拒答问题。
4. **JEV/backbone**：是对 belief state 或 action-value 的近似表示，不是数学问题本身。
5. **最终算法是否最优**：目前不能宣称。只有在固定信息权限、成本、反馈协议和评价约束后，才能比较哪种近似最有效。

这个版本已经把数学对象、信息边界、最优参考策略、在线更新和跨项目复用边界分开了。后续选 backbone 和训练方法时，应该问它在多大程度上逼近 $b_t$ 或 $Q_t$，而不是先问“哪个开源 JEV 最像我们”。
