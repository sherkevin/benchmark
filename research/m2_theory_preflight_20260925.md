# M1/M2 实验前理论审查

**日期：** 2026-09-25  
**结论：** 当前 M1/M2 runner 可以做合同验证；进入昂贵 Nebula 效果实验前，需要把目标、置信界、成本和评估时序固定下来。

## 1. 当前真正要估计的量

在首个 provider 已经给出结果的历史 $h$ 下，对候选 probe provider $p$ 定义固定终止规则下的潜在损失：

$$
L_i(0) = \text{现在直接停止的损失},
\qquad
L_i(p) = \text{调用 }p\text{ 后按固定规则终止的损失}
$$

追加调用的验证收益为：

$$
\Delta_i(p)=L_i(0)-L_i(p)
$$

如果一次调用的成本用和损失相同的单位表示，真正的决策量应是净收益：

$$
U_i(p)=\Delta_i(p)-c_p
$$

所以 gate 的目标不是“provider $p$ 的正确率高不高”，而是：

$$
\tau_p(h)=\mathbb E[\Delta_i(p)\mid h_i=h]
$$

以及是否有：

$$
\tau_p(h)-c_p>0
$$

当前 M1/M2 的 `after_loss` 是第二个 provider 的输出直接作为最终答案的损失；因此它是一个明确的 intervention contract，但还不是完整 verifier。

## 2. 随机 probe 为什么能识别 gain

令 $A_i(p)=1$ 表示在历史 $h_i$ 下实际 probe 了 $p$，并记录精确 propensity：

$$
\mu_i(p\mid h_i)=\Pr(A_i(p)=1\mid h_i),
\qquad
\mu_i(p\mid h_i)\geq\eta_{\min}>0
$$

只在 $A_i(p)=1$ 时观察到 $Δ_i(p)$。对固定的历史和潜在结果，IPW 项满足：

$$
\mathbb E\left[
\frac{A_i(p)\Delta_i(p)}{\mu_i(p\mid h_i)}
\middle|h_i,\Delta_i(p)
\right]
=\Delta_i(p)
$$

进一步取条件期望得到：

$$
\mathbb E\left[
\frac{A_i(p)\Delta_i(p)}{\mu_i(p\mid h_i)}
\middle|h_i
\right]
=\tau_p(h_i)
$$

这要求三件事同时成立：propensity 真的被记录、存在正概率覆盖、probe 的随机化不读取当前未观测的真值。M2 的 gate 可以依赖过去已经释放的状态，但不能依赖当前 episode 的真值；因此它不会破坏这个识别条件。

有限样本的比值估计为：

$$
\widehat\tau_p(h)=
\frac{\sum_i \mathbf 1[A_i(p)=1]\,\phi(h_i,p)\,\Delta_i(p)/\mu_i(p\mid h_i)}
     {\sum_i \mathbf 1[A_i(p)=1]\,\phi(h_i,p)/\mu_i(p\mid h_i)+\varepsilon}
$$

这里的分子和分母可以分别无偏估计；比值本身有限样本下不严格无偏，只能在 overlap、稳定分布和足够样本下讨论一致性或误差界。

## 3. 延迟反馈不会自动破坏识别，但会带来两个条件

令反馈在事件时间 $t_i$ 产生，在到达时间 $a_i$ 进入队列。若 $ρ=1$，状态只是已到达事件的加和：

$$
G_n=\sum_{i:a_i\le n}\frac{\phi_i\Delta_i}{\mu_i},
\qquad
Z_n=\sum_{i:a_i\le n}\frac{\phi_i}{\mu_i}
$$

事件的到达顺序不会改变最终加和；它只会让 gate 暂时看不到尚未到达的反馈。

若要使用 $ρ<1$，必须明确衰减的时间轴：

- 按事件时间衰减，目标是环境中的 drift；
- 按到达时间衰减，目标是“最近收到的证据”。

如果反馈延迟和结果好坏有关，还需要建模延迟或加入逆删失权重。当前 Task36 的固定离散 delay 只能支持 $ρ=1$ 的合同验证，不能支持复杂 drift 结论。

## 4. M2 gate 的可证明写法

对每个候选 provider 构造估计和置信半径：

$$
\mathrm{LCB}_p(h)=\widehat\tau_p(h)-r_p(h,\delta)
$$

