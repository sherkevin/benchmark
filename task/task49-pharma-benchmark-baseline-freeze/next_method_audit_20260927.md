# Task49：matched SGD 之后的下一方法审查

日期：2026-09-27  
范围：只读审查。没有提交 Nebula，没有改数据，没有把候选方法的预期当成结果。

## 先把问题钉住

当前每个 episode 的输入是分子性质请求 $x_t$、候选 provider 菜单 $A_t$ 和已经释放的历史反馈。策略选一个 provider $a_t$，工具终止后才收到所选 provider 的正确性 $r_t\in\{0,1\}$；反馈可能延迟。目标是在相同调用预算下减少之后的选择错误。

已有固定模型给出每个候选的冷启动分数 $p^0_a(x)$。因此在线模块真正能做的事情只有三类：

1. 把一次已选 provider 的标签转移到别的 provider；
2. 把一次标签转移到相似的未来分子；
3. 估计当前更新是否可信，避免一次选择性反馈把固定模型推坏。

如果一个方法仍然使用同一固定特征、只更新所选 provider 的独立 head，而且没有这三种信息来源之一，那么它和 Online-SGD/BayesianRouter 的差别只是优化器或先验，不能构成主创新。

这条判断与最近的真实开发结果一致：

- matched SGD full development：Frozen '0.8913'，Online-SGD '0.8991'，Replay-SGD '0.8964'，Best Fixed '0.9155'；
- BayesianRouter-style selected-only 更新在 delay '0/1/4' 上相对 Frozen 的均值分别为 '-0.00277/-0.00208/-0.00068'；
- 因此“加一个普通实时 head”已经被直接实验否定。下一步必须改变信息结构或改变更新的决策规则。

## 候选一：相关 provider 的低秩后验更新

### 要利用的真实信息

不同 provider 的错误并不一定独立。训练区的完整 provider 面板可以估计它们在同一类分子上的残差相关性。在线只观察 provider $a_t$ 的 $r_t$ 时，如果 provider $a_t$ 与 $b$ 的残差相关，反馈就应该改变 $b$ 的分数；现有 BayesianRouter 的独立后验不会做这件事。

这个信息不能凭空加入。provider 关系 $C$ 必须只用当前 outer fold 的 fit 区估计，evaluation 阶段禁止读取未选 provider 的标签。

### 一个可实现的最小形式

冻结一个基础 logit $\ell^0_a(x)$ 和低维分子特征 $h(x)\in\mathbb R^d$。从 fit 区估计 provider 关系矩阵 $C\in\mathbb R^{K\times K}$，再取保持半正定的低秩加对角近似 $C\approx UU^\top+D$，其中 $U\in\mathbb R^{K\times q}$、$D\succeq0$ 且 $q\ll K$。每个 provider 的在线残差参数为 $w_a\in\mathbb R^d$，预测为

$$
\ell_a(x)=\ell^0_a(x)+h(x)^\top w_a.
$$

把所有 $w_a$ 拼成 $w$，初始协方差使用

$$
\Sigma_0=(UU^\top+D)\otimes I_d.
$$

选中 provider $a_t$ 后，令 $\psi_t=e_{a_t}\otimes h(x_t)$，$p_t=\sigma(\ell_{a_t}(x_t))$。把观测写成残差参数的伪观测，并用 logistic 的 Laplace/extended-Kalman 近似做一次秩一更新：

$$
v_t=\frac{1}{p_t(1-p_t)+\epsilon},\qquad
z_t^{\mathrm{res}}=\psi_t^\top w_t+
\frac{r_t-p_t}{p_t(1-p_t)+\epsilon},
$$

$$
K_t=\frac{\Sigma_t\psi_t}{v_t+\psi_t^\top\Sigma_t\psi_t},\qquad
w_{t+1}=w_t+K_t(z_t^{\mathrm{res}}-\psi_t^\top w_t),
$$

$$
\Sigma_{t+1}=\Sigma_t-K_t\psi_t^\top\Sigma_t.
$$

这不是精确的 logistic 后验，而是一个明确的二阶近似；实现时要把近似写进方法合同。$\Sigma_{ab}\neq0$ 时，观察 $a_t$ 会改变 $b$ 的均值和不确定度。若 $C$ 退化成对角矩阵，它就退化成独立 provider 的 Bayesian 更新，正好是可检验的消融。

### 计算与风险

- 按上面的 $w_a$ 参数直接存完整协方差，状态是 $Kd\times Kd$，不适合实时部署。要达到 $O((qd)^2)$，必须把共享部分重新参数化为 $w_a\approx U_a u$，只对 $u\in\mathbb R^{qd}$ 做后验更新；若还保留 provider 独立残差，则另外计入 $O(Kd)$ 状态。这是实现选择，不能在不改参数化的情况下直接宣称低秩成本。
- 共享关系错了会把一个 provider 的错误传播给多个 provider；所以必须保留 Frozen fallback 或置信门，不能强制每条反馈都跨 provider 转移。
- 这是已有多任务 bandit、相关臂后验、neural-linear/低秩子空间更新的近邻。AAMAS 的新颖性风险为**中高**。论文只能主张“在医药工具的 correctness 反馈中利用 fit-only provider 关系进行实时转移”，不能主张发明了相关臂后验或 EKF。

