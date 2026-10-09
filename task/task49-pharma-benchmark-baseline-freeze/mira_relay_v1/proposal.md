# MIRA-Relay：family-shared residual 的动态工具选择候选

日期：2026-10-02
状态：**候选设计，未实现，未运行，不进入 confirmation。**

## 1. 由现有失败定位问题

ContextProbe 已经证明 selected-only 的 delayed feedback 可以被记录和释放，但当前 shield
分支没有出现非 fallback action change。现有 receipt 还没有保存 shield 可行集合和预测分数，
不能把原因归结为某一个阈值。下一候选先解决一个更基本的可识别性问题：让一条已选 provider
的反馈通过一个有明确语义的共享参数，改变同族 provider 的后续分数；然后用同 schedule 的
NoUpdate 对照判断改变是否真的来自在线反馈。

## 2. 输入和固定状态

每个事件只有已有的三类输入：分子表示 `φ(x)∈R^d`、候选菜单 `A`、provider family registry
给出的 `g(a)`。`w_a^0` 是当前 fold fit-only direct head 的冻结 logit 权重；它提供冷启动分数，
不读取 evaluation truth。provider family 是已有的审计字段，不新增外部分子标签。

令 $c$ 为 fit-only Best Fixed fallback。对每个 provider 保存一个 fit-only residual 参数
`u_g^0∈R^d` 和 provider bias `b_a^0∈R`。候选 provider 的 fit 分数为

$$
z_a^0(x)=(w_a^0)^{\top}\phi(x)+(u_{g(a)}^0)^{\top}\phi(x)+b_a^0.
$$

`u_g^0,b_a^0` 只在 fit 区按固定的带 L2 正则 BCE 拟合；fallback 的参考分数
`z_c^0(x)` 在整个 evaluation stream 中保持不变。

## 3. 在线共享残差

在线状态是所有 family residual 和 provider bias 的拼接向量

$$
\beta_t=(u_{1,t},\ldots,u_{G,t},b_{1,t},\ldots,b_{K,t}).
$$

对 challenger $a\ne c$，定义稀疏事件特征 $q_a(x)$：只有 family $g(a)$ 对应的 $d$ 个位置
等于 `φ(x)`，provider $a$ 对应的 bias 位置等于 `1`，其他位置为 `0`。当前 logit 为

$$
z_{a,t}(x)= (w_a^0)^{\top}\phi(x)+q_a(x)^{\top}\beta_t.
$$

fallback 仍使用不可变的 $z_c^0(x)$，所以一次在线更新不能把安全 fallback 改掉；同一 family
的其他 challenger 会共享更新后的 $u_{g,t}$。这正是要检验的 transfer 通路，且不是把一个
共同的 $\theta^{\top}\phi(x)$ 加到所有 provider 上，因为后者在按 logit 排序时会抵消，不能
改变选择。

## 4. 释放后的实时更新

只在 selected feedback release 后更新被选 challenger。端到端标签为
`system_success = runtime_success * chemical_correct`，并令 $y_t∈\{0,1\}$ 为该标签。
runtime failure 使用 $y_t=0$ 更新端到端 success residual，同时进入独立 hazard ledger，
`chemical_correct` 仍保持 null；有效返回但答案错误时 $y_t=0$ 且
`chemical_correct=0`。unknown/refusal 不更新 correctness residual；若终端状态已经成功返回，
仍可更新 runtime/availability 诊断账本。令当前被选 challenger
的局部状态为 $(\mu_t,s_t)$，其中 `s_t` 是对角 precision projection 的 variance，

$$
v_t=\sum_j q_{t,j}^2s_{t,j},\qquad
z_0=(w_a^0)^{\top}\phi(x_t)+q_t^{\top}\mu_t.
$$

用唯一标量根求局部 Bernoulli MAP：

$$
z^*=z_0+v_t\bigl(y_t-\sigma(z^*)\bigr),\qquad p^*=\sigma(z^*).
$$

令 $h^*=p^*(1-p^*)$。本候选明确采用 **diagonal-precision projection**，而不是把更新后
对角元素误称为完整 Laplace 逆矩阵的对角元素。令 $\tau_{t,j}=1/s_{t,j}$，则只更新非零坐标：

$$
\mu_{t+1,j}=\mu_{t,j}+s_{t,j}q_{t,j}(y_t-p^*),
$$

$$
\tau_{t+1,j}=\tau_{t,j}+h^*q_{t,j}^2,\qquad
s_{t+1,j}=1/\tau_{t+1,j}.
$$

一次 release 的计算量是 $O(d)$，选择时对 $K$ 个 provider 计算 $O(Kd)$；状态量是
`G·d+K`，比每个 provider 保存一个 `d×d` 协方差更适合实时更新。这个对角近似必须在
CPU 对照中固定，不能在看到 evaluation 结果后换成 full covariance。$w_a^0$ 的 offset、fit
BCE 的归一化、L2 和初始 precision 必须在 fit 区固定并写入配置。

## 5. 探索与安全约束

