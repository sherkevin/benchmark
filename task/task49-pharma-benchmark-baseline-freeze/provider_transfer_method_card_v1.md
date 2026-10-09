# Provider-transfer JEV：方法卡 v1

日期：2026-09-27

状态：候选方法合同，尚未做 Nebula 效果实验。

## 1. 任务

一题包含分子性质请求 $x_t$、固定候选菜单 $mathcal A_t$ 和已释放的 selected-only 反馈。每个 provider $a$ 的反馈是 $r_tin{0,1}$。决策时不能看未选 provider 的输出，也不能看本题真值。

已有冻结 JEV 给出每个 provider 的冷启动 logit $ell_a^0(x)$。动态模块只学习一个小的残差状态，不重算冻结 backbone。

贯穿算例：候选 provider 是 `admetlab`、`lgbm`、`deeppurpose`，所以 $K=3$。当前分子经过冻结编码器得到 $h(x)=(1,0.5)^	op$。冻结 logit 为 $ell^0(x)=(0,0,0)$，所以三个 provider 的冷启动概率都是 $0.5$。下面的二维数值只用于核对更新，不是新的模型输出。

## 2. fit-only provider 几何

在当前 outer fold 的 fit 区，对每个分子和 provider 得到正确性标签 $y_{ia}in{0,1}$，以及冻结概率 $p^0_{ia}=sigma(ell_a^0(x_i))$。残差为

$$
e_{ia}=y_{ia}-p^0_{ia}.
$$

把同一个分子的 $K$ 个残差排成矩阵 $E\in\mathbb R^{n_{\mathrm{fit}}\times K}$，只在 fit 区计算 provider 几何。先按 provider 中心化，$\widetilde E_{:a}=E_{:a}-\frac{1}{n_{\mathrm{fit}}}\sum_iE_{ia}$，再令 $G=\widetilde E^\top\widetilde E/(n_{\mathrm{fit}}-1)$。取 $G$ 的前 $q$ 个特征对 $(V_q,\Lambda_q)$，定义 $U=V_q\Lambda_q^{1/2}\in\mathbb R^{K\times q}$。第 $a$ 行记为 $u_a^\top$，于是 $UU^\top$ 是 fit-only provider 残差协方差的低秩近似。

这一步的含义很具体：如果两个 provider 在 fit 区对同一类分子一起高估或一起低估，它们的 $u_a^\top u_b$ 会较大；如果关系接近独立，内积接近零。evaluation 区不参与 $U$ 的计算。

## 3. 动态状态和选择分数

动态状态是一个小矩阵 $W_t\in\mathbb R^{q\times d}$，初始为零。对候选 provider $a$，残差修正为

$$
\delta_{t,a}=u_a^\top W_t h(x_t),
$$

选择分数为

$$
s_{t,a}=\ell_a^0(x_t)+\mathbf 1[n_t\ge n_0]\,\operatorname{clip}(\delta_{t,a},-b,b),
$$

其中 $n_t$ 是当前已经释放的反馈数，$n_0$ 和 $b$ 只用 validation 区选择。$n_t<n_0$ 时完全使用 Frozen JEV；这给动态模块一个明确的冷启动边界。选择动作为菜单内分数最高的 provider，平票按固定 provider 顺序处理。

算例中令 $q=1$、$u=(1,0.8,-0.2)^\top$，$W_0=0$，并假设 $n_t\ge n_0$、$b$ 足够大。此时三个 provider 的 $\delta$ 都是零，仍然平票。

## 4. selected-only 延迟更新

provider $a_t$ 的反馈到达时，保存的是决策时复制的 $(a_t,h_t)$。当前预测概率为

$$
p_t=\sigma\!\left(\ell^0_{a_t}(x_t)+u_{a_t}^\top W_t h_t\right).
$$

对带冻结 offset 的二元交叉熵加 Frobenius 正则做一步在线梯度下降：

$$
W_{t+1}=(1-\eta\lambda)W_t-
\eta\,\operatorname{clip}(p_t-r_t,-\rho,\rho)\,u_{a_t}h_t^\top.
$$

