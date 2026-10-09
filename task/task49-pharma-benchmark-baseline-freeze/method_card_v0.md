# Task49：实时更新的数学审查与下一轮方法选择

日期：2026-09-26。状态：计算构造可定义，主创新未成立；不据此启动效果实验。

## 我们到底要解决什么

对一个分子性质请求，判别模型选择一个具体预测工具。收到这次调用的真实对错后，更新决策模型，让后续请求选得更好。计算约束是每次更新足够快、历史维护成本可控；准确性目标是减少未来选择错误。数学推导可以证明更新算的是什么，不能代替真实数据证明它选得更准。

PILOT 和 BayesianRouter 已经处理“离线表征或先验 + 所选工具反馈 + 在线更新”。因此当前缺口不是有无在线更新，而是：**固定表征是否限制了适应；如果必须改变表征，怎样控制历史重算和更新成本？**前半句尚待医药数据验证。只有确实存在这个瓶颈，才有理由提出新的表征更新机制。

本卡审查“冻结历史写入向量，只学习当前读取变换”的候选，并给出可直接复用的 ridge 对照。它不强迫 provider 之间独立，也不要求它们共享参数；二者都是参数化选择，需要实证比较。

## 输入只有请求、菜单、已释放反馈

请求 $x$ 合并分子 SMILES 与性质字段。菜单 $\mathcal A$ 列出具体 provider 和公开工具卡，$K=|\mathcal A|$ 是其长度。调用完成后，反馈包含所选 provider $a$ 和正确性 $r\in\{0,1\}$。反馈集合由日志派生，不另立一个可任意加字段的原始状态。

贯穿实例：$x$ 是池内分子 z000282 及性质 BBB；菜单有 DeepPurpose-CNN 和 GraphB3。CNN 原始概率为 0.9352496862411499，GraphB3 为 0.02379637211561203；本地独立参考标签为 1。选择 CNN 后释放 $r=1$，选择 GraphB3 后释放 $r=0$。这两个原值只在相应 provider 被选择后对学习器可见。

以下二维向量是用于核验公式的人工算例，**不是声称已经运行 Laya 得到的 embedding**。真实编码维度以 checkpoint 配置为准。

## 候选：写入固定，读取可学

固定编码器 $f$ 和固定投影 $R$，得到候选特征；固定函数 $\phi$ 把写入 key 映射到非负空间：

$$
z_a=R f(x,\mathcal A)_a\in\mathbb R^d,\qquad k_a=\phi(z_a)\in\mathbb R_{\ge0}^d.
$$

实例：$f$ 可取已下载源码对应的 Laya candidate-marker 路径，$R$ 把其隐藏维度投影到预先选定的 $d$；冻结表示、tokenizer、菜单顺序和投影。二维算例令 CNN 的 $z=(1,0)^\top$、$\phi$ 为恒等映射，故 $k=(1,0)^\top$。真实模型的有符号特征需要明确的非负映射，不能直接套该算例。Laya marker 依赖整个菜单，故必须保留 $\mathcal A$，不能写成只依赖单个 provider。

每个 provider 保存两个长度为 $d$ 的向量 $S_a,F_a$，分别累积正确与错误的 key。初始值均为 $\frac12\mathbf1$。反馈到达时只做：

$$
S_a\leftarrow S_a+r k_a,\qquad F_a\leftarrow F_a+(1-r)k_a.
$$

实例：CNN 首条反馈正确、$k=(1,0)^\top$，则 $S_{\mathrm{CNN}}$ 从 $(0.5,0.5)^\top$ 变成 $(1.5,0.5)^\top$，$F_{\mathrm{CNN}}$ 不变。GraphB3 未被选择，其状态不变。这里的两个初值只是避免零分母的平滑项，不宣称构成严格 Beta 后验。

当前请求使用可学矩阵 $W\in\mathbb R^{d\times d}$ 生成正值查询 $q_a$，计算预测分数：

$$
q_a=\operatorname{softplus}(Wz_a),\qquad
p_a=\frac{q_a^\top S_a}{q_a^\top(S_a+F_a)}.
$$

实例：仍取 CNN 更新后的状态，若 $W=0$，两个 query 坐标都为 $\log 2$，则 $p_{\mathrm{CNN}}=(1.5+0.5)/(1.5+0.5+0.5+0.5)=2/3$。所有分数分别在 $(0,1)$；多个 provider 可同时获得高分，不做“只有一个正确工具”的 softmax 标签。

冻结状态后，可以用一条所选反馈的二元交叉熵更新 $W$：

$$
L=-r\log p_a-(1-r)\log(1-p_a),\qquad
W\leftarrow W-\eta\nabla_W L,\quad \eta>0.
$$

