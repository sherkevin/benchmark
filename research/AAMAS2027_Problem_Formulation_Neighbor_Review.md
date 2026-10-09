# AAMAS2027 Problem Formulation Neighbor Review

> 状态：task32 working review
> 目的：确定 Selective Challenger Onboarding 的真实 novelty boundary，避免用已有概念换名。

## 1. 先纠正一个过强叙事

“现有 contextual bandit 只优化 cumulative reward，而我们做 region identification”不能作为核心 novelty 论据。

原因：bandit literature 已明确存在 pure exploration / best-policy identification / thresholding objectives。例如：

- Du, Huang, Sun, ICML 2023, *Multi-task Representation Learning for Pure Exploration in Linear Bandits*: contextual linear bandit 下直接研究 best policy identification，并用 shared representation 降 sample complexity. https://proceedings.mlr.press/v202/du23b.html
- Locatelli et al., ICML 2016, *An optimal algorithm for the Thresholding Bandit Problem*: 目标就是识别均值高于阈值的 arm 集合. https://proceedings.mlr.press/v48/locatelli16.html
- Cho et al., AISTATS 2025, *Good Arm Identification*: 明确讨论 reward maximization 与 pure exploration 的冲突，并识别超过 threshold 的 good arms. https://proceedings.mlr.press/v258/cho25a.html

因此可保留的说法只能是：**我们的 feedback asymmetry + passive contextual region + mature baseline + zero-history challenger 的组合，与标准 regret / pure-exploration setting 不同。**

## 2. Treatment effect / uplift / individualized policy learning

这是 estimand 上最接近的领域。

设 incumbent=control、challenger=treatment，则

\[
\tau(x)=\mathbb E[Y(1)-Y(0)\mid X=x]
\]

与本项目的 challenger advantage 完全同构。因此“学习 relative advantage / Delta(x)”本身绝不能声称 novelty。

- ACTS (Greenewald et al., NeurIPS 2017) 已在线学习 action-centered differential effect；本仓 Task30 已实作并验证。
- Gao, Liu, Zeng, JMLR 2022 将 individualized treatment rule 与 contextual bandit sequential trial 连接。https://www.jmlr.org/beta/papers/v23/21-0354.html
- uplifted contextual multi-armed bandit 直接把 individual treatment effect 放入动态 bandit objective。https://www.degruyter.com/document/doi/10.1515/jci-2020-0009/html
- Parnas et al., AISTATS 2026 继续研究 conditional treatment effects 与 policy learning。https://proceedings.mlr.press/v300/parnas26a.html

边界：这些工作证明 **relative-effect estimation 与 treat-if-positive policy 已有充分先例**。本项目必须把贡献放在 arrival-time data asymmetry、selected-action-only feedback、sample-efficient selective adoption，而不是 Delta 定义。

## 3. Selective prediction / reject option

Selective classification 研究“只在高置信区域作答”，并显式优化 error--coverage trade-off。

- Gangrade et al., AISTATS 2021 的 one-sided prediction 本质上就是寻找最大 decision set，同时控制 false positives。https://proceedings.mlr.press/v130/gangrade21a.html
- Franc & Prusa, ICML 2019 给出 reject-cost 与 bounded-selective-risk 两种 formulation 的关系。https://proceedings.mlr.press/v97/franc19a.html

这与“高 precision challenger adoption”非常接近，所以**高 precision + selective coverage 本身也不是 novelty**。

关键差异：selective prediction 通常在已有监督数据上学习 selector；是否预测不会改变标签可见性。本项目中 challenger 的 outcome 只有真正选择 challenger 时才进入学习器，拒绝 challenger 会保留 incumbent，但同时丢失 challenger counterfactual label。

因此更准确的关键词不是 ordinary selective classification，而是 **selective decision under action-dependent feedback**。

## 4. Active level-set estimation / thresholding

数学目标也高度接近：给未知函数 f(x)，识别 {x:f(x)>h}。

- Inatsu et al., ICML 2021 将可靠区域识别写成 level-set estimation，并主动选择高价值 evaluation points。https://proceedings.mlr.press/v139/inatsu21a.html
- Ngo et al., AISTATS 2026 继续研究高维 active level-set estimation 的 sample efficiency。https://proceedings.mlr.press/v300/ngo26a.html

把 f(x) 取为 tau(x)、阈值取 0，就得到 challenger positive region。