其中 $\eta>0$、$\lambda\ge0$、$\rho>0$ 在 validation 区固定。$\rho$ 只限制一次反馈的影响，不改变标签。反馈延迟时，更新在释放 tick 顺序执行；未释放的事件不更新，未选 provider 的输出不进入状态。

上式正是下列损失的一步有界梯度更新：

$$
L_t(W)=-r_t\log p_t-(1-r_t)\log(1-p_t)+\frac{\lambda}{2}\lVert W\rVert_F^2.
$$

因此它不是把“相关性”当作额外标签，而是用 fit-only 的 $u_a$ 把同一个 selected feedback 映射到共享残差坐标。

算例：令 $W_0=0$、$\ell^0_{a_t}=0$、所以 $p_t=0.5$；选中第一个 provider 且反馈 $r_t=0$，取 $\eta=0.1,\lambda=0,\rho=1$。则

$$
W_1=-0.1(0.5)(1)(1,0.5)=(-0.05,-0.025).
$$

对同一个分子，第二个 provider 的修正为 $0.8[-0.05-0.025(0.5)]=-0.05$，第三个 provider 的修正为 $-0.2[-0.05-0.025(0.5)]=+0.0125$。一次第一个 provider 的错误反馈会降低与它同向的第二个 provider 分数，而提高反向的第三个 provider 分数；这就是候选方法相对于独立 provider head 的可检验差异。

## 5. 为什么会发生跨 provider 转移

一次反馈导致的状态变化为

$$
\Delta W=-\eta\,g_t\,u_{a_t}h_t^\top,
\qquad g_t=\operatorname{clip}(p_t-r_t,-\rho,\rho).
$$

任意未选 provider $b$ 在未来请求 $x'$ 上的 logit 变化为

$$
\Delta\ell_b(x')=-\eta\,g_t\,(u_b^\top u_{a_t})\,(h_t^\top h(x')).
$$

所以转移强度由两个可审计的内积相乘：fit-only provider 关系 $u_b^\top u_{a_t}$，以及固定请求表示相似度 $h_t^\top h(x')$。若 provider 关系被置换或设成对角，跨 provider 项消失；这是必须做的消融。

## 6. 实时性和状态

先算 $v_t=W_t h_t$，再算全部候选的 $Uv_t$。因此一次请求的动态部分成本是 $O(qd+Kq)$；一次释放反馈的更新成本是 $O(qd)$；状态大小是 $O(Kq+qd)$。它不保存全量历史，也不重算旧 backbone 表示。

实际报告仍需把 tokenizer、冻结 backbone、候选打分、反馈队列和日志成本一起计入；不能只报告这一个小矩阵的耗时。

## 7. 与 baseline 的明确差异

- Frozen：$W_t=0$，不使用反馈。
- Online-SGD：每个 provider 有独立残差参数；等价于把 $U$ 换成 provider one-hot，selected feedback 不转移到别的 provider。
- Replay-SGD：在独立 provider 参数上回放历史 selected feedback；它增加历史样本，不增加 provider 关系。
- BayesianRouter：独立 provider 后验更新；把先验协方差设成对角时是本方法的独立关系消融。
- Provider-transfer JEV：只新增 fit-only 的 $U$，并用共享 $W_t$ 产生受约束的跨 provider 修正。

不能把低秩分解、梯度下降或协方差更新本身宣称为新算法。论文若成立，贡献只能是：在医药 direct tool correctness 的 selected-only、延迟反馈条件下，利用 fit-only provider residual geometry 做实时跨 provider 适应，并证明它在安全门和成本约束下确实比独立更新多解决了什么。

## 8. 必须先做的否定实验

在同一回放中先比较 Frozen、独立 Online-SGD、provider-transfer replay 和随机置换 $U$。只要 provider-transfer 在三个 delay 中至少两个低于 Frozen，或真实 $U$ 与置换 $U$ 没有可重复差异，就删除这条主方法，不申请完整确认实验。通过后才进入统一 runner 的 development 任务。
