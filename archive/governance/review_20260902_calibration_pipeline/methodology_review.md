# Methodology Review: Tool Conflict Adjudication for Pharma Agents

**Reviewer stance**: ICML/NeurIPS-level methodology review. No validation, only attack.

---

## Issue 1 — [FATAL] "Always verify" is the dominant strategy under the proposed reward

**What is wrong.** The reward table has a degenerate solution. Computing expected rewards for each action, given the agent's estimated probability of correctness `p`:

```
E[accept-one] = p(+1) + (1-p)(-λ) = p(1+λ) − λ
E[verify]     = p(+1−ε) + (1-p)(−ε) = p − ε
E[arbitrator] = not specified (assume similar to verify)
```

The agent should accept when `E[accept] > E[verify]`:

```
p(1+λ) − λ > p − ε
pλ > λ − ε
p > 1 − ε/λ
```

With `ε` small and `λ` large (as stated), the threshold `p*` approaches 1. The optimal policy is: **never accept unless near-certain; always verify.** But "always verify" means the agent never adjudicates — it always calls a third tool. This defeats the entire purpose. You wanted an agent that adjudicates conflicts by assessing content quality; the reward makes the safe play strictly dominant in all realistic scenarios.

This is specification gaming: the optimal policy under your reward is not the policy you want.

**How to fix.** Two options:

1. **Add a verification cost.** Make `E[verify] = p − ε − c_verify` where `c_verify` reflects latency, API cost, or a tool-call budget. Now the threshold is `p > 1 − (ε + c_verify)/λ`, giving a meaningful decision boundary.
2. **Replace outcome-based reward with a calibration reward.** Use a proper scoring rule (e.g., reward = `−(confidence − correctness)²` or Brier score). This incentivizes honest confidence calibration rather than risk-averse conservatism. The agent learns to output calibrated `p`, and the accept/verify/arbitrate decision becomes a downstream policy on that calibrated probability with explicit, tuned costs.

Without one of these, the RL stage trains the wrong objective.

---

## Issue 2 — [FATAL] Stage 1's "rotate property type" is a category error

**What is wrong.** The proposal says: "rotate property type (logP ↔ solubility ↔ IC50) so the same tool crosses properties in different samples." But ground truth is experimental BBB permeability only. There are two interpretations, both broken:

- **(a) Relabel BBB predictions as logP/solubility/IC50.** The tool output is a BBB permeability prediction; calling it an IC50 prediction is a false label. The model would learn to ignore property labels entirely (they carry no signal), which means your "content-conditioned" estimator is really a "ignore-the-label-and-read-the-number" estimator. That is not learning `P(correct | content)`; it is learning to discard a feature.
- **(b) Actually predict logP, solubility, IC50.** You need ground truth for all those properties, which you don't have. You cannot create training signal for properties you have no labels for.

Either way, the rotation does not achieve what it claims.

**How to fix.** Drop the rotation entirely. If the goal is "tool identity shouldn't matter," test it directly via **leave-one-tool-out (LOTO)**: train the estimator on 3 of 4 ADMET models, evaluate calibration on the 4th held-out model. If the estimator is calibrated on the held-out model, that is evidence of content-conditioned learning. If not, the claim is falsified. This is the clean experiment the proposal is missing, and it requires no rotation.

---

## Issue 3 — [SERIOUS] No control condition isolating "content-conditioned" from "identity-conditioned"

**What is wrong.** The core claim is that `P(correct | content)` is learnable *without* tool identity. But the proposal never runs the obvious control: train the same pipeline *with* tool identity in the state and compare. If the identity-conditioned version performs significantly better, the content-only version is just a degraded approximation — you have shown content is usable, not that it is *sufficient*.

**Required ablation matrix:**

| Condition | State features |
|---|---|
| Content-only (proposed) | (task, tool outputs) — no identity |
| Identity-only | tool name as feature, no content |
| Content + identity | both |
| Random baseline | uniform action |
| Majority-vote | always trust the majority tool |
| Confidence-from-repetition | call same tool k times, use variance |
| Conformal prediction | split-conformal on same features |

Without this matrix, a reviewer cannot assess whether content-only is the right design choice or an under-specified one. "It works" is not enough; you need "it works AND removing identity doesn't hurt AND identity-only is worse."

---

## Issue 4 — [SERIOUS] The cross-domain transfer claim is untestable with this data and likely false

**What is wrong.** "Test zero-shot transfer to non-pharma conflict tasks (e.g., two search tools disagree in software engineering)." The content distributions are entirely disjoint:

