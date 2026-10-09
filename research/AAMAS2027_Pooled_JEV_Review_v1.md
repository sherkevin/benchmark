# Pooled / hierarchical JEV 候选审查

日期：2026-09-25

这份审查只讨论一个问题：当前 4-cell exploration-only gate 全关时，能否通过 pooling 增加有效样本量，同时保留可证明的置信覆盖。

## 1. 无额外假设的最小候选：global pooled gate

对 provider $a$，固定 calibration stream 的潜在验证增益为 $G_{t,a}\in[-1,1]$，整体目标定义为

 $\mu_a^{\mathrm{pool}}=T^{-1}\sum_{t=1}^{T}G_{t,a}$。

探索 coin 和 provider 抽样独立于潜在增益，且每个 provider 的探索包含概率固定为 $q=\epsilon/K$。令

 $n_a=\sum_t\mathbf 1\{E_t=1,A_t=a\},\qquad \widehat\mu_a=n_a^{-1}\sum_{t:E_t=1,A_t=a}G_{t,a}$。

给定 $n_a=m>0$，被抽中的索引是固定 calibration stream 的均匀无放回样本。因此 Hoeffding 半径

 $r_a(\ell)=2\sqrt{\frac{\log(2/\delta_{a,\ell})}{2n_a}}$

覆盖 $\mu_a^{\mathrm{pool}}$，其中

 $\delta_{a,\ell}=\delta\frac{6}{\pi^2\ell^2K}$。

因为这是无放回抽样，也可以使用合法的 Serfling 修正

 $r_a^{\mathrm{Serf}}(\ell)=r_a(\ell)\sqrt{\frac{T-n_a+1}{T}}$。

全局门定义为

 $\max_a\{\widehat\mu_a-r_a\}>c$。

门打开后，winner 对所有 context 使用；门关闭时停止或继续探索。这个目标是整个 calibration stream 的平均调用收益，不提供每个 context 的保证。

## 2. 保留 context 目标时的最小 partial pooling

对 cell $c$ 定义

 $\mu_{c,a}=N_c^{-1}\sum_{t:C_t=c}G_{t,a}$，

并令 $\mu_{-c,a}$ 是其余 cell 按样本量加权的平均。分别用 cell 内和 leave-one-cell-out 的 exploration-only 样本得到 $\widehat\mu_{c,a}$ 与 $\widehat\mu_{-c,a}$。

固定 $\lambda\in[0,1]$，构造

 $\widetilde\mu_{c,a}(\lambda)=\lambda\widehat\mu_{c,a}+(1-\lambda)\widehat\mu_{-c,a}$。

若有实验前确定的异质性界

 $|\mu_{c,a}-\mu_{-c,a}|\le\Gamma_{c,a}$，

则在相应的 simultaneous event 上，合法的 cell LCB 是

 $\operatorname{LCB}_{c,a}(\lambda)=\widetilde\mu_{c,a}(\lambda)-r_{c,a}(\lambda)-(1-\lambda)\Gamma_{c,a}$，

其中可用

 $r_{c,a}(\lambda)=\sqrt{2\log(2/\delta_{c,a,\lambda,\ell})\left(\frac{\lambda^2}{n_{c,a}}+\frac{(1-\lambda)^2}{n_{-c,a}}\right)}$。

这里使用 leave-one-cell-out 是为了让两份探索样本来自不相交的 stream 部分；如果用同一份 pooled mean，协方差必须显式处理。若从有限候选集合 $\Lambda$ 选择 $\lambda$，应把 $|\Lambda|$ 纳入 alpha 分配，再取最大 LCB；不能用同一数据无校正地调半径。

对应的有效样本量是

 $n_{\mathrm{eff}}(\lambda)=\left(\frac{\lambda^2}{n_{c,a}}+\frac{(1-\lambda)^2}{n_{-c,a}}\right)^{-1}$。

pooling 获得的样本量收益必须支付 $(1-\lambda)\Gamma_{c,a}$ 的偏差项。没有外部或预注册的异质性界时，唯一普适界是 $\Gamma_{c,a}=2$，此时最优选择退化为 $\lambda=1$，即当前 cell-only gate。

## 3. 当前 BBB 支持量判断

当前 calibration 有 $T=616$，$\epsilon=0.8$，$K=2$，所以 pooled 每 provider 的期望探索支持约为 $246$；最大的单 cell 只有约 $154$。第 10 个 look、$C=4$、$K=2$、$\delta=0.05$ 时

 $\delta_{10}=0.05\frac{6}{\pi^2\cdot10^2\cdot4\cdot2}=3.80\times10^{-5}$。

因此报告中写成 $3.8\times10^{-6}$ 是算术错误。对应的 Hoeffding 半径在 $n=246$ 时约为 $0.297$，Serfling 修正后约为 $0.230$。若真实 gain 为 $0.3$、调用成本为 $0.1$，仍需要约

 $n>\frac{2\log(2/\delta_{10})}{(0.3-0.1)^2}\approx544$

个 pooled exploration 样本才能满足 LCB 门条件。当前 pooling 只能把约 $154$ 提到约 $246$，仍达不到要求。对最稀疏 cell，partial pooling 即使假设 $\Gamma=0.2$，半径加偏差仍约为 $0.48$，不能在当前流上稳定开门。

## 4. 审查结论

1. `global pooled gate` 是最小、干净、可证明的 baseline。它值得实现用于消融，但它改变了目标：从 context-specific JEV 变成整体平均 JEV。
2. `partial pooling` 只有在 $\Gamma$ 来自独立 pilot、外部历史或明确的生成假设时才成立。把 global prior 直接写进 cell 状态会破坏 coverage。
3. 当前 BBB 数据量不足以让 pooled gate 在主要 cost $0.1$ 下恢复利用动作；不能靠手工缩小半径解决。
4. 实现顺序应是先做 global pooled ablation，再决定是否值得为独立 pilot 和异质性界付出成本。没有可审查的 $\Gamma$ 之前，不建议实现 hierarchical 主线。

## 5. 覆盖范围

上述结论针对预先固定的 calibration stream、固定 provider snapshot、固定探索概率和固定 provider 菜单。若请求分布受历史 gate 自适应，或 provider 在校准期间在线更新，固定有限总体证明不再直接适用，需要另行建立条件平稳性或漂移假设。