### 最小零机时证伪实验

在每个 outer fold 内：

1. 只用 fit 区完整面板算 $e_{ia}=y_{ia}-p^0_a(x_i)$、provider 残差相关矩阵和 $U$；
2. 在 validation/evaluation 行上逐题随机选一个已知 provider，只把该 provider 标签交给模拟策略；
3. 比较独立后验、相关后验和 Frozen 对未来未选 provider 排名的变化，记录“选中 $a$ 的反馈是否让真实最优 provider $b$ 的分数朝正确方向移动”；
4. 再跑固定的 delay '0/1/4' 轨迹，与 Best Fixed、Frozen、Online-SGD、Replay-SGD、BayesianRouter 同表。

进入实现前的硬否定条件：fit 区相关矩阵的 held-out transfer AUC 不高于独立更新，或相关更新在三个 delay 中有两个以上低于 Frozen。满足任一条件就删除这条候选，不用 A800 调参。

## 候选二：有界的 context residual memory

### 要利用的真实信息

当前 head 失败的一个可能原因是把不同分子压成同一个全局参数。一次选中 provider 的反馈，通常只对结构相似的后续分子有参考价值。可以保存少量已释放的（分子表示，provider，残差），直接做局部校正，而不是用一条 SGD 梯度把所有分子一起推。

它保留 selected-only 边界：未选 provider 的标签不进记忆，真值不在决策前出现。因为记忆保留多条样本，不会落入“固定写入向量后再学读取”那种把正负样本先求和、随后无法恢复的碰撞问题。

### 一个可实现的最小形式

对已选 provider $a$，反馈释放时保存

$$
e_i=r_i-p^0_a(x_i),\qquad m_i=(h(x_i),e_i).
$$

令 $K$ 为预先固定的 cosine 或 RBF 核，从 provider $a$ 的有界记忆 $M_a$ 取最多 $B$ 个近邻：

$$
\Delta_a(x)=
\frac{\sum_{i\in M_a}K(h(x),h(x_i))e_i}
     {\epsilon+\sum_{i\in M_a}K(h(x),h(x_i))},
\qquad
\tilde p_a(x)=\sigma(\operatorname{logit}(p^0_a(x))+\gamma\Delta_a(x)).
$$

每个反馈只做一次写入，采用 FIFO 或固定容量 reservoir；决策时使用 top-$B$ 相似样本。$B$、核和 $\gamma$ 只能用 validation 选。若要做严格 $O(d)$ 更新，可以把固定随机特征 $z(h)$ 的加权残差和权重累积为 sketch，但此时必须报告近似误差，不能把 sketch 说成 exact memory。

### 计算与风险

- 它不需要反向传播，更新是常数次内存写入；查询成本是 ANN 检索和 $B$ 次核计算。
- 它只在相似分子重复出现、或 embedding 能表达局部错误模式时有用。对 scaffold-disjoint evaluation，局部记忆可能完全没有命中。
- kernel contextual bandit、nearest-neighbor bandit、episodic memory 都是已有方向。AAMAS 新颖性风险为**高**，更适合作为有信息优势的轻量对照或 Proposed 的消融，除非数据明确显示它解决了当前 head 的表示碰撞。

### 最小零机时证伪实验

对每个 fit fold 先建立 provider-specific 记忆。evaluation 中只按真实 selected-only 轨迹写入，分别测：

1. top-1/top-5 近邻的同 provider 残差符号是否比随机和全局残差更能预测下一题正确性；
2. 仅用 fit 记忆的静态 residual correction 是否超过 Frozen；
3. delay '0/1/4' 下有界 memory 是否超过同容量的 Replay-SGD。

如果近邻残差的 held-out AUC 不高于 Frozen logit，或 memory 在 scaffold-disjoint evaluation 上的 paired delta 不为正，则不实现 ANN，也不把检索记忆写进主方法。

## 候选三：随机探索加置信门的 direct JEV

### 要利用的真实信息

对于 correctness 任务，最干净的实时证据来自有已知抽样概率的探索。只要策略以预先固定的概率随机调用 provider，所收到的标签就能估计每个 context-provider 的真实正确率，而不是把“自己认为好的 provider”产生的标签再拿来证明自己好。

这条路线不依赖神经 head 的梯度。它把动态 JEV 定义成“冷启动预测 + 设计可审计的在线证据门”：证据不够时沿用 Frozen；证据足够时才切换到 context 内更可靠的 provider。

### 一个可实现的最小形式

用不读取真值的固定 context $c(x)$，例如当前已有的四个 TPSA/芳香环 cell。对每个 cell-provider 保存随机探索样本的数量和残差和：

$$
e_i=r_i-p^0_{a_i}(x_i),\qquad
\hat\delta_{c,a}=\frac{1}{n_{c,a}}
\sum_{i\in E_{c,a}}e_i.
$$

