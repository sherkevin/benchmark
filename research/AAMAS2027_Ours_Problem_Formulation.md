# AAMAS2027 Ours Problem Formulation

> 当前综合说明已收敛到 [医药论文主线与进度](../docs/AAMAS2027_论文主线与进度.md)，当前待办见 [任务清单](../docs/任务清单.md)。本文件继续保留原探索与更正过程，不作为最新完整方法稿；工具 Bandit 不以 GRPO/PPO 为前置。

> **2026-09-24 主线更新：** Owner 将论文对象明确为“动态工具适应模块＋实时学习的动态Jev＋主Agent可靠工具调用能力”。provider选择是动态模块的一个动作；动态Jev还要处理证据验证、复核、作答与拒答，主Agent需要学习这些动作。下文较早的“Bandit选具体工具”表述属于历史切片，按 [ADR-0012](../docs/adr/0012-three-layer-dynamic-jev-agent-loop.md) 与 [Task45设计卡](../task/task45-dynamic-decision-model/dynamic_jev_design.md)解释。

> **2026-09-22 Owner 纠偏：下文通用新工具接入方案不再作为论文主线。** 当前任务是医药科学工具可靠性：主 Agent 通过真实 SFT/RL 学习请求功能、复核、作答与弃答；同功能 Bandit 学习当前输入应选择哪个具体工具。以 Owner 指定的 `/Users/jingwu/Desktop/科学工具可靠性_Agent阶段汇报_v7_20260918.md` §2.5、§2.6、§4.3 及其当日澄清为意图依据。
>
> Owner 同日进一步选择具体模型/版本作为 Bandit 动作，家族只标来源关系，不先合成多数票。Task36 已实现并验收逐模型缓存回放及独立判分，入口见 `task/task36-pharma-provider-replay/report.json`；这是训练基础设施，不是已训练 Agent 或已冻结的方法贡献。
>
> Task31–35 的代码和结果原样保留作历史诊断，不能用 ASlib 的固定试用损失决定医药论文的目标，也不能据此把主 Agent 的参数训练降为可选附录。新工具冷启动保留为泛化验证支线。此前「净收益优先」是在助手误设的通用接入框架内提出的局部选择，不构成 Owner 批准该框架作为论文任务。
>
> 以下 Status/Scope 和各节均为当时的探索记录；不删除历史结果，不自动改写旧 Accepted ADR，也不据此启动下一批实验。

> Status: Task32 formulation candidate v2 — algorithm not yet frozen
> Scope: formalize the deployment regime after Task31; do not claim the generic problem class is new.

## 1. Corrected positioning

The earlier sentence “existing methods optimize cumulative reward, while we identify a region” is too strong. Pure-exploration contextual bandits, thresholding/good-arm identification, heterogeneous treatment-effect policy learning, selective prediction, apple tasting, conservative bandits, and active level-set estimation already cover individual pieces of that story.

The paper should instead study a **structured deployment regime inside contextual decision making**:

**a mature frozen incumbent policy faces an arriving zero-history challenger, and the system must selectively learn where the challenger improves on that incumbent from action-dependent feedback.**

The working name remains *Selective Contextual Challenger Onboarding (SCCO)*. The name is bookkeeping, not a novelty claim.

## 2. Environment

Let contexts arrive exogenously:

$$X_t \in \mathcal X,\qquad t=1,\ldots,T.$$

The main frozen protocol is passive-context: the learner does not choose $X_t$.

There are two deployment actions:

$$A_t\in\{0,1\},$$

where $A_t=0$ executes the frozen incumbent policy $\pi_0$ and $A_t=1$ executes the arriving challenger $c$.

For each context define potential rewards

$$Y_t(0),Y_t(1)\in[0,1],$$

with conditional means

$$\mu_0(x)=\mathbb E[Y(0)\mid X=x],\qquad
\mu_c(x)=\mathbb E[Y(1)\mid X=x].$$

The baseline-relative advantage is

$$\tau_c(x)=\mu_c(x)-\mu_0(x).$$

Task31's deterministic benchmark replay is the special case in which the per-instance potential rewards are already fixed by the offline reward matrix.

## 3. Arrival asymmetry