实例：上面的 $p=2/3$ 与 $r=1$ 给出 $L=-\log(2/3)$。$\eta$ 是一次梯度步长。**计算这个损失时，$S,F$ 必须来自当前反馈写入之前**，否则它已经包含待预测标签。延迟实现可在释放时，用当前 $W$ 和不含本条标签的状态重新计算损失，再写入本条 key；这优化的是释放时的目标，不应声称是选择时旧目标的精确梯度。

### 可以精确算，但表示能力有硬边界

令 $T_a=S_a+F_a$。定义第 $j$ 个坐标的成功率 $c_{a,j}$ 和 query 权重 $\omega_{a,j}$，上面的模型就能精确改写：

$$
c_{a,j}=\frac{S_{a,j}}{T_{a,j}},\qquad
\omega_{a,j}=\frac{q_{a,j}T_{a,j}}{\sum_l q_{a,l}T_{a,l}},\qquad
p_a=\sum_j\omega_{a,j}c_{a,j},\quad \sum_j\omega_{a,j}=1.
$$

实例：CNN 的 $T=(2,1)^\top$，$c=(0.75,0.5)^\top$。等值 query 给出 $\omega=(2/3,1/3)^\top$，所以 $p=2/3$。无论怎样更新 $W$，这份固定状态下的 $p$ 都只能在 $0.5$ 和 $0.75$ 之间。学习读取只能重分配现有坐标的权重，不能恢复已经被求和丢掉的样本信息。

其 query 梯度为：

$$
\frac{\partial p_a}{\partial q_{a,j}}
=\frac{T_{a,j}(c_{a,j}-p_a)}{q_a^\top T_a}.
$$

实例：若所有历史 key 都是 $(1,1)^\top$，且成功与失败各一次，则 $S=F=(1.5,1.5)^\top$，$c=(0.5,0.5)^\top$。任意 $W$ 都产生 $p=0.5$，上式两个坐标的梯度都为 0。模型无法学会区分两个本来可分的分子。近似相等的坐标会相应减弱读取梯度；这不是“在线训练次数还不够”的问题。

若把 softplus 去掉、直接使用线性 read，分子与分母又可写为：

$$
p_a=\frac{z_a^\top W^\top S_a}{z_a^\top W^\top T_a}.
$$

实例：二维时只需记两个有效系数 $W^\top S_a$ 与 $W^\top T_a$，并没有因为引入 $W$ 获得更强的分式线性函数类；有符号 query 还可能破坏分母正性。非线性 read 避免这一直接重参数化，却仍受上一式的凸组合限制。

**审查结论：不把这条构造定为论文主方法。**固定大小状态和可学读取都能实现，但尚无依据说它比普通 head 更有用或具有新颖性。保留推导和反例，用于筛除不值得花 A800 机时的方案。

### 完整成本

| 操作 | 成本，不含编码器 | 必须计入的额外项 |
|---|---|---|
| 单条 key 写入 | $O(d)$ | 反馈队列与去重索引 |
| 单候选 read 和一次 selected gradient | $O(d^2)$ | 当前 $W$ 下的矩阵乘法 |
| 完整菜单评分 | $O(Kd^2)$ | Laya 每个候选的 $z_a$ 不同；不能错写成 $O(d^2+Kd)$ |
| 状态 | $O(Kd+d^2)$ | 待释放样本若有 $M$ 条，还需 $O(Md)$ 的特征存储 |

表中实例为 $K=33,d=32$：状态保存 66 个 32 维向量和一个 $32\times32$ 矩阵；一次选择要处理 33 个候选。真实端到端时间还包括 tokenizer、Laya forward、数据转移和维护历史，不以小状态耗时冒充总时延。

这里没有 IPW，所以有限核的加权平均依赖实际采样到的上下文分布。即使加入 propensity 权重，也不会自动证明条件正确率一致性，更不会突破表示碰撞。模型误设、缺乏探索、反馈缺失和选择性延迟仍需单独分析。

## 可直接实现的参照：带先验的 ridge 头

用于判断固定表征是否足够，先明确一个不会混淆 objective 的参照。令 $\theta_0$ 是只用允许的初始化数据得到的系数，$\lambda>0$ 是正则强度。对已经释放的样本集合 $\mathcal O$：

$$
\theta^*=\arg\min_\theta\left[
\frac12\sum_{i\in\mathcal O}(r_i-z_i^\top\theta)^2+
\frac{\lambda}{2}\|\theta-\theta_0\|^2
\right],\quad
A=\lambda I+\sum_{i\in\mathcal O}z_i z_i^\top,\quad
b=\lambda\theta_0+\sum_{i\in\mathcal O}r_i z_i,\quad \theta^*=A^{-1}b.
$$

