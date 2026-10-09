# MIRA-ContextShield 候选方法卡

日期：2026-10-02  
状态：设计候选；尚未实现，尚未运行，不进入 confirmation。

## 要解决的具体问题

FamilyGuard/SafeSwitch 的 provider 边际账本不能判断当前分子上的 provider 可靠性。
v5 已经给出反例：第一次切换时 challenger 没有任何在线反馈，之后 143 个动作仍沿用
同一个 challenger。下一候选必须把“当前分子上的不确定性”放进决策公式，并保留一个
不会被一次错误切换永久改写的 fit-only fallback。

## 状态和信息边界

每个 provider $a$ 维护 context-conditioned Gaussian logit surrogate state 的均值 $\mu_{a,t}$ 和
协方差 $\Sigma_{a,t}$。初值只使用当前 fold 的 fit 区和冻结的 direct head；在线阶段
只在 selected feedback release 后更新被选 provider。当前候选只保留 FamilyGuard 的 hard availability ledger，
不使用其 soft family ranking factor；该差异必须由 no-family/family-factor 对照隔离。runtime failure 另维护 $F_{a,t}$ 和
$N^{term}_{a,t}$，不把运行失败当成 correctness 0。这里的 `direct_prior` 是 common-stratum
封存的 BCE/logit head；$\sigma(\phi^\top\mu)$ 是决策分数，不默认等于校准概率。

fit-only fallback 是 $c=\arg\max_a \operatorname{mean}_{i\in fit}r_{i,a}$。它是不可变的
参考 provider；每个决策都用它的 fit-only 后验作参照，不把上一次 challenger 永久设成新的
参照。被选中的 $c$ 仍可更新自己的在线状态，但这些更新不改写安全参考。

## fit-only 后验初值

令 $\phi_i$ 是冻结 encoder 输出，$w_a^{fit}$ 是 fit-only direct head。以
$p_{i,a}=\sigma((w_a^{fit})^\top\phi_i)$ 构造经验 Fisher：

$$
A_{a,0}=\lambda I+\frac{1}{N_{fit}}\sum_{i\in fit}p_{i,a}(1-p_{i,a})\phi_i\phi_i^\top,
\qquad \mu_{a,0}=w_a^{fit},
\qquad \Sigma_{a,0}=A_{a,0}^{-1}.
$$

这里的 $λ=0.01$ 在 CPU 预登记中固定，曲率求和除以 fit 行数，和 direct BCE 的 mean loss 同尺度；
不能看 evaluation 调整。它把 fit 区已有的上下文支持写进协方差，而不是把所有 provider 压成同一个 20-count Beta 先验。

## 释放后的实时更新

在线目标统一为 `system_success`。有效返回时 $y_t=chemical_correct$；运行失败时
$y_t=0$，但 `chemical_correct` 仍保持 null 并单独进入 runtime ledger；unknown/refusal 不更新。
对被选 provider $a_t$，以当前状态为局部先验，定义一条观测的 Bernoulli 目标：

$$
Q(w)=\frac12(w-\mu_{a_t,t})^\top\Sigma_{a_t,t}^{-1}(w-\mu_{a_t,t})
+\log(1+e^{\phi_t^\top w})-y_t\phi_t^\top w.
$$

令 $z_0=\phi_t^\top\mu_{a_t,t}$、$v=\phi_t^\top\Sigma_{a_t,t}\phi_t$。在固定 80 次二分、
残差容差 $10^{-12}$ 内求唯一标量根

$$
z^*=z_0+v\bigl(y_t-\sigma(z^*)\bigr),
\qquad p^*=\sigma(z^*).
$$

再令 $u=\Sigma_{a_t,t}\phi_t$，更新为

$$
\mu_{a_t,t+1}=\mu_{a_t,t}+u(y_t-p^*),
$$

$$
\Sigma_{a_t,t+1}=\Sigma_{a_t,t}-\frac{p^*(1-p^*)}{1+p^*(1-p^*)v}uu^\top.
$$

这是一条单观测局部 MAP 与 Laplace 曲率更新；它不是对完整历史后验的精确闭式求解，也不是安全/regret 定理。
其他 provider 的状态不变；反馈尚未 release 时所有状态不变。$v=0$ 时状态保持不变。

## 当前分子的安全门

