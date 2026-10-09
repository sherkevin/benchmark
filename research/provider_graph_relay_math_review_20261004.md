# Provider-Graph Relay v1：数学一致性审查

- 对象：候选更新
  $u_p\leftarrow u_p+a_{p,q_t}x_t\delta_{q_t}$。
- 固定设定：$a_{q,q}=1-\rho$，$a_{p,q}=\rho W_{p,q}$，$\rho=0.5$；$W$ 由 Pearson top-$k$ 正边按 selected-provider 的 column 归一得到。
- 结论：**该更新可以被严格解释为 selected-coordinate likelihood 的线性预条件更新，但当前的 top-k 有向 $A=[a_{p,q}]$ 通常不是 PSD 预条件器，也不能仅由无条件 correctness Pearson 相关推出邻居收益。** 这不是否定候选；它决定了必须采用“有条件的 relay heuristic”表述，或修改成对称 PSD 的可推导版本。

## 1. 记号和 selected-only Bernoulli likelihood

令 provider $p$ 在 query $x$ 上的 correctness 概率为

$$s_p=\sigma(z_p),\qquad z_p=b_p(x)+u_p^\top x,$$

其中 $b_p(x)$ 包含 frozen direct prior、fit bias 和其他固定项。selected provider $q$ 返回 correctness label $y_q\in\{0,1\}$；定义

$$\delta_q=y_q-s_q.$$

selected-only Bernoulli negative log-likelihood 是

$$\ell_q(U;x,y_q)=-y_q\log s_q-(1-y_q)\log(1-s_q).$$

将 $U$ 堆成 provider 行，只有第 $q$ 行的梯度非零：

$$\nabla_U\ell_q=-e_q x^\top\delta_q.$$

因此候选的一次更新可以写成

$$\Delta U=Ae_qx^\top\delta_q=-(A\otimes I_d)\nabla_U\ell_q,$$

其中 $A_{p,q}=a_{p,q}$。所以，**对单个已选事件，候选始终是 selected-coordinate BCE 梯度经过左侧矩阵 $A$ 的更新**。这并不等于已经找到了一个新的 likelihood：它只在 $A$ 满足适当条件时才是合法的固定预条件梯度。

### 1.1 什么时候它是一个明确的 likelihood 更新

若 $A$ 固定、对称、正半定，并且 selected provider $q$ 的抽样分布在该一步被视为外生，则它是

$$U^+=U-\eta(A\otimes I_d)\widehat g_q,$$

的预条件 selected-only SGD；其中 $\widehat g_q=e_qx^\top(\sigma(z_q)-y_q)$。若希望它代表所有 provider 的加权 likelihood，需要把目标明确写为

$$L_\pi(U)=\mathbb E_{q\sim\pi_t}\,[\ell_q(U;x_t,y_q)]$$

并说明 $\pi_t$ 是已定义的策略条件分布。若 $\pi_t$ 由当前 $U$ 决定，则该更新只是在“冻结当前 selection distribution”下的半梯度；它没有自动包含 selection policy 对 $U$ 的导数，也没有对未选 provider 做无偏 full-panel 估计。若论文不需要 full-panel likelihood 解释，可把它诚实地称为 selected-only preconditioned relay。

### 1.2 一个可严格成立的 latent shared-parameter 版本

若想让更新成为一个明确的 selected-only Bernoulli 模型，可以令 provider residual 不是 $P$ 个自由向量，而是

$$u_p=b_p^\top\Theta,\qquad b_p\in\mathbb R^r,\quad \Theta\in\mathbb R^{r\times d}.$$

对 selected $q$ 做普通 BCE SGD：

$$\Theta\leftarrow\Theta+\eta b_qx^\top\delta_q.$$

诱导的 provider 更新为

$$u_p\leftarrow u_p+\eta(b_p^\top b_q)x\delta_q.$$

