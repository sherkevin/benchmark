# Propensity-corrected residual JEV v1

日期：2026-09-27

## 要检验的缺口

当前 Frozen JEV 有一个固定的 correctness score，Online-SGD 和 BayesianRouter-style 更新只使用被选 provider 的反馈，容易把“策略经常选择它”误读成“它更正确”。本候选只增加一个信息来源：预先固定概率的随机探索，并用实际选择 propensity 校正 residual。

## 方法合同

每个 outer fold 的 fit 区用完整 provider 面板训练既有 BT backbone，并对每个 provider 的 BT score 做 affine logistic calibration，得到冻结概率 $q_a^0(x)$。校准只看 fit 区；validation 只选 checkpoint/校准停止点，evaluation 不回流。

evaluation 每题先以 $epsilon=0.20$ 的概率均匀随机抽 provider，否则选择当前分数最高的 provider。实际选择的 propensity 为

$$
\pi_t(a)=\epsilon/K+(1-\epsilon)\mathbf{1}[a=\arg\max_b s_t(b)].
$$

工具终止且 reward 释放后，只对被选 provider 做一次更新。保存决策时的 $q^0_{a_t}(x_t)$，并累计

$$
W_a\leftarrow W_a+1/\pi_t(a_t),\qquad
S_a\leftarrow S_a+(r_t-q^0_{a_t}(x_t))/\pi_t(a_t).
$$

校正为

$$
\widehat\delta_a=\frac{S_a}{W_a+\lambda},\qquad
\widetilde q_a(x)=\operatorname{clip}(q_a^0(x)+\widehat\delta_a,0,1),
$$

其中 $\lambda=20$ 固定，初始 $S_a=W_a=0$。这是一个锚定在冻结概率上的、带 ridge 收缩的 residual estimator；不反向传播，不读取未选 provider 标签。延迟 $d\in\{0,1,4\}$ 时，反馈只在 `release_tick=decision_tick+1+d` 写入，event id exactly-once。

## 对照

- Frozen：不探索，始终按 $q^0$ 选最高 provider。
- Audit-Frozen：使用完全相同的 $\epsilon=0.20$、随机种子和 propensity，但不做 residual 更新，隔离探索成本。
- PCR-JEV：本候选。
- 已有 Best Fixed、Online-SGD、Replay-SGD、BayesianRouter-style 结果作为同一 development 任务的背景对照。

## 预先固定的分析

5 outer folds × 2 seeds × 3 delays；每个 stream 的 evaluation 顺序、provider 菜单、随机数种子和 BT checkpoint 与 matched-SGD development 相同。报告 pooled accuracy、每流 accuracy、相对 Audit-Frozen 的 paired delta、探索比例、有效样本量、反馈更新数和选择/更新耗时。确认集保持关闭。

## 证伪条件

以下任一条件成立就停止该候选，不申请 A800、不调 $ε$ 或 $\lambda$：

1. PCR-JEV 在至少两个 delay 上不超过 Audit-Frozen，说明 residual 更新没有超过相同探索成本；或
2. PCR-JEV 三个 delay 的 pooled accuracy 都低于 Frozen，说明探索和校正不能形成实用的动态模块；或
3. 独立 NumPy 回放无法从事件重算 propensity、释放顺序、$S/W$ 和每次选择。

## 边界

Horvitz–Thompson、propensity scoring 和 ridge shrinkage 都是已有统计组件。本候选的可主张内容只能是：在医药 direct tool selection 的 delayed selected-only correctness 反馈中，把冻结 JEV 作为 control variate，用可审计的 propensity residual 做实时校正。若效果不超过同探索 Frozen，它不进入主方法。

