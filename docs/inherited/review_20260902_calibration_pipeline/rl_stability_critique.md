# RL Stability Critique: Calibration Pipeline

## Setup Recap

- **State**: (task, tool outputs with tool identity stripped)
- **Actions**: {accept-one, verify-with-another, call-arbitrator}
- **Reward**: correct+accept→+1; correct+verify→+1−ε (ε≈0.1); wrong+accept→−λ (λ≈2); wrong+verify→−ε
- **Goal**: implicitly train P(correct | content); calibration emerges as byproduct
- **Verbalized confidence**: supervised as "say what you do" consistency constraint only

---

## Issue 1: The Reward Function Has a Closed-Form Degenerate Solution — and RL Will Find It

**Severity: Fatal**

**What is wrong.** The proposer's Risk #1 ("reward collapse to always-verify") is real but the proposer mischaracterizes its severity. This is not a risk; it is the *analytically optimal policy* under the stated reward, and the RL will converge to it unless something actively prevents it.

Consider the expected reward for each action as a function of p = P(correct | content):

- E[R | accept] = p·(+1) + (1−p)·(−λ) = p − λ(1−p) = p(1+λ) − λ
- E[R | verify] = p·(1−ε) + (1−p)·(−ε) = p(1−ε) − ε(1−p) = p − ε

The verify-branch expected reward is p − ε (a line with slope 1, intercept −ε). The accept-branch is p(1+λ) − λ (steeper slope 1+λ, but starts at −λ). The two lines cross at:

  p(1+λ) − λ = p − ε  
  pλ = λ − ε  
  **p* = 1 − ε/λ**

With ε=0.1, λ=2: p* = 1 − 0.05 = **0.95**.

This is the cost-sensitive learning threshold theorem (Elkan, 2001, "The Foundations of Cost-Sensitive Learning," IJCAI). The optimal policy under asymmetric misclassification costs is a threshold rule on P(correct), and the threshold is 1 − (cost_of_false_positive / (cost_of_false_positive + cost_of_false_negative)) — here re-expressed in the accept/verify framing as 1 − ε/λ.

**The problem**: if the model's calibrated P(correct) distribution is concentrated below 0.95 for most inputs — which it will be, because most tasks are genuinely uncertain — then **verify is the optimal action for essentially every input**. The policy collapses to a constant action. An RL algorithm optimizing this reward has no incentive to discriminate: the gradient pushing toward "accept on high-confidence-correct" is negligible when the fraction of inputs above the threshold is ~0.

This is not "reward collapse" as a training instability. It is the **correct solution to the stated MDP**. The reward function *defines* a problem whose answer is "always verify" when the base rate of p > 0.95 is low.

**How to fix.** Three options, in order of preference:

1. **Make ε state-dependent, not constant.** If the verification cost ε depends on task difficulty or tool disagreement, the threshold p* varies per input, and the policy must actually use P(correct) to decide. This converts the degenerate threshold into a real decision boundary.

2. **Add a coverage constraint / budget.** Limit the fraction of inputs that can use verify (e.g., verify at most 20% of the time). This is the cost-sensitive learning with abstention budget framework (Elkan, 2001; also Geifman & El-Yaniv, 2017, "Selective Classification"). The policy must then rank inputs by P(correct) and verify only the marginal ones — which requires calibration.

3. **Raise ε relative to λ.** With ε/λ = 0.05, the threshold is at 0.95. If you want the threshold at, say, p* = 0.7 (so the model must discriminate on 30% of inputs), you need ε/λ = 0.3, i.e., ε = 0.6 when λ = 2. But this makes verification expensive enough that wrong+verify is worse than correct+accept — which may not match the real economics.

**Diagnostic**: Track the empirical action distribution per checkpoint. If P(verify) > 95% after the first 100 steps and stays flat, the policy has collapsed to the constant solution. Also compute the analytical threshold p* = 1 − ε/λ and compare to the empirical P(correct) distribution — if the mass is almost entirely below p*, collapse is guaranteed.

**Literature**: Elkan (2001), IJCAI — the threshold theorem. Geifman & El-Yaniv (2017, JMLR) — selective prediction with abstention. These are the primary sources.

---

## Issue 2: Calibration Is Not a Byproduct of Policy Optimization — It Is Orthogonal to the Reward

**Severity: Fatal**

