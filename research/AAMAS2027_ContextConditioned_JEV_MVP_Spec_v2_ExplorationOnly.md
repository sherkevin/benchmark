# Context-conditioned JEV MVP v2：exploration-only 更新

日期：2026-09-25

## 1. 目的

让实时 JEV 学到“当前 context 下，调用哪个候选 provider 值得”，同时避免把自适应 exploitation 结果当成随机证据。

## 2. 输入和 context

对第二次调用决策前可见的 canonical SMILES $x$，定义：

$c(x)=(\mathbf{1}[\operatorname{TPSA}(x)>60],\mathbf{1}[\operatorname{AromaticRings}(x)>0])$

因此 $c(x)\in\{0,1\}^2$，共 4 个 cell。阈值在实验前固定。

**Case：** `z000364` 的 SMILES 是 `COc1cc2c(c(OC)c1OC)-c1ccc(OC)c(=O)cc1[C@@H](NC(C)=O)CC2`，RDKit 得到 `TPSA=83.09`、芳香环数 `2`，所以 $c(x)=(1,1)$。这里只用 SMILES，不读 truth 或第二工具返回。

## 3. 只用探索样本的状态

探索 coin $E_t\sim\operatorname{Bernoulli}(\epsilon)$。当 $E_t=1$ 时，候选 provider 在 $K$ 个 provider 中均匀抽取；当 $E_t=0$ 时，执行 gate 或 STOP。

对 cell-provider 对 $(c,a)$，只维护探索样本：

$n_{c,a}=\sum_t\mathbf{1}[c_t=c,E_t=1,A_t=a]$

$S_{c,a}=\sum_t\mathbf{1}[c_t=c,E_t=1,A_t=a]g_t$

$\hat\mu_{c,a}=S_{c,a}/n_{c,a}$

其中 $g_t=L_{\mathrm{stop},t}-L_{\mathrm{after},t}\in[-1,1]$。当 $E_t=0$ 时，即使调用了 provider，也不更新 $n_{c,a}$ 或 $S_{c,a}$。

**Case：** 对 `(1,1), Morgan`，一次探索调用得到 `gain=1`，则 `n` 加 1、`S` 加 1；一次 exploit 调用得到 `gain=1`，终局记录保留，但该 cell 的 `n,S` 不变。

## 4. 置信半径和 gate

第 $\ell$ 次 gate look、cell 数 $C=4$、provider 数 $K=2$ 时：

$\delta_\ell=\delta\frac{6}{\pi^2\ell^2CK}$

$r_{c,a}=\min\left(2,2\sqrt{\frac{\log(2/\delta_\ell)}{2n_{c,a}}}\right)$

当 $n_{c,a}<m_{\min}$ 时，LCB 未定义，视为门关闭。当前预注册 $m_{\min}=4$。否则：

$\operatorname{LCB}_{c,a}=\hat\mu_{c,a}-r_{c,a}$

当 $\max_a\operatorname{LCB}_{c,a}>\operatorname{call\_cost}$ 时，利用分支调用最大 LCB 的 provider；否则 STOP。LCB 在实现中下截到 $-1$。

第 $\ell$ 个 look 只在一个 batch 起点计算一次，整个 batch 复用 gate 和 winner；当前实现每 64 个 episode 一个 batch。探索 coin 和 provider 抽样仍然逐 episode 进行。这样 alpha spending 覆盖的是每个确定性 batch look，而不是同一 look 内的重复检查。

## 5. propensity

门打开且 winner 为 $a^*$ 时：

$\pi_t(a)=\epsilon/K+(1-\epsilon)\mathbf{1}[a=a^*]$

门关闭时：

$\pi_t(a)=\epsilon/K$

STOP 的 propensity 是门关闭时的 $1-\epsilon$。探索抽中 winner 时仍要记录完整 mixture probability。propensity 继续写入每条选择日志，供审计和后续 IPW 对照；它不进入当前 exploration-only mean 的状态更新。

## 6. 冷启动和边界

新 cell 没有探索样本时不打开门。global prior 只能改变探索分支的排序，并不能增加 $n$ 或伪造置信度。正式置信目标是固定 calibration 流的 cell-provider 有限总体均值，不能自动覆盖 scaffold-heldout 流。适用范围依赖探索 coin、provider 抽样与潜在 gain 的条件独立；当前先在固定 BBB 回放上做 pilot，fresh confirmation 需要新的分子池、provider snapshot 和独立 truth。
