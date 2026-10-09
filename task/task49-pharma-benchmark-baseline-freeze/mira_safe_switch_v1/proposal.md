# MIRA-SafeSwitch 候选方法卡

日期：2026-10-02
状态：候选设计；未运行，未进入 confirmation。

## 问题

当前 MIRA-FamilyGuard 在 4,332 条 common-stratum action 上相对 fit-only Best Fixed
有 188 次 rescue、222 次 harm。主要损失不是 provider 没有信号，而是替代 provider
在证据不足时被选中。FamilyGuard 的 family hazard 能减少重复故障，但没有限制普通
correctness 证据不足时的切换。

## 目标

在同一个 selected-only、延迟反馈和 hidden-64 合同下，让动态模块只在“替代 provider
有足够证据超过 incumbent”时切换；没有足够证据时保留 incumbent。候选的主指标不是
一个更高的 pooled accuracy，而是同时压低 harmed、保留 rescued，并把 runtime failure
和 wrong label 分开。

## 状态和可见信息

每个 provider $a$ 维护四个只由已释放 selected feedback 更新的计数：

- $C_a^+$、$C_a^-$：valid return 且 correctness 为 1/0；
- $F_a$、$N^{term}_a$：runtime failure 次数和已终止调用数。

fit-only 区域给出每个 provider 的静态 correctness prior $\bar r_a$ 和 failure prior
$\bar f_a$。这些先验只来自当前 fold 的 fit 区，不读 validation/evaluation。

先对 fit-only 计数做 Beta(1,1) 平滑：$\bar r_a=(C^{+}_{a,\mathrm{fit}}+1)/(C^{+}_{a,\mathrm{fit}}+C^{-}_{a,\mathrm{fit}}+2)$，
$\bar f_a=(F_{a,\mathrm{fit}}+1)/(V^{ok}_{a,\mathrm{fit}}+F_{a,\mathrm{fit}}+2)$。先验的等效样本量直接取
$\kappa^r_a=\max(20,V^{ok}_{a,\mathrm{fit}})$、$\kappa^f_a=\max(20,V^{ok}_{a,\mathrm{fit}}+F_{a,\mathrm{fit}})$，
而不是把整个 fit 区压成一个固定的 20 个伪样本。初始化为
$\alpha^r_a=\kappa^r_a\bar r_a$、$\beta^r_a=\kappa^r_a(1-\bar r_a)$、
$\alpha^f_a=\kappa^f_a\bar f_a$、$\beta^f_a=\kappa^f_a(1-\bar f_a)$。因此先验的
来源和在线计数是分开的，且 runtime failure 不会被当成 correctness 0。

反馈未释放时，计数和版本不变。反馈释放后，如果是 valid return，只更新 $C_a^+$ 或
$C_a^-$；如果是 runtime failure，只更新 $F_a$ 和 $N^{term}_a$。因此 correctness 与 runtime
hazard 不会共用一个“失败”计数。

## 计算

以 Beta--Bernoulli 后验表示 correctness：

$$
\hat r_{a,t}=\frac{\alpha^r_a+C_a^+}{\alpha^r_a+\beta^r_a+C_a^++C_a^-},\qquad
s^2_{a,t}=\frac{\hat r_{a,t}(1-\hat r_{a,t})}{\alpha^r_a+\beta^r_a+C_a^++C_a^-+1}.
$$

runtime hazard 的上置信界为：

$$
h^U_{a,t}=Q_{1-\delta_h}\!\left(\operatorname{Beta}(\alpha^f_a+F_a,\,
\beta^f_a+N^{term}_{a,t}-F_a)\right).
$$

冻结 selector 给出当前分子的 base score $p_a(x_t)\in[0,1]$。用 fit-only provider
accuracy 对齐动态 correction：

$$
\tilde p_{a,t}(x_t)=\operatorname{clip}\!\left(
p_a(x_t)+\rho(\hat r_{a,t}-\bar r_a),\,0,\,1\right).
$$

候选 provider $a$ 只有同时满足下面三个条件才可替换 incumbent $c$：

$$
\tilde p_{a,t}(x_t)-z_{1-\delta_r}s_{a,t}
\ \ge\
\tilde p_{c,t}(x_t)+\gamma,\qquad \gamma=0.
$$

$$
h^U_{a,t}\le h^U_{c,t}+\eta_h,\qquad
N^{\mathrm{fit}}_a+C_a^++C_a^-\ge n_{\min}.
$$

若没有 provider 满足条件，则选择 $c$。满足条件时，在可行 provider 中取
$\arg\max_a\tilde p_{a,t}(x_t)$。FamilyGuard 的 family gate 仍可作为前置约束；本候选
新增的是 provider-level incumbent safety gate。

这里的 $N^{\mathrm{fit}}_a=V_{a,\mathrm{fit}}$ 是当前 fold fit 区中 provider $a$ 的有效返回数，
它是冻结配置的一部分，不是在线反馈。把 fit 样本计入资格门是 selected-only 冷启动的
必要条件：如果门槛只看 $C_a^++C_a^-$，又不允许额外探索，incumbent 会一直被选中，
替代 provider 永远得不到反馈，候选机制退化成永久固定。在线计数仍然只在释放 selected
feedback 后增加，因此这个修正没有引入未选输出或未来标签。这里不再额外搜索 margin，
统计 LCB 本身就是切换门。门是单侧的：incumbent
已经是当前部署动作，使用其后验均值作为参考；这不是对真实最优 provider 的统计置信保证，
候选是否减少实际 harmed 必须由 paired development receipt 验证。

## 复杂度和实时性

每次 delayed feedback 只更新一个 provider 的四个计数和两个 Beta 后验，时间为
$O(1)$、状态为 $O(|A_t|)$。每次选择计算 33 个 provider 的 correction、hazard bound
和 margin，时间为 $O(|A_t|)$；不重放历史、不读取未选输出。状态快照包含计数、后验
参数、incumbent、版本和 release queue hash。

## 最近邻和区别

- Frozen：没有在线 correction 或切换。
- Matched Online-SGD：直接更新 score，没有 incumbent safety margin。
- MIRA-FamilyGuard：有 family hazard，但没有 correctness confidence gate。
- MIRA-SafeSwitch：在同一 MIRA state 上增加 provider-level correctness confidence、
  runtime hazard 上界、最小证据量和 incumbent margin。

候选贡献是把工具选择中的动态适应写成一个可恢复的“安全切换”问题：收益来自可观测
selected feedback，代价用 harmed/rescued/runtime failure 分解，而不是只报最终准确率。
该贡献仍须由后续 ablation 和 matched receipt 证伪。

## 失败条件

在相同 common-direct-prior 和事件合同下，若候选不能同时满足：

1. harmed 至少比当前 FamilyGuard 的 222 次下降 15%；
2. rescued 保留当前 188 次的 80% 以上；
3. runtime failure 不高于当前 10 次；

则不把 SafeSwitch 写成主方法收益。即使通过，也必须继续做 scaffold/family paired
uncertainty 和冻结 confirmation。
