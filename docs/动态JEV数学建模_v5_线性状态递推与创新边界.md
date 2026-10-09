# 动态 JEV v5：线性状态递推、可计算性与创新边界

**状态：** 方法候选与创新性审查版，尚未冻结为最终算法。

## 1. 先给结论

当前 v4 的任务定义是合理且可运行的，但“动态 JEV + 在线 RLS/LinUCB + 延迟队列”本身还不足以构成新算法。contextual routing、selected-arm 二元反馈、在线线性头、delayed feedback、闭式 JEV head、abstention 和 linear attention 都已有先例。

linear attention 能提供一个很强的**可计算性证明模板**：把历史压缩成有限维状态，证明递推状态与一个明确的加权目标完全等价，再给出每次更新和查询的复杂度。它不能单独证明任务效果，也不能把这个递推式本身当成创新点。

当前真正可能形成贡献的方向是：

> 在只观察已调用 provider、真值延迟到达的工具环境中，用随机复核探针识别“追加调用带来的条件收益”，再用固定大小的线性状态和风险门控实时更新。

这条主张还需要实验和理论边界验证，不能先写成“首次提出实时 JEV”。

本文中的“动态 JEV”必须精确定义为：**冻结的 Laya/AnyJev 表示 + 可审计的在线状态头**。上面的递推更新状态头，不会更新 Laya/AnyJev 的全部 backbone 权重，也不等于对 AnyJev 的原始 `observe` 做逐条替换。若以后要声称 backbone fine-tune，需要另外定义梯度、更新成本、稳定性和遗忘控制。

## 2. linear attention 的可迁移结构

Katharopoulos 等人的推导使用非负特征映射 $\phi$ 定义核 $K(q,k)=\phi(q)^\top\phi(k)$。因矩阵乘法结合律，历史不必保存为完整矩阵，只维护：

$$
S_t=\sum_{j\leq t}\phi(k_j)v_j^\top,
\qquad
Z_t=\sum_{j\leq t}\phi(k_j).
$$

查询写成：

$$
y_t=\frac{\phi(q_t)^\top S_t}{\phi(q_t)^\top Z_t}.
$$

所以状态递推为：

$$
S_t=S_{t-1}+\phi(k_t)v_t^\top,
\qquad
Z_t=Z_{t-1}+\phi(k_t).
$$

这是一条代数恒等式：对指定的有限特征核，递推结果和完整历史求和结果相同。若 $\phi$ 只是用来近似 softmax，等价性只对近似核成立。

Schlag 等人进一步把 $S_t$ 解释成可写入的 fast weight，并用 delta rule 修正旧关联。这说明“递推状态”和“在线纠错”是成熟结构，不是动态 JEV 的直接新颖性来源。

## 3. M0：只学习 selected provider 的正确概率

### 3.1 可见反馈

第 $i$ 个 episode 只在 provider $p_i$ 被实际调用、真值到达且可判分时产生反馈：

$$
e_i=(h_i,p_i,c_i,\pi_i,\tau_i),
$$

其中 $h_i$ 是调用时可见历史，$c_i\in\{0,1\}$ 是该 provider 的正确性，$\pi_i=\pi(p_i\mid h_i)$ 是调用概率，$\tau_i$ 是反馈到达事件步。未调用 provider 不产生 $c_i$。

为了让 selected-only 数据能估计同一策略下的 provider 质量，第一版策略必须有显式探索，并满足 $\pi_i\geq\pi_{\min}>0$。定义逆倾向权重：

$$
w_i=\frac{1}{\pi_i}.
$$

失败返回不写入 $c_i=0$；失败只进入单独的执行成功通道 $z_i=\mathbf 1[\mathrm{status}_i=\mathrm{ok}]$。

### 3.2 线性状态

冻结 Laya 或 AnyJev 表示后，用固定非负特征映射得到：

$$
k_i=\phi(h_i,p_i)\in\mathbb R_+^D.
$$

