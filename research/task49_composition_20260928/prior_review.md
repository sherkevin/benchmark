# Task49：组合贡献与动态 JEV 有机整体审查

日期：2026-09-28
范围：只读先验审查；没有修改生效主线、没有提交 Nebula、没有读取 confirmation、没有把候选方案写成已验证方法。
目的：在不把“组合”一票否决的前提下，判断哪些组合能形成一个有机、可计算、可证伪的整体贡献。

## 1. 先给结论

组合本身可以成为论文贡献。审稿人通常不会因为某个模块以前出现过，就自动否定一个把它们重新组织成新机制的工作。真正要审查的是：

1. 组合是否共同解决一个清楚的任务缺口，而不是把多个热门名词并排放在系统图里；
2. 组件之间是否有必须的输入输出关系，去掉任意一个组件是否会破坏方法要满足的性质；
3. 这种组合是否改变了可观测信息、优化目标、可计算性或安全性质，而不只是换了调用顺序；
4. 论文是否用 matched ablation 把整体收益归因到这条关系，而不是只报告一个最终分数。

因此，本审查不采用“旧组件组合 = 不是创新”的规则。当前更准确的判断是：

> “冻结 JEV/Laya + 一个普通在线 head”已有近邻，而且在 Task49 上没有通过开发诊断；但“selected-only 二值正确性事件 → 共享 provider/context 状态 → 表示变化时的充分统计传输 → 保守选择/验证决策”可以形成一条新的、任务约束驱动的整体机制。它现在仍是候选，需要零机时可识别性检查和 matched 实验，不能直接写成已成立贡献。

这条路线的潜在贡献不在于声称发明了 JEV、低秩分解、RLS、置信门或线性注意力，而在于把它们组织成一个针对科学工具正确性反馈的**闭合状态接口**：每一条已释放的 selected feedback 都能被解释、被传递、被恢复；表示变化不会让历史统计失效；不确定时回退到冷启动 JEV。

## 2. 已有论文如何把组合做成贡献

下面的例子不是“组合论文”的统计结论，而是已经逐页阅读的可核查参照。它们说明组合要成为贡献，必须改变问题中的约束、目标或保证。

### 2.1 AAMAS 2024：DOL-RM

