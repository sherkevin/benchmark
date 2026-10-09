# 动态 JEV 数学建模 v3：最小原始符号与真实案例

> 更新说明（2026-09-25）：Task36 真实性、接口和实时性审查以 [v4 可实现版](动态JEV数学建模_v4_可实现版.md) 为准；本文件保留为上一版最小符号草稿。

**状态：** 当前建模候选。  
**写法要求：** 只把任务最开始就存在的对象作为原始符号；其余符号必须由原始对象计算得到。每个公式下面给出同一个真实 BBB case 的实例化。

## 0. 先固定问题

我们研究一个最小任务：

> 给定一个分子和一个性质，系统可以依次调用若干具体模型。每次调用只看到被调用模型的返回。最后提交一个已经看到的结果，或者拒答。实验真值延迟到达。

第一版不允许自由融合多个模型输出。这样可以把问题先收敛到“选择哪个具体 provider，以及什么时候停止”。

## 1. 原始对象只有七个

| 符号 | 原始概念 | 在 BBB case 中的实例 |
|---|---|---|
| $x$ | 任务输入 | entity_id 为 z002905 的分子，性质为 BBB_prediction |
| $\mathcal P$ | 允许调用的具体 provider 集合 | DeepPurpose CNN、DeepPurpose Morgan 等 |
| $a_t$ | 第 $t$ 步动作 | CALL(CNN)、CALL(Morgan)、ANSWER(0)、ABSTAIN |
| $o_t$ | 被调用 provider 的即时返回 | CNN 返回概率 0.0030845485161989927 |
| $y$ | 实验真值 | z002905 的 BBB 标签为 1 |
| $B$ | 本题允许的最大调用次数 | 例如本题预设 $B=2$ |
| $\Delta$ | 真值从调用结束到到达的延迟 | 回放中终止后才给判分反馈 |

除此之外的 $h_t$、$r_t$、$\pi$、$Q_t$、$V_t$ 都是由这些原始对象计算出来的量，不是新的环境输入。

### Case 0：固定实例

$$
x=
\left(
\text{z002905},
\text{BBB\_prediction},
\text{NCC[C@H](O)C(=O)N[C@@H]1C[C@H](N)[C@@H](O[C@H]2O[C@H](CN)[C@@H](O)[C@H](O)[C@H]2O)[C@H](O)[C@H]1O[C@H]1O[C@H](CO)[C@@H](O)[C@H](N)[C@H]1O}
\right).
$$

**Case 0：** 这里的 $x$ 就是仓库中的 entity_id、性质名和 canonical SMILES。没有额外的“分子状态”“任务状态”输入。

## 2. 历史不是新的原始概念

历史只是已经发生的动作和返回的序列：

$$
h_t
=
\bigl(
x,\,
(a_1,o_1),\,
(a_2,o_2),\,
\ldots,\,
(a_{t-1},o_{t-1})
\bigr).
$$

**Case 1：** 如果第一步调用 CNN，历史就是：

$$
h_2=
\left(
x,\,
\bigl(
\mathrm{CALL}(\mathrm{CNN}),
0.0030845485161989927
\bigr)
\right).
$$

这里没有放入真值，也没有放入 Morgan 的返回，因为它还没有被调用。

从历史中只派生出两个集合和一个剩余预算：

$$
\mathcal P(h_t)
=
\left\{p:\mathrm{CALL}(p)\text{ 出现在 }h_t\right\}.
$$

**Case 1a：** 对 $h_2$，有 $\mathcal P(h_2)=\{\mathrm{CNN}\}$。

$$
B_t
=
B-\left|\mathcal P(h_t)\right|.
$$

**Case 1b：** 若预设 $B=2$，则 z002905 在 $h_2$ 时的剩余预算为 $B_2=1$。

这两个量都由 $h_t$ 和原始预算 $B$ 计算得到，不是新的输入。

## 3. 动作只有“调用”或“结束”

在历史 $h_t$ 下，动作集合为：

$$
\mathcal A(h_t)
=
\left\{
\mathrm{CALL}(p):
p\in\mathcal P\setminus\mathcal P(h_t)
\right\}
\cup
\left\{
\mathrm{ANSWER}(r):
r\text{ 是历史中已经看到的返回}
\right\}
\cup
\left\{
\mathrm{ABSTAIN}
\right\}.
$$

其中 $\mathcal P(h_t)$ 只是从历史中读出的“已经调用过的 provider 集合”，不是新的原始对象。

**Case 2：** 在 Case 1 的历史下，合法动作包括：