实现时必须保持非负性，例如使用 $\mathrm{softplus}(W h+b)$ 或 $\mathrm{ELU}(W h+b)+1$；普通有符号线性特征不能直接套用这个分母和概率界。$\lambda>0$ 的冷启动先验用于避免无反馈时出现 $0/0$。

$h_i$ 必须包含已经观察到的工具结果和调用身份。这样即使状态按 provider 分开维护，第二次调用 Morgan 时的 $\phi(h_2,\mathrm{Morgan})$ 仍然可以条件化于此前 CNN 的返回；只有把特征退化成只看分子 $x$，才会把 provider 关系错误地压成边际独立。

M0 为每个 provider 维护两个向量状态：

$$
S_{n,p}=\rho S_{n-1,p}
 +\sum_{i:\tau_i=n,\,p_i=p}w_i k_i c_i,
$$

$$
Z_{n,p}=\rho Z_{n-1,p}
 +\sum_{i:\tau_i=n,\,p_i=p}w_i k_i,
$$

其中 $\rho\in(0,1]$ 是遗忘系数。$S_{n,p},Z_{n,p}$ 表示处理完到达事件步 $n$ 的反馈后、供 episode $n+1$ 使用的状态。第一版先设 $\rho=1$；这样乱序反馈只影响“何时可用”，不影响加法结果。需要漂移实验时再用 $\rho<1$，并明确按反馈到达时间衰减。

使用 Beta 先验 $\alpha_0,\beta_0>0$，查询为：

$$
\widehat q_{n,p}(h)
=
\frac{\phi(h,p)^\top S_{n,p}+\alpha_0}
     {\phi(h,p)^\top Z_{n,p}+\alpha_0+\beta_0}.
$$

因为 $k_i\geq0$、$c_i\in\{0,1\}$ 且先验为正，始终有 $0<\widehat q_{n,p}(h)<1$。

这个查询还可以写成一个有明确解的局部加权目标。令

$$
\alpha_i=\rho^{n-\tau_i}w_iK((h,p),(h_i,p_i)),
\qquad
\lambda=\alpha_0+\beta_0,
\qquad
u_0=\frac{\alpha_0}{\alpha_0+\beta_0}.
$$

定义：

$$
\mathcal L_{n,p}(u)
=
\frac12\sum_i\alpha_i(c_i-u)^2
+\frac{\lambda}{2}(u-u_0)^2.
$$

对 $u$ 求导并令其为零，得到：

$$
u^*
=
\frac{\sum_i\alpha_i c_i+\lambda u_0}
     {\sum_i\alpha_i+\lambda}
=\widehat q_{n,p}(h).
$$

所以 $S/Z$ 递推精确计算的是“固定核、固定先验下的局部加权平方损失最小解”。这是比“把一个在线头接上去”更清楚的数学对象；但它仍然只是局部估计器的最优解，不是全局 Bellman 或 Bayes-optimal 策略。

**z002905 的实例：** 如果只调用 CNN，延迟反馈为 $c=0$，只更新 $S_{\mathrm{CNN}}$ 和 $Z_{\mathrm{CNN}}$；Morgan 没有被调用，因此它的状态不更新。如果 CNN 和 Morgan 都被调用，反馈分别为 $c_{\mathrm{CNN}}=0$、$c_{\mathrm{Morgan}}=1$，两个 provider 各自更新自己的状态。

### 3.3 递推等价性

递推式展开后得到：

$$
S_{n,p}
=
\sum_{i:\,p_i=p,\,\tau_i\leq n}
\rho^{n-\tau_i}w_i k_i c_i,
$$

$$
Z_{n,p}
=
\sum_{i:\,p_i=p,\,\tau_i\leq n}
\rho^{n-\tau_i}w_i k_i.
$$

令 $K((h,p),(h_i,p_i))=\phi(h,p)^\top\phi(h_i,p_i)$，则查询的无先验部分恰好等于：