于是 $A_{p,q}=b_p^\top b_q$ 是 Gram 矩阵，必为对称 PSD。这是一个充分条件：若候选 $A$ 不能写成这样的 Gram 结构，就不能用该 latent likelihood 解释。它也说明了为什么“Pearson top-k 后再按列归一”不能默认具有 likelihood 或 covariance 含义。

## 2. recipient BCE 的一阶损失变化

为了分析邻居 $p$ 是否真的受益，令其反事实 correctness label 为 $y_p$，$s_p=\sigma(z_p)$，$\delta_p=y_p-s_p$。候选更新给 $p$ 的变化是

$$\Delta u_p=a_{p,q}x\delta_q.$$

BCE 梯度为 $\nabla_{u_p}\ell_p=-x\delta_p$，所以一阶变化是

$$\Delta\ell_p^{(1)}
=\nabla\ell_p^\top\Delta u_p
=-a_{p,q}\|x\|_2^2\delta_p\delta_q.$$

因此，对正边 $a_{p,q}>0$：

- $\delta_p\delta_q>0$：一阶上降低 recipient BCE；
- $\delta_p\delta_q<0$：一阶上增加 recipient BCE；
- $\delta_p\delta_q=0$：一阶无收益。

selected provider 自身 $p=q$ 的一阶变化为

$$\Delta\ell_q^{(1)}=-a_{q,q}\|x\|_2^2\delta_q^2\le0,$$

但这只在 $\delta_q$ 真的是更新前 $y_q-\sigma(z_q)$ 时成立。若实现用 release 时重算的 MAP residual、裁剪 residual 或其他 posterior residual，必须重新推导；不能直接套用这个符号结论。

对 logistic BCE，沿该方向的 Hessian 上界为 $\frac14\|x\|_2^2$。一个有用的二阶上界是

$$\Delta\ell_p
\le -\eta a_{p,q}\|x\|_2^2\delta_p\delta_q
+\frac18\eta^2a_{p,q}^2\|x\|_2^2\delta_q^2.$$

即使一阶项为负，也需要足够小的 $\eta$ 和正的 alignment margin 才能保证实际损失下降；当 $\delta_p$ 接近零时没有统一的正步长保证。候选卡目前没有显式 $\eta$ 或 $\|x\|$ 约束，不能宣称每次 relay 都安全。

## 3. Pearson correctness correlation 不足以保证 residual gradient alignment

令

$$m_p(x)=\mathbb E[c_p\mid x],\qquad m_q(x)=\mathbb E[c_q\mid x],$$

其中 $c_p,c_q$ 是 fit 中的 correctness indicator。若当前 logits 完全校准，$s_p=m_p,s_q=m_q$，则

$$\mathbb E[\delta_p\delta_q\mid x]
=\operatorname{Cov}(c_p,c_q\mid x).$$

而候选图用的是未条件化 Pearson correlation，它反映的是

$$\operatorname{Cov}(c_p,c_q)
=\mathbb E[\operatorname{Cov}(c_p,c_q\mid x)]
+\operatorname{Cov}(m_p(x),m_q(x)).$$

第二项只表示两个 provider 在“容易/困难分子”上的平均准确率一起变化，不表示在给定当前 molecule 后两者的 residual 方向一致。若 logits 未校准，还会多出偏差项：

$$\mathbb E[\delta_p\delta_q\mid x]
=\operatorname{Cov}(c_p,c_q\mid x)
+[m_p(x)-s_p(x)][m_q(x)-s_q(x)].$$

### 3.1 具体反例：全局 Pearson 为正，但 relay 一阶上有害

令 $X\in\{A,B\}$ 等概率。两个 provider 在两种 context 的边际正确率均为

$$m_p(A)=m_q(A)=0.9,\qquad m_p(B)=m_q(B)=0.1.$$

在 $A$ 上令联合分布为

$$P(1,1)=0.8,\;P(1,0)=0.1,\;P(0,1)=0.1,\;P(0,0)=0,$$

在 $B$ 上令联合分布为

$$P(1,1)=0,\;P(1,0)=0.1,\;P(0,1)=0.1,\;P(0,0)=0.8.$$