- Pharma: ADMET model outputs — continuous molecular property predictions, bounded ranges, known physics.
- Software engineering: search-tool results — discrete document retrievals, unbounded text.

There is no shared content representation. A content-conditioned estimator trained on pharma will have learned features like "agreement level between numeric predictions" and "prediction extremity" — none of which transfer to text retrieval disagreement. The claim is hand-waving and will be rejected at review.

**How to fix.** Either:

1. **Design a domain-agnostic content representation from the start.** Use meta-level features: agreement between tools, tool self-reported confidence (if available), output entropy, prediction stability under perturbation. These are domain-agnostic and could transfer. This requires changing the data construction approach.
2. **Embed all tool outputs into a common embedding space** via a pretrained encoder (e.g., a text/numeric encoder), then condition on embeddings rather than raw outputs.
3. **Drop the transfer claim.** Frame it as future work and focus the contribution on the pharma benchmark + calibration method.

---

## Issue 5 — [SERIOUS] The framing is an empirical study, not a scientific contribution

**What is wrong.** NeurIPS/ICML will read this as "we applied SFT+RL to a pharma pipeline." The proposal lacks:

- **A formal definition of "tool overfitting."** "Tool overfitting = agent overconfidence on familiar tools" is an assertion. Operationalize it: `P(accept | tool=A) > P(accept | tool=B)` when `P(correct | content)` is equal. Without this definition, you cannot measure the phenomenon you claim to address.
- **A theoretical argument for why content-conditioning should work.** What property of the content signal makes it a sufficient statistic for correctness? Under what assumptions? Without this, the method is unprincipled.
- **A novel algorithmic contribution.** "SFT then RL" is standard. The contribution must be clearly articulated as novel — in the calibration-aware reward design, the content-conditioned estimator architecture, or the LOTO evaluation protocol.
- **Related work positioning.** Selective prediction (Geifman & El-Yaniv), conformal prediction (Vovk, Angelopoulos), tool-use calibration in LLM agents, uncertainty estimation in LLMs. If a reviewer asks "how is this different from conformal prediction?" and you have no answer, the paper is dead.

**What is missing for a top venue.**

- **Falsifiability**: What experiment proves this approach is wrong? The LOTO experiment (Issue 2 fix) is the key falsification test: if the content-conditioned estimator fails to calibrate on a held-out tool, the central claim is falsified. This must be the centerpiece, not an afterthought.
- **Conformal baseline**: The proposal does not mention conformal prediction at all. Content-conditioned `P(correct | content)` *is* what conformal prediction produces (prediction intervals conditional on features). You must either (a) show your method outperforms conformal on calibration, or (b) argue why conformal is insufficient (e.g., conformal gives marginal coverage, not conditional; your method gives instance-level). Without engaging with conformal, the contribution is unclear.
- **Data split rigor**: The 400 conflict / 330 no-conflict split is small. How is the train/test split done? Molecule-wise (to prevent leakage) or random? If random, same-molecule instances in train and test leak molecular features. Must split by molecule scaffold or scaffold-cluster to be credible.
- **Statistical power**: 730 adjudication instances total. With a 80/20 split, ~146 test instances. Confidence intervals on calibration metrics will be wide. You need bootstrap CIs and power analysis showing the effect size is detectable at this `n`.

---

## Summary

| # | Issue | Severity | One-line |
|---|---|---|---|
| 1 | Degenerate reward → "always verify" dominant | FATAL | RL trains the wrong objective; needs verification cost or proper scoring rule |
| 2 | Property rotation is a category error | FATAL | Only BBB ground truth exists; rotation either lies or needs labels you don't have |
| 3 | No content-vs-identity control | SERIOUS | Cannot claim content-sufficiency without the ablation matrix |
| 4 | Cross-domain transfer untestable | SERIOUS | Disjoint content distributions; needs domain-agnostic representation |
| 5 | Empirical study, not scientific contribution | SERIOUS | No formal definition, theory, novelty articulation, or related work positioning |

**Bottom line.** Issues 1 and 2 are structural — the reward is degenerate and the data construction contains a logical error. These must be fixed before any experiment is run. Issues 3–5 are what separates a workshop paper from a NeurIPS/ICML paper. The single most impactful addition would be the LOTO experiment with the full ablation matrix (Issue 3) anchored against a conformal prediction baseline — that simultaneously addresses falsifiability, the control condition, and the related work gap.
