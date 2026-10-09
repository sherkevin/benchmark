# Provider-Graph Relay v1：一手 prior-art 核验与新颖性风险

日期：2026-10-04
范围：只核验图反馈 bandit、关系/协同 bandit、图标签传播和在线算法选择的原始论文或作者版本。本文不是完整系统综述。

## 结论

候选机制的“图上共享反馈”不是空白问题。已有工作覆盖了：

1. **选择动作后通过图获得其他动作的反馈**（feedback-graph bandits）；
2. **已知关系图上的参数/奖励共享**（collaborative/relational contextual bandits）；
3. **用相似度图传播标签或平滑信号**（label propagation / graph-signal bandits）；
4. **依据实例反馈动态选择算法**（online algorithm portfolios）。

因此，`u_p <- u_p + η W_{pq} x_t δ_t` 这一更新若没有严格的任务边界、信息时钟、图构造依据、预算匹配和独立增益，很容易被审稿人归为已有图传播或协同 bandit 的任务适配。候选仍可能有贡献，但新颖性必须落在“部署工具 provider 的 outcome graph、selected-only delayed feedback、一次性可审计 warm-start、稀疏实时 relay 与工具调用回放合同的整体闭环”，不能只落在加了一个 graph。

## 1. Feedback-graph bandits：最直接的概念重叠

### Mannor & Shamir, NeurIPS 2011

