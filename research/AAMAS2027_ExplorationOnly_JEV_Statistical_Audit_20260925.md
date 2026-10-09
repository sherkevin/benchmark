# Exploration-only context JEV：统计目标与置信边界审查

日期：2026-09-25  
审查对象：`src/probe_headless/m3_context_jev.py`、`ExplorationOnlyState`、
`ContextGatePolicy` 以及 v2 MVP 规格。

## 结论

这个更新思路本身是成立的：固定回放序列上，探索 coin 和 provider 抽样独立于潜在 gain，因而只用探索结果做每个 context-provider 的均值，能够避免 exploitation 结果带来的选择偏差。

但是当前实现的置信门还不能作为有覆盖保证的实时门，原因有四个：

1. gain 的范围是 $[-1,1]$，代码中的 Hoeffding 半径少了一个范围长度因子 2；
2. 半径按 batch look 分配 $\delta$，但同一 look 内每个 episode 都会重新检查门，alpha spending 没有覆盖这些重复检查；
3. 校准流的置信区间只对应校准流的 context-provider 均值，不能自动覆盖 scaffold-held-out 流的均值；
4. 只要探索概率、候选菜单或 provider 抽样概率随时间改变，未加权的探索样本均值就不再是原目标的无偏估计。

因此目前可以把它称为“探索样本均值的在线更新原型”，还不能在论文中写成“具有 anytime coverage 的动态 JEV”。

## 1. 估计的准确目标

设第 $t$ 个回放样本的 context 是 $C_t$，候选 provider 为 $a$，潜在 gain 为

$$G_{t,a}=L_{t,\mathrm{stop}}-L_{t,a}\in[-1,1].$$

对固定校准序列，定义 cell 的样本集合和目标均值：

$$I_c=\{t:C_t=c\},\qquad N_c=|I_c|,$$

$$\mu^{\mathrm{cal}}_{c,a}=\frac{1}{N_c}\sum_{t\in I_c}G_{t,a}.$$

当前代码记录

$$Z_{t,c,a}=\mathbf 1[C_t=c,E_t=1,A_t=a],$$

$$n_{c,a}=\sum_t Z_{t,c,a},\qquad
\widehat\mu_{c,a}=\frac{\sum_t Z_{t,c,a}G_{t,a}}{n_{c,a}}.$$

如果序列 $C_t$ 和所有潜在 gain 在随机探索前已经固定，$E_t\sim\mathrm{Bernoulli}(\epsilon)$，且探索时 $A_t$ 独立均匀抽取，那么给定 $n_{c,a}=m>0$，被选中的 $m$ 个索引是 $I_c$ 的均匀子集。因此

$$\mathbb E[\widehat\mu_{c,a}\mid n_{c,a}=m,C_{1:T},G_{1:T}]
=\mu^{\mathrm{cal}}_{c,a}.$$

这说明当前均值在固定回放校准目标上是条件无偏的。$n_{c,a}=0$ 时没有估计量；代码用 `min_observed` 关闭门是合理的。

这个结论不等于校准均值覆盖测试均值。对 scaffold-held-out 流应该另定义

$$\mu^{\mathrm{test}}_{c,a}=\frac{1}{N^{\mathrm{test}}_c}
\sum_{t\in I^{\mathrm{test}}_c}G_{t,a},$$

除非预先假设两个流在给定 context 后可交换或来自同一分布，否则由校准探索样本构造的半径没有覆盖 $\mu^{\mathrm{test}}_{c,a}$ 的理由。

## 2. 成立所需的条件

当前无加权均值至少需要以下条件：

1. 回放中的实体顺序、context 和每个 provider 的潜在 gain 在探索随机化前固定，或者对当前随机化是可预测且不受当前动作影响；
2. 每个 $c,a$ 的探索包含概率相同，当前实现是 $q=\epsilon/K$；
3. 探索抽样与潜在 gain 条件独立；
4. 一次工具调用不会改变另一个 provider 的潜在结果，truth 和 cache 不随调用顺序改变；
5. provider 菜单固定。若菜单或 $q_t(a)$ 改变，样本必须记录可见菜单和实际包含概率；
6. 反馈延迟只影响之后的决策，不改变已经定义的 $G_{t,a}$。