每个 provider 的整体正确率都是 $0.5$。整体

$$\operatorname{Cov}(c_p,c_q)=0.4-0.5\cdot0.5=0.15>0,$$

所以 Pearson 图会保留强正边。然而两种 context 中

$$\operatorname{Cov}(c_p,c_q\mid A)=\operatorname{Cov}(c_p,c_q\mid B)=-0.01.$$

在 logits 校准时，$\mathbb E[\delta_p\delta_q\mid X]<0$，正 relay 边会增加 recipient BCE 的期望，而不是降低它。这个反例不依赖实现 bug；它说明无条件 correctness correlation 不是 transfer correctness 的充分条件。

## 4. 最小可行充分条件

以下任一组条件可以让候选主张变得可检验；它们不是当前数据已经满足的事实。

### 条件 A：保留 directed relay，但只做条件 alignment claim

对每个保留边 $(p,q)$，在未来 selected-query 分布上要求

$$\mathbb E[\delta_p\delta_q\|x\|_2^2\mid p\leftarrow q]\ge \gamma_{pq}>0,$$

且 $0<\eta a_{p,q}$ 足够小，使上面的二阶项不超过一阶收益。fit Pearson 只能作为建边启发，必须在不用于调参的 validation/held-out slice 报告这项 alignment；若不满足，边应被拒绝、降权或安全门禁。此条件支持“经验 relay”而不支持 Bayesian/PSD 叙述。

### 条件 B：改成 symmetric PSD preconditioner

先由 pairwise similarity 构造对称矩阵 $S=(S+S^\top)/2$，再使用 PSD 的核/预条件器 $A\succeq0$，例如

$$A=(1-\rho)I+\rho D^{-1/2}SD^{-1/2},$$

其中 $S$ 需为非负对称邻接，$D$ 为度矩阵，并取 $\rho\le 1/2$；或直接对候选矩阵做预先登记的 PSD projection。然后固定 $A$，明确 update 为 selected-coordinate BCE 的 preconditioned step。这里需要重新处理总更新质量，因为对称归一化不再自动保持每列和为 1。

### 条件 C：采用 latent Gram model

学习或固定 provider embedding $b_p$，令 $A_{p,q}=b_p^\top b_q$，从而保证对称 PSD，并用 $u_p=b_p^\top\Theta$ 给出明确 selected-only Bernoulli likelihood。图稀疏性可作为近似或通过稀疏 embedding 约束，但必须在正文中说明近似误差和实时成本。

### 条件 D：采用一致的 Gaussian/Laplace posterior

若要声称不确定性迁移，可设 $U\sim\mathcal N(0,K\otimes I_d)$，$K\succeq0$。对 selected $q$ 的 logistic 观测使用一阶 Laplace/extended-Kalman 更新，均值沿 $K_{:,q}x$ 更新，并同步做 rank-one covariance downdate，例如

$$K^+=K-\beta K e_qe_q^\top K,\qquad
0\le\beta\le (K_{qq}+\text{noise})^{-1}.$$

候选固定 $A$ 只更新 mean、不更新 variance，不能称为 Bayesian posterior；若保留固定 variance，应明确它是 score heuristic，并测试 uncertainty 不会把邻居过度吸引。

## 5. 非对称 top-k、fallback 和不确定性问题

### 5.1 top-k column-stochastic 不等于 PSD

Pearson $s_{pq}=s_{qp}$，但每列独立 top-$k$ 后 $W$ 一般不对称；再做 column normalization 仍不对称。一个合法的 $k=1,\rho=0.5$ 例子是

$$A=
\begin{bmatrix}
0.5&0.5&0.5\\
0.5&0.5&0\\
0&0&0.5
\end{bmatrix},$$

每列和都是 1，且每列都有 self weight 0.5、最多一个 relay neighbor。但其对称部分

$$\frac{A+A^\top}{2}=
\begin{bmatrix}
0.5&0.5&0.25\\
0.5&0.5&0\\
0.25&0&0.5
\end{bmatrix}$$

