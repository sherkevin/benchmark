# 可复用基线合同（审计建议，尚未冻结或运行）

对应 [report.md](report.md)。目的：辨别新增信息、informative release 和缓存代填的作用。只需现有小维度 ridge/logistic 与 paid-event ledger，不需要新推理服务或模型训练平台。

## 共同输入与账目

每条真正取得的 pair 保存 `event_id, acquisition_round, context_before_second_call, second_provider, original_propensity, weight, phi_snapshot, verifier_version, d1, d2, kappa, loss_version, release_round, provisional_inserted, status`。φ 必须在选择第二工具前可计算。d2 仅在付费返回后可读；它可以作为后续更新的中间状态，不能成为当前选择特征。loss 支持由任务声明决定。

构造账目案例（非实测）：某分子在第 10 轮以概率 $0.2$ 取得第二工具输出，调用前 $\phi$ 已冻结，追加前拒答、追加后答 1，真值约定第 14 轮释放。第 10 轮能记录动作变化，不能把第 14 轮的 $Y$ 提前写入；若采用逆概率权重，则该条 $w=1/0.2=5$，之后释放仍用这个值。

独立 evaluator 保管 Y，在统一时钟释放；learner 可提前算 `g(0), g(1)`，仅两者相等时标 exact。当前固定损失下 `d1=d2` 确保该条件。所有基线使用同一个已取得 pair 流、A0/b0、固定权重、λ、tie-break 和释放顺序。单条 release 幂等：重复要拒绝，不能再次加 Gram。

fixed-stream 第一阶段可用 w=1，只比较该采样集。闭环阶段另预登记 acquisition exploration 与 propensity；使用 IPW 时固定原始采样概率、记录有效样本量，不在 release 时重新计算，也不把 clip 后的权重称为无偏。不能把离线 B 的 1/3 当成在线 propensity。

## 必须先有的三个 ridge 对照

| 对照 | 得到 pair 时 | 真值到达时 | 解释用途 |
|---|---|---|---|
| 全等待 ridge | 保存 φ、d1/d2，不写学习状态 | A 加一次 wφφᵀ，b 加 wφg；之后 resolve | 原始延迟合同 |
| 精确标签 ridge | 若 `g(0)=g(1)`，当场写一次真实 A/b；否则 pending | 只给尚未写过的 pair 写一次 A/b | 精确早信息的直接价值；没有伪标签 |
| 缓存代填候选 | 每个 pair 的 A 立即写一次；b 写精确值或当时的合法预测，并保存该数值 | 只补 `wφ(g−inserted)`，A 不再写 | 与上项之差是 pending 代填影响，不能都归给早信息 |

构造更新案例（非实测）：取 $\kappa=0.01,w=1$。前后都答 1 的 pair，精确标签 ridge 可立即写 $g=-0.01$；由拒答改答 1 的 pair，若候选插入 $\widetilde g=0.02$，随后 $Y=1$ 使实际 $g=0.24$，则仅给 $b$ 加 $0.22\phi$，$A$ 不再写入该条。全等待 ridge 则在真值到达时一起写该条的 $A,b$。

刷新均值代填可以作为候选的第四实现，或以 report 的恒等式作为精确标签 ridge 的别名：每次已知标签改变后重算 θ_R，把 pending 代填全刷成 `φᵀθ_R`，得到同样的点预测。没有需要新增训练的独立 baseline。若要用 A_all 改探索半径，必须单独标注探索变更；第一阶段固定 paid stream 不允许靠这一差异改变 acquisition。

## 一个更强的结构对照：用中间状态分开估计

第一原则：每个 paid pair 的动作是否改变 $C$ 当场可知，**$C=1$ 也有即时监督信息**。只把 $C=0$ 化成负收益训练，会浪费 $C=1$ 的即时信息。构造案例（非实测）：追加前拒答、追加后答 1 时，$C=1$ 在第二输出返回后已知；是否救对仍等待 $Y$。由定义：

$$
G=-\kappa+C\Delta,\qquad
E[G\mid X,j]=-\kappa+P(C=1\mid X,j)\,E[\Delta\mid X,j,C=1].
$$

其中 $\Delta=\ell(d_1,Y)-\ell(d_2,Y)$ 是不含调用成本的损失改善，$G$ 为净收益；$X$ 是合法 pre-call 上下文，$j$ 是拟调用 provider。这个分解是恒等式。两项估计正确、足够支持和泛化能力不是恒等式的结论。