探索概率 $\epsilon$ 预先固定；探索时在菜单内均匀抽 provider。用预先登记的 bounded confidence radius $\rho_{c,a}$，只在

$$
n_{c,a}\ge n_0,\qquad
\operatorname{LCB}(\hat\delta_{c,a})=
\hat\delta_{c,a}-\rho_{c,a}>\tau
$$

时才允许用 context residual 修正 Frozen 分数；否则继续 Frozen。这里 $\tau\ge0$ 是实验前固定的最小正增益，不应把“没有证据变差”写成“已经证明有收益”。利用分支和探索分支都保留完整 propensity，延迟释放时按 event id exactly-once 更新。这个形式可以复用仓内 exploration-only JEV 的状态、alpha spending 和 batch-frozen gate，但目标变量必须从旧的 verification gain 改成当前 direct-selection 的 correctness residual；两者不能混称。

### 计算与风险

- 每次反馈只更新一个 cell-provider 的计数和均值，$O(1)$；决策只需比较 $K$ 个分数。它是真正的实时更新，状态和证据容易独立恢复。
- 它会付出探索错误的准确率代价。样本少时置信门应保持关闭，短流中可能永远不切换。
- 置信序列、epsilon-greedy、contextual bandit 和 IPW 都是成熟方法。AAMAS 新颖性风险为**中高**。可主张的是“把 direct tool correctness 的 selected-only 反馈转成可审计的 confidence-gated JEV”，不能主张发现了 UCB 或探索本身。
- 仓内 context JEV 旧结果使用的是二次调用的 verification gain，而且 v1 半径曾经错误；那些结果不能直接当作这条 direct-selection 候选的效果证据。只能复用其实现和测试思想，重新注册目标、反馈和否定条件。

### 最小零机时证伪实验

用当前 722 个 BBB development 行做严格回放：

1. 先固定四个 context cell、$\epsilon$、$n_0$、$\tau$ 和 batch look，不读取 evaluation 真值来调它们；
2. 策略每题只看到所选 provider 的 0/1 reward，记录完整 propensity 和 delay；
3. 同时跑 Frozen、Best Fixed、Online-SGD、Replay-SGD、BayesianRouter-style 和该 confidence-gated JEV；
4. 报告累计错误、gate open rate、探索调用率、每次更新耗时，以及每个 cell 的 support。

硬否定条件：三个 delay 中至少两个的 paired accuracy 不高于 Frozen，或 gate open rate 低于预先设定的可观测阈值而没有准确率收益。触发时保留为负结果，不通过缩小半径或事后调 $\epsilon$ 修复。

## 三个候选的比较

| 候选 | 新增信息 | 与普通 SGD/BayesianRouter 的差异 | 实时成本 | AAMAS 新颖性风险 | 当前定位 |
|---|---|---|---|---|---|
| 相关 provider 低秩后验 | selected arm → 相关未选 arm | 非对角 provider 协方差、二阶不确定性 | 低到中 | 中高 | 最值得先做信号审查的主候选 |
| context residual memory | selected 样本 → 相似未来分子 | 保留局部样本，不做全局梯度 | 低到中 | 高 | 轻量对照或表示瓶颈诊断 |
| 探索 + 置信门 direct JEV | 随机抽样 → 可审计正确率证据 | 设计型估计与安全切换，不是参数后验 | 低 | 中高 | 最干净的实时安全方案 |

## 建议的下一步，不申请新的 A800

先做一个零机时的“信息优势筛选”，而不是直接实现三套新算法：

1. **provider-transfer 表**：每个 outer fit fold 计算残差相关矩阵的谱、非对角强度，以及一个 selected-$a$ 反馈预测未选-$b$ 残差的 held-out AUC。这决定候选一是否有数据依据。
2. **local-memory 表**：按 Morgan 和已导出的 Laya 表示分别计算 top-1/top-5 同 provider 残差命中率。这决定候选二是不是只是在换一种 replay。
3. **direct confidence-gate 回放**：只实现候选三的计数、propensity、delay 和独立恢复，不调 backbone。它的结果可以和已有的 Context JEV 统计审计对照，但不能复用旧 gain 结果。
4. 只有至少一个候选在所有 fold 的预先登记筛选中留下清楚的正信号，才写方法合同并接入统一 runner；否则接受结论“当前数据不支持动态 JEV 主创新”，把论文收束为强负结果和 benchmark/审计贡献。

无论选哪一个，第一版正式 runner 必须同时保留 Best Fixed、Frozen、Online-SGD、Replay-SGD 和 BayesianRouter-style；confirmation 集在候选冻结、代码审查和独立恢复之后才打开。

## 审查结论

现在没有一个候选可以直接称为最终方法。候选一最可能产生真正的额外信息，但依赖 provider 关系在 scaffold-heldout 上稳定；候选二最容易实现但新颖性最弱；候选三最容易给出清楚的实时证据和安全边界，但探索成本与成熟先验会限制论文贡献。

因此下一小任务应是三张零机时信息筛选表和一份统一否定判据。筛选没有通过前，不再为普通 selected-only head、学习率、replay 大小或 prior 方差申请新的 Nebula 任务。