的最小特征值约为 $-0.0590$。因此它不是 PSD 预条件器的合法实例；不能由“权重非负、列和为 1”推出凸 likelihood 的单调下降。

### 5.2 fallback 不可变与 graph relay 冲突

若 fallback provider $f$ 的 residual/score 按 v3 immutable，而某个 selected $q\ne f$ 的图邻居包含 $f$，候选公式会写入 $u_f$，直接违反 fallback 不变式。至少需要：

- 从所有 target neighborhoods 删除 $f$，并固定重归一化；或
- 把 fallback 从 graph state 中完全移除，fallback 只保留 fit score；或
- 允许 fallback 更新并重新定义 hazard/safety 合同。

第一种会改变列和/PSD 性质，第二种最干净。该边界必须进入 manifest 和 synthetic test。

### 5.3 uncertainty update 未定义

如果 score 使用 `mean + kappa * sqrt(variance)`，候选只改变邻居 mean 而不减少其 variance，那么它不是共轭或 Laplace posterior。若 variance 保持不变，论文应称 fixed uncertainty relay；若 variance 也更新，必须给出 PSD-preserving covariance downdate 和 selected-only timing。否则 graph 可能传播均值却保留过大的探索 bonus，造成动作变化无法归因。

## 6. 延迟反馈的数学边界

必须预先固定 $\delta_q$ 的计算时刻：

1. **decision-time residual：** 存储当时的 $z_q$，release 时使用旧 residual；这是延迟 SGD，轨迹可重放，但不再是当前参数的梯度；
2. **release-time residual：** 用 release 时的当前 $z_q$ 和旧 $y_q$ 重算；这是当前模型对旧样本的梯度，需说明旧 query 的 selected-policy 分布和 stale label；
3. **MAP residual：** 用后验 mode 求解后的 residual；必须同时说明对应的 objective 和 Hessian。

三者不能在代码中混用。若要把更新写成 likelihood/preconditioner，建议使用 release-time current-logit BCE gradient，并在日志保存 `x`, `y_q`, `z_q_before_update`, `delta_q`, `release_tick`。

## 7. 最小决策测试

在不做效果回放前，checker 至少应通过：

1. **likelihood sign test：** synthetic $\delta_p,\delta_q\in\{-1,0,1\}$ 验证一阶项 $-a_{pq}\|x\|^2\delta_p\delta_q$；
2. **matrix test：** 计算实际 $A$ 的对称性、最小特征值和 $\|A\|_2$；若 claim 使用 PSD/preconditioner，非对称或负特征值直接 BLOCKED；
3. **conditional-alignment test：** 不把全局 Pearson 当通过条件；在 held-out fit/validation 上独立估计 residual product，只作预先登记的边诊断；
4. **fallback test：** 任意 selected provider 下 fallback state/score exact invariant；
5. **uncertainty test：** 若不更新 variance，配置必须明确 `uncertainty_mode=frozen_heuristic`；若更新，检查 covariance PSD 和 rank-one downdate；
6. **delay test：** 决策时和释放时保存的 residual 口径唯一，重放 state hash 一致。

## 8. 审查结论

- **已成立：** 候选更新对 selected provider 是 BCE 的梯度方向（当 $\delta_q=y_q-\sigma(z_q)$），并可写成矩阵 $A$ 的 selected-coordinate 预条件更新。
- **未成立：** Pearson correctness correlation 不保证 recipient residual gradient alignment；top-k column-stochastic $A$ 不保证对称 PSD；固定 mean relay 不自动产生合法 uncertainty posterior；fallback 和 delayed residual 时点尚未闭合。
- **保留方向：** 候选仍可作为经验性的 fit-derived provider relay。最小干净版本是：把图关系和 relay 明确为 transfer heuristic，先验证 held-out conditional alignment、随机图对照和 paired utility；或者将图改成 symmetric PSD/Gram 结构，再主张 selected-only preconditioned likelihood。二者需在预登记中二选一，不能同时含混。
