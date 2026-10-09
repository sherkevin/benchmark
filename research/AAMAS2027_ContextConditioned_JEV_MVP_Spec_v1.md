# Context-conditioned JEV MVP：实现规格

日期：2026-09-25

## 目标

当前 M2 只估计 provider 的全局平均 verification gain。v2 对照显示，这个状态没有稳定带来在线学习收益。下一版把状态改成少量、预先固定的 context cell，让同一个 provider 在不同结构条件下有不同的 gain 估计。

第一版只做 4 个 cell，不引入神经 backbone，也不使用真值训练上下文编码器。

## 可见输入与 context

对分子 $x$，在第二次调用决策前只读取首工具已经可见的 canonical SMILES。定义两个固定结构位：

$c(x) = (c_1(x), c_2(x))$

$c_1(x) = \mathbf{1}[\operatorname{TPSA}(x)>60]$

$c_2(x) = \mathbf{1}[\operatorname{AromaticRings}(x)>0]$

所以 $c(x)\in\{0,1\}^2$，一共 4 个 cell。阈值在实验开始前固定，不能根据 held-out 结果修改。

**Case：** 对 `z000364`，canonical SMILES 是 `COc1cc2c(c(OC)c1OC)-c1ccc(OC)c(=O)cc1[C@@H](NC(C)=O)CC2`。RDKit 计算得到 `TPSA=83.09`、`AromaticRings=2`，因此 $c_1=1,c_2=1$，context cell 是 `(1,1)`。这个计算不读取 BBB 真值，也不读取 Morgan/MPNN 的第二次返回。

## 每个 cell-provider 的状态

候选 provider 集合为 $A=\{\mathrm{Morgan},\mathrm{MPNN}\}$。对每个 cell $c$ 和 provider $a$，只维护：

$N_{c,a}$：已经评分的 eligible episode 数。即使本次没有选中 $a$，也要计入。

$H_{c,a}=\sum_{i:c_i=c} \frac{\mathbf{1}[A_i=a]g_i}{\pi_i(a)}$

$O_{c,a}=\sum_{i:c_i=c}\mathbf{1}[A_i=a]$

其中 $g_i=L_{\mathrm{stop},i}-L_{\mathrm{after},i}\in[-1,1]$ 是延迟到达的 verification gain，$\pi_i(a)$ 是本次决策对 provider $a$ 的完整 inclusion propensity。估计为：

$\hat\mu_{c,a}=\operatorname{clip}_{[-1,1]}(H_{c,a}/N_{c,a})$

当 $N_{c,a}=0$ 或 $O_{c,a}=0$ 时，估计未知，不能把 global prior 当作 certified observation。

**Case：** 若某个 `(1,1), Morgan` episode 的 gain 是 $g_i=1$，完整 propensity 是 $\pi_i(\mathrm{Morgan})=0.4$，则该 episode 给 $H_{(1,1),\mathrm{Morgan}}$ 加 `2.5`；如果它是 eligible 但没有选中 Morgan，则给该 cell-provider 的 H 加 `0`，同时 $N$ 仍加 `1`。

## 置信半径与门

声明全局探索概率 $\epsilon=0.8$、候选数 $K=2$，所以确定性 overlap floor 为 $\pi_{\min}=\epsilon/K=0.4$。对每个 cell-provider 使用相同的 deterministic Freedman 半径：

$B=\max(1,1/\pi_{\min}-1)$

$V=N_{c,a}\max(0,1/\pi_{\min}-1)$

第 $\ell$ 次 gate look 使用 alpha spending：

$\delta_\ell=\delta\frac{6}{\pi^2\ell^2CK}$

其中 $C=4$。令 $q_\ell=\log(2/\delta_\ell)$，则

$r_{c,a}=\frac{Bq_\ell/3+\sqrt{2Vq_\ell+(Bq_\ell/3)^2}}{N_{c,a}}$

并定义 $\mathrm{LCB}_{c,a}=\hat\mu_{c,a}-r_{c,a}$。只有满足 $O_{c,a}\ge4$ 且 $\max_a\mathrm{LCB}_{c,a}>\mathrm{call\_cost}$ 时，利用分支才打开；否则利用分支 STOP。

**Case：** 在 `(1,1)` cell 中，Morgan 的 LCB 是 `0.18`，MPNN 的 LCB 是 `0.07`，调用成本是 `0.10`。利用分支选择 Morgan，因为 `0.18 > 0.10`。如果 Morgan 的 LCB 是 `0.08`，则利用分支 STOP；探索分支仍按预先声明的概率调用候选。

## propensity

探索分支以 $\epsilon$ 概率均匀选择 provider。利用分支以 $1-\epsilon$ 概率选择最大 LCB 的 provider，或在门关闭时 STOP。若门打开且 winner 为 $a^*$：

$\pi_i(a)=\epsilon/K+(1-\epsilon)\mathbf{1}[a=a^*]$

如果门关闭：

$\pi_i(a)=\epsilon/K$

STOP 的 propensity 是门关闭时的 $1-\epsilon$。即使探索抽中了 winner，也必须记录完整混合概率，不能只记录探索概率。

## 冷启动边界

global provider prior 只能改变探索分支的候选排序，并且必须保留最小探索概率。它不能写进 $H/N$，不能单独打开 certified gate。新 cell 在没有足够 scored support 时，LCB 视为未知并保持关闭。

## 失败条件

- context 计算读取了真值或第二工具输出；
- 某个 cell-provider 的 eligible $N$ 没有计入 STOP 或未选中事件；
- propensity 没有包含完整混合概率；
- 新 cell 在 $O<4$ 时打开 certified gate；
- held-out cell 上相对 no-update gate 的 utility 没有改善，或 gate coverage 低于预注册阈值。

## 实现顺序

1. 先为 context 函数、cell 独立状态、STOP 计数、delayed exactly-once 和 propensity 写单测。
2. 在 synthetic potential-gain 数据上验证 HT/Freedman coverage。
3. 在当前 722 条回放上只做 pilot，结果仍标为 retrospective。
4. 拿到新的 provider snapshot 和独立 truth 后，再做 fresh scaffold confirmation。

