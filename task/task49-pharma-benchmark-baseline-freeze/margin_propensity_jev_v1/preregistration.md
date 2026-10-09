# Margin-triggered propensity residual JEV v1

日期：2026-09-27

## 动机

统一随机探索的 PCR-JEV 已停止：它把探索成本支付在所有题上，且把 residual 修正作用到高置信题。新候选只改变探索和更新的决策区域：只处理冻结 JEV 的 top-two 分数接近的题。

## 方法合同

outer-fold fit 区得到校准后的冻结 correctness 概率 $q_a^0(x)$。同一 fit 区计算每题最高和次高 provider 分数的 margin，并固定

$$
\tau_f=\operatorname{median}_{x\in\mathrm{fit}}\left(q^0_{(1)}(x)-q^0_{(2)}(x)\right).
$$

evaluation 中只有 $m_t=q^0_{(1)}(x_t)-q^0_{(2)}(x_t)\le\tau_f$ 的模糊题进入探索；其余题直接使用 Frozen，propensity 为 1。模糊题以 $\epsilon=0.5$ 的概率均匀抽 provider，否则按当前动态分数的 argmax。对模糊题的实际选择，propensity 是

$$
\pi_t(a)=\epsilon/K+(1-\epsilon)\mathbf{1}[a=\arg\max_b s_t(b)].
$$

反馈释放后，只更新该题所属的“模糊”状态：

$$
W_a\leftarrow W_a+1/\pi_t(a_t),\qquad
S_a\leftarrow S_a+(r_t-q^0_{a_t}(x_t))/\pi_t(a_t),
$$

$$
\widehat\delta_a=S_a/(W_a+20),\qquad
s_t(a)=q_a^0(x_t)+\widehat\delta_a\quad (m_t\le\tau_f).
$$

分数截断到 $[0,1]$。反馈延迟 $d\in\{0,1,4\}$，只在 release tick exactly-once 更新；未选 provider 和未释放事件不进入状态。高 margin 题永远保持 Frozen，因此动态模块只承担“是否值得改变当前选择”的区域。

## 对照与指标

- Frozen：所有题都按 $q^0$ argmax。
- Margin-Audit-Frozen：完全相同的 fit margin、epsilon、随机种子和探索区域，但不做 residual 更新。
- Margin-PCR-JEV：本候选。

报告 pooled/stream accuracy、相对 Margin-Audit-Frozen 的 paired delta、模糊题比例、探索率、provider support、反馈更新数以及选择/更新耗时。5 folds × 2 seeds × 3 delays，confirmation 关闭。

## 证伪条件

1. Margin-PCR-JEV 在至少两个 delay 上不超过 Margin-Audit-Frozen；或
2. 三个 delay 的 pooled accuracy 都低于 Frozen；或
3. 独立 NumPy 重放不能重算 fit margin、propensity、延迟释放和 $S/W$。

任一条件成立即停止，不申请 A800，不把门限或 epsilon 调到 evaluation 结果上。

## 边界

propensity correction、margin exploration 和 shrinkage 都有成熟先例。可主张内容仅限于：把动态 JEV 的实时更新限制在“当前选择不确定”的医药工具选择区域，并用 delayed selected-only feedback 做可审计的 residual 校正。没有超过同探索对照时不进入主方法。