令候选 provider 的当前 logit surrogate 分数为 $m_{a,t}=\sigma(z_{a,t})$，其中
$z_{a,t}=\phi_t^\top\mu_{a,t}$、$v_{a,t}=\phi_t^\top\Sigma_{a,t}\phi_t$。安全参考使用不可变的
fit-only 分数 $m_{c,0}=\sigma(z_{c,0})$。在 logit 空间使用固定的单侧半径：

$$
\ell_{a,t}=\sigma\bigl(z_{a,t}-z_{1-\delta}\sqrt{\max(v_{a,t},0)}\bigr).
$$

运行失败使用独立的 Beta 账本。令 $V^{fit}_a$ 和 $F^{fit}_a$ 分别是 fit 区有效返回数和
运行失败数，在线释放后的计数为 $V_{a,t}$ 和 $F_{a,t}$，则候选的运行失败上界定义为

$$
h^U_{a,t}=\operatorname{BetaQuantile}_{1-\delta_h}
\bigl(1+F^{fit}_a+F_{a,t},\;1+V^{fit}_a+V_{a,t}\bigr).
$$

固定参考使用 fit-only 上界 $h^U_{c,0}=\operatorname{BetaQuantile}_{1-\delta_h}
(1+F^{fit}_c,1+V^{fit}_c)$。

候选 provider $a$ 可以替换固定 fallback $c$，当且仅当

$$
\ell_{a,t}\ge m_{c,0},
\qquad h^U_{a,t}\le h^U_{c,0}+\eta_h.
$$

没有可行候选时选择 $c$。由于本协议没有 refusal action，$c$ 始终保持 action-eligible；
可行候选中取 $\arg\max m_{a,t}$。这是一条工程上的单侧 logit-space context shield，不是
校准概率置信区间，也不是 formal safety/regret theorem；当前数据不满足 positivity 时，不能把
未选 provider 的潜在结果当作已识别。

## 符号实例化

以一条具体回放事件为例：分子行 `z000282` 的编码向量是 `phi_t=hidden[z000282]`，形状
为 `(64,)`；provider `b3clf_xgb_classic_adasyn` 的 `direct_prior` 是 sealed snapshot 中
该 provider 的 `(64,)` BCE/logit 权重；`system_success=1` 表示工具正常返回且预测正确，
`system_success=0` 表示正常返回但预测错误或运行失败。候选菜单是 33 个 provider 的完整列表，
`K=33,d=64`。更新时 `u`、`v`、`z*`、`p*` 分别是形状 `(64,)`、标量、标量、标量；
释放前 $\mu,\Sigma$ 不变，释放后只改变被选 provider 的一行均值和一个 `64×64` 协方差。

参考 provider $c$ 是该 fold fit 区 `system_success` 平均值最高的 provider；例如 fold0 的
候选 smoke 中它是 `admetlab_3_0`。它的参考均值和 hazard 都从 fit-only state 计算，后续
即使再次被选中也不改写 gate 的 reference。

## 实时性

完整协方差版本每次选择计算 $K$ 个二次型，时间 $O(Kd^2)$；一次 release 的 rank-one
更新时间为 $O(d^2+I)$，其中 $I=80$ 是固定标量求根迭代次数；状态为 $O(Kd^2)$。$K=33,d=64$ 时应直接记录 p50/p95，若成本不达标
再登记对角或低秩近似，不能在实验后静默替换。

## 反例和停止条件

- 如果 ContextShield 与 Frozen/Best Fixed 一样没有任何可行 challenger，说明当前 fit 表示
  没有足够的上下文 headroom；不把它包装成动态收益。
- 如果它产生的 fixed-reference harmed 不低于 SafeSwitch，或 rescued 大幅下降，停止该
  gate，回到表示/反馈可识别性问题。
- 如果 improvement 只出现在 full-panel fit prior、没有 selected-only release 后的
  action/state 改变，不能称实时学习。
- 如果独立 verifier 不能从 raw event 重算 posterior、action、delay 和 state hash，
  不进入 full common-stratum receipt。

## 最近邻和区分

BayesianRouter/Greedy 使用 context state 直接选最高分；ContextShield 的新增接口是
固定 fit fallback、posterior-predictive one-sided shield、独立 runtime ledger 和
fixed-reference harmed/rescued 分解。它仍须在 matched prior、同一 feedback clock 和
相同预算下证明这组组合确实解决 SafeSwitch 的失败模式。