**What is wrong.** The setup claims "calibration emerges as a byproduct" of training P(correct | content) through the RL reward. This is the central design assumption, and it is wrong.

The reward function only constrains the *decision boundary* (accept vs. verify as a function of P(correct)). It says nothing about whether P(correct) itself is calibrated — i.e., whether P̂(correct) = p̂ matches the empirical frequency of correctness at that confidence level.

Two failure modes:

**A. The marginal predictor trap (proposer's Risk #2 — this one is real).** The reward gradient is sensitive only to whether p̂ is above or below the threshold p*. Any monotonic transformation of p̂ that preserves the ranking above/below the threshold yields the same reward. So the model can learn a decision boundary that is *monotonically correct* but *miscalibrated in absolute terms* — p̂ = 0.99 when the true rate is 0.80, as long as 0.99 is still "above threshold" and the model accepts. The reward provides zero gradient for absolute calibration. This is the well-known problem that discriminative training optimizes ranking, not calibration (Cohen & Goldszmidt, 2004, "On the Recoverability of Probabilities from Classification'; also Niculescu-Mizil & Caruana, 2005, "Predicting Good Probabilities with Supervised Learning").

**B. RLHF makes calibration worse, not better.** Leng et al. (2024, "Taming Overconfidence in LLMs: Reward Calibration in RLHF," arXiv:2410.09724) demonstrate that PPO-based RLHF *inherently biases reward models toward high-confidence scores regardless of actual response quality*, leading to verbalized overconfidence. Tian et al. (2023, "Just Ask for Calibration," EMNLP) show that RLHF-tuned models are less calibrated in their verbalized confidence than base models. Xiong et al. (2023, "Can LLMs Express Their Uncertainty?") provide systematic evidence that verbalized confidence from RLHF models is systematically overconfident.

The "say what you do" consistency constraint on verbalized confidence does not fix this — it only enforces internal consistency between the decision and the stated number, not external calibration. A model can be perfectly self-consistent ("I always say 0.95, and I always accept") while being completely miscalibrated (true accuracy at p̂=0.95 is 0.70).

**How to fix.** Calibration must be an *explicit optimization objective*, not a byproduct:

1. **Add a calibration loss to the RL objective.** Use a proper scoring rule (Brier score or log loss) on the verbalized confidence against the ground-truth correctness label. This provides a direct gradient for absolute calibration. The reward function can include a term: R_calibration = −(p̂ − y)² where y ∈ {0,1} is the correctness label. This is the standard approach in the calibration literature (Kuleshov & Liang, 2015, "Calibrated Structured Prediction").

2. **Use post-hoc temperature scaling or isotonic regression** on the learned P(correct), evaluated on a held-out calibration set with ground-truth labels. This is the Platt (1999) / Zadrozny & Elkan (2002) approach, and it is the minimum viable calibration.

3. **Monitor ECE (Expected Calibration Error) and reliability diagrams** during training, not just reward. If ECE is increasing while reward is increasing, calibration is degrading — this is the known RLHF failure mode.

**Diagnostic**: At each checkpoint, compute ECE on a held-out set where ground-truth correctness is known. Plot ECE alongside training reward. If ECE increases while reward increases, calibration is being sacrificed for reward optimization. Also plot reliability diagrams (binned predicted vs. empirical accuracy) to detect systematic miscalibration.

**Literature**: Leng et al. (2024, arXiv:2410.09724) — RLHF causes verbalized overconfidence. Tian et al. (2023, EMNLP) — RLHF degrades verbalized calibration. Xiong et al. (2023, arXiv:2306.13063) — systematic evaluation of confidence elicitation. Niculescu-Mizil & Caruana (2005, ICML) — discriminative training optimizes ranking, not calibration. Kuleshov & Liang (2015, NeurIPS) — calibration as explicit objective.

---

## Issue 3: The MDP Is Partially Observable — Tool Identity Stripping Destroys the Markov Property

**Severity: Serious**

**What is wrong.** The state is "(task, tool outputs without tool identity)." This is a deliberate design choice (presumably to prevent the policy from learning tool-specific shortcuts), but it creates a POMDP, not an MDP.

Consider two scenarios that produce the *same observation* (same task, same tool output content, same confidence) but with different latent states:
- Tool A (known reliable, accuracy 0.95) outputs answer X with confidence 0.8
- Tool B (known unreliable, accuracy 0.50) outputs answer X with confidence 0.8