- 论文：*Learning to Schedule Online Tasks with Bandit Feedback*。
- 原文：[AAMAS 2024 proceedings PDF](https://www.ifaamas.org/Proceedings/aamas2024/pdfs/p1975.pdf)。
- 本地证据：`research/paper_sources/aamas_related/aamas2024_online_scheduling.pdf`；逐页摘要见 `research/paper_sources/reading_agent_2023_2024.md`，问题与方法 pp.1–6，实验 pp.7–8。

这篇工作把 optimistic reward/cost estimation 与 Robbins–Monro arrival learning 放在同一个算法中。两部分单独都不是新工具；组合之所以有意义，是因为论文把“只看到 selected action 的完成反馈”和“任务到达分布未知”同时写进了目标，局部 reward/cost ratio greedy 有明确反例，两个模块分别处理两个误差源，最后由同一 regret 目标验证。它不是把两个 baseline 叠加后报告平均准确率。

对我们的启发：如果动态 JEV 只是冷启动分数后面接一个在线 head，贡献很弱；如果每个组件都对应 selected-only、延迟、provider 关系、表示变化中的一个不可省略的失效源，并且最终指标直接测这些失效源，组合可以是一个整体机制。

### 2.2 AAMAS 2025：Soft Condorcet Optimization

- 论文：*Soft Condorcet Optimization for Ranking of General Agents*。
- 原文：[AAMAS proceedings PDF](https://www.ifaamas.org/Proceedings/aamas2025/pdfs/p1253.pdf)，[arXiv](https://arxiv.org/abs/2411.00119)。
- 本地证据：`research/paper_sources/aamas_related/aamas2025_soft_condorcet_arxiv.pdf`；方法 pp.4–5，性质与实验 pp.5–8。

作者把 noisy partial comparisons、sigmoid ranking loss 和单事件在线更新放在同一个 ranking 问题里。其贡献不是“用 SGD 排序”，而是损失、在线更新和 Condorcet 性质之间的对应关系。论文同时保留非凸、数值不稳定和 online overfitting 的边界。

对我们的启发：二值 correctness 反馈可以成为一种有语义的 event，而不是普通训练样本。若我们把 `r=1/0` 转成相对于冷启动 JEV 的 correctness residual，再让它更新共享的 provider/context 状态，必须同时说明 residual 的单位、更新目标、在线性质和失败边界。

### 2.3 AAMAS 2024：PBIM

- 论文：*Potential-Based Reward Shaping For Intrinsic Motivation*。
- 原文：[AAMAS 2024 proceedings PDF](https://www.ifaamas.org/Proceedings/aamas2024/pdfs/p589.pdf)，[arXiv](https://arxiv.org/abs/2402.07411)。
- 本地证据：`research/paper_sources/aamas_related/reading_agent_2023_2024.md` 中的 A24-PBIM；正文 pp.4–8，限制 p.8。

PBIM 将 intrinsic motivation、potential conversion 和 policy-preservation property 绑定起来。normalized 版本不是把一个模块换成另一个模块，而是为了同时控制短期 bias 和保持最优策略集合。其有效性来自可检查的不变量，论文也明确写出不覆盖 future-action-dependent reward 的条件。

对我们的启发：动态 JEV 的组合应有一个可验证不变量。例如，表示适配后，新的 sufficient statistics 与“用同一批合法事件完整重算”一致；当证据不足时，决策与 Frozen JEV 完全一致。这个不变量比“模块数量”更能说明组合是一个整体。

### 2.4 AAMAS 2026：Truthful Reverse Auctions

- 论文：*Truthful Reverse Auctions for Adaptive Selection via Contextual Multi-Armed Bandits*。
- 原文：[AAMAS 2026 proceedings目录](https://ifmas.csc.liv.ac.uk/Proceedings/aamas2026/forms/contents.htm)，[arXiv](https://arxiv.org/abs/2602.14476)。
- 本地证据：`research/paper_sources/aamas_related/truthful_reverse_auctions_contextual_mab.pdf`；问题 pp.1–3，机制 pp.7–9，实验 pp.17–18。

该论文已经把 provider selection、contextual MAB、在线 reward 和 provider 约束放在一起；真正的新意在 monotone allocation、ROSA、truthfulness 和 regret 的闭合，而不是“使用 contextual bandit”。这篇论文是我们必须正面比较的最近邻，也说明了一个组合如果没有新的约束或性质，很容易被审稿人归入已有路由方法。

### 2.5 其他直接近邻

- P06 *Online Multi-LLM Selection via Contextual Bandits under Unstructured Context Evolution*：本地 `research/paper_sources/aamas_related/online_multi_llm_selection_context_evolution.pdf`，形式化 pp.4–6，实验/限制 pp.8–12。它已经覆盖固定模型池下的 query-dependent selected reward 更新，并把 delayed/noisy feedback 和 dynamic model pool 留作未来工作。
- P07 *RouterBench*：本地 `routerbench.pdf`，pp.1–3、5–12。它是静态 full-panel routing benchmark；预生成所有模型输出，不能证明 selected-only delayed learning。
- P08 *GraphRouter*：本地 `graphrouter.pdf`，pp.4–10。它是图结构和 new-LLM few-shot 的静态/离线适应；不能替代延迟 selected-only 在线更新。
- Laya：开源仓库 [NandhaKishorM/laya](https://github.com/NandhaKishorM/laya)，本地快照 `task/task45-dynamic-decision-model/reference/laya-8465935/` 和输入审计 `task/task49-pharma-benchmark-baseline-freeze/laya_input_audit/`。Laya 的 typed decision/candidate-marker 表示适合作为冷启动 backbone；当前没有已核验的 selected-only delayed correctness update 接口。
- AnyJev：方法说明 [AnyJev method v3](https://github.com/nokia-applied-research/AnyJev/blob/main/docs/method_v3.md)，本地复用审计 `task/task46-open-source-composition/report.md`。`observe`/周期性 closed-form head 已覆盖“标签到达后重拟合小 head”的工程骨架，但默认保存的是 observations，不是本项目的 selected-only、多 provider、延迟真值合同。

这些近邻不否定组合贡献；它们规定了组合必须落在一个更窄的、可证明或可审计的差异上。

## 3. Task49 当前证据说明了什么

### 3.1 已经被否定的是“普通拼接”

现有 development 结果来自同一 33-provider BBB 任务和 scaffold-disjoint 评价，不是最终 confirmation。关键数值：

| 组合/方法 | 结果 | 直接含义 |
|---|---:|---|
| train-only Best Fixed | `0.9155` | 强冷启动 incumbent，不能只和 Random 比 |
| Morgan MLP BCE | `0.8961 / 0.8906` | 普通非线性 correctness head 没有超过 fixed |
| Morgan BT+CLS | `0.8878 / 0.8947` | 排序/分类目标的简单组合没有变成选择收益 |
| Laya MLP BCE | `0.8850 / 0.8587` | 换静态 backbone 不能自动解决直接选择 |
| Laya BT+CLS | `0.8753 / 0.8684` | Laya 表示和离线 ranking/classification 组合也未过闸门 |
| BayesianRouter online vs Frozen | `-0.00277/-0.00208/-0.00068`（delay 0/1/4） | 独立 selected-only 后验更新没有稳定收益 |
| calibrated provider transfer | 三个 delay 均低于 Frozen | 有关系信号不等于安全的跨 provider 决策收益 |
| context residual memory | 三个 delay 均未超过 independent | 局部记忆本身没有解决 scaffold-disjoint 泛化 |

完整来源：

- `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/report.md`；
- `task/task49-pharma-benchmark-baseline-freeze/laya_static_diagnostic_v1/report.md`；
- `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/online_bayesianrouter_effect_result.md`；
- `task/task49-pharma-benchmark-baseline-freeze/calibrated_provider_transfer_v2/report.md`；
- `task/task49-pharma-benchmark-baseline-freeze/context_memory_replay_v2/report.md`。

这些结果不应被表述成“组合没有创新”。它们否定的是一个具体弱假设：只要把静态 JEV/Laya 表示、普通预测头、selected-only 更新或一个 transfer gate 接起来，就能自动得到动态选择收益。

### 3.2 仍然存在可利用的任务空间

Best Fixed 为 `661/722=0.9155`，evaluator Oracle 为 `720/722=0.9972`。固定 provider 错的 61 个分子中，59 个至少有一个其它 provider 正确，另 2 个所有 provider 都错。这个结果给出两个同时成立的事实：

1. 任务并非没有可救空间；
2. 关键困难是只看到被选 provider 的反馈时，怎样判断何时应离开强 incumbent，以及怎样把一条标签合法地传递给未来请求或其他 provider。

因此下一步创新应围绕**反馈如何变成共享可恢复状态**，而不是继续更换 backbone 或优化器。

## 4. 可形成有机整体的候选机制

下面是一条候选方法骨架，不是最终方法卡。它把四个组件放在同一个因果链中；每个组件都由上面的任务缺口逼出来。

### 4.1 事件：二值 correctness 不是普通标签

冻结 JEV 在训练区校准出 provider-specific 冷启动概率 $p_a^0(x)$。测试流中一次合法反馈事件写成

$$
E_i=(x_i,A_i,a_i,r_i,\tau_i^{\mathrm{release}}),
\qquad r_i\in\{0,1\}.
$$

其中 $a_i$ 是真正被调用的 provider，$r_i$ 只在工具结束且参考判定可用后释放；未选 provider 不产生隐含负标签。定义事件残差

$$
e_i=r_i-p_{a_i}^0(x_i).
$$

**案例。** 某 BBB 分子 `z000282` 的冻结概率为 $p_{A}^0=0.60$。A 被调用并在 release tick 收到 $r=1$，所以 $e=+0.40$；若收到 $r=0$，$e=-0.60$。反馈未释放时，事件不进入状态。这个残差有明确单位：相对于冷启动正确率的偏差。

这一步使 JEV 的原始 decision score 与工具 correctness 反馈对齐；不能把 Laya 的 choice score 直接当成 correctness probability。

如果 provider 返回的是关于同一二元性质的有效输出 $z_i\in\{0,1\}$，同一个事件还可以恢复这次已选样本的性质标签：

$$
\widetilde y_i=
\begin{cases}
z_i,&r_i=1,\\
1-z_i,&r_i=0.
\end{cases}
$$

**案例。** `z=1,r=1` 给出 $\widetilde y=1$；`z=1,r=0` 给出 $\widetilde y=0$；`z=0,r=0` 给出 $\widetilde y=1$。如果是 timeout、`unknown truth` 或多分类/连续输出，不能套这个式子，也不能把 `system_success=0` 当作性质标签。

这个恢复标签提供了比单一 residual 更直接的共享监督通道。可以用共享属性头 $q(x,e)=\Pr(Y=1\mid x,e)$ 和 provider 条件正确性头 $h_a(x,e,y)=\Pr(R_a=1\mid x,e,Y=y)$ 构成调用前分数

$$
s_a(x,e)=q(x,e)h_a(x,e,1)+\bigl(1-q(x,e)\bigr)h_a(x,e,0).
$$

**案例。** 若当前 $q=0.7$，provider A 在真值为 1/0 时的条件正确率分别为 $0.9/0.6$，则 $s_A=0.7\times0.9+0.3\times0.6=0.81$。这个分数不读取当前 A 的输出，也不读取未选 provider；它只使用历史中合法恢复的 $(\widetilde y,r)$ 事件。q/h 共同进入下一次 provider 选择，因此这不是两个独立的辅助预测头。

该通道与 residual/shared-state 通道可以有两种关系：

- 若 q/h 的全概率分数直接作为动态 JEV，必须和普通 selected-reward SGD 做 matched 对照；
- 若 residual state 是主分数，q/h 可以作为恢复标签的共享辅助状态，但必须证明它改变了后续 provider 选择，而不只是提高离线属性 AUC。

这一分支是目前更值得优先做的候选，因为它把“工具输出正确性”转成同一个分子的共享标签；但它仍受二元可逆性、选择偏差、provider coverage 和版本漂移限制。

### 4.2 共享状态：一条反馈同时约束 provider 与 context

从 outer-fold fit 区估计 provider 几何 $u_a\in\mathbb R^q$，从冻结 backbone 得到请求表示 $h(x)\in\mathbb R^d$。定义选中事件的共享特征

$$
\psi_i=u_{a_i}\otimes h(x_i)\in\mathbb R^{qd}.
$$

维护已经释放事件的 residual sufficient statistics

$$
G_t=\lambda I+\sum_{i\in\mathcal O_t}\psi_i\psi_i^\top,
\qquad
b_t=\sum_{i\in\mathcal O_t}e_i\psi_i,
\qquad
w_t=G_t^{-1}b_t,
$$

其中 $\mathcal O_t$ 是截至决策时真正释放的事件集合。候选 provider $a$ 的动态修正为

$$
\delta_t(a,x)=\bigl(u_a\otimes h(x)\bigr)^\top w_t,
\qquad
\ell_t(a,x)=\ell_a^0(x)+\delta_t(a,x).
$$

**案例。** 若 $q=1$、当前选中 A 的 provider 坐标为 $u_A=1$、B 为 $u_B=0.8$，同一分子表示为 $h=(1,0.5)^\top$，A 的错误 residual 为 $e=-0.6$，那么这条事件会让 B 的未来修正与 A 同向变化，幅度由 $u_Bu_A$ 和未来分子相似度共同决定。B 没有被调用，因此没有把 B 的当前结果伪造为标签。

这里的有机关系是：

`selected correctness event → residual → provider/context shared coordinate → all candidate scores`。

它和“独立 provider head + replay”不同；但是否有效必须通过 held-out transfer 和 permutation 对照验证。provider geometry 若只在 fit 区成立、evaluation 中不转移，就删除该组件。

### 4.3 表示更新：只更新低维 adapter，并传输充分统计

如果只更新 $w_t$，当前表示瓶颈可能无法表达新的错误模式；如果直接改 backbone，又会使历史 $G_t,b_t$ 不再对应当前坐标。候选解决办法是只允许一个线性或低秩请求 adapter $A_t\in\mathbb R^{d\times d}$，令

$$
\widetilde h_t(x)=A_t h(x),
\qquad
T_t=I_q\otimes A_t,
\qquad
\widetilde\psi_i=T_t\psi_i.
$$

不把正则项混进可运输的历史矩；保存未加正则的原始统计

$$
G_t^{\mathrm{raw}}=\sum_{i\in\mathcal O_t}\psi_i\psi_i^\top,
\qquad
b_t^{\mathrm{raw}}=\sum_{i\in\mathcal O_t}e_i\psi_i.
$$

adapter 改变后直接计算

$$
\widetilde G_t=T_tG_t^{\mathrm{raw}}T_t^\top+\lambda I,
\qquad
\widetilde b_t=T_tb_t^{\mathrm{raw}},
\qquad
\widetilde w_t=\widetilde G_t^{-1}\widetilde b_t.
$$

这是线性 adapter 下的精确矩变换；它与把所有已释放事件用新表示重新跑一遍得到的 ridge 解相同。它不是“线性 attention 首次发明”，而是把有限状态递推和动态表示的历史一致性绑定在本任务中。

**案例。** 若 $q=1,d=2$，$A=\operatorname{diag}(2,1)$，事件特征为 $\psi=(1,0)^\top$，原始矩为 $G^{\mathrm{raw}}=\psi\psi^\top$、$b^{\mathrm{raw}}=e\psi$。变换后特征为 $(2,0)^\top$；直接重算得到的矩正好是 $T G^{\mathrm{raw}}T^\top$ 和 $T b^{\mathrm{raw}}$。若 adapter 是非线性 MLP，这个等式不成立，不能继续称为 exact transport，只能保存有界 replay 并报告近似误差。

这一步是目前最值得审查的组合点：表示更新不是独立模块，而是因为更新表示会使在线 sufficient statistics 失效，才必须与状态传输一起设计。

### 4.4 决策门：证据不足时回到冷启动

selected-only 反馈有选择偏差，一次错误事件也可能把全局状态推坏。为保留冷启动能力，候选策略不强制使用 $\delta_t$，而是对每个候选维护由 $G_t^{-1}$ 导出的不确定性，并只在动态候选的保守下界超过 incumbent 的保守上界时切换；否则继续使用 Frozen JEV。

这个门的具体形式仍需冻结，至少要满足两个不变量：

1. $\mathcal O_t$ 只包含已释放且 exactly-once 的 selected 事件；
2. 门关闭时选择结果与同一 checkpoint 的 Frozen JEV 完全一致。

**案例。** 若动态候选的置信下界没有超过 incumbent 上界，虽然 $\delta_t$ 为正，也继续调用 incumbent；日志中必须记录 `gate_closed`，不能把这次没有切换写成动态模型胜出。

## 5. 为什么这是一个整体，而不是四个模块并排

这条候选的依赖关系是强制的：

| 组件 | 它消费什么 | 它修复什么 | 去掉后可观察的失败 |
|---|---|---|---|
| correctness residual | selected、delayed $r\in\{0,1\}$ 与校准 $p^0$ | 把 JEV 分数和真实工具对错放到同一单位 | 直接把 choice score 当 correctness；静态 calibration 失真 |
| provider/context shared state | residual、fit-only $u_a$、$h(x)$ | 让一次选中反馈能够约束未来候选 | 独立 Bayesian/SGD 只改被选 provider |
| exact statistic transport | 释放事件的 raw moments、线性 adapter | 表示改变后保持历史样本含义 | 更新 representation 后旧统计失效，必须全量重放 |
| conservative gate | 动态均值/不确定性、incumbent | 防止选择偏差和稀疏支持破坏冷启动 | provider-transfer v2/v3 的错误传播 |

四项共同实现一条可审计的 `event → state → representation → decision` 闭环。论文应把它写成一个机制和一个状态转换，而不是四个独立贡献点；摘要中可以只给一个中心动词，例如“把延迟的 selected correctness 反馈转成可传输的动态工具决策状态”。

## 6. 最强反例与必须保留的边界

### 6.1 selected-only 下的不可识别反例

若策略永远选择 A，构造两个环境：A 在两者中的反馈分布完全相同；环境 P 中 B 的正确率为 `0.95`，环境 Q 中 B 的正确率为 `0.05`。只看 A 的 selected feedback，两个环境产生同样的历史，任何更新器都无法同时知道 B 的真实状态。

因此组合方法至少需要一个明确的信息来源：fit-only provider 关系、可重复 context、预先固定探索、provider/version 事件或调用后的额外观测。没有这些信息，更新器再复杂也不能解决不可识别性。

### 6.2 聚合碰撞反例

如果历史 key 全相同，成功和失败各一次，任何只保存 `success sum / total sum` 的读取头都只能得到 `0.5`。后续学习 query 不能恢复两个本来可分的分子，梯度也可能为零。

因此若要更新表示，必须保存事件级 raw moments、受控 reservoir 或其他可证明的 sufficient statistic；“固定写入、学习读取”不能自动恢复被聚合丢掉的信息。

### 6.3 provider 关系可能不稳定

即使 fit 区残差相关，evaluation 的 scaffold、工具版本或输入分布变化后，关系可能失效。`calibrated_provider_transfer_v2` 显示真实 transfer 与 permutation 有差异，但三个 delay 都低于 Frozen；这说明“关系存在”与“关系可安全用于选择”是两个命题。

### 6.4 线性传输的适用范围

精确 transport 只对线性 adapter 和保存的原始二阶矩成立。若直接微调 Laya/MLP 或改变 tokenizer，历史事件必须重新编码；不能把矩阵变换公式当作任意 backbone fine-tune 的证明。

### 6.5 没有动态事件时不能宣称动态恢复

固定 provider 池、固定 checkpoint、没有 drift/arrival/retirement 的流，只能证明 fixed-pool online selection。若论文主张 dynamic，至少要有一种预注册事件并报告 recovery steps；否则组合的表示更新没有明确目标。

## 7. 合法的 matched baseline 与消融

建议把下列方法作为一个 family 统一实现。每条策略共享相同的 fold、输入顺序、菜单、delay、事件、随机种子、静态 JEV checkpoint 和调用预算；在线 evaluation 只接收 selected、已释放的二值反馈。

| 角色 | 保留的组件 | 去掉的组件 | 要回答的问题 |
|---|---|---|---|
| Frozen JEV | 冷启动表示与分数 | 全部在线状态 | 任务的强静态 incumbent 是多少 |
| Matched Online/Replay SGD | 同一表示、独立 selected head | provider transfer、adapter、gate | 普通在线更新是否已足够 |
| BayesianRouter/PILOT-adapted | 同一静态先验、selected posterior | 共享几何、transport、gate | 现代在线近邻是否已覆盖收益 |
| Transfer-only | correctness residual、$u_a$、共享状态 | representation adapter | 跨 provider 传递本身有没有信号 |
| Adapter-only | correctness residual、$A_t$、统计 transport | provider transfer | 表示变化本身有没有信号 |
| Full candidate | residual + shared state + adapter transport + gate | 无 | 整体是否有超出各部分的收益 |
| Permuted/diagonal $U$ | 同样参数量和计算量 | 真实 provider 几何 | 收益是否来自正确的 provider 关系 |
| Full replay transport | 同一 adapter、每次全量重编码 | 只替换 transport 实现 | exact transport 是否等价且更快 |
| Oracle/full panel | evaluator 读取全 panel | deployability | 可救空间上界，不能进入公平排名 |

如果 Full candidate 只超过 Frozen，却不超过 Matched Online/Replay、PILOT 或 BayesianRouter，不能归因于整体机制；如果只在 full-panel 或未延迟流中有效，也不能写 selected-only delayed contribution。

## 8. 组合贡献的量化验收条件

以下是内部候选闸门，目的是让“讲得更好”最终落到可测量证据，不是 AAMAS 官方分数，也不是当前已同意的最终方法门槛。

### A. 机制正确性

1. **事件权限**：独立恢复器证明所有状态更新只来自已释放、已选、exactly-once 事件；任何未选标签或当前真值泄漏即失败。
2. **transport 等价性**：在线 transport 与用同一合法事件全量重算的 ridge 解在人工算例、随机合成和真实缓存上分别达到预注册数值容差；否则只称近似 replay。
3. **provider transfer 可识别**：fit-only 关系在一个预先指定的 development held-out diagnostic fold 上的 transfer AUC 高于随机水平，并且真实 $U$ 与置换/对角 $U$ 的方向差异有跨 fold 证据。这个诊断只能由 evaluator 读取 held-out full panel，不能把未选标签送进策略、调参或 confirmation；只有 fit 区相关而没有 held-out transfer，不能打开共享更新。

### B. 决策效果

4. **强基线比较**：Full candidate 必须与 Frozen、Online/Replay SGD、PILOT-adapted、BayesianRouter-style 在同一 delay 和事件合同下比较；不能只超过 Random 或一个未匹配的弱 head。
5. **动态事件**：至少一种 provider arrival、retirement 或 drift 事件上，报告 recovery accuracy、达到固定阈值所需 selected feedback 数、首次 gate-open 时间和额外调用数。
6. **冷启动保护**：在没有足够证据的流段，Full candidate 与 Frozen 完全一致；在全流上不能为了切换率牺牲 incumbent 的显著性能。若只改善后半段，主指标必须报告前后分段而不是只报平均值。

### C. 效率与可恢复性

7. **状态规模**：明确报告 $O(qd)$、$O((qd)^2)$ 或实际状态字节数；把 backbone forward、队列和日志成本计入端到端 p50/p95。
8. **历史重算**：adapter 更新不需要无界历史重放；若采用有界 reservoir，报告容量、近似误差和丢弃规则。
9. **独立恢复**：从原始 JSONL、输入 hash 和源码 hash 可以重建每一步状态、选择和 gate；恢复器不能直接调用被测策略的更新函数来“自证”。

## 9. 零机时下一步

在写正式方法卡或申请新的 A800 前，先做三项 CPU/代数筛选：

1. **统计传输核验**：用随机低秩 adapter 与已有 selected event snapshot，对比全量重算和 transport 的 logits、选择、状态；保存逐事件误差。
2. **provider 关系筛选**：从现有 fit snapshots 计算 held-out transfer AUC、真实/置换 $U$ 的跨 fold 差异；不通过就保留 shared state 作为消融，不把它定为主方法。
3. **动态事件最小回放**：在不打开 confirmation 的 development 流上注入预登记的 provider version drift 或 arrival，比较 Frozen、independent、transfer-only、adapter-only 和 full candidate 的 recovery；先固定事件强度和预算，不看结果调门槛。

只有三项都能给出可复核信号，才把这条组合写成正式方法合同并提交一条 A800。任何一项失败都不会推翻“组合可能成为贡献”这个原则，只会缩小我们能诚实主张的机制范围。

## 10. 当前应如何写进论文（如果候选通过）

正文不要写“我们把已有模块简单组合起来”，也不要主动替审稿人下“这不是创新”的结论。应当把故事写成一个问题驱动的机制：

1. 冷启动 JEV 给出可用但不完美的 provider correctness prior；
2. 工具只返回 selected、延迟的二值 correctness，普通独立更新无法识别未选 provider；
3. 我们把事件 residual 放到 fit-only provider/context 坐标中，得到共享动态状态；
4. 当表示改变时，我们用精确 statistic transport 保持历史事件语义；
5. 不确定时，决策门回退到 incumbent，并在 provider/version 事件后测恢复。

这五步必须在问题定义、公式、算法、复杂度、消融和实验指标中使用同一组术语。若某个组件无法对应一个可测的失败源，就从主方法移到 baseline 或 ablation；这是为了让组合更紧密，而不是否定组合创新。

## 11. 本审查的边界

- 本文件不是最终方法决议，不修改三份生效研究主文档，不关闭任何已同意的创新方向。
- 本文件不承诺 AAMAS 录用；它只把“组合可否形成贡献”转成可审查、可证伪的条件。
- 现有 Task49 结果是 development 诊断；不得把它们包装成 confirmation 或最终论文效果。
- 组合是否最终成立，仍需 matched baseline、动态事件、独立恢复和用户确认后的方法冻结。