但经典 active LSE 的 learner 通常可以主动选择要 evaluate 的 x。本仓 frozen protocol 是 **passive context stream**：x_t 外生到达；learner 只能决定在这个 x_t 上是否冒险调用 challenger。因此不能直接把本问题写成普通 active LSE。

最接近的结构是：passive contextual level-set identification + selective label acquisition。

## 5. Conservative bandit / safe policy improvement

“有可靠 baseline，证据不足就 fallback”也已有成熟 literature。

- Kazerouni et al., NeurIPS 2017 CLUCB：要求累计表现以高概率保持在 baseline 的固定比例以上，同时学习 contextual policy. https://proceedings.neurips.cc/paper/2017/hash/bdc4626aa1d1df8e14d80d345b2a442d-Abstract.html
- Laroche et al., ICML 2019 SPIBB：数据不足的 state-action pair 回退 baseline，并给 safe policy improvement guarantee. https://proceedings.mlr.press/v97/laroche19a.html
- Simão et al., AAMAS 2020 进一步处理 baseline policy 本身需要估计的情形。https://www.microsoft.com/en-us/research/publication/safe-policy-improvement-with-an-estimated-baseline-policy/

所以“保守”“安全回退 incumbent”不能单独作为贡献。

差异：这些工作主要约束 aggregate policy value / cumulative safety，或者在离线 MDP 中限制偏离 baseline；本项目要识别 **新 challenger 的 per-context beneficial region**，而新 action 在 arrival 前没有 reward history。

## 6. 当前最小未闭合 formulation

综合以上文献，不能声称新的部分：

- contextual routing；
- relative advantage / CATE；
- threshold / positive-region identification；
- high-precision selective coverage；
- baseline fallback / conservative safety；
- shared-representation transfer；
- newcomer burn-in。

仍然没有被上述任一 formulation 单独覆盖、且与本仓协议一致的组合是：

**A mature frozen incumbent faces a zero-history challenger on a passive context stream. The learner observes only the selected action's reward and must learn a high-precision challenger-adoption region quickly enough that incremental utility over the incumbent becomes positive.**

这可暂命名为 **Selective Contextual Challenger Onboarding (SCCO)**。名称只是工作名，novelty 必须由 formal assumptions / objective / guarantees 或实证机制支撑，不能靠命名成立。

## 7. Apple tasting / one-sided feedback: partial feedback itself is not novel

The closest online-learning analogue to “only learn about a candidate when you accept it” is apple tasting.

- Helmbold, Littlestone, Long, *Apple Tasting*, Information and Computation 2000: an instance is accepted/rejected and the true label is revealed only on acceptance. https://www.sciencedirect.com/science/article/pii/S0890540100928700
- Raman et al., COLT 2024 revisit the model and characterize minimax rates under apple-tasting feedback. https://proceedings.mlr.press/v247/raman24a.html

Therefore passive contexts plus asymmetric label acquisition are not, by themselves, a novelty claim.

However, SCCO does **not** receive the binary benefit label when it selects the challenger. A challenger call reveals only its realized reward Y(1); the counterfactual incumbent reward Y(0) for that same context remains hidden from the learner. The sign label 1{Y(1)>Y(0)} is therefore latent and must be inferred using the mature incumbent model/history. This is weaker feedback than canonical apple tasting.

## 8. Risk-sensitive abstention: “trusted region + cautious exploration” is also covered

Liaw & Plaut, AISTATS 2026, formalize a two-action contextual bandit with abstention. At each context the learner either abstains for fixed reward 0 or commits to a preexisting policy whose reward can be arbitrarily negative; their algorithm learns a trusted region and commits cautiously. https://proceedings.mlr.press/v300/liaw26a.html

This is a very close conceptual neighbor. It means the paper cannot claim novelty from any of the following alone:

- two-action contextual decision;
- abstain/fallback action;
- trusted-region discovery;
- cautious exploration under possible severe harm.

Remaining distinction: SCCO compares an arriving unknown action against an **active, context-dependent incumbent policy with extensive pre-arrival history**, and the latent target is the relative advantage tau(x)=mu_c(x)-mu_0(x). The fallback is not a constant reward and challenger competence has zero direct pre-arrival history.

## 9. Arriving / expanding arms: newcomer arrival is already a bandit topic

Dynamic action sets are established rather than novel.

