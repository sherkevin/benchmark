# Exploration-only 动态 JEV：统计合同 v1

日期：2026-09-25

这份文档只回答一个问题：随机探索样本形成的状态，究竟估计什么，以及在什么条件下可以开 gate。它不把回放分数写成泛化保证。

## 1. 原始对象和目标

第 $t$ 个校准样本的 context 是 $C_t$，候选 provider 是 $a\in\mathcal A$，若分别调用停止动作和 provider $a$，对应的任务损失是 $L_{t,\mathrm{stop}}$ 和 $L_{t,a}$。定义潜在验证增益：

$G_{t,a}=L_{t,\mathrm{stop}}-L_{t,a}\in[-1,1]$。

对一个预先固定的校准流 $t=1,\ldots,T$，定义 cell $c$ 的索引集合和有限总体目标：

$I_c=\{t:C_t=c\},\qquad N_c=|I_c|$。

$\mu^{\mathrm{cal}}_{c,a}=\frac{1}{N_c}\sum_{t\in I_c}G_{t,a}$。

这个目标是“当前固定校准流中，cell $c$ 调用 provider $a$ 的平均验证增益”。它不是 held-out 流的均值，也不是未来流的保证。

**Case：** 如果校准流中 `(1,1)` 有 40 个分子，Morgan 的潜在 gain 总和为 12，则 $\mu^{\mathrm{cal}}_{(1,1),\mathrm{Morgan}}=12/40=0.3$。潜在 gain 由完整 truth 和冻结 provider 输出离线计算；在线状态不能读取未调用 provider 的结果。

## 2. 随机探索样本

每个样本先独立抛探索 coin $E_t\sim\operatorname{Bernoulli}(\epsilon)$。当 $E_t=1$ 时，从固定菜单中的 $K$ 个 provider 均匀抽取 $A_t$。定义：

$Z_{t,c,a}=\mathbf 1[C_t=c,E_t=1,A_t=a]$。

$n_{c,a}=\sum_{t=1}^T Z_{t,c,a},\qquad S_{c,a}=\sum_{t=1}^T Z_{t,c,a}G_{t,a}$。

$\widehat\mu_{c,a}=S_{c,a}/n_{c,a}$，仅在 $n_{c,a}>0$ 时定义。

利用分支即使真的调用了 provider，也不更新 $n_{c,a}$ 或 $S_{c,a}$；它只进入终局日志。这样自适应利用不会反过来定义训练标签。

**Case：** `(1,1), Morgan` 的探索样本依次观察到 $0.5,-0.5,1,0$，则 $n=4$、$S=1$、$\widehat\mu=0.25$。同一 cell 的 exploitation 结果不改变这三个数。

## 3. 可证明的无偏性

证明只需要下面的前提：在探索随机化前，校准流的 $C_t$ 和全部潜在 $G_{t,a}$ 已固定；探索 coin 与 provider 抽样独立于这些潜在量；每个 cell/provider 的包含概率相同，即 $q=\epsilon/K$。

给定 $n_{c,a}=m>0$、$C_{1:T}$ 和 $G_{1:T}$，被选中的 $m$ 个索引是 $I_c$ 的均匀子集。因此：

$\mathbb E[\widehat\mu_{c,a}\mid n_{c,a}=m,C_{1:T},G_{1:T}]=\mu^{\mathrm{cal}}_{c,a}$。

这说明当前均值对固定校准目标条件无偏。它不说明 $\widehat\mu$ 对 scaffold-held-out 均值无偏；后者需要额外的可交换性或同分布假设。

## 4. 置信半径和 batch-frozen gate

gain 的范围是 $[-1,1]$，范围长度为 2。第 $\ell$ 个 gate look 对每个 cell/provider 分配：

$\delta_\ell=\delta\frac{6}{\pi^2\ell^2CK}$。

在 $n_{c,a}\ge m_{\min}$ 时使用：

$r_{c,a}(\ell)=\min\left(2,\;2\sqrt{\frac{\log(2/\delta_\ell)}{2n_{c,a}}}\right)$。

$\operatorname{LCB}_{c,a}(\ell)=\max\{-1,\widehat\mu_{c,a}-r_{c,a}(\ell)\}$。

如果 $n_{c,a}<m_{\min}$，LCB 未定义，cell 的 gate 视为关闭。若 $\max_a\operatorname{LCB}_{c,a}(\ell)>\mathrm{call\_cost}$，则在利用分支调用最大 LCB 的 provider；否则 STOP。

同一个 look 只在 batch 起点计算一次，之后整个 batch 复用该 cell 的 gate 和 winner。当前实现每 64 个 episode 建立一个 look；因此 alpha spending 不被同一 look 内的重复检查消耗。探索 coin 仍然每个 episode 独立抛，探索抽样不被冻结。

**Case：** 若 batch 5 起点 `(1,1)` 的 winner 是 MPNN，LCB 为 0.24、成本为 0.1，则 batch 5 的 exploitation 都只允许 MPNN；batch 内新到的反馈要到下一个 batch 才能改变 gate。

## 5. 适用范围和明确不主张的内容

需要满足：

1. 分析集在实验前固定，当前 BBB 回放先排除 8 个 unresolved 分子；
2. provider 菜单固定，探索概率和 provider 抽样概率固定；若它们变化，必须改用已知包含概率的 HT/IPW 状态；
3. 一次调用不会改变另一个 provider 的潜在输出，反馈延迟只改变后续决策；
4. gate 只对固定校准流的当前 prefix 均值作统计判断。

以下内容不由该半径保证：

- 校准均值等于 scaffold-held-out 均值；
- held-out 上的 future gain 一定超过调用成本；
- 当 context 分布或 provider 质量发生漂移时仍有 coverage；
- 未评分、工具失败或动态菜单下的 complete-case 样本仍代表全部请求。

因此论文中应把它写成“fixed calibration stream 上的 exploration-only confidence gate”，并把 BBB 结果写成 structural retrospective utility evaluation。真正的 fresh confirmation 仍需要新分子池、冻结 provider snapshot 和独立 truth。

## 6. 复核结果

有限总体前缀审计固定了 context 和异质潜在 gain，只随机化探索 coin/provider 抽样；500 个 seed、1024 个 episode、每 64 个 episode 一次 look，共 63,706 个有足够 support 的检查，覆盖 63,706 个，pointwise coverage 和 family coverage 都为 `1.0`。commit `2f42546c` 的原始日志：`experiments/logs/exploration_only_finite_population_prefix_audit_v3_20260925/`。

这个结果验证的是上述抽样合同，不是对未来 BBB 分子的泛化承诺。