$$
\mathcal A(h_2)
=
\left\{
\mathrm{CALL}(\mathrm{Morgan}),
\mathrm{ANSWER}(0.0030845485161989927),
\mathrm{ABSTAIN}
\right\}.
$$

CNN 已经调用过，所以不能重复调用 CNN。Morgan 可以继续调用。

## 4. 返回值和真值分开

调用动作只产生即时返回：

$$
o_t
=
\mathrm{Tool}(x,a_t).
$$

**Case 3a：** 当 $a_1=\mathrm{CALL}(\mathrm{CNN})$ 时，z002905 的即时返回是 $o_1=0.0030845485161989927$。

正确性标签要等 $y$ 到达后才能计算：

$$
r_t
=
\mathbf 1
\left[
\mathrm{label}(o_t)=y
\right].
$$

这里 $\mathrm{label}$ 是该性质预先登记的阈值规则。BBB 概率的阈值为 $0.5$。

**Case 3b：** z002905 的真值是 $y=1$，而 $\mathrm{label}(o_1)=0$，所以 $r_1=\mathbf 1[0=1]=0$。如果第二步调用 Morgan，则 $o_2=0.977143406867981$、$\mathrm{label}(o_2)=1$，于是 $r_2=\mathbf 1[1=1]=1$。

真实来源分别是 report.json 和 BBB screening_ledger.jsonl，不是人为编造的例子。

## 5. 延迟反馈只更新实际调用过的模型

一个 episode 结束后，反馈事件为：

$$
F
=
\left(
y,\,
\{(a_t,r_t):a_t=\mathrm{CALL}(p)\}
\right).
$$

未调用的 provider 不出现在 $F$ 中，也不补成负样本。

**Case 4：** 如果先调用 CNN，再调用 Morgan，终止后收到：

$$
F
=
\left(
1,\,
\left\{
(\mathrm{CNN},0),
(\mathrm{Morgan},1)
\right\}
\right).
$$

如果只调用 CNN 就拒答，则反馈只有：

$$
F
=
\left(
1,\,
\left\{
(\mathrm{CNN},0)
\right\}
\right).
$$

Morgan 的结果即使存在于离线缓存，也不能进入这个 episode 的观察或反馈。

## 6. 策略的输入和输出

策略只是一个函数：

$$
\pi(a_t\mid h_t).
$$

它读取历史 $h_t$，输出下一步动作的概率。

**Case 5：** 在 Case 1 的历史下，一个策略可能输出：

$$
\pi\bigl(
\mathrm{CALL}(\mathrm{Morgan})\mid h_2
\bigr)=0.7,
\qquad
\pi\bigl(
\mathrm{ABSTAIN}\mid h_2
\bigr)=0.3.
$$

这两个数是策略的动作概率，不是 Morgan 正确率，也不是最终答案置信度。

## 7. 最优策略的定义

提交一个已经看到的结果 $r$，其期望错误损失为：

$$
Q_t^{\mathrm{answer}}(h_t,r)
=
\Pr(r\neq y\mid h_t).
$$

**Case 6a：** z002905 第一步调用 CNN 后，如果直接提交 CNN 的标签 0，而真实标签为 1，则：

$$
Q_1^{\mathrm{answer}}(h_2,0)
=
\Pr(0\neq 1\mid h_2)
=
1.
$$

拒答的损失是评测协议预先规定的常数：

$$
Q_t^{\mathrm{abstain}}(h_t)
=
c_{\mathrm{abstain}}.
$$

**Case 6b：** 对 z002905，如果当前只看到 CNN 的错误结果，拒答的损失就是预先登记的 $c_{\mathrm{abstain}}$。这个数不能看完测试结果后再调整。

继续调用 provider $p$ 的期望损失为：

$$
Q_t^{\mathrm{call}}(h_t,p)
=
c_{\mathrm{call}}(p)
+
\mathbb E_{o\sim
\Pr(\cdot\mid h_t,\mathrm{CALL}(p))}
\left[
V_{t+1}
\bigl(
h_t\oplus(p,o),B_t-1
\bigr)
\right].
$$

**Case 6c：** 对 z002905，$p=\mathrm{Morgan}$ 时，实际可能观察到：

$$
o=0.977143406867981.
$$

调用成本加上看到这个新结果后继续作答或拒答的最小损失，就是 $Q_1^{\mathrm{call}}(h_2,\mathrm{Morgan})$。

所有动作中最小的期望损失定义为：