只在下面两个条件都满足时调用：

$$
\mathrm{LCB}_p(h)-c_p>0
$$

$$
\mathrm{UCB}\bigl(\Pr(\mathrm{unsafe}\mid h,p)\bigr)\le r_{\max}
$$

第一版尚未实现第二个 risk gate，因此当前 M2 只能叫 gain gate。

如果暂时只研究 bounded gain，令 $\Delta\in[-1,1]$，IPW 权重满足 $1/\mu\le 1/\eta_{\min}$，可以用有效样本量构造保守半径：

$$
n_{\mathrm{eff},p}
=
\frac{(\sum_i \alpha_i)^2}{\sum_i \alpha_i^2},
\qquad
\alpha_i=\frac{\mathbf 1[A_i(p)=1]K(h,h_i)}{\mu_i(p\mid h_i)}
$$

$$
r_p(h,\delta)
=
\sqrt{\frac{\log(2/\delta)}{2n_{\mathrm{eff},p}}}
$$

这条式子只能作为在固定核、独立有界反馈下的 pilot 级界。正式论文还需要处理自适应策略、核权重和延迟的更严格条件；不能直接把它写成任意 online setting 下的高概率保证。

## 5. 当前实现必须修正或明确的地方

### 5.1 `call_cost` 要进入同一个目标

当前 M2 用 `call_cost` 控制 gate，但 smoke 使用 `call_cost=0`，gain 状态仍然只记录错误损失差。正式实验必须预先固定 $c_p$，并报告：

$$
\text{net utility}
=
\text{terminal utility}
-\sum_i c_{p_i}
$$

不能只报告 accuracy。

### 5.2 one-hot 特征目前只是合同特征

当前 `bias + probe-provider one-hot` 会让不同 provider 通过 bias 共享一部分状态。它适合验证 IPW 更新和队列合同，不足以回答“当前 CNN 返回了什么之后，Morgan 是否值得调用”。进入方法实验前，要换成冻结的历史特征 $\phi(h,p)$，并锁定 encoder/version。

### 5.3 评估必须和更新时序分开

当前 30-case 对照在同一串 episode 上边更新边评估，只能作为 pipeline smoke。正式实验应使用 prequential 口径：

1. episode $i$ 只能读取 $i$ 之前已经 release 的状态；
2. episode $i$ 的最终错误、调用成本和风险单独记录；
3. episode $i$ 产生的反馈只能在规定到达时间后影响 episode $i+1$ 或更晚；
4. 与 frozen/static baseline 使用同一 stream、同一 seed、同一预算。

### 5.4 STOP 不是负 gain 标签

没有 probe 时不知道 $\Delta_i(p)$，不能给未调用 provider 写 `gain=0` 或负样本。STOP 只进入策略会计和 propensity 日志；gain learner 只更新实际 probe 的 provider。这一点当前 M2 已遵守。

## 6. 进入 Nebula 前的放行条件

只允许在下面条件满足后提交一条任务：

1. 冻结实验卡：provider 集合、首 provider、probe 概率、调用成本、loss、risk 指标、delay、seed 和 evaluation split；
2. 先用本地 frozen replay 验证 propensity、延迟 release、STOP 计数、无 truth leakage；
3. 远程 entry 只跑一个最小切片并写出 config/raw/events/summary，不提交大批量训练；
4. 结果目录只写 `jingwu/` 前缀，任务不挂 OSS 时不使用 `--mount_oss`；
5. 代码以已推送 commit 提交，Nebula dry-run 输出审查通过后再真正提交。

当前本地状态：合同回归 `109 passed`；Nebula 直连 queue 查询成功，A800 可用 21 卡、backlog 0。`neb doctor` 仍因缺少 OSS 凭据返回 1；虽然本次代码切片不需要 OSS，按现有 wrapper 硬约束还不能提交任务。

## 7. 当前建议

可以尝试的第一个远程方案是：**固定首 provider + 固定 frozen history encoder + 随机 probe/IPW gain learner + 有置信界的 gain gate**。风险 gate、backbone fine-tune 和大规模训练全部后置。

这条方案已经有可验证的识别式和递推式，但在修正 cost、confidence、encoder 和 prequential evaluation 之前，不应把当前 M2 smoke 直接推成昂贵实验。