- **论文：** *From Bandits to Experts: On the Value of Side-Observations*。
- **一手来源：** [NeurIPS paper page](https://papers.nips.cc/paper/4366-from-bandits-to-experts-on-the-value-of-side-observations)，[作者 arXiv 版本](https://arxiv.org/abs/1106.2436)。
- **核心设定：** 每轮选择 action；除了所选 reward，还通过有向图获得其他 action 的 side-observation。图可固定或随时间变化，并据图结构给 regret bounds。
- **与候选的重叠：** 都有 provider/action 节点、selected-only 反馈、图边和跨节点信息利用。
- **关键差异：** 该文的边定义“选择 i 会直接观测 j 的 reward 信息”；候选中未选 provider 的结果并未被观察，而是用 selected provider 的反馈在算法内部 relay 到邻居。因此候选不能把自己的 $W$ 称为环境 feedback graph，也不能套用 side-observation 的 regret 结论。
- **新颖性风险：** 若候选只报告“图邻居机会率”或把传播后的 pseudo-update 当作未选 reward observation，审稿人会认为信息模型被偷换。必须把 graph 定义为 **transfer prior/relay graph**，并验证传播不是额外观测。

### Alon et al., COLT 2015；Alon et al., SJoC 2017

- **论文：** *Online Learning with Feedback Graphs: Beyond Bandits*。
- **一手来源：** [PMLR v40](https://proceedings.mlr.press/v40/Alon15.html)。
- **核心设定：** 反馈图的可观测性决定 minimax regret；区分 strongly/weakly/unobservable，并允许图随时间变化。
- **与候选的重叠：** 图的方向、邻居覆盖和图结构复杂度都是核心对象。
- **关键差异：** 该线研究可观测反馈带来的统计信息量；候选只得到一个 selected outcome，graph 用于参数迁移，不改变可见数据。
- **新颖性风险：** 候选的 `graph_opportunity_rate` 若被当成“有效反馈覆盖”，会错误暗示 side-observation；应报告“潜在 transfer target 可用性”，并单独检查未选输出仍不可见。

## 2. 关系/协同 bandits：更新形式已有近邻

### Cesa-Bianchi, Gentile & Zappella, NeurIPS 2013

- **论文：** *A Gang of Bandits*。
- **一手来源：** [NeurIPS PDF](https://proceedings.neurips.cc/paper_files/paper/2013/file/18997733ec258a9fcaf239cc55d53363-Paper.pdf)。
- **核心设定：** 图上的多个 contextual bandit 实例共享 contexts/payoffs；GOB.Lin 用 graph Laplacian kernel 让相邻节点的参数协同更新；还研究图噪声和聚类。
- **与候选的重叠：** provider-local contextual parameter、图邻居信息共享、稀疏/噪声图和 Independent 对照都已有直接先例。
- **关键差异：** 原文通常把图作为用户/节点关系并允许共享 observed signals；候选的图由 fit-only provider correctness 共变构造，在线只有 selected provider delayed outcome。
- **新颖性风险：** 简单的邻居加权 residual 更新属于同一类“networked contextual bandit”思想。必须加入 degree-matched shuffled graph、图噪声敏感性和与 Laplacian/soft-sharing 近邻的适配对照，否则不能说明方法不是 GOB.Lin 的任务换皮。

### Wu et al., SIGIR 2016

- **论文：** *Contextual Bandits in a Collaborative Environment*（CoLin）。
- **一手来源：** [ACM DOI](https://dl.acm.org/doi/10.1145/2911451.2911528)，[作者公开 PDF](https://huazhengwang.github.io/papers/SIGIR16_CoLin_Wu.pdf)。
- **核心设定：** 利用用户邻接图在 online contextual bandit 中共享 context/payoff，给出协同 LinUCB。
- **与候选的重叠：** “独立模型”与“图关系共享模型”的 matched comparison、冷启动和在线反馈迁移已有成熟范式。
- **关键差异：** CoLin 的图通常是用户关系；候选 graph 是工具 provider 的历史 outcome-error relation，反馈具有 delayed release 和 runtime failure 语义。
- **新颖性风险：** 若只展示 Graph vs Independent 的提升，审稿人会要求 CoLin/GOB.Lin 类 relational baseline 或至少说明为何不能直接适配。需要写清图构造和 selected-only relay 是任务特有约束，并做统一接口适配。

### Gupta et al., 2021

- **论文：** *Multi-Armed Bandits with Correlated Arms*。
- **一手来源：** [arXiv:1911.03959](https://arxiv.org/abs/1911.03959)。
- **核心设定：** 不同 arm 的 reward 分布相关；C-UCB 等方法利用相关结构减少探索，并分析 latent-source correlation。
- **与候选的重叠：** 从 arm correlation 推断未直接拉取 arm 的收益，是 candidate provider graph 最接近的统计直觉。
- **关键差异：** correlated-arm 模型通常给出结构化 reward correlation/观测关系和置信更新；候选从 fit full-panel labels 估计 graph，然后仅用一个 selected binary correctness 更新邻居 residual。
- **新颖性风险：** 候选需要说明为何 provider correctness graph 不是普通 correlated-arm prior；至少给出 correlation 估计误差、边误配时的安全/有害边界，以及与 correlated-bandit baseline 的关系。

### Qi, Fei & Zhu, UAI 2024

- **论文：** *Graph Feedback Bandits with Similar Arms*。
- **一手来源：** [PMLR v244](https://proceedings.mlr.press/v244/qi24a.html)，[作者 arXiv 版本](https://arxiv.org/html/2405.11171)。
- **核心设定：** 相似 arm 构成图；选中一个 arm 后可得到相似 arm 的反馈，提出 D-UCB/C-UCB。
- **与候选的重叠：** 都关心 arm/provider similarity graph、graph degree/coverage 和 online selection。
- **关键差异：** 该文假设图反馈本身可观察；候选不获得邻居的实际 outcome，只做 transfer update。
- **新颖性风险：** 候选卡的 opportunity gate 和“neighbor available”叙述很容易让人误解为 graph feedback。必须把 available 定义为决策前菜单/运行资格，明确不含邻居 output/reward。

## 3. 图标签传播与图信号：更新算子本身不是新颖点

### Zhu & Ghahramani, 2002

- **论文：** *Learning from Labeled and Unlabeled Data with Label Propagation*。
- **一手来源：** [作者技术报告 PDF](https://mlg.eng.cam.ac.uk/zoubin/papers/CMU-CALD-02-107.pdf)。
- **核心设定：** 在相似度图上沿高密度区域传播 labeled information，给出迭代传播和 harmonic-extension 解释。
- **与候选的重叠：** $W$ 加权邻居传播、图边可信度、一次或多次扩散都属于同一类操作。
- **关键差异：** label propagation 传播样本标签，候选传播的是 selected provider 的 online residual 到 provider-specific state；图节点和时间合同不同。
- **新颖性风险：** 仅把 `W` 乘 residual 称作新算法不够。需要展示 selected-only delayed event、provider state/fallback、安全更新和工具调用任务导致的新约束。

## 4. Algorithm selection / dynamic portfolios：任务问题已有长期传统

### Leyton-Brown et al., IJCAI

- **论文：** *A Portfolio Approach to Algorithm Selection*。
- **一手来源：** [作者公开 PDF](https://www.cs.ubc.ca/~kevinlb/papers/portfolio-IJCAI.pdf)。
- **核心设定：** 不同算法在不同实例上表现不同；用实例特征预测每个算法表现并按实例选算法，比较 single-best 与 per-instance portfolio。
- **与候选的重叠：** “给定输入，在 provider/algorithm portfolio 中选最可靠工具”是同一 algorithm-selection 总问题。
- **关键差异：** 经典工作通常是静态 per-instance selection；候选强调 selected outcome 的实时、延迟反馈和跨 provider 更新。
- **新颖性风险：** 论文应先把静态 selector/Best Fixed 与 dynamic feedback selector 分开。不能把 provider graph 的 fit warm-start 静态预测能力写成实时适应。

### Gagliolo & Schmidhuber, 2007

- **论文：** *Learning Dynamic Algorithm Portfolios*。
- **一手来源：** [IDSIA technical report / author version](https://www.cs.ubc.ca/~hutter/EARG.shtml/earg/papers08/IDSIA-02-07.pdf)。
- **核心设定：** 在线学习算法运行时间分布，不设独立训练阶段，依据问题和算法状态动态分配资源。
- **与候选的重叠：** 动态 portfolio、online outcome feedback、运行时选择和资源成本都已有先例。
- **关键差异：** 原文关注 solver runtime allocation；候选关注 scientific-tool correctness、provider failures、延迟终止和 frozen truth。
- **新颖性风险：** “实时学习的工具选择”本身不是新 claim。创新需绑定药物工具调用中 correctness/error semantics、selected-only delayed release、provider graph 初始化和可审计回放合同。

## 5. 建议的 novelty 防线

1. **换术语并不够。** 明确 graph 是 `fit-derived provider transfer graph`，不是环境 feedback graph；不要声称获得未选 provider 的 counterfactual reward。
2. **必须补近邻 baseline。** 至少加入 `W=I`（Independent）、人工 family、degree-matched shuffled graph；若实现预算允许，增加 graph-Laplacian/CoLin-style adapter 或在 related-work 中给出不可直接适配的理由。
3. **把图构造与在线更新分开消融。** 固定同一 graph 比较 relay/no-relay；固定同一 relay 比较 learned graph/family/random graph；固定同一 update mass 比较不同 degree。
4. **预登记 graph misspecification。** 报告 edge stability、fit→validation pair transfer、反相关/随机图、singleton 与 family 分层；负结果不能用增大 $k$ 或调 $\eta$ 覆盖。
5. **主张范围收窄。** 在没有 model-fresh、多端点或环境漂移证据前，只能说 fixed deployed provider menu 上的 feedback-adaptive selection；不能声称一般 graph bandit、通用 online algorithm selection 或普适 cold-start。

## 6. 证据/判断/建议分开

- **已核验事实：** 图反馈 bandit、协同 contextual bandit、correlated arms、label propagation 和 dynamic algorithm portfolios 都有一手先例。
- **判断：** 候选最容易被认为是“任务特化的 graph-based parameter sharing”，而不是全新 graph learning principle；它是否成立取决于严格的信息边界、预算匹配、强对照和跨折 paired utility。
- **建议：** 先把公式、图来源、可见性、初始化、公平预算和最强近邻补齐，再做 CPU 机制 gate；在 degree-matched random graph 未通过前不要把 graph relay 升为论文主方法。

## Companion reviews and implementation boundary

- 数学一致性审查：`research/provider_graph_relay_math_review_20261004.md`。结论是 Pearson top-k relay 只能先作为 selected-coordinate BCE 的迁移启发式；无条件 correctness correlation 不足以保证 recipient residual alignment，不能写成 Bayesian/PSD posterior。
- 合同实现审查：`research/provider_graph_relay_contract_audit_20261004.md`。它促成了独立 NumPy reference、fit-state index 交叉核验、tie tolerance、fallback exclusion 和 out-degree-matched control 的补强。
- 零机时收据：`experiments/logs/task49_provider_graph_relay_zero_cost_20261004/`。该收据只证明结构合同，不提供效果证据。
