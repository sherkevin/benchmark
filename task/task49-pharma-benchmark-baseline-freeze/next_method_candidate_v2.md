# Audit-Gated Calibrated JEV（AG-JEV）候选方法卡与审查

日期：2026-09-27

状态：已在样本支持审查中停止；没有提交 Nebula，也没有把它写成论文方法。

## 1. 要解决的具体缺口

一题给出分子性质请求 $x_t$ 和具体 provider 菜单 $A_t$。策略选一个 $a_t$，工具终止且参考判分完成后才收到被选 provider 的 $r_t\in\{0,1\}$。反馈可能延迟；未选 provider 的结果和本题真值在决策时不可见。

普通 Online-SGD、Replay-SGD 和独立 BayesianRouter 都只改被选 provider 的状态。它们没有解决一个更基本的问题：被选样本不是随机样本，直接把它当作 provider 正确率会把“策略偏好”混进“工具是否正确”。AG-JEV 只新增一个信息来源：预先固定概率的随机审计。审计标签只用于估计，利用分支的标签不进入该估计。

## 2. 冷启动分数与统计单位

先在 outer-fold 的 fit 区，将冻结 BT 分数 $\ell_a^{\mathrm{BT}}(x)$ 做 per-provider affine logistic calibration：

$$
q_a^0(x)=\sigma\!\left(\alpha_a\ell_a^{\mathrm{BT}}(x)+\beta_a\right),\qquad \alpha_a\ge 0.
$$

$q_a^0(x)$ 的单位是“该具体 provider 的端到端 `system_success` 概率”。$\alpha_a,\beta_a$ 只由 fit 区决定；没有校准参数就不能运行本候选。这里的标签把工具执行失败记为 $0$，unknown/unscorable 不更新。

固定 fit-only context $c(x)$ 为四个 cell：$c(x)=(\mathbf 1[\mathrm{TPSA}(x)>60],\mathbf 1[\mathrm{aromatic\_rings}(x)>0])$。第一版固定完整 33-provider 菜单；菜单变化时必须把 menu hash 加入 context key，不能跨菜单合并统计。

贯穿算例（只核对单位，不是实验结果）：某题的 context 是 $(1,0)$，冻结概率为 $q_A^0(x)=0.60$。A 被随机审计后收到 $r=1$，释放时记录残差 $d=1-0.60=0.40$；如果反馈尚未释放，这条记录不存在。

## 3. 只从随机审计建立证据

每题先掷一个预注册的审计 coin，概率为 $\varepsilon$。命中时在当前菜单中均匀抽一个 provider；否则进入利用分支。无论哪条分支，都只调用抽到的一个 provider，完整记录实际 propensity。

对一个已经释放的审计事件 $i$，只保存

$$
d_i=r_i-q_{a_i}^0(x_i)\in[-1,1],\qquad
n_{c,a}=\sum_i\mathbf 1[c(x_i)=c,a_i=a],\qquad
S_{c,a}=\sum_i\mathbf 1[c(x_i)=c,a_i=a]d_i .
$$

因此 $\widehat\delta_{c,a}=S_{c,a}/n_{c,a}$ 估计的是固定 context-provider 条件下的 correctness residual。利用分支的 $r_i$ 永远不写入 $S$；这不是遗漏，而是为了保留已知抽样概率下的统计含义。延迟反馈只在到达 tick 写入，event id exactly-once；无效真值不计入 $n$。

每个 gate look $\ell$（第一版每 64 个题一次）使用 alpha-spending 半径

$$
\rho_{c,a,\ell}=\min\!\left(2,\sqrt{\frac{2\log(2CK\ell^2/\delta)}{n_{c,a}}}\right),
$$

其中 $C=4$、$K=33$、$\delta$ 和最小支持数 $n_{\min}$ 在看 evaluation 前固定；只有 $n_{c,a}\ge n_{\min}$ 时才计算该式，否则直接令 gate 关闭。半径取 $[-1,1]$ 残差的正确范围，不能沿用只适用于 $[0,1]$ 的半径。

## 4. 置信收缩和选择

只保留置信度足够的 signed correction：

$$
\Delta_{c,a,\ell}=\begin{cases}
\operatorname{sign}(\widehat\delta_{c,a})
\bigl(|\widehat\delta_{c,a}|-\rho_{c,a,\ell}-\tau\bigr), & |\widehat\delta_{c,a}|>\rho_{c,a,\ell}+\tau,\\
0, & \text{otherwise},
\end{cases}
$$

其中 $\tau\ge0$ 是最小实际修正，提前由 validation 固定。当前题 $x_t$ 的动态概率分数是

$$
s_{t,a}=\operatorname{clip}\!\left(q_a^0(x_t)+\Delta_{c(x_t),a,\ell},0,1\right),
\qquad a_t=\arg\max_{a\in A_t}s_{t,a}.
$$