实例：$d=2,\lambda=1,\theta_0=(0,0)^\top$，只收到 $z=(1,0)^\top,r=1$，则 $A=\operatorname{diag}(2,1)$、$b=(1,0)^\top$，解为 $(0.5,0)^\top$。该解来自对严格凸目标求导，并非对 sigmoid 交叉熵的闭式解。

记 $P=A^{-1}$，下一条反馈到达时可用 Sherman--Morrison 恒等式更新：

$$
h=\frac{Pz}{1+z^\top Pz},\qquad
\theta'=\theta+h(r-z^\top\theta),\qquad
P'=P-\frac{Pzz^\top P}{1+z^\top Pz}.
$$

实例：上一状态 $P=\operatorname{diag}(1/2,1)$，再收到 $z=(0,1)^\top,r=0$，得到 $h=(0,1/2)^\top$、$\theta'=(0.5,0)^\top$、$P'=\operatorname{diag}(1/2,1/2)$，与重新解两条样本的 ridge 完全相同。正则 $\lambda$ 已在初始 $P=\lambda^{-1}I$ 中，分母为 **1** 加二次项，不能再把正则系数随意放进分母。

此处为固定特征，乱序反馈最终只改变加法顺序；中途决策仍受延迟影响。per-provider 独立头需 $O(Kd^2)$ 状态、每条更新 $O(d^2)$；共享头需 $O(d^2)$ 状态，但引入跨 provider 共享假设。使用 Cholesky 重算作为数值核对，长期浮点递推不声称逐字节一致。

该参照属于已有方法。若想在 JEV logit 上加 residual，训练目标必须真的包含这个 offset，不能一边用二元标签拟合无 offset 的 ridge，一边在决策时额外加未经校准的 JEV logit。本卡不采用这种不一致的构造。

## 下一项有意义的实践

先完成 **表示与学习瓶颈诊断**，不先发明算法名称。沿用当前真实 BBB development 池和未选输出隔离：

1. 固定相同训练区、到达流和反馈规则，比较化学指纹、冻结 Laya marker 的线性头及小型非线性 head。上限诊断可在训练区使用全反馈，但所有对照获得相同信息，和主在线表分开。
2. 将最有信息的冻结表示接到 PILOT、BayesianRouter、Online SGD、Replay SGD；保留 train-only Best Fixed 和同反馈标签的直接性质预测器。各策略得到同样的反馈规则和预算，**因为选择不同，它们不应被迫取得完全相同的标签记录**。
3. 只有冻结表示确实存在稳定瓶颈，才比较解冻 Laya 最后层/低维参数子空间与固定 head，并记录更新后的历史特征处理成本。若普通 replay 或子空间 EKF 已足够，新增算法需转向另一个未解决的具体限制。
4. 冷启动、较低错误率和更低计算开销分别可被否定，不能看到任意一个正数就保留全部故事。最终 confirmation 在机制冻结后才打开。

数学支持的用途是筛除错误构造并确保实验可解释；不能要求在实验前证明未知数据上的准确率一定提升，也不能因效果未知就无限延期小规模诊断。

## 最近邻和可复用内容

- [PILOT](https://aclanthology.org/2025.findings-emnlp.1301/)：复用先验与 selected-arm ridge 的目标和对照；保留其 cosine mean 与 UCB，不能擅改成普通 dot-product 后仍称完整 PILOT。
- [BayesianRouter](https://proceedings.iclr.cc/paper_files/paper/2026/hash/8dc9b0192713f44f035b8c0bd3f4153a-Abstract-Conference.html)：复用先验注入与后验更新；源码差异见 baseline 合同。
- *Online Limited Memory Neural-Linear Bandits with Likelihood Matching*，ICML 2021：已经研究表征变化时历史统计与有限记忆；本地正文线索在 method_audit/neural_linear_lim2.html，在进一步创新判断前补齐正式 PMLR 版本。
- [Efficient Online Bayesian Inference for Neural Bandits](https://proceedings.mlr.press/v151/duran-martin22a.html)，AISTATS 2022：已经在低维参数子空间做在线神经网络更新；不能把“低维在线微调”本身当新颖性。
- [Linear attention](https://proceedings.mlr.press/v119/katharopoulos20a.html)：借用有限核历史求和与递推等价性的证明方式，不借用“linear”名称暗示新算法。

本卡所有算例是代数演示，没有模型权重运行、训练或性能结果。先前工作区草稿中的交互维度、logit offset 与 ridge 更新不一致已在本次审查中撤回；未据该草稿运行实验。