Before challenger arrival, the system owns a substantial incumbent/history dataset

$$\mathcal H_0=\{(x_s,a_s,y_s):a_s\in\mathcal A_{inc}\},$$

from which $\pi_0$ and an estimate of its context-dependent value can be trained and frozen.

For the new challenger,

$$\mathcal H_c(0)=\varnothing.$$

The main protocol supplies no challenger competence metadata, no offline challenger labels, and no challenger-specific warm-start samples. Historical tools may be used to learn generic structure, but not to reveal the held-out challenger's outcomes.

## 4. Feedback semantics

After observing $X_t$, the learner chooses $A_t$ and observes only

$$O_t=Y_t(A_t).$$

It does **not** observe the counterfactual $Y_t(1-A_t)$.

In particular, even when the challenger is selected, the learner does not receive the binary benefit label

$$Z_t=\mathbf 1\{Y_t(1)>Y_t(0)\}.$$

This matters for the relation to apple tasting: apple tasting reveals the true binary label after acceptance, whereas SCCO observes only the challenger reward and must infer benefit relative to a context-dependent incumbent from historical/online evidence.

The evaluator can retain both potential rewards in an offline replay, but the learner cannot access the unchosen reward.

## 5. Target region and margin notation

The zero-margin beneficial region is

$$\mathcal G^+=\{x:\tau_c(x)>0\}.$$

For theoretical statements under stochastic reward, it is useful to separate a margin band:

$$\mathcal G^+_\gamma=\{x:\tau_c(x)\ge\gamma\},\quad
\mathcal G^-_\gamma=\{x:\tau_c(x)\le-\gamma\}.$$

The frozen benchmark metrics continue to use $\gamma=0$; introducing $\gamma>0$ later would be a theorem/robustness device, not a silent change of the evaluation labels.

## 6. Policy value relative to the incumbent

Let $g(x)\in\{0,1\}$ be a challenger-adoption rule. Its population incremental value is

$$V_{inc}(g)=\mathbb E[g(X)\tau_c(X)].$$

The Bayes oracle under unconstrained expected reward is

$$g^*(x)=\mathbf 1\{\tau_c(x)>0\}.$$

Thus SCCO is still a special case of contextual reward maximization at the population optimum. We should **not** claim a different Bayes-optimal decision rule merely by writing $\tau(x)$.

The research difficulty is finite-data acquisition under the arrival asymmetry: how fast can an online policy approach $g^*$ without spending most early traffic on harmful challenger probes?

The corresponding expected onboarding regret is

$$R_T=\sum_{t=1}^T\left[\max\{\mu_0(X_t),\mu_c(X_t)\}-\mu_{A_t}(X_t)\right].$$

Task31's IOU is the realized baseline-relative value:

$$\mathrm{IOU}_T=\frac1T\sum_{t=1}^T\left(Y_t(A_t)-Y_t(0)\right).$$

## 7. Selective-risk diagnostics

Because the deployment requirement emphasizes precision, regret/IOU alone is insufficient to characterize behavior.

Define challenger coverage

$$C_T=\frac1T\sum_t\mathbf1\{A_t=1\}.$$

Beneficial-adoption precision and recall are

$$P_T=\frac{\sum_t\mathbf1\{A_t=1,\tau_c(X_t)>0\}}
{\sum_t\mathbf1\{A_t=1\}},$$

$$Q_T=\frac{\sum_t\mathbf1\{A_t=1,\tau_c(X_t)>0\}}
{\sum_t\mathbf1\{\tau_c(X_t)>0\}}.$$

A magnitude-sensitive harm quantity is

$$H_T=\sum_t\mathbf1\{A_t=1\}\,[-\tau_c(X_t)]_+.$$

These do not create a new objective class; they expose whether a method obtains reward by genuinely locating the challenger niche or by indiscriminate exploration.

## 8. Why structure is necessary

With zero challenger history and no challenger metadata, no learner can infer an arbitrary function $\mu_c(x)$ on unseen contexts without some cross-context or cross-tool structure.

Intuitively, if two environments agree on every challenger observation collected so far but assign opposite challenger advantages to all unqueried contexts, then a policy that adopts on those contexts can be made harmful in one environment, while a policy that never adopts has zero recall in the other.