$$
\frac{\phi(h,p)^\top S_{n,p}}
     {\phi(h,p)^\top Z_{n,p}}
=
\frac{\sum_i\rho^{n-\tau_i}w_iK((h,p),(h_i,p_i))c_i}
     {\sum_i\rho^{n-\tau_i}w_iK((h,p),(h_i,p_i))}.
$$

因此它是一个带遗忘、带逆倾向权重的核加权 Bernoulli 估计器。这个等式可以用归纳法直接证明：$n=0$ 时状态为先验初值；从 $n-1$ 到 $n$ 只增加当步到达的反馈并乘以 $\rho$。

**可计算性结论：** 每条反馈更新和每次查询都是 $O(D)$，每个 provider 的状态是 $O(D)$，全体状态是 $O(|\mathcal P|D)$，不需要回放完整历史。这是可以直接写进论文的计算性结果。

**不能从中推出的结论：** 该等式不证明 $\widehat q$ 是真实正确概率，也不证明它优于 LinUCB、RLS、AnyJev 或静态 JEV。它只证明了在指定特征核和反馈合同下，递推实现与目标估计器一致。

## 4. M1：学习追加调用的 verification gain

单 provider 正确率不能代替复核价值。对已经有一个基线答案的 episode，随机决定是否追加调用 $p$，并在真值到达后计算：

$$
g_{i,p}
=
\ell(\widehat y_i^{\mathrm{base}},y_i)
-
\ell(\widehat y_i^{\mathrm{after}\ p},y_i).
$$

这里 $\ell$ 是预先固定的错误/拒答损失。$g_{i,p}>0$ 表示追加调用降低损失，$g_{i,p}<0$ 表示追加调用反而变差。只有随机探针实际调用了 $p$，$g_{i,p}$ 才可观测；没有探针就不能识别 verification gain。

设第二次调用的随机 probe 概率为 $\eta_{i,p}=\Pr(\mathrm{probe}\ p\mid h_i)$，并要求 $\eta_{i,p}\geq\eta_{\min}>0$。由于 $g_{i,p}$ 只有在 probe 发生时可见，使用 $w^g_i=1/\eta_{i,p}$。增益可以为负，因此不使用 M0 的 Beta 先验，而维护一个带符号的线性状态：

$$
G_{n,p}=\rho G_{n-1,p}
 +\sum_{i:\tau_i=n,\,\mathrm{probe}\ p}w^g_i k_i g_{i,p},
$$

$$
Z^g_{n,p}=\rho Z^g_{n-1,p}
 +\sum_{i:\tau_i=n,\,\mathrm{probe}\ p}w^g_i k_i,
$$

$$
\widehat g_{n,p}(h)=
\frac{\phi(h,p)^\top G_{n,p}}
     {\phi(h,p)^\top Z^g_{n,p}+\varepsilon}.
$$

若要同时校正第一步 provider 的选择和第二步 probe，需要把相应的 action propensity 相乘；M1 第一版固定基线结果，只校正 probe propensity。若增益有界在 $[-1,1]$，可以用有效样本量构造置信半径；定义加权系数 $\alpha_i=\rho^{n-\tau_i}w^g_iK((h,p),(h_i,p_i))$：

$$
n_{\mathrm{eff}}
=
\frac{(\sum_i\alpha_i)^2}{\sum_i\alpha_i^2}.
$$

在独立、有界反馈和固定特征的条件下，核加权均值的误差半径可取：

$$
\epsilon_{n,p}(\delta)
=
\sqrt{\frac{\log(2/\delta)}{2n_{\mathrm{eff}}}}.
$$

这只是对加权邻域均值的集中界；要把它解释成真实条件增益，还需要平滑性、覆盖性和漂移假设。由于 IPW 查询是加权比值，有限样本下通常不能直接声称严格无偏；在 overlap 和稳定分布条件下应讨论一致性或误差界。

