# Linear attention 到动态 JEV 的数学迁移审查

日期：2026-09-25

## 结论

linear attention 提供的是一个可证明的计算恒等式：对固定有限维非负特征核，历史的核加权平均可以压缩成固定大小的 numerator/denominator 状态。它能证明在线 JEV 的更新是可计算的，但不能单独证明准确率、最优性或新颖性。

## 原始推导

设当前 query 为 $q_t$，历史 key/value 为 $(k_j,v_j)$，并取非负特征映射 $\phi$，使 $K(q,k)=\phi(q)^\top\phi(k)$。因果 kernel attention 为：

$$
y_t=\frac{\sum_{j\le t}K(q_t,k_j)v_j}{\sum_{j\le t}K(q_t,k_j)}.
$$

把核代入并使用矩阵乘法结合律：

$$
y_t=\frac{\phi(q_t)^\top S_t}{\phi(q_t)^\top Z_t},
\qquad
S_t=\sum_{j\le t}\phi(k_j)v_j^\top,
\qquad
Z_t=\sum_{j\le t}\phi(k_j).
$$

因此：

$$
S_t=S_{t-1}+\phi(k_t)v_t^\top,
\qquad
Z_t=Z_{t-1}+\phi(k_t).
$$

这是对选定有限特征核的精确等价变换。若用有限特征近似 softmax，只有核近似部分带来误差。

## 动态 JEV 的最小迁移

对每个 provider $p$，把调用时可见的历史和候选 provider 编码为 $r_i=\operatorname{enc}(h_i,p)$，固定得到 $k_i=\phi(r_i)\in\mathbb R_+^d$。反馈可以是一个标量或两个目标组成的向量：

$$
v_i=\begin{bmatrix}c_i\\g_i\end{bmatrix},
$$

其中 $c_i$ 是该 provider 的正确性标签，$g_i$ 是在随机复核协议下获得的 verification gain。只在反馈到达后更新：

$$
S_{p,n}=\rho^{\Delta n}S_{p,n-1}+
\sum_{i:\tau_i=n,\,p\in A_i}w_i k_i v_i^\top,
$$

$$
Z_{p,n}=\rho^{\Delta n}Z_{p,n-1}+
\sum_{i:\tau_i=n,\,p\in A_i}w_i k_i,
$$

$$
\widehat v_{p,n}(h)=
\frac{\phi(\operatorname{enc}(h,p))^\top S_{p,n}+\lambda v_0}
{\phi(\operatorname{enc}(h,p))^\top Z_{p,n}+\lambda}.
$$

当 $v_i$ 是二维向量时，$S_{p,n}\in\mathbb R^{d\times2}$，一次查询同时得到正确性估计和复核收益估计。$w_i=1/\pi_i(p\mid h_i)$ 是已记录的调用 propensity；第一版必须通过显式随机探针保证 $\pi_i\ge\pi_{\min}>0$。$\rho=1$ 时，乱序到达的反馈是可交换加法；第一版建议设 $\rho=1$，避免把到达顺序和时间漂移混在一起。

**z002905 case：** CNN 首次返回 $0.0030845485161989927$，映射为标签 $0$；若下一步把 Morgan 作为候选，则查询输入是 $\operatorname{enc}(h_2,\mathrm{Morgan})$，其中 $h_2$ 包含 z002905、BBB、CNN 的返回值和剩余预算。Morgan 的到达反馈是 $c_i=1$，复核增益按预注册损失定义为 $g_i=L_{\mathrm{stop}}(h_2)-L_{\mathrm{after\ Morgan}}(h_2)$。该样本到达时具体更新为：

$$
S_{\mathrm{Morgan}}\leftarrow S_{\mathrm{Morgan}}+w_i\,\phi(\operatorname{enc}(h_2,\mathrm{Morgan}))\begin{bmatrix}1 & g_i\end{bmatrix},
\qquad
Z_{\mathrm{Morgan}}\leftarrow Z_{\mathrm{Morgan}}+w_i\,\phi(\operatorname{enc}(h_2,\mathrm{Morgan})).
$$

原有 z002905 固定动作回放没有随机化概率 $\pi_i$，所以只能验证环境和反馈字段，不能直接作为 IPW 训练证据；M1 必须在随机复核协议下重新产生 $\pi_i$ 和 $g_i$。

对 verification gain，必须把它写成潜在结果而不是 provider 边际准确率。令 $G_i(p)$ 表示在相同 $h_i$ 下调用 $p$ 后相对于立即停止的损失下降；实际只在 $A_i=p$ 时观测到 $G_i(p)$。若调用由已记录的 propensity $\pi_i(p\mid h_i)$ 随机化，则：