- Ballooning Multi-Armed Bandits studies arm sets that grow over time and dynamic regret. https://arxiv.org/abs/2001.10055
- Zheng, Xu, Yang (2026), *Multi-Armed Bandits with Arriving Arms*, explicitly studies sequential screening of arriving arms and the arrival-information discrepancy; as of this review it is a 2026 arXiv preprint, so it should be treated as concurrent work rather than peer-reviewed authority. https://arxiv.org/abs/2606.09002
- Oetomo et al., 2023 study warm-start contextual bandits and the early-round cost of cold start. https://link.springer.com/article/10.1007/s10115-023-01861-2

Thus “a new arm arrives with no observations” is not sufficient novelty either.

What remains specific here is the combination of a mature context-dependent incumbent, sparse local challenger advantage, no challenger-side prior/metadata in the main protocol, and baseline-relative selective adoption under bandit feedback.

## 10. Dynamic LLM routing: StageRoute is mandatory related work, but not protocol-equivalent

Li & Li, *Keeping Up with the Models: Online Deployment and Routing of LLMs at Scale* (ICLR 2026) explicitly models LLMs arriving over time, stage-wise deployment, and per-query routing under deployment/cost/capacity constraints. https://openreview.net/pdf?id=JJ3cwtUxzo

It is closer to our application narrative than ordinary static routing and must appear in Related Work.

Its formal reward model is primarily model-level (unknown mean performance per model) coupled to deployment and cost constraints; its main difficulty is selecting a changing active model set and routing under budgets. Our frozen setting instead removes deployment-capacity optimization and isolates context-specific **relative challenger advantage against a frozen incumbent policy**. Therefore StageRoute is not protocol-equivalent to the current binary onboarding benchmark, but it prevents us from claiming that “dynamic LLM/model arrival” itself is new.

## 11. Revised novelty boundary after the close-neighbor audit

After adding apple tasting, risk-sensitive abstention, warm-start/arriving-arm bandits, and StageRoute, a new-problem claim would be too strong if stated as “contextual bandits for newly arriving tools.” Generic contextual bandits already subsume that at a high level.

The defensible research object is narrower:

> **baseline-asymmetric, selective policy improvement for an arriving action**: a mature frozen context-dependent incumbent is well estimated from history; a challenger has zero direct history; contexts arrive passively; only the executed action's reward is observed; the challenger-benefit label is never directly revealed; and the objective is to acquire enough challenger evidence to obtain positive incremental value with high beneficial-adoption precision.

This should be presented as a **structured special case / deployment regime**, not as an entirely new class of online learning unless Task32 later proves a genuinely distinct lower bound or identifies a new assumption/guarantee.

The likely algorithmic opportunity is therefore not another name for the setting. It must exploit structure that the nearest baselines do not: e.g. baseline-relative residual structure, calibrated evidence acquisition, or another assumption that is empirically validated before algorithm design.

## 12. Stronger reduction: known incumbent value turns SCCO into abstention-on-residuals

There is an important reduction that further narrows the novelty boundary.

If the incumbent conditional mean $\mu_0(x)$ were known exactly, define the challenger residual reward

\[
R_c^{rel}(x)=Y(1)-\mu_0(x).
\]

Choosing the incumbent is then equivalent, in expected baseline-relative value, to an abstain action with reward 0; choosing the challenger is equivalent to committing and observing a noisy reward with mean $\tau_c(x)$. Under this normalization, the two-action decision is extremely close to Liaw & Plaut's risk-sensitive abstention model.

The remaining gap is therefore not fallback vs challenger. It is that our fallback value is learned from a mature context-dependent incumbent and is not known perfectly. In the actual online feedback, after a challenger call we observe $Y(1)$ but not $Y(0)$; a residual learner can only subtract an incumbent value estimate $\hat\mu_0(x)$ unless the incumbent is also executed.

This yields an explicit decomposition:

\[
\hat R_c^{rel}(x)=Y(1)-\hat\mu_0(x)
=\tau_c(x)+\epsilon_c(x)+\{\mu_0(x)-\hat\mu_0(x)\}.
\]

Thus baseline-estimation error enters directly into challenger evidence. Any eventual theoretical claim must account for this nuisance error rather than treating the incumbent as a free zero-reward action.

The official AISTATS 2026 simulation code was cloned for fidelity audit at .srcprobe/abstention-bandits-simulations, HEAD 69994acc6679bdc368a26ed9d2612aac67db4fee. Its main algorithm discretizes context space, commits by default inside a trusted region, and abstains after a bin is certified negative by a Lipschitz confidence bound. This is sufficiently close that it must be audited before Ours is designed.

## 13. Train-only falsification of the first two structural hypotheses

Two diagnostics were run without touching the frozen formal test partitions.

### H1: relative-residual transfer