构造案例（非实测）：对某分子的指纹和第一次输出组成的 $X$，考虑候选第二工具 $j=2$。假定真实条件概率为 $P(C=1\mid X,2)=0.4$，改变动作后的平均损失改善为 $0.10$，成本为 $0.01$，则条件期望净收益是 $-0.01+0.4\times0.10=0.03$。在单条“拒答改答 1、$Y=1$”事件中，$\Delta=0.25$、$G=0.24$；条件均值与单次结果不同。

实现建议：

1. 使用与候选完全相同的 φ。用一份带截断输出的 ridge 或现有 logistic 拟合 `p_hat(C=1|φ)`；每个已取得 pair 都立即更新，目标 C 为 0/1。
2. 用另一份 ridge 拟合 `m_hat(Δ|φ,C=1)`；只在改变动作的 pair 真值释放后更新。当前 D=0/1/4 固定调度要在所有 C=1 行中一致；若这些行内部 delay 仍依赖 Δ，也不能沿用普通 released-only 拟合。
3. 选择前用 `g_hat=−κ+clip(p_hat,0,1)*m_hat` 排序，沿用同一个 budget/tie/paid-exploration 合同。没有第二输出时不得查看 C 或 d2。
4. 用 A/B 的同一开发划分初始化两项；所有“用于挑 λ/阈值”的 truth 都计入同一调参账本。这个 baseline 有两个小模型，明确报告参数量与计算差异；不要事后为它扩大搜索。冷启动对 C=1 支持不足时，m 使用 fit-only 的先验并报告 fallback，不根据 held-out 结果补样本。

构造预测案例（非实测）：若两个模型输出 $\widehat p=0.4$、$\widehat m=0.10$，则组合预测为 $0.03$。若 gate 的 ridge 原始输出为 $1.2$，先截断到 1，再计算 $-0.01+1\times0.10=0.09$；这仍只是预测，不是已知真值。

它复用 Esposito et al. 的中间状态建模思想，却保留当前任务所需上下文，避免假设所有改变动作的样本具有同一损失均值。**这是本审计提出的任务适配基线，不是原文 MetaBIO 的忠实实现，也没有继承其 regret bound。** 当前 fixed D 下，收益模型不再混合“即时负标签”和“延迟其他标签”；它仍会受到 acquisition support 与模型失配影响。

如两份模型的容量成为归因争议，应先完成上面的三个 ridge 匹配比较；不因多一个基线同时修改状态表示和候选更新。分解基线是关闭“新机制超越标准 partial feedback 建模”疑问的后续强比较。

## 与 Evolving FTRL 的可复用边界

ALT 2025 Algorithm 2 可直接复用的账本步骤是：每次反馈修订，将原始选择动作那一项累计估计加上 `(new_value−old_value)/original_p`。收益要固定仿射映射为有界 loss，不能每轮按当前样本范围动态缩放。

构造 loss 账目案例（非实测）：某次原始采样概率 $p=0.2$，临时 loss 为 $0.6$，后来修正为 $0.4$，累计该动作的估计加 $(0.4-0.6)/0.2=-1$。即使当前策略概率已经变为 $0.5$，也不能改用这个新分母。

若要做正式 Evolving FTRL baseline，选项应是固定有限动作（或预注册的一小组冻结 cascade policies），使用原文 negative-entropy + log-barrier 正则，保证采样概率为正。它与 contextual ridge 比较的是不同函数类。付费调用预算和选择性获取还需预算 wrapper；不能说“原文原样支持医药预算”。因此本轮优先完成相同 φ 的 ridge 对照与分解基线，不为跑一个名称相似的 FTRL 偷换任务。

## 检查与停止规则

- 固定相同 acquired set 排空后，三种 ridge 的 A、b、θ 应相同；数值容差提前冻结，不能只核预测四舍五入。该检查不适用于上面的分解估计器，它优化不同目标。
- D=0 必须在同一 release-before-next-decision 合同下没有伪延迟差异。
- D>0 同时记录：pending 数量和年龄、C=0/C=1 分组误差、实际增益正/负分组误差、已到均值与全 mature cohort 均值的差。成熟目标仅由 evaluator 做事后诊断，不回流当前学习。
- 无法用相同合法信息重算的 hint 不得纳入对照；inserted 值与版本必须可复核。
- 闭环主要证据是终局 utility 与正收益 pair 获取率，维持相同总调用预算和预登记探索。只减少 interim training loss 或 drain error 不通过效果判断。
- 若缓存代填只胜过全等待，却不胜过精确标签 ridge，证据仅支持提前精确反馈；若只胜过错误的“pending 都填 −κ”，候选机制不获支持。

本文件仅提供下一张实验卡可直接采用的实现合同；没有执行数值实验，没有改动主方法或源代码。