Therefore a claim of simultaneous high precision and non-trivial recall must explicitly state **what makes challenger advantage generalizable from a few observations**. This is the mathematical bottleneck exposed by Task29/30; optimism alone is not an assumption that provides this structure.

Task32 should close only after choosing and validating such a structural hypothesis.

## 9. Candidate structural hypothesis H1 — relative-residual transfer

For each historical tool $j$, define a baseline-relative residual function

$$\delta_j(x)=\mu_j(x)-\mu_{0,j}(x),$$

where $\mu_{0,j}$ is the frozen incumbent constructed without tool $j$ under the same leave-one-tool-out protocol.

Candidate H1 states that historical residuals share a lower-complexity representation

$$\delta_j(x)\approx w_j^\top\phi_{rel}(x),$$

and that the arriving challenger's advantage $\tau_c(x)$ is better approximated in this **relative residual representation** than its absolute reward function is in an absolute shared-tool representation.

This is deliberately narrower than P-LinUCB/P-TS. Task30 transfers a shared affine subspace of **absolute tool reward parameters**; H1 asks whether the reusable object in a mature incumbent system is instead the geometry of *where tools beat the incumbent*.

H1 is also not automatically novel: few-shot/meta-learning of heterogeneous treatment effects exists. Its relevance here must be established under the frozen online bandit-feedback protocol and against those neighboring methods rather than asserted from terminology.

### Falsification criterion for H1

Before designing an algorithm, use train/calibration data only to compare held-out-tool few-shot prediction of baseline-relative advantage under:

1. raw context model;
2. absolute shared-tool representation;
3. relative-residual representation.

If the residual representation does not improve sign/region prediction at small challenger sample counts, H1 is rejected and must not become the core algorithm assumption.

## 10. Candidate structural hypothesis H2 — selective probe value

Even if H1 holds, fixed blind burn-in is inefficient because challenger-positive regions are sparse. A second candidate hypothesis is that some arriving contexts have higher **value of challenger information per expected deployment loss** than others.

This would justify a probe rule that separates:

- evidence acquisition: call challenger because this observation is informative;
- exploitation: call challenger because current evidence supports positive advantage.

ParetoBandit-style fixed burn-in does not perform that separation. H2 must likewise be validated rather than assumed.

## 11. Current novelty statement — intentionally conservative

At this stage the defensible statement is:

> We study the finite-data onboarding of a zero-history challenger against a mature frozen context-dependent incumbent under passive contexts and bandit feedback, with explicit measurement of beneficial-adoption precision and baseline-relative utility. Existing work covers the surrounding primitives; our eventual contribution must be a validated structural assumption and algorithm that exploits this arrival asymmetry better than relative-effect, nonlinear, transfer, conservative, and newcomer-onboarding baselines.

What we should **not** currently claim:

- first contextual bandit with new arms;
- first relative-advantage formulation;
- first positive-region / threshold identification problem;
- first high-precision selective decision rule;
- first conservative baseline fallback;
- first transfer solution for cold-start arms;
- first dynamic LLM/model onboarding problem.

## 12. Task32 exit condition

Task32 is complete only when a train-only diagnostic either validates H1/H2 (or a better replacement) or falsifies them, and the final formulation is updated accordingly. Algorithm design starts only after that evidence exists.

## 13. Diagnostic outcome: H1 is rejected

The preregistered train-only diagnostic was executed in src/probe_headless/t32_residual_transfer_diagnostic.py, writing data/tool_results/divergence/t32_residual_transfer_trainonly.json. Formal test partitions were held untouched by nested history/stream splits.

Across budgets 4/8/16/32, relative_shared did not dominate absolute_shared, raw_absolute, or raw_residual, and all variants remained negative in IOU. Representative budget-32 IOU values are:

- LLMRouterBench: relative-shared **-0.01058** vs absolute-shared **-0.00943**;
- ASlib: relative-shared **-0.00213** vs absolute-shared **-0.00184**;
- BBB: relative-shared **-0.00144** vs absolute-shared **-0.00029**.

Therefore H1 is falsified as the paper's universal structural assumption. It may remain a diagnostic feature, but it must not motivate Ours.

