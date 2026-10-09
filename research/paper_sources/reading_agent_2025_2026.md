# AAMAS 2025–2026 近邻论文原文复读记录

日期：2026-09-29
阅读目的：补足“我们是否真的读过相关论文”的证据。本文只记录原文中的可核查事实，不修改三份生效评价标准，也不把下述规律冒充 AAMAS 官方评分规则。

## 阅读材料和版本

| 编号 | 论文 | 原文来源 | 本地阅读件 | SHA-256 |
|---|---|---|---|---|
| A25-1 | Yiting Hu, Lingjie Duan. *Truthful Mechanisms for Linear Bandit Games with Private Contexts*. AAMAS 2025, pp.1006–1014 | [AAMAS 2025 official PDF](https://www.ifaamas.org/Proceedings/aamas2025/pdfs/p1006.pdf) | `research/paper_sources/aamas_related/aamas2025_official_p1006.pdf` | `1580250e86c50cb793abcf2fe927fde152420a9c4434a593675803fbea9749b3` |
| A25-2 | Marc Lanctot et al. *Soft Condorcet Optimization for Ranking of General Agents*. AAMAS 2025, pp.1253–1262 | [AAMAS proceedings record](https://www.ifaamas.org/Proceedings/aamas2025/pdfs/p1253.pdf); [arXiv full text](https://arxiv.org/abs/2411.00119) | `research/paper_sources/aamas_related/aamas2025_soft_condorcet_arxiv.pdf` | `0f72fb4be6a45a7b0914b7438d22d739b358d8e9f6ee6ba00201fa075e2a1b07` |
| A26-1 | Pronoy Patra et al. *Truthful Reverse Auctions for Adaptive Selection via Contextual Multi-Armed Bandits*. AAMAS 2026, pp.578–586 | [AAMAS 2026 proceedings contents](https://ifmas.csc.liv.ac.uk/Proceedings/aamas2026/forms/contents.htm); [arXiv full text](https://arxiv.org/abs/2602.14476) | `research/paper_sources/aamas_related/truthful_reverse_auctions_contextual_mab.pdf` | `a56c878d052ec5fe8370a28cd8b7a30f4b7eee12753eb1eab6e6a364b290d882` |

说明：A25-1 使用官方 proceedings PDF；A25-2 的官方 proceedings PDF 下载端点在本机多次返回不完整内容，因此实验与公式核对使用作者 arXiv v4，同时保留官方页码和官方 URL；A26-1 同样以作者 arXiv 全文核对正文，并用 AAMAS 2026 官方目录核对收录和页码。任何投稿引用仍应以正式 proceedings 版本为准。

---

## A25-1：Truthful Mechanisms for Linear Bandit Games with Private Contexts

### 1. 论文到底解决什么

作者研究的是一个带**策略性私有 context** 的 stochastic contextual linear bandit：每轮来一个一次性 agent，真实 context $x_t$ 只有 agent 知道；agent 可以报告另一个 context，系统按报告选择 arm，随后只观测被选 arm 的 reward。模型定义在 PDF pp.1–2：context 集合是有限集合 $X=\{\chi_1,\ldots,\chi_N\}$，每个 arm $k$ 有未知参数 $\theta_k$，观察到的 reward 为 $r_t=x_t^\top\theta_{a_t}+\eta_t$。

关键困难不是“把 TS 用到 contextual bandit”，而是**普通低 regret 算法在新约束下会诱导 agent 伪报 context**。论文 pp.3–4 给出具体几何例子，并证明普通 Thompson sampling 在最坏情况下可能有 $O(T)$ regret（Lemma 3.1）；确定性 UCB 的可预测选择也会被利用，ETC 和 $\epsilon$-greedy 虽然 truthful，但 regret 更高（pp.2–3）。

### 2. 方法、公式和保证

作者先计算普通 TS 对每个可能报告 context 的 arm probability $p_t(x)$，再在每一轮解一个线性规划，得到新分布 $q_t(x)$（Eq. (6)，p.4）。约束包括：

1. 任意真实 context $\chi_i$ 不会因报告 $\chi_j$ 获得更高期望 reward：$\chi_i^\top\Theta_t(q_t(\chi_i)-q_t(\chi_j))\ge 0$；
2. 保持按 context 到达概率加权后的 arm distribution 不变；
3. 每个 $q_t(x)$ 都是合法概率向量。

论文给出一个按 conflict cluster 求加权平均的可行解（Eq. (7)，p.4），并在可行性条件下分析 regret。两 context、Gaussian prior/noise 下，Theorem 5.1 给出 truthful-TS 的问题依赖 $O(\log T)$ 上界（pp.5–6）；不同最优 arm 的两 context 情形在 Theorem 6.1 分析（pp.6–7）。算法逐轮执行：计算 $p_t$、解 LP、接受报告 context、采样 arm、只用被选 reward 更新 posterior（Mechanism 1，p.4）。

### 3. 实验和边界

- 小规模数值实验：$N=d=K=2,T=50$，相同 Gaussian prior，比较 Gaussian 与 Laplace noise；以“假设 agent 始终 truthful 的理想 TS”作参照（p.7）。
- 扩展实验：$N\in\{3,5,9\}$，$d\in\{5,9\}$，$K=6$；context 从 $[0,1]^d$ 的 multivariate uniform 采样；每个配置 100 次，重新从 prior 采样参数；报告 cumulative regret（pp.7–8）。
- 结论是 truthful-TS 保持次线性趋势，但 context 越多，机制因 conflict 增加而收敛更慢（p.8）。
- 论文并没有动态 arm/provider 池、反馈延迟、selected-only correctness label 或真实工具调用；它的 strategic misreporting 假设也不适用于我们的工具 provider。

### 4. 对本项目的直接影响

- **故事线**：不能把“contextual TS/LinUCB”写成创新；创新必须来自我们确实存在、且普通选择器会失败的现实约束。该论文的写法值得借鉴：先构造失败例子，再把失败条件写进形式模型，最后设计只针对该条件的机制。
- **方法论**：如果我们将拒答、验证或反馈可信度作为机制，必须给出类似“选择前可见信息—选择—反馈到达—更新”的明确时序，并证明普通方法为何不满足该时序；不能只加一个 heuristic gate。
- **Benchmark/Baseline**：ordinary TS、LinUCB、greedy/ETC/epsilon-greedy 必须作为相应的 canonical controls；但不能复用其 truthfulness/regret 结论，因为工具不会策略性伪报 context。

---

## A25-2：Soft Condorcet Optimization for Ranking of General Agents

### 1. 论文到底解决什么

作者面对的是 agent 评测中的**不完整、稀疏、噪声化比较结果**。经典 Elo 或 voting 方法可能把非 Condorcet winner 排在最高；论文把每个 agent 的评分 $\theta_a$ 作为参数，把观测到的 partial ranking/vote 当作 noisy preference data，目标是得到最少 pairwise ranking mistake 的排序（pp.1–2、4）。

### 2. 方法、公式和可计算性

核心 sigmoid loss 是 soft Kendall-tau distance：

$$
\widetilde L([\succ],A,V,\theta)=\sum_{v\in V}\sum_{(i,j)\in I_2(v)}\sigma(\theta_{v[j]}-\theta_{v[i]}),
$$

其中 $\sigma$ 是 temperature 控制的 logistic 平滑（Eq. (5)–(7)，pp.4–5）。作者给出三种优化路径：

1. sigmoid loss 的 stochastic gradient descent，可 batch，也可单个 vote 在线更新（Algorithm 1，p.5）；
2. convex Fenchel–Young loss，通过 Gumbel perturbation 和排序梯度优化（p.5、Appendix B）；
3. 小规模下的 sigmoidal programming/branch-and-bound（p.5、Appendix C）。

Theorem 1（p.5）证明：若存在 Condorcet winner，sigmoid loss 在该候选评分上单调下降，因此受限全局最优会把它排在最高；作者同时明确指出 sigmoid loss 本身非凸、无约束时会趋向无穷，实际依赖 rating box constraint。Fenchel–Young loss 是凸的、有全局收敛保证，但不保证 Condorcet consistency（pp.5、8）。

### 3. 实验设计和限制

- **PrefLib**：12,680 个 preference profiles；$|A|\le 10$ 时可算 Kemeny–Young，比较 SCO 与 Kemeny–Young 的 normalized Kendall-tau distance 和 Condorcet match proportion。每个实例 3 seeds，batch size 32，学习率 $\{0.01,0.1\}$，迭代 $10^4/10^5$（p.6）。结果表明 $|A|\le 500$ 时 gradient descent 对存在 Condorcet winner 的实例通常 92%–100% top-rank，Kemeny 距离不超过 0.043 的组内均值（p.6）。
- **稀疏合成 tournament**：20 agents、4-player contests；uniform 或 skill-matched 参赛；200 个 instances/每个 missingness setting，10,000 iterations，batch 16；指标是到真实 ranking 的 Kendall-tau distance 与 misranked pairs 的 Mean True Rating Distance；报告 95% CI（pp.6–7）。
- **Diplomacy 人类对局**：52,958 players、31,049 games；50 个 train/test splits，训练 28,049、测试 3,000，保证测试 agent 在训练中至少出现一次但 game outcome 不重叠；在线迭代用 held-out test 的 Kendall-tau distance，batch size 32（pp.7–8）。sigmoid 与 Fenchel–Young 都优于 Elo/VasE，但曲线后期出现 overfitting，作者建议 early stopping/annealing/regularization（p.8）。
- 限制：sigmoidal programming 对 $m\ge5$ 常有数值不稳定且涉及 $\Omega(m^2)$ 变量；Fenchel–Young 虽凸却牺牲 Condorcet 性质；作者没有把这种 ranking 更新用于 selected-only delayed tool feedback（pp.6、8）。

### 4. 对本项目的直接影响

- **方法论**：这篇论文给了我们一个很有价值的“动态 JEV 可计算”参考：把每次终止反馈看成一个局部 pairwise/order evidence，定义平滑损失，允许单事件更新，同时保留一个可验证性质。能借的是“目标函数—在线更新—性质—离线/在线验证”这一链条，不能直接声称 JEV 等同 SCO。
- **故事线**：动态性要由真实事件驱动；不能只把静态模型叫作 dynamic。论文的 online form 明确规定单个 outcome 到达后更新 ranking（p.5），我们也必须记录 label 到达时刻和更新前后状态。
- **Benchmark/Baseline**：应同时报告单事件更新、batch 更新、早停/正则化和数值稳定边界；不能只给最终平均准确率。若采用 pairwise provider evidence，必须说明 pairwise 关系来自同一 query 的什么可观测事实，不能凭空把不同 query 的 provider 结果当作偏好票。

---

## A26-1：Truthful Reverse Auctions for Adaptive Selection via Contextual Multi-Armed Bandits

### 1. 论文到底解决什么

作者研究 LLM provider selection：用户面对多个 provider，每个 provider 报告 cost；不同 query 的 provider quality 不同且未知，用户要一边学习 quality，一边选择 cost-aware provider。形式化是 reverse auction + contextual MAB，而不是单纯 router（pp.1–3、5–6）。

每个 provider $i$ 的期望 reward 写成 $v_{i,t}=\theta_i^\top x_t$，成本经过 virtual cost $\Psi_i(b_i)$ 调整。算法要同时满足 provider 的 Bayesian incentive compatibility/ex-post individual rationality 和用户侧 sublinear regret（Definitions 1–2、Eq. (4)，pp.4–6）。

### 2. 方法、理论和实现成本

- 先定义 ex-post monotone allocation：provider 报价越低，累计 allocation 不能减少（Definition 3，p.7）。
- 给出 `REV-BaseLinUCB-S` 与分阶段 `REV-SupLinUCB-S-OPT`，每个 provider 独立估计 $\theta_i$；通过 round-robin 限制 learning，保持 allocation monotonicity（p.7）。
- 设计 reverse self-resampling procedure `ROSA`，对 bids 做随机重采样，再用 generic transformation 给出 payment；Theorem 3 证明在 monotone allocation 下具有 EPIC/EPIR（pp.7–9）。
- Theorem 6 给出 `REV-SupLinUCB-S-OPT` 的 $O(\sqrt{T})$ regret，Corollary 1 将其用于 reverse auction（p.16）。实现用 Sherman–Morrison，使每轮 Gram inverse 更新为 $O(d^2)$（p.17）。

### 3. 实验和边界

- 全部是合成仿真：4 个 strategic providers，5 维 Gaussian context；另测 exponential reward/noise；默认 horizon $T=100K$，扩展到 $10^6$；探索参数 $\alpha=0.75$、regularization $\lambda=1$（p.17）。
- 每个配置 40 个独立 seeds；生成 fresh context/bid，报告 cumulative realized regret、per-round regret 和 actual revenue 相对 clairvoyant benchmark 的曲线；实验在单工作站上完成（pp.17–18）。
- Future work 仍包括 minimum cost thresholds、query token uncertainty、user budget constraints（p.18）。没有 dynamic provider arrival/departure、延迟或 noisy correctness feedback、外部科学工具和真实 benchmark。

### 4. 对本项目的直接影响

- **故事线**：这篇 2026 AAMAS 论文已经覆盖“query-dependent provider selection + contextual online learning”。因此我们不能把“tool/provider selection + contextual bandit”本身当创新；新切口必须是 selected-only delayed binary correctness、provider/version lifecycle 或可恢复实时更新。
- **方法论**：必须把新增现实约束写进形式定义，并证明/验证现有方法在该约束下的具体损失。若没有 strategic cost，本论文的 ROSA、truthfulness 和 reverse-auction payment 不应被移植成装饰性模块。
- **Benchmark/Baseline**：它是最近邻强 baseline/对照的来源。我们至少要实现其适配后的 contextual selection 核心（去掉不适用的 bid/payment 后，清楚注明改动），并保持相同候选池、context、反馈可见性和预算；否则无法说明我们的 JEV 相比最近邻到底解决了什么。

---

## 交叉得到的可复用规律（有原文支撑的范围）

1. **创新来自新增约束造成的可证伪失败**：A25-1 用 misreporting 反例，A25-2 用 Elo 的 Condorcet failure，A26-1 用 strategic cost/truthfulness 约束。仅更换模型名称、把标准 bandit 接到新数据上，都不够。
2. **形式模型必须覆盖方法声称解决的那件事**：A25-1 将 report/selection/reward 写进每轮时序；A25-2 将 noisy partial ranking 写进损失；A26-1 将 bid、allocation、payment 和 history 分开定义。我们的模型若声称“实时”，必须显式定义 feedback arrival、pending queue、update tick、provider lifecycle。
3. **理论/算法性质与实验指标一一对应**：A25-1 的 regret bound 配 regret 曲线；A25-2 的 Condorcet consistency/Kemeny approximation 配 Kendall、match proportion 和 held-out prediction；A26-1 的 EPIC/EPIR/regret 配 regret、revenue、clairvoyant gap。我们的 JEV 至少要让更新成本、反馈延迟下的恢复、正确率/拒答质量能从原始事件日志重算。
4. **边界和负结果必须保留**：A25-2 明确 sigmoidal programming 数值不稳定和 online overfitting，A26-1 明确 token/budget 等未覆盖条件，A25-1 明确多 context 会放大 conflict。我们的 benchmark 不能只展示动态 JEV 赢的设置，必须报告延迟、冷启动、漂移和错误标签下的失败。
5. **强 baseline 不是越多越好，而是回答不同问题**：canonical bandit/control、最近邻方法、matched backbone、privileged oracle 各自承担不同问题；三篇论文都没有用一个“大而全”的 baseline 列表替代公平的机制对照。

## 对三份验收标准的暂时影响（不改生效文档）

| 文档 | 本轮增加的证据约束 |
|---|---|
| 故事线与创新 | H3/S3/S5：写出一个具体失败反例；说明为什么 A26-1 已覆盖的 contextual provider selection 不足；把动态事件和 JEV 的必要性写成可证伪主张。 |
| 方法论 | H1/H2/M4/M5/M7：给出选择前信息边界、反馈到达时序、更新公式/伪代码一致性；若借鉴 SCO 的 pairwise/sigmoid loss，必须证明它对应真实可观测的 provider evidence，并报告计算与数值边界。 |
| Benchmark 与 Baseline | H5/H6/B6/B7/B10：最近邻至少包含 A26-1 的适配控制和 standard LinUCB/TS；所有方法共享 selected-only feedback、delay、candidate lifecycle 和 budget；Oracle/full-panel 只能作 privileged control。 |

这些是“把原文事实映射到现有闸门”的建议，不是对三份生效标准的单方面修订。任何标准升级仍需先讨论并形成新版本。

## 证据边界

- 本文完成了 3 篇 2025–2026 论文的原文级核对，不等于完成 2023–2026 每年 5–10 篇的系统综述。
- A25-2 的官方 PDF 下载失败原因和 arXiv 版本差异已记录；如果最终论文引用页码，提交前必须再对正式 proceedings PDF 做一次页码核对。
- 论文中描述的实验数字只用于理解其设计和作为我们验收标准的证据，不代表这些方法已在我们的医药 benchmark 上复现。