After stripping tool identity, the state is identical: (task, answer X, confidence 0.8). But the optimal action differs: accept in scenario 1, verify in scenario 2. The policy cannot distinguish them.

This is not a theoretical concern — it is the standard POMDP credit-assignment problem (Kaelbling, Littman & Moore, 1996, "Reinforcement Learning: A Survey," JAIR). When the state is partially observable, the optimal policy is a function of the *belief state* (distribution over latent states given the observation history), not the current observation alone. A memoryless policy over observations is suboptimal.

The practical consequence: the RL will learn a policy that averages over the latent tool-identity distribution. If the tool mix is 70% reliable / 30% unreliable, the optimal memoryless policy treats every observation as if it came from a 0.7-weighted mixture — which is neither the right policy for the reliable tool nor the unreliable one. The model will be *systematically miscalibrated* because it cannot condition P(correct | content) on the tool that produced the content.

**How to fix.** Three options:

1. **Do not strip tool identity.** Include tool identity in the state. This makes the MDP fully observable and lets the policy learn tool-specific P(correct) calibration. If the concern is generalization to new tools, use a learned tool embedding rather than a one-hot, and handle new tools via a special "unknown tool" token.

2. **Use a recurrent policy (RNN/Transformer with history).** If tool identity must be hidden, the policy needs to infer it from the observation history (e.g., patterns in tool outputs that are characteristic of specific tools). This is the belief-state approach. But this adds significant complexity and requires sufficient history length.

3. **Add tool reliability as an observed feature.** Instead of stripping tool identity entirely, include a *learned* or *pre-computed* reliability score for the tool (e.g., historical accuracy of this tool on similar tasks). This preserves the "no tool shortcuts" goal while giving the policy the information it needs for calibration.

**Diagnostic**: Compare the policy's performance when tool identity is included vs. stripped. If there is a large gap, the partial observability is causing significant suboptimality. Also check whether the policy's P(correct) estimates differ across inputs that are observationally identical but come from different tools — if they do not (they cannot, by construction), the model is being forced to miscalibrate.

**Literature**: Kaelbling, Littman & Moore (1996, JAIR) — POMDPs and belief states. Singh, Jaakkola & Jordan (1994, NeurIPS) — partially observable MDPs and memoryless policies.

---

## Issue 4: GRPO Is the Wrong Algorithm — It Will Amplify the Collapse

**Severity: Serious**

**What is wrong.** The choice of RL algorithm matters enormously here, and the likely candidate (GRPO, Group Relative Policy Optimization, from DeepSeek) is particularly ill-suited.

GRPO works by sampling K completions per prompt, computing the reward for each, then normalizing advantages *within the group* (subtracting the group mean and dividing by group std). This local normalization is the key mechanism.

The problem: if the group of K samples for a single prompt all get the same action (e.g., all verify), the group-mean advantage is zero (or near-zero), and the gradient is ~0. The policy gets no signal to change. This is the "local advantage normalization bias" identified by Hu et al. (2025, "REINFORCE++: Stabilizing Critic-Free Policy Optimization with Global Advantage Normalization," arXiv:2501.03262), who show that GRPO's prompt-level normalization is a *theoretically biased estimator* and tends to overfit to prompt-specific reward variance.

In this setup, the failure mode is acute: once the policy collapses to "always verify" (Issue 1), every group of K samples will have near-identical rewards (all ≈ p − ε), the group std will be tiny, and the advantage estimates will be numerically unstable or zero. The policy is stuck in a flat region of the loss landscape with no gradient to escape.

**Algorithm comparison for this reward structure:**

- **PPO** (Schulman et al., 2017): Would work better than GRPO because it uses a learned value function (critic) that provides a global baseline, not a per-prompt group baseline. The critic can learn that "always verify" is suboptimal if there exist prompts where accept yields higher reward. But PPO requires a separate critic network, adding complexity, and the critic itself can be miscalibrated.

- **REINFORCE with global baseline**: The simplest option. Use the batch-level mean reward as the baseline. This avoids the local-normalization bias of GRPO. But it has higher variance than PPO.

- **REINFORCE++ (Hu et al., 2025)**: The recommended option. Uses global batch-level advantage normalization, which is *effectively unbiased* (bias vanishes as batch size increases). Specifically designed to fix GRPO's local normalization problem. The paper shows it outperforms both GRPO and PPO in agentic settings.