## 14. Diagnostic outcome: simple H2 is rejected

src/probe_headless/t32_context_receptivity_diagnostic.py tested a challenger-independent receptivity prior using historical pseudo-challenger opportunity; output is data/tool_results/divergence/t32_context_receptivity_trainonly.json.

ASlib had binary enrichment at top-4 contexts (precision 0.379 vs prevalence 0.114), but those probes still had mean baseline-relative reward **-0.231** and harmful rate 0.605. LLMRouterBench and BBB did not show robust small-budget enrichment.

Hence contexts where challengers often help are not enough. The relevant evidence-acquisition criterion must account for **advantage magnitude and downside**, not only probability of a positive sign.

## 15. Reduction to a close existing model

If the incumbent mean $\mu_0(x)$ were known, define a normalized challenger reward

$$
\tilde Y_t=Y_t(1)-\mu_0(X_t).
$$

Then incumbent fallback has expected residual reward 0 and challenger commit has expected residual reward $\tau_c(x)$. This reduces the binary online decision to a form very close to risk-sensitive contextual abstention (Liaw & Plaut, AISTATS 2026).

With an estimated incumbent, the observable residual after challenger selection is instead

$$
Y_t(1)-\hat\mu_0(X_t)=\tau_c(X_t)+\epsilon_t+e_0(X_t),
$$

where $e_0(x)=\mu_0(x)-\hat\mu_0(x)$ is a nuisance term induced by baseline estimation. This estimated-baseline asymmetry is more precise than the earlier generic challenger cold-start description.

Because the AISTATS 2026 method is a close neighbor discovered after Task31 freeze, Task32 does not authorize algorithm design. A separate baseline-amendment task must first establish whether that method is applicable and competitive under the frozen protocol.

## 16. Revised Task32 state

- H1 relative-residual representation transfer: **rejected as universal core assumption**.
- H2 simple historical context receptivity: **rejected as universal core assumption**.
- Generic SCCO formulation: **substantially covered by neighboring online-learning primitives**.
- Remaining candidate gap: estimated context-dependent baseline + zero-history challenger + sparse, magnitude-sensitive relative advantage under action-dependent feedback.
- Blocking item: risk-sensitive abstention comparator fidelity/applicability audit.

No Ours algorithm is frozen at this point.

## 17. Risk-sensitive abstention audit outcome

Task33 directly replayed Liaw & Plaut's official MainAlgorithm under Task31's frozen contexts and estimated-incumbent residual feedback. The adapter did not expose unchosen incumbent rewards.

The mechanism collapses to near-always-commit in the learned context spaces: for L>=0.1 it selects the challenger essentially 100% of the time on all three benchmarks, yielding IOU -0.21195 / -0.46715 / -0.03065 (LLMRouterBench / ASlib / BBB). L=0 can certify some negative bins but remains negative.

This closes the comparator as an appendix/related-work diagnostic, but it does **not** validate a new algorithmic assumption. The next step remains structural-gap discovery, not immediate Ours design.

## 2026-09-22 — Task32 r2 隔离及 estimand 更正（覆盖 §16 的过强结论）

H1/H2 原始诊断存在间接 challenger-history 泄漏；本轮已建立真实 j / pseudo-k 双重排除的独立 r2，实现、原结果封存、poisoning 测试及两进程逐字节复验见 `task/task32-AAMAS2027-problem-formulation/history_isolation_audit.md` 与 `history_isolation_results.md`。

§16 的“rejected as universal core assumption”改读为：**本次具体 rank-8 线性 residual-transfer 构造未得到稳定支持；简单 receptivity 不能据当前单个内层划分证明跨域稳健，但 BBB 小预算正例客观存在。** 不以失败配置排除整个方法族，不以有偏 r1 结果支撑 novelty。

严格区分三件事：

1. Task31 reward matrix 的 realized difference d_i 与随机环境 conditional mean difference tau(x)。现有 precision/recall/oracle 仍按 d_i 计分，不能改解释为 tau 的识别准确率。
2. `E[max(Y0,Y1)]` 的 hindsight headroom 与 `E[max(mu0(X),mu1(X))]` 的可实现均值最优策略；前者为正不证明稀少 feedback 能学到。
3. `Y1-m0(X)` 的 proxy residual 包含 baseline estimation error；若证明置信下界，不能省略该误差项，也不能把平均校准当逐 context 保证。

