# AAMAS2027 Structural Gap Review

> 状态：task34 working review
> 目的：在 Ours 前关闭 modern safe learning / calibration / new-action transfer 近邻，避免把已有 primitive 重新命名为创新。

## 1. Current frozen deployment regime

Task31 v1 的 learner 面临：

- mature frozen incumbent policy，拥有大量 pre-arrival history；
- challenger 在 arrival 时对自身 reward 为 zero-history；
- context 被动到达；
- 每轮只观察 executed action reward；
- challenger 被调用时不同时观察 realized incumbent counterfactual；
- challenger positive region 稀疏，且 harmful magnitude 可能远大于 positive frequency 暗示的风险；
- 评价看 IOU / regret，同时显式报告 beneficial-adoption precision/recall。

这一组合仍属于 contextual online decision 的 special regime，不能靠命名本身形成 novelty。

## 2. Modern conservative contextual bandits (ICLR 2025)

Deb, Ghavamzadeh, Banerjee, Conservative Contextual Bandits: Beyond Linear Representations, ICLR 2025，提出 C-SquareCB / C-FastCB：用 general online regression oracle + IGW 探索，并以 cumulative performance constraint 保证不比 baseline 差太多。

Paper: https://proceedings.iclr.cc/paper_files/paper/2025/hash/dbca58f35bddc6e4003b2dd80e42f838-Abstract-Conference.html

本地审计件：.srcprobe/papers/conservative_contextual_bandits_2025_final.pdf 和 .txt。

关键事实：

- main theorem 假设每轮 baseline expected cost h(x_t,b_t) 已知；
- Appendix E 放松成 noisy baseline cost y_{t,b_t}；
- 但其 safety condition (43) 使用历轮 baseline observations，包括 learner 选择非-baseline action 的轮次，因此仍拥有 current-round / side baseline feedback。

Task31 frozen feedback 没有这条 side observation：选 challenger 后只看到 challenger reward；同一 x 的 realized incumbent reward不可见。

因此该工作关闭了 nonlinear conservative contextual bandit 这一 broad objection，但没有直接实现 frozen feedback 下的 baseline-relative evidence acquisition。

## 3. Safe Exploration Algorithm (SEA)

Jagerman, Markov, de Rijke, 2020, Safe Exploration for Optimizing Contextual Bandits：从 production baseline policy 出发，用 baseline logged data 做 counterfactual learning + high-confidence OPE；只有 candidate policy 的估计表现至少不差于 baseline 才部署。

Paper: https://arxiv.org/abs/2002.00467

覆盖：production baseline、safe policy improvement、high-confidence gate。

差异：SEA 的安全判定是 policy-level / logged-policy OPE；新 policy 可由 baseline logging support 评估。对于 Task31 的 completely new challenger action，baseline logs 对 challenger reward 没有 support，因此不能直接提供 local challenger-benefit evidence。

结论：不能 claim first production-baseline safe exploration；但 zero-history new-action local onboarding 仍需额外结构。

## 4. Cross-domain OPE/L with new actions

Natsubori, Ushiku, Saito, ICLR 2025, Cross-Domain Off-Policy Evaluation and Learning for Contextual Bandits 明确处理 few-shot、deterministic logging 和 new actions，通过 source-domain logged datasets 修复 target-domain support 不足。

Paper: https://proceedings.iclr.cc/paper_files/paper/2025/hash/87eb265e8898a7e245d61f01cea4d906-Abstract-Conference.html

这进一步说明 new action + transfer 不是 novelty。

与 Task31 的差异：它假设 source domains 对新 action / action relationship 提供可用 logged support；当前 main protocol 刻意不给 held-out challenger reward/metadata，只允许旧工具形成 generic prior。若未来 Ours 引入 source challenger examples，就必须与该工作直接对齐，而不能声称 zero-history。

## 5. Few-shot conformal / meta calibration

Fisch et al., ICML 2021, Few-shot Conformal Prediction with Auxiliary Tasks：把 auxiliary tasks 视为 exchangeable task collection，通过 meta-learning / meta-calibration 在 target task 仅有少量 support 时得到更紧的 conformal set，同时保持 task-marginal guarantees。

Paper: https://proceedings.mlr.press/v139/fisch21a.html

Park, Cohen, Simeone, TPAMI 2024 进一步做 meta-learned cross-validation-based conformal calibration，并强调 per-task calibration guarantees。

因此用历史工具作为 auxiliary tasks 来校准新工具 few-shot uncertainty 本身也不能作为新概念。