- **DPO** (Rafailov et al., 2023): Requires *preference data* (pairs of (preferred, rejected) trajectories). This setup has scalar rewards, not preferences. DPO is inapplicable unless you convert rewards to preferences, which loses information (the magnitude of the reward difference matters here — ε vs. λ encode different costs, and DPO's Bradley-Terry model discards magnitude).

**Recommendation**: Use REINFORCE++ or PPO with a learned critic. Do NOT use GRPO — its local normalization will amplify the collapse and provide no escape gradient. If using PPO, monitor the critic's calibration separately (the critic estimates V(s), which should correlate with P(correct) — if it does not, the critic is miscalibrated).

**Diagnostic**: If using GRPO, log the per-prompt group reward variance. If it drops below a threshold (e.g., std < 0.01) for >50% of prompts, the policy is in the flat region and GRPO cannot escape. Switch algorithms or add exploration.

**Literature**: Hu et al. (2025, arXiv:2501.03262) — REINFORCE++, global advantage normalization, GRPO bias. Schulman et al. (2017, arXiv:1707.06347) — PPO. Rafailov et al. (2023, arXiv:2305.18290) — DPO. Shao et al. (2024, DeepSeek-Math) — GRPO.

---

## Issue 5: The ε/λ Ratio Is Not Learnable — It Encodes a Value Judgment That Must Be Set Externally

**Severity: Serious**

**What is wrong.** The proposer asks whether ε/λ is "learnable or needs grid search." The answer is: it is neither, and this is a deeper problem than hyperparameter tuning.

ε and λ are not free parameters to be optimized against the reward — they *define* the reward. They encode the *real-world costs* of verification and error:
- ε = cost of calling a second tool (API cost, latency, user experience)
- λ = cost of accepting a wrong answer (downstream error, user harm, system failure)

These are business/product constraints, not ML hyperparameters. You cannot learn them from the RL data because they are inputs to the reward function, not outputs. Trying to "learn" ε/λ from the data is circular: the optimal policy depends on ε/λ, so you cannot use the policy's behavior to infer ε/λ without already knowing the optimal policy.

However, there *is* a principled way to set them: **align ε/λ with the empirical cost ratio of the deployment environment.** If a wrong answer costs \$C_wrong (in dollars, user churn, downstream errors) and a verification costs \$C_verify (in API costs, latency), then:

  ε/λ should be set to C_verify / C_wrong

and the threshold p* = 1 − ε/λ = 1 − C_verify / C_wrong.

This is not a grid search — it is a business decision. If a wrong answer costs $100 and verification costs $1, then ε/λ = 0.01, p* = 0.99, and the policy should verify almost everything (which is correct — verification is cheap, errors are expensive). If a wrong answer costs $2 and verification costs $1, then ε/λ = 0.5, p* = 0.5, and the policy should accept low-confidence answers (verification is expensive relative to errors).

**The real risk**: if ε/λ is set incorrectly (e.g., ε too small relative to the true cost of verification, or λ too small relative to the true cost of errors), the "optimal" policy under the learned reward will be *systematically wrong* in deployment. This is a form of reward misspecification (Casper et al., 2023, "Open Problems and Fundamental Limitations of RLHF," arXiv:2307.15217) — the reward function does not match the true objective.

**How to fix.**

1. **Measure the real costs.** Before setting ε/λ, quantify the actual downstream cost of a wrong answer and the actual cost of verification in the deployment environment. Set ε/λ = C_verify / C_wrong.

2. **Sensitivity analysis.** Run the RL with 3-5 values of ε/λ (e.g., 0.01, 0.05, 0.1, 0.3, 0.5) and measure the resulting policy's accept-rate, accuracy, and verification cost. If the policy is highly sensitive to ε/λ (which it will be, since p* = 1 − ε/λ), the deployment cost estimate must be precise.

3. **Online adjustment.** If the costs change over time (e.g., API costs decrease, or error costs increase as the system scales), ε/λ must be re-tuned. Consider an online bandit approach to adjust ε/λ based on observed deployment outcomes.

**Diagnostic**: Compute the sensitivity ∂P(accept) / ∂(ε/λ) at the converged policy. If this is large (>10% change in accept rate for a 10% change in ε/λ), the system is fragile to reward misspecification and the cost estimate must be precise.

**Literature**: Casper et al. (2023, arXiv:2307.15217) — reward misspecification in RLHF. Gao, Schulman & Hilton (2022, arXiv:2210.10760) — reward model overoptimization scaling laws.

---

## Issue 6: Three Discrete Actions Introduce an Unnecessary Arbitrator-Action That May Be Redundant or Harmful

**Severity: Minor**

**What is wrong.** The action space has three actions: {accept-one, verify-with-another, call-arbitrator}. The reward function only specifies rewards for accept and verify — the arbitrator action is mentioned but not in the reward table.

If the arbitrator is a third option with its own cost/reward, it needs to be in the reward function. If it is equivalent to verify (same cost ε, same correctness outcome), it is redundant and the action space should be reduced to {accept, verify} — the smaller action space is easier to optimize and less prone to degenerate solutions.

If the arbitrator is a "meta-action" (e.g., escalate to a human or a more powerful model), it likely has a different cost (higher than ε) and a different correctness guarantee (higher accuracy than verify). This needs to be explicitly modeled.

**How to fix.** Either:
1. Remove the arbitrator action and merge it into verify (if functionally equivalent)
2. Add it to the reward table with its own cost/reward (e.g., correct+arbitrator → +1−δ, wrong+arbitrator → −δ, where δ > ε reflects the higher cost of escalation)

**Does discrete vs. continuous matter?** The deeper question is whether the policy should output a continuous confidence value (and threshold it) rather than a discrete action. A continuous policy over P(correct) is more flexible — it can be thresholded at deployment time with any ε/λ ratio, without retraining. The current discrete-action design *bakes the ε/λ threshold into the policy at training time*, making it inflexible to deployment cost changes (see Issue 5). This is a design flaw, not a bug.

**Recommendation**: Train the policy to output P(correct) (continuous, with a calibration loss — see Issue 2), and apply the accept/verify/arbitrator decision at *deployment time* using the cost ratio ε/λ. This decouples calibration learning from the business decision, making the system robust to cost changes.

---

## Summary Table

| # | Issue | Severity | Proposer's Risk? |
|---|-------|----------|-----------------|
| 1 | Reward has closed-form degenerate solution (always-verify) | Fatal | Risk #1 (real, but worse than stated) |
| 2 | Calibration is not a byproduct of policy optimization | Fatal | Risk #2 (real) |
| 3 | Partial observability destroys the Markov property | Serious | Not flagged |
| 4 | GRPO will amplify collapse via local normalization bias | Serious | Not flagged |
| 5 | ε/λ is not learnable — it is a business constraint | Serious | Not flagged |
| 6 | Three discrete actions bake in the threshold, reducing flexibility | Minor | Not flagged |

## On the Proposer's Risk #3 (Distribution Shift)

Distribution shift on transfer to new domains is a real risk but is *not specific to this RL setup*. It is a generic problem for any learned system and is not caused by the reward function or action space design. The proposer is right to flag it but it is not an RL-specific stability issue. The fix is standard: evaluate on held-out domains, monitor calibration drift, and use domain-adaptation techniques if needed. The deeper version of this risk *is* RL-specific: if the training distribution of P(correct) is narrow (e.g., mostly 0.7-0.9), the calibration learned in that range will not transfer to domains where P(correct) is in a different range. But this is a consequence of Issue 2 (calibration is not explicitly optimized), not a separate risk.

## Recommended Algorithm

**REINFORCE++ (Hu et al., 2025)** or **PPO with explicit calibration loss**. Do not use GRPO. If using PPO, add a Brier score or log-loss term on verbalized confidence to the objective function, and monitor ECE on a held-out set with ground-truth labels throughout training.

## Core Design Recommendation

**Decouple calibration from the decision.** Train the policy to produce calibrated P(correct | content) with an explicit calibration loss (Brier/log-loss). Apply the accept/verify/arbitrator threshold at deployment time using the real cost ratio ε/λ. This makes the system robust to:
- Cost ratio changes (Issue 5)
- Action space rigidity (Issue 6)
- The degenerate-solution problem (Issue 1, because the calibration objective provides gradient even when the decision boundary is trivial)
- Partial observability (Issue 3, because calibration can be conditioned on full information including tool identity, while the decision threshold can be applied post-hoc)