固定 $\epsilon=0.20$ 的 probe 先在当前 safe menu 的 family 集合中均匀抽取，再在选中的
family 内均匀抽取 provider。safe menu 只包含不在 provider hard block、family block 或 runtime
hazard 排除集合中的 challenger。这样不会让 33 个 provider 中占多数的 `b3clf` 家族垄断
探索；family-level 和 provider-level 分母都要报告。若 $\mathcal G_t$ 是 eligible family 集合、
$P_{g,t}$ 是 family $g$ 的 eligible provider 集合，实际 provider 的 propensity 是
`ε/(|𝒢_t|·|P_{g,t}|)`。这只是 probe branch 的概率；若 exploitation action 为
$a_t^{\mathrm{exp}}$，完整行为概率必须记录为

$$
\pi_t(a)=\epsilon q_t(a)+(1-\epsilon)\mathbf{1}[a=a_t^{\mathrm{exp}}],
$$

其中 $q_t(a)$ 是 safe family/provider 抽样分布；若 safe menu 为空，fallback 的概率为 1。
动态 family/provider menu、branch propensity 和完整 $\pi_t$ 必须写入每条 decision。不同策略
共用随机数组只是 common-random-number coupling，不保证动态 safe menu 变化时的 provider action
相同；主要比较不能把它误写成逐 tick 因果配对。seed 对同一 fold×seed 的不同 delay 共用，
但 delay 改变反馈状态后可能改变 safe menu，因此 delay 是预登记 policy condition。

非 probe 分支使用固定的 logit-UCB 分数

$$
s_{a,t}=z_{a,t}+\kappa\sqrt{v_{a,t}},
$$

并先过滤 runtime hazard 上界不低于 fit-only fallback 上界加固定 slack 的 provider；剩余者与
不可变 fallback 一起取最大分数。这里的 UCB 只是探索/排序分数；hazard ledger 约束运行失败，
不等于化学正确性的安全保证。$\kappa$、hazard 置信度和 slack 在 CPU 预登记，不能调参追求
正结果。每条 decision 还必须记录 `available`、`feasible_challengers`、`score` 和
`gate_action`，否则只能作合同检查，不能作机制归因。

## 6. 一个具体实例

例如事件 `z000282` 的输入是已有 frozen encoder 输出 `φ(z000282)`，形状 `(64,)`；选中
`b3clf_xgb_classic_adasyn`，其 registry family 为 `b3clf`。`q_a` 把这 64 个数写入 `b3clf`
对应的 family block，并在该 provider 的 bias 位置写入 `1`。如果该工具返回正确，释放时
只更新 `b3clf` residual 和该 provider bias；同 family 的其他 `b3clf_*` 工具在下一轮共享
这次 residual 改变，其他 family 不被这次更新直接改变。`z_c^0` 始终保持 fit-only 值。

fallback 的 selected feedback 不更新 residual mean/variance 或 provider bias；它仍可更新
runtime success/failure、诊断正确计数和 availability/hazard ledger，并且事件中必须记录
`residual_update=false`。因此本候选主张的是“被探测 challenger 的反馈如何共享”，不是“所有
selected outcome 都训练同一个模型”。这项限制必须在利用率和成本分析中单列。

## 7. 必须先完成的对照和停止条件

CPU full matrix 固定比较：

1. `NoProbeShield`：无 probe，只使用 fit state 和同一 hazard 规则；
2. `ProbeNoUpdate`：同一 probe schedule，但 release 不更新 residual；
3. `ProbeIndependentResidual`：同一 schedule、同一 fit 初始化和同一 diagonal update，但每个
   provider 的 residual 独立，不共享 family block；
4. `ProbeMIRA-Relay`：同一 schedule，使用 family-shared residual 更新；
5. Frozen、Best Fixed 和已停止的 ContextProbe 作为外部和负对照。

主机制 estimand 是 `ProbeMIRA-Relay − ProbeNoUpdate` 的 release 后 shield action change，
共享结构 estimand 是 `ProbeMIRA-Relay − ProbeIndependentResidual`；两者都同时报告
`harmed/rescued/switch-away/runtime failure`。按 molecule/scaffold 做 paired bootstrap；
delay/seed 是重复 block，不把 4,332 行当作独立样本。动作改变只是实现门，不能替代收益门。

以下任一条件触发停止，不申请 A800：

- fit-only 静态检查中非 fallback exploitation 永远不可行动；
- `ProbeMIRA-Relay − ProbeNoUpdate` 没有 release 后可复核的 action change；
- `ProbeMIRA-Relay` 相对 `ProbeNoUpdate` 的 paired system-success 差值未达到预登记最小效果，
  或相对 `ProbeIndependentResidual` 没有正的 family-sharing 差值；
- 作用只来自 probe 直接命中，或 shared residual 的 provider/family permutation 不改变结果；
- harmed 没有改善且 rescued 明显下降；
- 独立 verifier 不能重算 family block、residual state、propensity、hazard、延迟和 gate payload。