$$
\mathbb E\left[\frac{\mathbf 1[A_i=p]G_i(p)}{\pi_i(p\mid h_i)}\middle\vert h_i\right]
=\mathbb E[G_i(p)\mid h_i].
$$

因此 IPW 写入的 gain 状态在条件期望上对应潜在复核收益；这一步是识别 verification gain 的必要条件。若没有随机探针，$g_i$ 只描述被策略挑中的样本，不能解释成候选 provider 在同一 context 下的可泛化复核价值。

展开查询式：

$$
\widehat v_{p,n}(h)=
\frac{\sum_i w_iK(\operatorname{enc}(h,p),r_i)v_i+\lambda v_0}
{\sum_i w_iK(\operatorname{enc}(h,p),r_i)+\lambda}.
$$

所以它不是凭空引入的神经网络，而是有限特征核下的精确加权经验估计。更新和查询的复杂度为 $O(dM)$，每个 provider 保存 $O(dM)$ 状态，$M=2$ 时就是两个输出列。

## 它到底在优化什么

对固定 query $(h,p)$，令 $\alpha_i=w_iK(\operatorname{enc}(h,p),r_i)$。加入一个冷启动先验 $(\lambda,v_0)$ 后，查询结果可写成一个局部加权平方损失的精确最小解：

$$
\mathcal L_{n}(u\mid h,p)
=\frac12\sum_i\alpha_i\lVert v_i-u\rVert_2^2
 +\frac\lambda2\lVert u-v_0\rVert_2^2,
$$

$$
u^*_{n}(h,p)
=\frac{\sum_i\alpha_i v_i+\lambda v_0}
{\sum_i\alpha_i+\lambda}.
$$

因此实现时只需在查询式的分子加上 $\lambda v_0$、分母加上 $\lambda$。对每个 query，linear-state 递推精确计算这个局部目标的 minimizer；它不等于整个序列上的 Bayes-optimal policy，也不等于全局最小化 Bellman 损失。这个边界必须写清楚。

## 可证明的统计边界

令 $\alpha_i=w_iK(\operatorname{enc}(h,p),r_i)\ge0$，并令 $v_i\in[0,1]$ 为一个标量目标。给定 context 和 action 后，定义加权邻域均值：

$$
\bar v(h)=\frac{\sum_i\alpha_i\,\mathbb E[v_i\mid r_i,p_i]}{\sum_i\alpha_i}.
$$

条件独立时，归一化估计满足加权 Hoeffding 界：

$$
\Pr\left(|\widehat v(h)-\bar v(h)|\ge\eta\right)
\le
2\exp\left(-2\eta^2n_{\mathrm{eff}}\right),
\qquad
n_{\mathrm{eff}}=\frac{(\sum_i\alpha_i)^2}{\sum_i\alpha_i^2}.
$$

这证明了估计会随有效样本量收敛到核邻域均值。要把它解释为真实条件正确率或真实 VOI，还需要额外的平滑性、有限特征近似误差和 overlap 假设；IPW 的分子、分母各自可无偏，二者的比值有限样本下一般不是严格无偏，只能在 overlap 和样本量条件下保持一致性。

## 创新性边界

上述 $S/Z$ 递推已经由 linear attention 和 fast-weight 文献给出；kernel bandit 也已有 delayed feedback 算法。因此不能把“把 linear attention 状态接到 JEV”单独写成新算法。

可能形成任务专属贡献的地方是：

1. 只用已调用且已到达的 selected-only 反馈；
2. 用随机复核探针识别条件 verification gain，而不是把 provider 的边际准确率当成复核价值；
3. 用同一个固定状态同时估计正确性和复核收益，并在预算约束下做 risk gate；
4. 给出延迟、propensity、漂移和状态复杂度的定理或可检验界。

如果没有这些反馈合同、可识别性条件和强基线，线性状态只是合理的实现选择，不足以构成方法创新。

## 来源

- Katharopoulos et al., *Transformers are RNNs: Fast Autoregressive Transformers with Linear Attention*, arXiv:2006.16236，https://arxiv.org/abs/2006.16236
- Schlag et al., *Linear Transformers Are Secretly Fast Weight Programmers*, arXiv:2102.11174，https://arxiv.org/abs/2102.11174
- Vakili et al., *Delayed Feedback in Kernel Bandits*, arXiv:2302.00392，https://arxiv.org/abs/2302.00392
