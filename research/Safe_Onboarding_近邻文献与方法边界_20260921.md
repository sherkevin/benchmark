# Safe Incremental Tool Onboarding — 近邻文献与方法边界

> 日期：2026-09-21
> 目的：T3 pilot 暴露 full-pool dilution 后，重新核验“成熟 incumbent + 新工具 onboarding”是否已被经典 bandit 文献直接覆盖。

## 1. Conservative bandit：安全相对 baseline 不是新问题

### NeurIPS 2017 — Conservative Contextual Linear Bandits / CLUCB
Kazerouni et al. 定义安全约束：学习策略的累计表现必须持续不低于 baseline policy 的一个比例。
它已经直接覆盖“线上系统有一个已部署 baseline，探索不能明显伤害它”这个思想。

因此本文不能声称：
- 首次提出 baseline-safe contextual bandit；
- 首次提出 harmful exploration 约束；
- 首次用 UCB 做 conservative switching。

### AAAI 2020 — CLUCB2
进一步改进 conservative exploration，并讨论只在 checkpoint 检查安全约束的现实设定。

### ICLR 2025 — C-SquareCB / C-FastCB
把 conservative contextual bandit 从 linear representation 推到 regression-oracle / nonlinear setting。
因此“我们不是 linear，所以有新意”也站不住。

主 baseline 必须至少覆盖 CLUCB/CLUCB2；若最终方法非线性，应对 C-SquareCB / C-FastCB。
## 2. Warm-start / cold-start：短期探索代价也不是新问题

2023《Cutting to the chase with warm-start contextual bandits》已经：
- 用 prior/pre-training 初始化 LinTS；
- 扩展到 LinUCB / epsilon-greedy；
- 给 regret bound；
- 研究 accurate prior 与 misspecified prior；
- 强调 cold-start early-round performance。

因此本文不能把“用历史知识 warm-start new tool”本身当创新。

AISTATS 2023 MTSS 进一步用 hierarchical model 通过 item features 共享信息，并明确写明适用于 frequently added new items。

所以 shared/hierarchical prior 本身也不是创新。

## 3. Arriving arms：动态工具加入也已有直接近邻

2026 UCB-AA《Multi-Armed Bandits with Arriving Arms》直接研究 available arms 随时间增加：
- new arm preliminary screening；
- dynamic regret；
- elimination / wasted pull reduction。

它是 non-contextual/concurrent work，但“arm arrival”这个问题名词已经被占。

因此我们的区别必须落在 context-dependent competence + transfer + safety / behavioral identification，而不是 arrival 本身。
## 4. 目前还剩下什么可能成立的 gap

把上述文献叠起来后，候选 gap 不是任意一个单点，而是以下交叉：

1. 系统已经有一个 mature contextual incumbent router，而不是从 t=1 同时学习所有 arms；
2. new tool 是运行中动态加入；
3. new tool 没有足够可信的 tool-side competence feature，不能像 MTSS 直接靠 item metadata 给它完整 prior；
4. 工具优势函数是 task-context dependent，而非一个全局均值；
5. 只能通过少量真实 behavioral probes 认识它；
6. 探索必须相对 incumbent 控制 regression；
7. 若有离线 calibration pool，还要选择“测哪些 context”最有信息且风险可控。

因此更精确的问题是：

> **How can a mature agent safely identify the context-dependent advantage region of a newly arriving black-box tool using only a few behavioral probes?**

数学对象由 K-arm ranking 收敛为相对 incumbent 的 advantage function：

`g_n(x) = mu_new(x) - mu_incumbent(x)`

核心是快速、保守地识别 `g_n(x)>0` 的区域。
## 5. 为什么这个 formulation 和我们的数据吻合

### BBB
Best Fixed 很强，直接 argmax contextual routing 会因 harmful switches 输掉收益。
这天然是 conservative gate：默认 incumbent，只有高精度判断 new/alternate 真能救回时才切。

### LLMRouterBench / ASlib
在 function-level subpool K=3/5 时，新 arm 相对 incumbents 有明显边际价值；full pool 反而被 saturation 稀释。