## 5. 风险门控，而不是直接近似 Bellman

第一版不要声称已经估计了 $\Pr(o\mid h,p)$，也不要把上面的 $\widehat q$ 直接当成完整 Bellman $Q$。先用 gain head 做一个可识别的门控：

$$
\mathrm{CALL}(p)
\quad\Longleftrightarrow\quad
\widehat g_{n,p}(h)-\epsilon_{n,p}(\delta)>c_{\mathrm{call}}(p).
$$

如果不满足门槛，固定规则选择已有答案或拒答。这个门控有清楚的实验含义：只有在追加调用的保守估计收益超过调用成本时才调用。

**z002905 的实例：** CNN 给出错误标签 0 后，M1 的随机 probe 才能观测 Morgan 是否把最终错误改正；单看 Morgan 在全体分子上的准确率，不能得到这个条件增益。

## 6. 创新性审查结果

### 已有先例覆盖的部分

| 组件 | 已有覆盖 | 对我们的含义 |
|---|---|---|
| Linear attention 的 $S/Z$ 递推 | Katharopoulos et al. 2020 | 不能把递推式本身当新算法 |
| Fast-weight / delta rule | Schlag et al. 2021 | 在线纠错和写入率已有先例 |
| selected-arm 二元反馈 + online linear routing | PILOT, EMNLP Findings 2025 | 不能只把 JEV 换成 LinUCB/RLS |
| JEV 闭式 head + observe | AnyJev | 不能声称首次有在线 JEV head |
| delayed-feedback online learning | Joulani et al. 2013 等 | 延迟队列本身不是新颖性 |
| active model selection / cost-aware routing | CAMS、PAK-UCB、NeMoS 等 | “选工具、省调用、在线适应”单独不够新 |

### 目前还可能形成的最小贡献

需要同时满足以下条件，才有机会构成一篇方法论文：

1. **反馈合同专属：** 只更新真实调用且已到达的 typed feedback，严格处理失败、unscorable、乱序和 exactly-once。
2. **增益可识别：** 用随机复核 probe 估计条件 verification gain，而不是把 provider accuracy 当 VOI。
3. **固定状态可证明：** 给出上面的递推等价性、复杂度和延迟可用性证明。
4. **风险门控有界：** 在明确的覆盖、探索和漂移假设下给出 gain/risk 的误差界，至少说明 $\rho$ 和反馈延迟的 trade-off。

如果没有第 2 和第 4 点，论文很可能被评价为“把已有 JEV、在线 bandit 和 linear attention 拼在一起”。

## 7. 实验前必须冻结的理论边界

- M0 只验证 $\widehat q$ 是否比 fixed、random、LinUCB/LinearTS、static JEV、AnyJev observe 更好。
- M1 用随机 probe 验证 $\widehat g$ 是否能预测追加调用的真实收益。
- M2 才让 gain gate 控制调用。
- 报告 prequential loss、risk、coverage、unsafe-answer rate、calls/episode、Brier/NLL/ECE、反馈样本效率和 update latency。
- 如果静态 JEV 或普通 online head 已经达到相同结果，撤回“动态 JEV 方法创新”的主张。

## 8. 参考来源

- [Transformers are RNNs: Fast Autoregressive Transformers with Linear Attention](https://proceedings.mlr.press/v119/katharopoulos20a.html)
- [Linear Transformers Are Secretly Fast Weight Programmers](https://proceedings.mlr.press/v139/schlag21a.html)
- [Adaptive LLM Routing under Budget Constraints (PILOT)](https://aclanthology.org/2025.findings-emnlp.1301/)
- [AnyJev method v3](https://github.com/nokia-applied-research/AnyJev/blob/main/docs/method_v3.md)
- [Online Learning under Delayed Feedback](https://proceedings.mlr.press/v28/joulani13.html)
- [Contextual Active Model Selection](https://arxiv.org/abs/2207.06030)