可研究的只能是：这种 auxiliary-task calibration 在 baseline-relative latent benefit + bandit feedback 下是否真的可迁移，以及它是否能改善 onboarding utility。

## 6. Conformal risk / online partial feedback

Angelopoulos et al., ICLR 2024 Conformal Risk Control 可以对任意 monotone loss 的期望风险做 finite-sample control；因此用 conformal 控 harmful adoption 也不是自动 novelty。

Paper: https://research.google/pubs/conformal-risk-control/

2026 年又出现两条更近的 limited-feedback 方向：

- Yang, Kim, Park, ICLR 2026, Online Conformal Prediction with Adversarial Semi-bandit Feedback via Regret Minimization：true label 只在落入 prediction set 时部分揭示。https://proceedings.iclr.cc/paper_files/paper/2026/hash/4eb5daabc45b45a9a312aa2c8fca8a74-Abstract-Conference.html
- Gollapudi et al., 2026 preprint, Efficient Online Conformal Selection with Limited Feedback：bandit feedback 下选择低成本 subset，并控制 success target / efficiency regret。https://arxiv.org/abs/2605.14953

结论：limited-feedback risk control 已有直接邻居。任何 calibration-based Ours 必须贡献到 baseline-relative new-action structure，不能只贡献在线 conformal + bandit feedback。

## 7. Expected improvement / value of information

Tran-The et al., NeurIPS 2022 Expected Improvement for Contextual Bandits 已把 Expected Improvement 从 BO 系统化引入 contextual bandit，并给 linear / neural 两类算法与 regret analysis。

Paper: https://proceedings.neurips.cc/paper_files/paper/2022/hash/8f0942c43fcfba4cc66a859b9fcb1bba-Abstract.html

所以 Task32 H2 后续即使改成 value-of-information / expected-improvement probe，也必须非常谨慎：EI contextual exploration 已存在。

真正可能不同的不是 EI 名称，而是 probe utility 是否把 incumbent-relative downside、estimated-baseline nuisance 和 cross-tool calibrated error 一起纳入。

## 8. Current coverage matrix

| Family | Mature baseline | New action zero-history | No side counterfactual | Local relative target | Auxiliary-task transfer | Explicit safety/risk |
|---|---|---|---|---|---|---|
| LinUCB/LinTS | partial | yes in our adapter | yes | implicit | no | no |
| ACTS | baseline/control | possible | yes | yes | no | propensity-bounded exploration |
| P-LinUCB/P-TS | yes | yes | yes | implicit absolute reward | yes | no |
| ParetoBandit-style | no special value model | yes | yes | no | no | burn-in only |
| Risk-sensitive abstention 2026 | fallback=known 0 | unknown commit | yes | after normalization | no | yes, Lipschitz trusted region |
| C-SquareCB/FastCB 2025 | yes | generic actions | no for frozen protocol: baseline cost side info | policy reward | no | cumulative constraint |
| SEA 2020 | yes | weak support for new action | offline logs | policy-level | no | HCOPE gate |
| Cross-domain OPE/L 2025 | yes | yes | offline | policy-level | yes, source-domain data | OPE uncertainty |
| Few-shot meta CP 2021/2024 | n/a | new task | supervised support labels | prediction uncertainty | yes | coverage |
| Online conformal partial-feedback 2026 | n/a | not the focus | partial feedback | prediction/selection success | no | validity/risk |
| Task31 frozen SCCO | yes | yes | yes | yes | old tools only, no heldout reward | IOU + precision/harm |

## 9. Remaining falsifiable hypothesis H3

H1 (mean representation transfer) failed. H2 (context receptivity) failed.

A weaker possibility remains：

> Historical pseudo-arrival tasks need not transfer their mean advantage function; they may transfer only the one-sided prediction-error distribution of a few-shot baseline-relative learner.

For a challenger few-shot residual predictor tau_hat_m(x) trained from m challenger calls, historical pseudo-arrival tasks can estimate an upper quantile q of over-estimation error

E = tau_hat_m(X) - tau(X).

A conservative adoption certificate is

tau_hat_m(x) - q_(1-alpha) > 0.

H3 is explicitly not a novelty claim: few-shot meta-conformal literature already motivates auxiliary-task calibration. The diagnostic question is only whether this primitive is empirically valid/useful in our frozen regime.

Task34 will reject H3 if historical one-sided calibration either collapses to zero coverage or fails to achieve positive IOU/high precision on nested train-only pseudo-arrival tests.