### BEIR SciFact
same-function 但 specialization 很弱，正确 onboarding 应接近 Never-New。
它可以作为 negative control，检查方法是否因为 uncertainty 而无脑探索。

## 6. 对 method 的约束

后续方法如果只是：
- LinUCB 加一个 prior；
- CLUCB 换个名字；
- MTSS 套在工具上；
- UCB-AA 加 context；

都不够。

方法必须实质解决至少一个经典方法没有同时解决的难点，例如：
- 无 tool-side features 时，从旧工具行为学习“函数空间 prior”；
- few-shot behavioral probes 下识别 advantage region；
- information gain 与 safety budget 联合选 probe；
- prior misspecification 时自动退回 incumbent，避免 negative transfer。

最终是否有足够算法增量，要在 canonical baselines 跑完后再裁，不提前承诺 novelty。
## 7. 2026-09-21 补充：Action-Centered CB 与 SEA 比 CLUCB 更贴近我们的 gate

进一步检索后发现两个更直接的近邻，必须进入 baseline / related-work：

### Action-Centered Contextual Bandits — NeurIPS 2017
它把 reward 显式拆成：
- 一个允许非常复杂、甚至 time-varying 的 baseline reward；
- 一个相对 baseline 的较简单 treatment/action effect。

它真正学习的就是 differential reward：`r(action)-r(baseline)`，并通过 action centering 解耦 baseline 与 treatment effect。
这和我们当前 `g_new(x)=mu_new(x)-mu_incumbent(x)` 的数学对象高度相似。

因此：**“学习 new tool 相对 mature incumbent 的 advantage function”本身仍然不能算 novelty。**
ACTS 必须进入核心 baseline；它还是一个很好的 sanity check，因为它专门针对“默认动作/成熟 baseline + 新处理动作”结构。

### Safe Exploration Algorithm (SEA) — TOIS 2020
SEA 直接从 production baseline policy 出发，用 counterfactual learning + high-confidence OPE 判断新策略何时至少不差于 baseline，再允许探索。
它与本文“上线新工具不能伤害 mature system”的 deployment motivation 高度一致。

因此当前真正可能剩下的增量进一步缩窄为：
> **zero-history black-box tool arrival + cross-tool transfer / few-shot behavioral identification + active calibration，在 conservative/action-centered baseline 之上取得更快 adaptation。**

后续若 Ours 不能显著超过 ACTS / conservative baselines 的 few-shot sample efficiency，则当前方法创新不足，不能靠换标题回避。## 8. 2026-09-21 补充：ParetoBandit 已直接覆盖 runtime newcomer onboarding

进一步检索到 **ParetoBandit: Budget-Paced Adaptive Routing for Non-Stationary LLM Serving**（arXiv:2604.00136，2026）。

它与本项目的重叠比普通 fixed-pool router 更直接：
- contextual multi-model routing；
- model registry 可在 runtime hot-swap；
- 新加入 model 可以无历史冷启动；
- newcomer 先经历短 forced-exploration burn-in，再进入 UCB selection；
- sufficient statistics 可从 offline priors bootstrap；
- 实验专门报告 newcomer 的 cold-start onboarding，约 142 steps 达到 meaningful adoption；
- 同时包含 price/quality drift 与预算约束。

来源：https://arxiv.org/abs/2604.00136

因此以下 claim 也必须删除：
- “首次在运行时 onboarding 新 model/tool”；
- “首次对 newcomer 做短期探索后进入 exploitation”；
- “首次用 offline prior warm-start dynamic provider”。

ParetoBandit 当前仍与本文候选问题有差别：它主要是 forced burn-in + UCB 下的系统级 onboarding，并没有把主问题定义为 mature incumbent 下、**稀疏 context-specific positive residual region** 的 few-shot 高精度识别，也没有把 cross-old-tool transfer + 极小 behavioral budget 作为核心方法对象。

所以它应至少作为 concurrent nearest work；formal T3 还应加入一个 ParetoBandit-style comparator（相同 frozen-incumbent/challenger 协议下的 brief forced burn-in + UCB/prior warm-start）。如果这个简单方案已能在 B 很小时获得正 IOU，则我们的方法创新门槛必须相应提高。