逐式推导、可观测分布相同但 hindsight oracle 不同的反例，以及解释边界仅在 `task/task32-AAMAS2027-problem-formulation/estimand_audit.md` 维护。本轮不把这些标准数学事实包装成 Ours。

Task34 使用同一旧历史 pack 的 auxiliary-calibration 路径也需要真实 j / pseudo-k 双重排除；以该 task 的 isolated revision 与 verification 为准。问题定义任务仍进行中，未冻结新算法，未修改 Task31 正式实验。

## 2026-09-22 — Owner 裁定：净收益优先

Owner 在本轮方案选择中明确选择「净收益优先」：首要目标是在有限反馈下，使新工具接入后的总回报高于同一输入流上的冻结旧策略。试用损失计入总收益，累计伤害、采用比例、采用精度与召回率同时报告；不预设全程不退化或高概率风险保证。

当前故事线是：智能体已有成熟工具策略，新工具加入后，系统通过有限试用判断哪些输入值得交给它，而不是重新学习整个工具池。该主张不等于新颖性；旧策略估值误差与试用信息价值仍是待验证的机制候选。

首批工作限于现有真实数据训练区内的诊断：区分当前模型的可学习性、旧策略估值误差和新工具估值误差。任何使用充分 challenger 历史或未选动作真实回报的参照都标为额外信息诊断，不能作为可部署方法、零历史成果或随机环境中的均值真值。

本裁定不修改 Task31 的 split、反馈语义和正式计分，不取代旧项目的 Accepted ADR，也不授权无边界 GPU 训练。诊断及其否定条件独立登记；真实结果返回后再决定方法，不提前给 Ours 命名。

## Task35：试用成本不能后补

Task35 按事前实验卡完成单批 CPU 诊断：三个环境各使用既有的一份外层训练区，所有挑战工具轮换，试用次数为 4 和 32，各用三种固定输入顺序。共 348 个 episode、2436 条策略记录；同配置独立重放逐字节一致，64 条定向测试通过。完整结果为 `data/tool_results/divergence/t35_net_gain_diagnostic_v1.json`，独立复核为 `task/task35-net-gain-diagnostic/verification.json`。

下表都是相对始终使用旧策略的平均回报变化，已经计入四次强制试用的损失。LLMRouterBench 按十五个数据集等权平均；ASlib 是归一化求解效用；BBB 是准确率变化。三类单位不能跨域相加。

| 环境 | 少量反馈，两边都用预测值 | 给足新工具历史，旧策略仍用预测值 | 少量反馈，但知道旧策略当次真实回报 |
|---|---:|---:|---:|
| LLMRouterBench | -0.029561 | -0.001299 | +0.056146 |
| ASlib SAT12-ALL | -0.016276 | -0.011670 | -0.007426 |
| BBB | -0.025445 | -0.001091 | +0.024718 |

后两列都是额外信息诊断：充分历史同时改变反馈的数量与来源；当次真实回报不是可部署的旧策略均值估计。补足历史明显减亏，但在当前表示与 Ridge1 下没有形成正平均收益，不能把 hindsight headroom 直接当成已证可学习性。只替换新工具预测为其当次真实回报也有相似改善，因此不能把旧策略估值误差认作唯一瓶颈。

ASlib 的 32 次强制试用更能限定下一步：平均试用贡献为 -0.085667，之后即便逐样本使用两边已实现回报的最佳选择，也只能贡献 +0.009474，总计 -0.076193。这是在当前 162 个输入、固定强制试用规则和全部挑战工具平均下的严格诊断，不是所有 32-feedback 策略的不可能性定理；允许更早停止、改变试用时机或改变 horizon 都不在本结论内。

本轮因此不支持直接把「修正旧策略估值」写成独立创新，也不支持通过增大固定试用次数解决问题。下一候选方向是选择何时继续试用、何时停止投入，并把估值改进留作必要对照；是否将它作为下一轮主线仍由 owner 裁定。现成序贯筛选、信息导向采样若能解释全部改善，就应作为基线，不另起 Ours 名称。