若全部 $\Delta$ 为零，AG-JEV 与 Frozen JEV 完全相同（利用分支仍按审计协议抽样）。gate 在一个 look 内冻结，释放的反馈只影响下一个 look；这避免同一 alpha-spending 被一个 batch 内重复检查。这个式子是在同一概率单位上相加，不把 BT 排序 logit 当作 correctness 概率。

算例：某 cell 中 A 的四个审计残差为 $0.4,-0.6,0.4,0.4$，所以 $\widehat\delta_A=0.15$。若预先计算的半径为 $0.10$、$\tau=0.02$，则 $\Delta_A=0.03$，该题 A 的分数从 $0.60$ 变成 $0.63$。这个数值只是公式实例；真实短流中半径很可能大于该例，gate 应保持关闭。

## 5. 与已有对照的边界

- **Best Fixed**：只在 fit 区选一个 provider；没有 context、审计或在线状态。
- **Frozen**：使用同一个 $q^0$ 做逐题 argmax；AG-JEV 的 $\Delta=0$ 消融。
- **Online-SGD / Replay-SGD**：使用释放的 selected 标签更新 provider 参数；AG-JEV 不反向传播，不把利用分支标签混进审计估计，只维护 $O(CK)$ 个计数和和。
- **PILOT / BayesianRouter**：可用所有合法 selected feedback 做各自的后验/回归更新；AG-JEV 的差异是设计型随机审计与置信收缩，而不是 UCB、Thompson 或最近邻的新包装。
- **Audit-Frozen**：使用完全相同的 $\varepsilon$、随机种子和调用预算，但永远用 Frozen 分数；它隔离“审计造成的探索损失”和“证据门本身的收益”。

一次释放的状态更新为 $O(1)$，一次菜单评分为 $O(K)$，状态为 $O(CK)$；反馈队列和随机数状态必须计入端到端时间。它没有历史特征重算成本，但审计错误和样本稀疏会直接导致 gate 不开。

## 6. 只用已有证据的 CPU 证伪

下一步不做 GPU。使用已恢复的 722 条 BBB development evaluation 流、每个 outer fold 的 fit-only calibrator 和冻结 hidden/BT snapshot；回放时完整面板只由 evaluator 在动作之后取出被选一格，策略端不读取其余标签。固定 5 folds、2 seeds、delay $\in\{0,1,4\}$、审计 seed、$\varepsilon$、$n_{\min}$、$\tau$、look 大小后再运行。

逐流记录累计错误、paired accuracy（AG-JEV、Audit-Frozen、Frozen、Best Fixed、Online-SGD、Replay-SGD）、gate-open 比例、首次开启位置、审计比例、释放更新次数、选择/更新 p50/p95 和峰值状态。独立 NumPy verifier 只读 decision/release JSONL，重算残差、半径、gate 和选择；不能导入 AG-JEV 实现本身。

预先登记的停止线：校准参数、context/menu key 或 selected-only 权限不满足就停止；任意两个 delay 中 AG-JEV 低于 Audit-Frozen 就停止；三个 delay 都没有一次合法 gate open 就报告“当前数据没有可识别的动态信号”，不调小半径或扩大 context 追分。即使超过 Frozen，只要没有超过 train-only Best Fixed，就只能称安全适配对照，不能称论文主创新。

## 审查结论

这是一条统计语义最干净、能直接在旧证据上证伪的候选：随机审计解决 selected-only 的可识别性，校准概率让残差有单位，置信收缩保留 Frozen fallback，延迟只改变状态何时生效。它的主要风险也很明确：$33$ 个 provider 乘 $4$ 个 cell 使每格支持很少，审计成本可能让 gate 永远关闭；当前 stream-drift audit 也没有证明需要它。

方法新颖性目前不足以单独支撑 AAMAS 主张。随机探索、置信序列和 context gate 都有成熟近邻；论文若成立，贡献只能是：在医药 direct tool correctness 的 selected-only、延迟反馈合同下，把可审计随机证据接到校准 JEV 的安全切换，并证明它在相同调用预算下比强 online baseline 多解决了什么。CPU 回放没有清楚正信号时，结论应是“当前 benchmark 不支持动态 JEV 主创新”，不再申请 A800。

它只复用仓内 exploration-only JEV 的抽样和 batch gate 机制；目标已经改成 direct `system_success` residual。旧版二次调用 verification-gain 的结果不能充当本候选的效果证据。

## 先验可行性闸门（2026-09-27）

该候选在当前 722 条 development 流上没有足够的随机审计支持。即使取审计概率 $\varepsilon=1$、去掉四格 context 只剩 $C=1$，每个 provider 的期望审计数也只有 $722/33\approx21.9$。在预登记的 $\delta=0.05$、最后一个 look $\ell=12$ 下，半径约为 $1.05$；因为 residual 范围是 $[-1,1]$，任何正的 $\tau$ 都不可能打开 gate。若保留四格 context，期望每格-provider 只有约 $5.5$ 条，半径被截到 $2$。因此 CPU 回放前已可证伪“当前 benchmark 上能打开动态 gate”这一必要条件，停止本候选，不用调小半径、不扩大 context、不提交 A800。
