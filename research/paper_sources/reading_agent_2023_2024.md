# AAMAS 2023–2024 原文级补读：online learning / bandit / agent learning

- **阅读人**：`/root/paper_sample_2023_2024`
- **日期**：2026-09-28
- **目的**：补充三份投稿前验收标准的论文证据。本文档只记录我实际下载并逐页核对的原文事实，不把这些论文包装成对 AAMAS 全部优秀论文的统计结论，也不修改三份生效评价标准。
- **页码约定**：下面的“p.”是论文 PDF 中印刷的主文页码；arXiv 版本若与 proceedings 排版不同，另注明 PDF 页码。方法和数字只在列出的页码范围内使用。

## 1. 来源、原文和校验

| ID | 论文 / AAMAS 状态 | 原文来源 | 本地阅读文件 | SHA-256 |
|---|---|---|---|---|
| A23-TR | Mingfei Sun, Sam Devlin, Jacob Beck, Katja Hofmann, Shimon Whiteson. *Trust Region Bounds for Decentralized PPO Under Non-stationarity*. AAMAS 2023 Main Track / Full Research, pp. 5–13（官方目录与 PDF） | [官方 proceedings PDF](https://www.ifaamas.org/Proceedings/aamas2023/pdfs/p5.pdf)；[arXiv:2202.00082](https://arxiv.org/abs/2202.00082) | `/tmp/aamas_read/trust_p5.pdf`（arXiv PDF，16-page manuscript） | `2c65bb2b90156772a9b6e960f495ed9c4bb45eaa1b120e769985b72e0f46c352` |
| A24-OS | Yongxin Xu, Shangshang Wang, Hengquan Guo, Xin Liu, Ziyu Shao. *Learning to Schedule Online Tasks with Bandit Feedback*. AAMAS 2024 Main Track / Full Research, pp. 1975–1982 | [官方 proceedings PDF](https://www.ifaamas.org/Proceedings/aamas2024/pdfs/p1975.pdf) | `research/paper_sources/aamas_related/aamas2024_online_scheduling.pdf` | `88bbb1e974b3f4954a6b9e81cb1d9ae847c9053af35f9505708db1c9bf217f4` |
| A24-PBIM | Grant C. Forbes, Nitish Gupta, Leonardo Villalobos-Arias, Colin M. Potts, Arnav Jhala, David L. Roberts. *Potential-Based Reward Shaping For Intrinsic Motivation*. AAMAS 2024 Main Track / Full Research, pp. 589–598 | [官方 proceedings PDF](https://www.ifaamas.org/Proceedings/aamas2024/pdfs/p589.pdf)；[arXiv:2402.07411](https://arxiv.org/abs/2402.07411) | `/tmp/aamas_read/pbim_arxiv.pdf`（arXiv PDF，11-page manuscript） | `610fcfdae565937d5a23feacf55cd9120788cc1fbf6c1b6425c76adfa7bc3bd7` |

**来源边界。** A24-PBIM 的正文事实来自 arXiv 原文 PDF；官方 proceedings URL 作为出版版入口。A23-TR 的正文事实来自 arXiv 原文 PDF，另用官方 AAMAS PDF URL 核对了出版信息。两篇均不是我根据摘要或二手博客复述。A24-OS 的正文来自官方 proceedings PDF。PDF 不纳入论文代码发布包；如对外发布，需重新核对作者/会议许可。

## 2. AAMAS 2023：Trust Region Bounds for Decentralized PPO Under Non-stationarity

### 2.1 问题切口和真实技术困难

作者研究 cooperative MARL 中的 decentralized policy optimization。核心现实条件是：IPPO/MAPPO 用 independent probability ratios 更新各 agent，而其他 agent 同时更新会改变某个 agent 看到的 transition dynamics；因此从单个 agent 的角度，训练环境是 non-stationary。直接套用单智能体 PPO/TRPO 的 monotonic improvement 分析会失效（A23-TR pp. 1–3）。

论文没有把“换一个 PPO 实现”当作问题。它先指出：joint ratios 能继承单智能体分析，但 independent ratios 会产生由其他 agent 更新造成的 exogenous transition shift（Definition 3.1, Proposition 3.2，A23-TR p. 3）；然后把所有 decentralized policies 的变化放进 centralized trust-region 约束。

### 2.2 方法、理论和算法对应关系

- Dec-MDP 形式化为 agent 集合、局部状态/动作、联合状态/动作、transition kernel、reward、初始分布和 discount（A23-TR p. 2）。
- 论文先给 joint-ratio PPO（JR-PPO）作为可直接套用的对照（Eq. 7–8，p. 3）。
- 对 independent ratios，定义 decentralized advantage，并给出新的 monotonic improvement lower bound（Theorem 3.7，A23-TR p. 4）。该 bound 依赖各 agent policy 的 total-variation 变化之和，而不是单独一个 agent 的 ratio。
- Proposition 3.8 把 independent-ratio clipping 与 joint trust-region 约束联系起来：clip 范围需要随 agent 数量和训练轮次调整（p. 4）。论文的实际结论是，clipping 不是凭经验加的稳定化技巧，而是用来近似控制中心化 trust region 的可计算机制。

**理论边界。** Theorem 3.7 的推导建立在其 Dec-MDP、advantage 和 trust-region 假设上；论文没有声称对任意 MARL 或任意非平稳过程自动成立。作者还指出 joint-ratio 方法在 agent 数量多时需要 joint advantage，计算会变得困难（p. 4）。

### 2.3 实验设计、baseline 和结果证据

- 环境：StarCraft Multi-Agent Challenge（SMAC），覆盖 2s3z、3s5z、1c3s5z、10m_vs_11m、27m_vs_30m、bane_vs_bane 等不同难度和 agent 数量的地图（A23-TR pp. 5–8）。
- 实验问题分三层：
  1. 不同 clipping 值和优化 epoch 下，independent ratio 是否真的限制 ratio/trust region（p. 5，Fig. 1）；
  2. 各地图上的 empirical centralized TV 与 return 是否随 agent 数量、clipping 变化（pp. 5–7，Figs. 2–5）；
  3. IPPO 与 MAPPO 在不同地图上的性能是否相近，以及 joint/independent clipping 的差异（pp. 6–8，Figs. 3–6）。
- 主要比较对象：no clipping、independent-ratio clipping 的不同 $ε$，joint-ratio clipping，以及 IPPO/MAPPO 变体；论文将现有 IPPO/MAPPO 作为现实算法对象，而不是只与一个自制弱 baseline 比较（pp. 1–2, 5–8）。
- 关键现象：$ε=0.1$ 在多张 SMAC 地图上更有效地控制 centralized TV；$ε$ 过大时容易快速落入局部最优；$ε$ 过小时更新变慢（pp. 5–8）。论文明确指出 clipping range 对 agent 数量敏感，不能不报告这个边界。

### 2.4 限制和对本项目的具体影响

- **可迁移部分**：这是一篇“先给出现有方法在现实约束下为何失效，再给只针对该失效的 formal mechanism，再让 theorem 和实验测同一个量”的范例。对我们的 dynamic JEV，不能只写“实时 bandit + 新模块”；需要先构造 selected-only、延迟/缺失反馈或 provider pool 变化下现有更新为何失效的反例或可复核诊断，再提出更新机制。
- **不可直接迁移部分**：它研究的是 cooperative MARL policy optimization，不是 tool/provider selection；TV trust region 和 PPO clipping 不能直接当作我们的算法或 baseline。
- **评价标准影响**：
  - 故事线：把“现实约束导致旧方法的明确失效”作为创新必要性，而不是把复杂模块数量作为创新。
  - 方法论：要求 formal object、更新机制和 theorem/可计算 guarantee 对齐；任何剪裁、门控或缓存都必须能解释为控制某个可测风险量。
  - benchmark+baseline：必须测机制所控制的量（例如 delayed feedback 下的决策损失、recovery time、更新开销），不能只报最终 accuracy。
  - 实验：必须包含压力条件/边界条件（变化的延迟、候选池规模、失效 provider），并保留不占优的设置。

## 3. AAMAS 2024：Learning to Schedule Online Tasks with Bandit Feedback

### 3.1 问题切口和形式化

作者研究 online task scheduling：每轮到达一个任务类型 $S_t$，可选决策集合为 $A(S_t)$；选择决策后，只有该 decision 的 reward $R_{S_t,a_t}$ 和 cost $C_{S_t,a_t}$ 在任务完成时揭示，因此是 selected-action bandit feedback（A24-OS pp. 1–3）。任务 arrival distribution 也未知，不能预先假定 Poisson 或使用先验。

目标是最大化一系列任务的 cumulative reward-to-cost ratio（Eq. 1，A24-OS p. 3）。作者强调，如果只对每个任务选择局部 reward/cost ratio 最大的 action，稍微改变任务到达分布就可能得到 arbitrarily suboptimal 的整体策略（toy example, Fig. 1 and Table 1，pp. 1–2）。

### 3.2 方法和理论

DOL-RM（double-optimistic learning based Robbins–Monro）由两个耦合模块组成（A24-OS pp. 2, 4）：

1. **Double-optimistic learning**：对 reward 做 optimistic estimate，对 cost 做 pessimistic estimate，构造 optimistic reward-to-cost score（Eqs. 2–3）。
2. **Robbins–Monro decision**：维护平衡变量 $θ_t$，按 estimated reward $-θ_t$ estimated cost 选 action（Algorithm 1, Eq. 7），并用 stochastic approximation 隐式学习 task arrival 分布。

假设 reward 是有界区间内的 sub-Gaussian 变量、cost 是正的有界 sub-Gaussian 变量（Assumptions 1–2，A24-OS p. 5）。在这些假设下，Theorem 4.1 给出 DOL-RM 的 regret $R(T)=O(T^{3/4})$（p. 5）；证明把 reward/cost optimistic estimation error 和 Robbins–Monro iteration error 分开控制，并用 Lyapunov drift（pp. 5–6）。

### 3.3 实验设计、baseline 和事实边界

- **Synthetic**：两任务类型（两种 arrival probabilities）和七任务类型；reward/cost 观测加 Gaussian noise；比较 DOL-RM、Thompson Sampling、classical UCB、variance-aware $ω$-UCB 和 full-information Oracle（A24-OS pp. 7–8）。Oracle 使用决策前已知的 reward/cost/optimal solution，只做 privileged upper-bound；作者明确说明它不能用于真实场景（p. 7 footnote）。
- **Real-world application**：共享服务器上调度五类图像分类训练任务；每种任务选择训练 epoch 数，训练完成后用测试 AUC 作为 reward、训练 time 作为 cost。五类任务及 epoch choices 写在 p. 8；使用 satellite/weather/rice/natural-scene/cat-dog 数据。实验在 RTX 3080 Ti、Ubuntu 18.04 上运行；arrival probabilities 设为 `{0.1, 0.4, 0.2, 0.2, 0.1}`（p. 8）。
- 真实实验中因 $ω$-UCB 和 Oracle 需要 non-causal information，只比较 DOL-RM、UCB、TS；图 5 报告 cumulative reward-to-cost ratio 曲线和标准差（p. 8）。
- 论文把“bandit feedback”落实成任务完成后才反馈；不是使用事先生成的完整 panel 结果（pp. 2–3）。

### 3.4 限制和对本项目的具体影响

- **不可直接声称的内容**：DOL-RM 的 selected-only feedback、延迟完成和未知 arrival distribution 与我们接近，但它不是 dynamic provider/JEV 方法，也没有新工具版本到达、旧工具失效、non-stationary provider correctness 或科学真值核验。
- **可复用结构**：
  - 先用一个小反例说明 greedy/local score 为什么整体失败；
  - 把两个相互耦合的不确定因素分别建模、分别分析，再在一个算法里组合；
  - baseline 分成可在线运行的 UCB/TS 与 privileged Oracle，不把 Oracle 混进公平在线排名；
  - 真实实验按实际反馈到达时刻运行，而不是离线把未选结果写入更新。
- **评价标准影响**：
  - 方法论评价需要列出 action、feedback、completion/delay、更新时点和信息边界；
  - benchmark+baseline 评价必须明确每个 baseline 能看到什么，以及 Oracle 为什么只作上界；
  - 实验要有 synthetic pressure case + real tool replay，不能只在静态表格上跑一遍。

## 4. AAMAS 2024：Potential-Based Reward Shaping For Intrinsic Motivation

### 4.1 问题切口和方法

作者关注 intrinsic motivation（IM）reward 可能改变原环境的 optimal policy，导致 agent 被“reward hacking”或探索奖励吸引。传统 PBRS 的形式通常只覆盖较局部的 state-transition reward，而许多 IM 函数依赖更广泛变量或完整 trajectory（A24-PBIM pp. 1–3）。

论文的切口不是“再加一个 reward module”，而是：能否把任意（在假设下）IM reward 转为 potential-based 形式，同时保持底层环境的 optimal-policy set 不变。

- Theorem 1 给出 arbitrary-variable potential 需要满足的 sufficient boundary condition（Eq. 22, A24-PBIM p. 4）。
- 在 Assumption 1 下，作者给出 PBIM 的 potential/reward conversion（Eqs. 30, 34），并证明 Theorem 2：加入 PBIM shaping reward 不改变 optimal-policy set（A24-PBIM pp. 5–6）。Assumption 1 的关键是当前 shaping term 对未来 action 不依赖；作者明确说依赖 future actions 的 IM 不在本论文覆盖范围内（p. 5）。
- Normalized 与 non-normalized 两个版本不是随意的超参变体：normalized 版本用于降低短期 bias，同时保留 policy-preservation guarantee（pp. 5–6）。

### 4.2 实验、baseline 和限制

- **MiniGrid DoorKey 8×8**：tabular exploration reward；对比 PBIM、baseline IM（未做 policy-preserving conversion）和 no IM。运行 16 个并行 process、累计 10 million steps；报告 episode length、convergence time、标准差，并对收敛结果做 one-sided t-test（A24-PBIM pp. 6–7）。
- **Cliff Walking**：比较 no IM、RND、non-normalized PBIM 和 normalized PBIM；主实验聚合 10 runs，报告 average return 和 episode length。RND 与 non-normalized PBIM 在该设置下不能稳定到达目标，normalized PBIM 达到与 no-IM 相同的平均外在回报（pp. 7–8）。
- **Longer Cliff Walking**：4×50 更长稀疏奖励环境，继续比较 return；作者明确说在给定预算内多个 agent 未稳定收敛，结果只是 preliminary evidence，需要未来工作进一步刻画条件与边界（p. 8）。
- **限制**：Assumption 1 排除了 shaping reward 依赖未来 action 的一类 IM；方法不保证“所有 reward 都能安全转换”。论文明确保留该边界，而没有将有限环境中的结果泛化成普遍结论（pp. 5, 8）。

### 4.3 对本项目的具体影响

- **可迁移结构**：一个好的实时 JEV 方法应有一个可检查的不变量，例如“在允许的信息边界内，每次更新只改变与已到达 selected feedback 相容的状态”；如果这个不变量可证明、可从日志恢复，再谈性能改进。
- **不要把效率说成保证**：PBIM 同时给出 policy-preservation theorem 和实验加速，但仍保留 worst-case setting；我们也应把“实时”定义成 per-event update cost、memory、recovery semantics，而不是只说“在线 fine-tune”。
- **实验必须主动证明边界**：除平均正确率外，至少做 feedback delay/noise、provider arrival/retirement 和 cold-start 条件；显示方法在哪些条件下失效，才支持严谨 claim。
- **评价标准影响**：故事线必须明确要保护/改善的对象；方法论必须写出 formal invariant 或可计算保证及其假设；benchmark+baseline 必须包含 no-update/fixed、最近邻 online 方法、privileged Oracle 和压力场景；限制表应和 theorem 的 assumptions 一一对应。

## 5. 三篇论文共同显示的可核查规律（不是统计定律）

1. **先把失效写清，再提出机制。**
   - A23-TR：independent ratios 造成 non-stationary transition，旧 monotonic analysis 不再适用。
   - A24-OS：局部 reward/cost ratio greedy 会被 arrival distribution 反例击败。
   - A24-PBIM：普通 IM 改变 optimal policy，产生 reward hacking。

2. **方法和困难是一一对应的。**
   - A23-TR 用 agent-count-aware clipping 控制 centralized trust region；
   - A24-OS 用 double optimism + Robbins–Monro 分别处理 unknown outcome 和 unknown arrival；
   - A24-PBIM 用 boundary-preserving potential conversion 保护 optimal-policy set。
   “加入 bandit/缓存/模块”本身不是创新，必须说明它消除了哪个失效源。

3. **理论指标、实验指标和 baseline 角色分开。**
   - 三篇都把 privileged/oracle 与可运行 baseline 区分开；
   - 方法声称控制的量在实验中直接测（TV trust-region、regret/reward-cost ratio、policy preservation/convergence）；
   - 负面设置、统计波动或未收敛结果没有被删掉。

4. **限制必须与假设绑定。**
   - A23-TR 的保证依赖 Dec-MDP / advantage / trust-region 条件；
   - A24-OS 的 $O(T^{3/4})$ 依赖 bounded sub-Gaussian reward/cost 与其 arrival model；
   - A24-PBIM 明确排除 future-action-dependent reward。
   这些是论文事实；不能把三篇文章概括成“优秀论文一定有定理/一定有 real-world benchmark”。

## 6. 对三份验收文档的证据化建议（不自动修改生效版本）

### 故事线评价标准

- 继续保留“具体可复核失败 → 对应机制 → 可证伪主张”的硬门槛。
- 将 `最近邻失效证据` 写成必填证据：至少给出一个最近方法在本任务信息边界或动态条件下的明确缺口/失败构造，不能只写“我们更适合”。
- 要求限制段把每条强 claim 限定在对应 assumption 和实验覆盖范围内。

### 方法论评价标准

- 要求每个新增模块有“受控量”：trust-region 类方法控制某种 divergence；我们的 JEV 方法必须定义和控制可计算的 stale/delay/recovery 或 selection-risk 量。
- 理论推导与在线算法必须同一信息合同；不能理论上读 future/full-panel，代码里却声称 selected-only。
- 若无法得到 regret theorem，至少提供日志可恢复的不变量、每事件更新复杂度和明确失败边界；不要把普通 LinUCB/TS 换名写成创新。

### Benchmark + Baseline 评价标准

- baseline 合同应把 Oracle/privileged control 与可在线执行 baseline 分栏；A24-OS 的真实实验只比较可因果的 UCB/TS，是直接先例。
- 评估套件至少含 fixed/no-update、最近的 selected-feedback online baseline、最强可复用 router/representation baseline、Oracle 上界和压力/故障场景。
- 记录真实反馈到达时刻、未选工具不更新、provider/version 事件和失败，不允许从预生成 full-panel 结果偷偷泄漏。

## 7. 事实边界与未完成工作

- 这三篇是定向近邻/质量参照，不是对 AAMAS 2023–2024 全部 Main Track 论文的系统抽样，也不能推出录用概率。
- A23-TR 与 A24-PBIM 是 MARL/RL 方法论文，不能直接当成药物工具选择 baseline；它们提供的是“困难—机制—理论—实验”结构证据。
- A24-OS 是最接近 online selected-feedback 的方法，但其 reward/cost 和 arrival assumptions 不等于医药工具 correctness、provider version drift 或 delayed verification；需要在我们的 benchmark 上重新实现并公平对齐。
- 本文件没有修改三份生效评价标准。若要据此升级标准，需另做 v003 候选，逐条经过双方确认。