固定 BBB 回放满足第 1、3、4、6 条的工程近似，但只在候选菜单固定、探索概率固定时满足第 2、5 条。

## 3. 当前置信半径的两个硬错误

### 3.1 gain 范围因子少一倍

代码使用

$$r=\sqrt{\frac{\log(2/\delta)}{2n}}.$$

这个式子适用于变量范围为 $[0,1]$。当前 $G_{t,a}\in[-1,1]$，范围长度是 2，Hoeffding 半径应为

$$r=2\sqrt{\frac{\log(2/\delta)}{2n}}
=\sqrt{\frac{2\log(2/\delta)}{n}}.$$

例如 $\delta=0.05$、$C=4$、$K=2$、第一个 look、$n=4$ 时，当前代码的半径约为 $0.885$，正确的半径约为 $1.770$。因此当前 LCB 可能过高，coverage 不能成立。

`min(1.0, radius)` 也不能修复这个问题。真实均值和样本均值都在 $[-1,1]$，误差上界可以接近 2；安全做法是保留正确半径，并将最终 LCB 下截到 $-1$，而不是把半径上截到 1。

### 3.2 同一 look 内重复检查没有被 alpha spending 覆盖

代码把 $\ell=\lfloor t/64\rfloor+1$ 作为 look index，但每个 episode 都重新调用 `lcb`。同一个 $\delta_\ell$ 实际被最多 64 次自适应检查复用。原规格中的

$$\delta_\ell=\delta\frac{6}{\pi^2\ell^2CK}$$

只足以对每个 look 检查一次做 union bound，不能覆盖这 64 次选择性检查。

推荐二选一：

- 每个 batch 只在一个固定时刻计算 LCB，并在整个 batch 内冻结 gate；
- 使用真正的 confidence sequence，或者为每个 episode 分配独立的 $\delta_t$。

第一种改动更干净，也更容易审稿人复核。

## 4. 反例和边界

### 抽样概率随样本变化

若困难分子使用更大的 $\epsilon_t$，则这些分子更容易进入探索均值。未加权均值估计的是按 $q_t$ 重加权的 gain，而不是所有 cell 样本的均值。此时应使用已知包含概率的 HT/IPW 估计，或将目标改为探索分布下的均值。

### 动作改变后续样本

如果真实工具调用会改变外部状态，或者 gate 的动作会改变后续实体的选择，那么探索事件不再只是固定序列上的随机子集。当前证明不能直接迁移到这种环境；需要把潜在轨迹和干扰假设写进问题定义。

### scaffold shift

同一个二值 context 内，校准 scaffold 和测试 scaffold 的 provider gain 可能不同。此时 calibration CI 即使对 $\mu^{\mathrm{cal}}_{c,a}$ 有效，也不对 $\mu^{\mathrm{test}}_{c,a}$ 有效。当前 BBB 结果应标注为 structural retrospective evaluation，不能把门的 calibration coverage 当作 held-out coverage。

## 5. 建议的最小修正顺序

1. 先把 gain 的 Hoeffding 半径改成范围 $[-1,1]$ 的版本，取消 `min(radius,1)`；
2. 将 gate 检查改为每 64 个样本一次，并冻结下一个 batch 的决策；
3. 在实验报告中把置信目标明确写成 $\mu^{\mathrm{cal}}_{c,a}$，删除对 scaffold-held-out 均值的 coverage 暗示；
4. 增加一个变 $\epsilon_t$ 的仿真，验证未加权均值的偏差，并保留 HT/IPW 作为后续可选版本；
5. 用异质固定 gain 做 10,000 次 coverage 仿真，分别检查“当前半径”和“修正半径”在重复 look 下的覆盖率。

完成以上步骤之前，M3 可以继续作为工程候选比较，但不应作为已经有理论保证的最终方法。