$$
V_t(h_t,B_t)
=
\min
\left\{
\begin{aligned}
&\min_{r\in\mathrm{Returns}(h_t)}
Q_t^{\mathrm{answer}}(h_t,r),\\
&Q_t^{\mathrm{abstain}}(h_t),\\
&\min_{p\in\mathcal P\setminus\mathcal P(h_t)}
Q_t^{\mathrm{call}}(h_t,p)
\end{aligned}
\right\}.
$$

**Case 6d：** 在 z002905 的 $h_2$ 下，系统比较三件事：提交 CNN 的 0、拒答、继续调用 Morgan。$V_1(h_2,B_1)$ 是这三者期望损失的最小值。

最优策略选择达到最小值的动作：

$$
\pi^\star(h_t)
\in
\arg\min_{a\in\mathcal A(h_t)}Q_t(h_t,a).
$$

**Case 6e：** 如果预先登记的成本使得继续调用 Morgan 的期望损失最低，则 $\pi^\star(h_2)=\mathrm{CALL}(\mathrm{Morgan})$；如果拒答最低，则 $\pi^\star(h_2)=\mathrm{ABSTAIN}$。这个选择由成本、返回分布和真值分布共同决定。

## 8. 为什么 provider 之间的关系只在复核时出现

第一次选择 provider 时，可以分别估计每个 provider 的边际正确率：

$$
q_p(h_t)
=
\Pr(r(o,y)=1\mid h_t,\mathrm{CALL}(p)).
$$

**Case 7a：** 在 z002905 的 $h_1$ 中没有已调用结果。对 CNN，这个量是 $q_{\mathrm{CNN}}(h_1)=\Pr(r(o,y)=1\mid h_1,\mathrm{CALL}(\mathrm{CNN}))$；对 Morgan 同理。这里的 $r(o,y)$ 就是第 4 节由返回值和真值计算的正确性。

在已经看到 CNN 返回后，是否继续调用 Morgan，需要考虑条件返回分布：

$$
\Pr(o_{\mathrm{Morgan}},y
\mid
h_2,\mathrm{CALL}(\mathrm{Morgan})).
$$

**Case 7b：** 对 z002905，$h_2$ 已经包含 CNN 的 0.0030845485161989927。上式描述在这个已有证据下，Morgan 会返回什么以及真实标签是什么；它不是 Morgan 的全局平均准确率。

这一步才需要 provider 之间的条件关系。

**Case 8：** z002905 中：

- ADMET-AI 和 CNN 都输出 BBB 阴性，但真值为阳性；
- Morgan 输出 BBB 阳性，且正确。

这个 case 展示了“多个模型可能共同出错，也可能互相补充”。它本身不能估计总体相关系数；相关关系要在多个分子上统计。它只说明为什么复核决策不能只看 Morgan 的单模型平均正确率。

所以：

- 单步 provider 选择：逐 provider 建模即可；
- 多步复核：需要估计新 provider 相对已有结果的增量信息；
- 第一版实现：可以先用边际模型，再用随机复核样本学习增量价值。

## 9. 实时更新只更新一个已有对象

设第 $n$ 个 episode 的反馈已经到达，在线参数为 $\theta_n$，更新为：

$$
\theta_{n+1}
=
U
\left(
\theta_n,
F_n
\right).
$$

**Case 9：** z002905 终止并收到反馈：

$$
F_n=
\left(
1,
\{(\mathrm{CNN},0),(\mathrm{Morgan},1)\}
\right).
$$

更新器使用这两个已调用 provider 的标签更新下一题的策略。没有调用的第三个 provider 不产生更新。

如果反馈在下一天才到达，则到达前仍使用 $\theta_n$；到达后才切换到 $\theta_{n+1}$。这就是延迟实时学习。

## 10. 最终的最小数学对象

在当前任务里，真正需要建模的原始对象只有：

$$
(x,\mathcal P,a_t,o_t,y,B,\Delta).
$$

**Case 10：** 对 z002905：

- $x$：分子和 BBB_prediction；
- $\mathcal P$：允许调用的具体 BBB provider；
- $a_t$：调用 CNN、调用 Morgan 或结束；
- $o_t$：实际调用工具返回的概率；
- $y=1$：实验真值；
- $B=2$：调用预算；
- $\Delta$：终止到真值反馈到达的时间差。

$h_t$、$r_t$、$F$、$\pi$、$Q_t$、$V_t$ 都是由这些对象按上面的规则生成的派生量。

这就是当前项目的最小、可追溯数学模型。后续 backbone 只需要回答一个问题：它要近似的是 $\pi^\star$、$Q_t$，还是 provider 的边际概率 $q_p$。不能在没有说明近似对象的情况下直接添加新的网络输出或状态变量。
