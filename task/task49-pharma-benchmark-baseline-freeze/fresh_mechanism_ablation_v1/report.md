# Fresh mechanism ablation — correction report

## Status

This CPU replay is retained as an **exploratory implementation diagnostic**.
It is not a preregistered mechanism experiment and it does not support a
selected-provider credit-assignment claim. The experiment card was written
after the two executions, so neither its mechanism hypothesis nor its
falsification rule is treated as preregistration. The raw receipts and the
earlier interpretation remain archived; this report is the authoritative
interpretation.

## What was executed

The replay used the sealed provider prediction receipt, confirmation manifest,
and five-fold fit-prior cache. It ran locally on CPU; no provider inference,
LLM call, or new GPU job was used. Natural and controlled conditions each had
the same 210 streams (7 methods × 5 folds × 2 seeds × 3 delays) and 98,910
events. The Shared-Credit implementation and its independent replay checker
were both internally consistent.

## The design flaw

The implemented Shared score is

$$q^{\\mathrm{Shared}}_{a,t}(x)=q^0_a(x)+c_t-\\bar r_a,$$

where $c_t$ is common to every provider. Consequently,

$$\\arg\\max_a q^{\\mathrm{Shared}}_{a,t}(x)
 =\\arg\\max_a\\left[q^0_a(x)-\\bar r_a\\right].$$

The pooled feedback moves every score by the same amount and therefore cannot
change the ranking. It also does not share MIRA-Relay's cold-start action
sequence: the fit-rate subtraction is present from the first decision. The
observed Local--Shared difference therefore mixes a static initial
reordering with the Local update rule. It cannot identify provider-local
credit assignment or show that pooled feedback is insufficient.

## Observed values (descriptive only)

The following values are retained to make the failure auditable. They are not
paper mechanism evidence. Values are pooled correctness differences in
percentage points. No confidence interval is attached to these pooled
event-level values in this correction report.

| method | natural pooled Δ vs Frozen | controlled pooled Δ vs Frozen |
|---|---:|---:|
| MIRA-Local | 0.00 | +14.18 |
| MIRA-Shared (static recentering) | −0.25 | +7.13 |
| MIRA-Relative | 0.00 | +10.15 |

The direct Local−Shared pooled differences are +0.2548 pp (natural) and
+7.0488 pp (controlled). A separate equal-weight scaffold analysis gives
−0.4211 pp (natural, 95% CI [−2.44,+1.34]) and +6.4875 pp (controlled,
95% CI [+3.11,+9.98]); these are a different estimand and are reported only
with their own point estimates. The earlier pairing of scaffold intervals with
pooled point estimates was an estimand mismatch and is withdrawn.

## What this does and does not establish

The replay confirms that the recorded static recentering implementation,
state transitions, action sequence, and receipt mutations are reproducible.
It does not isolate the source of the controlled gain. The controlled MIRA
result remains a real response to the declared evaluator-side drift when
compared with Frozen and the modern online controls, but the mechanism
necessary for that response remains open. The natural result is not a safety
or equivalence proof; it reports no aggregate improvement in this stationary
panel while containing both rescued and harmed switches.

The four fresh providers are singleton families. This run cannot identify
family-shared versus provider-independent residuals, and it says nothing about
verification, refusal, provider arrival, hERG/AMES, or end-to-end agent
training.

## Evidence and reuse

The raw JSONL, configs, summaries, and derived analyses are unchanged and
remain separate. The Shared replay checker verifies the implementation but is
not evidence for the causal interpretation:

- runner: `src/probe_headless/run_fresh_mechanism_ablation_v1.py`, SHA256
  `8198b8756cb25240b7ff665f1f2e2c280c55ec4881991539e17a19752ac3ba2b`;
- policy: `src/probe_headless/fresh_shared_credit_policy.py`, SHA256
  `7998d0f5ba735d766a5bbfca6d5201dfc776b09ae82fdaba827e29b68a205c1d`;
- analyzer: `src/probe_headless/analyze_fresh_mechanism_ablation_v1.py`, SHA256
  `978d9989ffc8d0f8fb5e11ef37ab33c2c5837f27813b573cc5a7d5a908071a90`;
- Shared replay checker: `src/probe_headless/verify_fresh_shared_credit_replay_v1.py`,
  SHA256 `6bfdbc2a73b079756b59360e2760795c890f698e5649d827eb97264df90e495e`.

The next valid mechanism test must match Local's initial scores and actions,
then vary only released-state consumption: a no-update scorer, a fixed
provider-permutation credit control, and Local. Its primary contrast must use
one estimand for both the point estimate and scaffold-cluster interval.
