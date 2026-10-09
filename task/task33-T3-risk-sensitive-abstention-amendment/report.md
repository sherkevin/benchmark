# task33 report — Risk-Sensitive Abstention Baseline Amendment

> 状态：⚡️进行中

## 1. Official fidelity audit

Official simulation repo frozen at HEAD 69994acc6679bdc368a26ed9d2612aac67db4fee.

MainAlgorithm assumptions/code behavior:

- two actions: abstain reward exactly 0, commit observes noisy reward r(x,1)+noise;
- commit mean reward is assumed Lipschitz in Euclidean context;
- known T, dimension n, Lipschitz constant L, subgaussian noise sigma;
- trusted radius m=log(T);
- grid width w=T^(-1/(n+2));
- outside trusted region -> abstain;
- inside trusted region -> commit unless the local bin has enough evidence to certify negative;
- update only after commit.

This is much closer to SCCO than ordinary UCB and therefore must be closed before Ours.

## 2. Mapping to frozen onboarding

If incumbent expected value mu0(x) is known, normalize challenger feedback as y_new - mu0(x). Then incumbent fallback has residual value 0 and challenger commit has mean tau(x), matching the abstain/commit reward semantics in expectation.

In the actual protocol mu0 is estimated by the frozen history-only incumbent. A deployable adapter therefore observes, after challenger selection only:

  challenger_reward - frozen_incumbent_prediction(x)

not challenger_reward - realized_counterfactual_incumbent_reward.

This introduces nuisance baseline-estimation error. It also preserves the frozen selected-action-only feedback rule.

## 3. Immediate assumption mismatch to audit

Our manager contexts are learned/high-dimensional representations rather than a physical Euclidean domain with a justified known Lipschitz constant. L is therefore not a frozen benchmark quantity. A single L chosen after test results would be invalid; the correct audit is a preregistered sensitivity grid plus explicit applicability caveat.

## 4. Frozen-protocol mechanical replay

Implemented src/probe_headless/t33_risk_abstention_adapter.py. The adapter imports the official MainAlgorithm directly from the audited repository/HEAD rather than reimplementing its decision rule.

Mapping:

- official abstain -> execute frozen incumbent;
- official commit -> execute challenger;
- commit feedback -> challenger realized reward minus frozen history-only incumbent prediction;
- realized unchosen incumbent reward is never exposed to the learner;
- benchmark reward/stream/split/context remain Task31-frozen.

Because no defensible benchmark-specific Lipschitz constant L exists, preregistered sensitivity L={0,0.1,1,5} is reported with sigma=0 (the offline replay reward matrix is deterministic). No value is selected by test performance.

### Results

LLMRouterBench:
- L=0: IOU **-0.17238**, challenger use 87.05%, precision 5.86%, recall 91.68%.
- L>=0.1: IOU **-0.21195**, challenger use 100%, precision 5.58%, recall 100%.

ASlib SAT12-ALL:
- L=0: IOU **-0.41678**, use 91.42%, precision 11.66%, recall 95.05%.
- L>=0.1: IOU **-0.46715**, use 99.94%, precision 11.34%, recall 100%.

BBB:
- L=0: IOU **-0.03043**, use 99.71%, precision 3.90%, recall 99.26%.
- L>=0.1: IOU **-0.03065**, use 100%, precision 3.92%, recall 100%.

Artifacts:
- data/tool_results/divergence/t33_risk_abstention_llm.json
- data/tool_results/divergence/t33_risk_abstention_aslib.json
- data/tool_results/divergence/t33_risk_abstention_bbb.json

## 5. Why the official mechanism collapses here

For every L>0 sensitivity point, no negative bins are certified and the algorithm effectively commits everywhere. This is not a coding discrepancy: in the frozen high-dimensional learned context spaces, w=T^(-1/(n+2)) is coarse and the Lipschitz confidence/lip-correction term prevents negative certification. Bin reuse is also limited (mean collision roughly 19.7% LLM, 10.0% ASlib, 1.5% BBB), so local evidence does not accumulate fast enough.

L=0 removes the Lipschitz correction and can certify some negative bins, but it assumes a constant reward function inside the geometry and still yields strongly negative IOU. It is a sensitivity endpoint, not a defensible tuned setting.

## 6. Amendment decision

**Decision: related-work + appendix diagnostic, not a mandatory main-table baseline under Task31 v1.**

Reason:

1. The problem/formulation is close enough that the paper must cite and discuss it.
2. A mechanical official-code adaptation is possible and has now been executed.
3. Its core known-L Euclidean smoothness/trusted-region assumptions are not justified by any of the three frozen manager representations. Choosing a new metric/representation/L specifically for this algorithm would change the protocol and create an arbitrary weak/strong adaptation.
4. Under the faithful no-representation-change audit, the method degenerates to near-always-commit and is clearly noncompetitive.

Therefore the close-neighbor objection is closed without silently altering the Task31 main table. The appendix may report this audit as an assumption-mismatch diagnostic.

> 状态：✅ 已完成