Artifact: data/tool_results/divergence/t32_residual_transfer_trainonly.json.

At challenger observation budgets 4/8/16/32, a rank-8 representation learned from historical baseline-relative residual functions was compared with raw-context and absolute shared-tool representations.

Result: **H1 is not supported as a cross-benchmark core assumption.** Relative-shared representation never produced positive IOU; on LLMRouterBench its average-precision and precision were generally below the raw/absolute alternatives; on ASlib the absolute-shared representation was at least as strong; on BBB the relative version only gave a small budget-8 precision/harm improvement and was worse again at larger budgets.

Therefore the paper must not be built around the claim that relative residual geometry transfers better than absolute tool geometry.

### H2: challenger-independent context receptivity

Artifact: data/tool_results/divergence/t32_context_receptivity_trainonly.json.

A train-only score predicted how often historical pseudo-challengers improved over their matched incumbents at similar contexts. It was compared with simple incumbent weakness.

Result: **the simple H2 is also not robust.** ASlib shows meaningful binary enrichment (top-4 precision 0.379 vs prevalence 0.114, about 3.82x lift), but the same probe set has strongly negative mean residual value because harmful misses are large. LLMRouterBench shows no reliable precision lift at the smallest budgets; BBB is essentially uninformative at top-4/top-8.

This is an important negative result: binary opportunity likelihood is insufficient. Probe/adoption decisions must be magnitude-aware, not merely probability-of-positive aware.

## 14. Consequence for the project sequence

Task32 cannot legitimately hand off to Ours yet. The close-neighbor audit discovered a missing 2026 comparator/formulation family — risk-sensitive abstention — after Task31 freeze. Under the project rule, this is handled by a **new amendment task**, not by silently editing the frozen baseline protocol.

The next required action is to audit whether Liaw & Plaut can be faithfully adapted under the frozen manager contexts and estimated-incumbent residual feedback. Only after that closure should the project decide whether there remains an algorithmic gap large enough for Ours.

## 2026-09-22 — 历史信息隔离与文献核验更正

上文 H1/H2 的 r1 数字不是合格的 held-out-tool 证据：仅删除真实 challenger j 的直接列，未从其他 pseudo-tool 的 incumbent 中删除 j。原因、修复前封存与新版完整结果分别见 `task/task32-AAMAS2027-problem-formulation/history_isolation_audit.md`、`r1_artifact_snapshot.json`、`history_isolation_results.md`。r1 保留作历史，不继续用于论证。

有效诊断为 `data/tool_results/divergence/t32_history_isolation_r2_serial.json`。修复后仅能判定本次 rank-8 线性构造缺乏跨 benchmark 稳定优势，不能否定整个 residual-transfer 方法族。BBB receptivity 的 top-4/top-8 有正 probe IOU；这两个例外必须保留，不能继续沿用“BBB 小预算完全无信号”的表述。排序不是 passive-stream 策略，few-shot heldout evaluation 不含 probe 损失。

本轮重新打开并核对的四篇一手来源如下；核验层级是 proceedings metadata + abstract，不冒充完整算法/定理/官方代码 fidelity audit。

| 论文 | 核验得到的范围 | 一手出处 |
|---|---|---|
| Liaw & Plaut, AISTATS 2026, Learning When Not to Learn | 两行动、固定零奖励 abstain；commit 奖励上有界但负向可无界；Lipschitz 与 iid 条件，不能省略 | https://proceedings.mlr.press/v300/liaw26a.html |
| Du, Huang & Sun, ICML 2023, Multi-task Representation Learning for Pure Exploration in Linear Bandits | 同时覆盖 linear best-arm 与 contextual best-policy identification；依赖共同低维表示 | https://proceedings.mlr.press/v202/du23b.html |
| Raman, Subedi, Raman & Tewari, COLT 2024, Apple Tasting | 只有预测 1 才看到二元真标签；不等同于执行 challenger 后直接看到相对增益符号 | https://proceedings.mlr.press/v247/raman24a.html |
| Gangrade, Kag & Saligrama, AISTATS 2021, Selective Classification via One-Sided Prediction | 以较少 false positives 扩大 decision sets，说明 accuracy/coverage 不是新的目标类别 | https://proceedings.mlr.press/v130/gangrade21a.html |

其他文献沿用其原记录的核验级别，本轮未全量重新验证。反事实 estimand 与 realized-matrix oracle 的区别见 Task32 `estimand_audit.md`；这些澄清不改变 Task31 计分协议。
