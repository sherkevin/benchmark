# Guide v08 target specification design (review only)

Date: 2026-10-08  
Scope: read-only synthesis of the effective benchmark contract, evaluation standard v006, Guide v07, current Gate closure record, development baseline report, and public baseline snapshot. This file proposes the target matrix for a future Guide v08. It does not edit the main Guide and does not report new experiments.

## 1. Source ledger and confirmed facts

| ID | Confirmed fact | Source |
|---|---|---|
| S1 | Primary submission slice is public TDC BBB truth, four fresh providers, 157 scaffold-disjoint confirmation molecules, one call, selected-only feedback, delays `D={0,1,4}`, and evaluator-side controlled correctness drift. | `docs/研究主线/生效/03-Benchmark与Baseline.md` |
| S2 | Gate A/B/C are independent; each is scored on four 0–4 subitems, requires at least 12/16, and cannot be rescued by total score. Current snapshot is A=11/16 HOLD, B=11/16 HOLD, C=10/16 HOLD. | `docs/研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md`; `gate_closure_decision_v1.md` |
| S3 | H1–H10 must all PASS; B1–B8 ≥3, B9–B15 ≥2, total ≥49/60. Stage A needs E1–E5 evidence; Stage B adds frozen untouched E6. | evaluation standard v006 |
| S4 | Current v07 is a fixed-cohort distribution reference: 128 uniform arrival orders over the viewed 157 molecules and existing policies. It explicitly does not predict a future candidate, new molecules, or deployment drift. | `versions/v07_20261008/README.md`; `guide_v07_distribution_coherence_review.md` |
| S5 | Observed natural result permits MIRA below Frozen (order mean MIRA–Frozen −0.29 pp; 36/128 wins); controlled drift gives MIRA +4.74 pp vs matched SGD (125/128 wins), with three losses. | `versions/v07_20261008/guide.tex`; v07 distribution review |
| S6 | Development evidence shows current strict Matched Online-SGD below its Frozen (−0.946 pp), and modern adapted routers do not provide consistent superiority. | `task/task49-pharma-benchmark-baseline-freeze/development_master_table_v1/report.md` |
| S7 | Public papers provide motivation for LinUCB/LinTS and Bayesian feedback routing, but their raw numbers are not comparable to BBB accuracy and cannot populate this Guide. | `/Users/jingwu/work/reasonable_results/pharma_tool_selection_public_baselines_v1/report.md` |
| S8 | Candidate evidence must use its own Frozen, matched online/replay controls, permutation/attribution control, event logs, failure/unknown denominators, paired uncertainty, cost, and recovery. Old v07 distributions cannot be inherited. | `gate_closure_decision_v1.md`; evaluation standard v006 |

## 2. v08 design principle

Guide v08 should be a **target and acceptance matrix**, not a forecast table. Every target row has: claim, comparator, estimand, expected direction (which may be null or adverse), distribution object, error/failure accounting, evidence receipt, and a kill rule. “Passing” means the declared contract and evidence close; a positive effect is required only for claims that explicitly assert an improvement.

The primary asymmetry is intentional: natural performance may be equal to or below Frozen; controlled feedback performance may exceed the matched online baseline. A natural loss is not a kill if the claim is scoped to controlled recovery, but it is a kill for a generic natural-accuracy superiority claim.

## 3. Gate closure target matrix

| Gate/subitem | v08 target evidence | Direction/acceptance | Distribution and error requirements | Kill / hold rule |
|---|---|---|---|---|
| **A1 truth** | Frozen truth manifest, source/version/hash, label regeneration and conflict receipt for BBB; confirmation manifest hash. | PASS only with reproducible public truth and fixed endpoint semantics. | Raw and cleaned denominators; duplicate, conflict, unknown and timeout counts. | Any mutable download, silent conflict handling, or unrecoverable label generation → Gate A FAIL. |
| **A2 routing protocol** | Machine-validated selector contract: input/menu/action, selected-only release, delays, budget, failure semantics, scope. | PASS when every method can be replayed under one schema. | Per-event visible history/release time; illegal traces rejected. | Full-panel/pre-choice truth or unselected output access → H5 FAIL. |
| **A3 split/independence** | Development/confirmation and molecule/scaffold overlap audit; provider training-member and checkpoint lineage audit. | PASS when confirmation is untouched and overlap risk is explicit. | Molecule and scaffold denominators; lineage-aware strata. | Any confirmation leakage or undisclosed provider exposure → H3/H8 FAIL. |
| **A4 provider/family independence** | Four-provider fresh checkpoint receipts, family registry, correlated-error report, family-balanced sensitivity. | PASS with claim narrowed to fixed deployed menu where model-fresh proof is absent. | Family macro, pooled result, exposure, joint error patterns; no provider-count inflation. | Treating correlated variants as independent evidence or missing lineage → Gate A HOLD/FAIL. |
| **B1 roles** | Random, fit-only Best Fixed, Retrospective Best Fixed (evaluator-only), candidate Frozen, matched Online/Replay-SGD, LinUCB, BayesianRouter, and declared mechanism controls. | PASS if each row answers a distinct question; Retrospective row excluded from deployable ranking. | Same menu, features, feedback, delay, budget and seeds. | Missing strongest legal nearest or role-mixing privileged controls → B1 <3. |
| **B2 task match** | Adapter contract for every baseline; public results retained as motivation only. | PASS when all ranked rows are re-run under BBB contract. | Record likelihood/prior/feature and adaptation differences separately. | Copying cross-domain values into BBB table → B2/B10 FAIL. |
| **B3 fidelity** | Source commit/checkpoint/license, adapter tests, hyperparameter and tuning budget receipt. | PASS on independent action/state recovery. | Per-method failure/unknown/refusal and cost fields. | Unverifiable adapter, hidden tuning or mixed priors → HOLD. |
| **B4 fairness** | Independent fairness checker over input/menu/feature/init/feedback/delay/budget/compute. | PASS when candidate may use only declared state and no extra labels. | Cold/common-warm/online initialization strata; p50/p95 selection/update, memory, calls. | Any candidate-only privilege → H6/H8 FAIL. |
| **C1 coverage** | A single matrix containing E1–E5 plus frozen E6; each row mapped to one RQ and one claim. | PASS requires every required experiment receipt, including negative results. | Complete stream counts, molecule/scaffold units, seed/delay strata. | Missing E1–E5 or only reference values → C1 <3. |
| **C2 closure** | Same-contract raw JSONL and recovery for all primary baselines and candidate. | PASS when all primary rows are jointly comparable. | Paired episodes/orders; no pooled independent bootstrap over events. | Any baseline missing recovery or contract mismatch → C2 <3. |
| **C3 scientific direction** | Candidate-specific natural and controlled contrasts, mechanism and cost tables. | Natural: equality or candidate ≤ Frozen is allowed. Controlled: candidate should beat matched online comparator only for a controlled-recovery claim. | Report mean CI, order/scaffold quantiles, win frequency, rescue/harm/both-wrong, failure/unknown. | Generic natural-superiority claim fails if candidate < Frozen; controlled claim fails if CI/effect direction misses preregistered practical margin or harms exceed cap. |
| **C4 uncertainty/recovery** | Independent verifier, paired/scaffold-cluster uncertainty, failure and cost accounting, preserved negatives. | PASS with correct statistical object labels; no inherited v07 SD/CI. | Separate fit SD, arrival-order distribution, molecule/scaffold CI, and controlled intervention uncertainty. | Manual-only summary, missing denominator, or predictive language from v07 reference → C4 <3. |

## 4. E1–E6 target matrix

Thresholds below are **pre-registration slots**, not asserted results. Where no defensible numeric margin is available, the target is contract closure plus a directional test; the human must choose any practical margin before execution.

| Exp | Claim and comparator | Allowed / desired direction | Distribution object | Required error and failure accounting | Kill criteria |
|---|---|---|---|---|---|
| **E1 task validity** | There is provider disagreement/headroom; compare Random, fit-only Best Fixed, Retrospective Best Fixed. | Best Fixed > Random on development; candidate need not beat Frozen here. Retrospective is an evaluator control only. | Per-fold OOF accuracy and provider joint correctness; no arrival-order claim yet. | Unique molecule/scaffold counts, conflict/unknown/timeout, provider family macro, all-correct/mixed/all-wrong patterns. | No measurable headroom, truth/provider mismatch, or leakage; do not open confirmation. |
| **E2 primary dynamic comparison** | Candidate vs **own Frozen**, matched Online-SGD/Replay, LinUCB, BayesianRouter under selected-only `D={0,1,4}`. | Natural: candidate may be below Frozen; report as boundary. Controlled: candidate should be > matched online baseline, with preregistered practical margin `m_ctrl` chosen by human; no requirement to beat Frozen if Frozen is advantaged by static labels. | Paired stream/episode means; order distribution only if full adaptive trajectories are rerun. Report mean CI, empirical quantiles and win-frequency interval separately. | Rescue, harm, both-wrong, neutral changes, runtime failure, unknown/refusal, calls and delay strata. | Kill generic natural gain if lower CI ≤0 or candidate < Frozen; kill controlled-recovery claim if lower CI ≤ `m_ctrl` (or harms/failures exceed cap); hold if any missing baseline receipt. |
| **E3 mechanism ablation** | Remove one mechanism at a time; compare candidate with own ablated versions, own Frozen and own permutation. | Mechanism should improve the declared controlled contrast or reduce harm/failed switches; natural superiority is optional. | Paired molecule/scaffold deltas within identical streams and fits; no inherited v07 budgets. | Harmed/rescued by mechanism, switch-away, incumbent errors, family and scaffold clusters, runtime failures. | If full candidate cannot beat its own ablation on the claimed mechanism metric, mechanism claim dies; if only old comparator improves, attribution fails. |
| **E4 dynamic/pressure** | Delay, selected-only queue, provider/family failure, and declared controlled correctness drift. | Candidate may be neutral or worse in natural/no-drift; should recover more than matched online under the specified controlled stress. Transient and persistent outage results remain separate. | Pre/post intervention trajectories, delay-stratified paired effects, persistent/transient distributions, family exposure. | Pending queue, release clock, outage injection receipt, failure/unknown/refusal, target-family exposure, cost. | Any early feedback before release, changed histories, or stress without injection receipt; controlled claim killed if no recovery advantage or cost cap violation. |
| **E5 errors/cost/recovery** | Utility and feasibility versus Frozen/matched controls at same call budget. | Candidate may trade natural accuracy for controlled recovery only if utility contract permits; no unpriced refusal/escalation. | Per-stream calls, latency p50/p95, update cost, memory, crash/replay, utility curves; report Pareto status. | Every failure/unknown/refusal in denominator; break-even refusal/escalation thresholds; raw recovery. | Missing live resource measurements, hidden extra calls, or candidate only winning after dropping failures → E5 FAIL. |
| **E6 frozen confirmation** | One-time untouched confirmation on 157 scaffold-disjoint fresh molecules after method/menu/threshold/epoch/baseline freeze. | Natural result can be tie or below Frozen and must be disclosed. Controlled result may support only the preregistered stress claim and comparator. | Confirmation molecule-level and scaffold-cluster paired CI; no 128-order reference transport unless new orders are actually run. | Confirmation labels never enter tuning; raw JSONL/config/hash, all failures/unknowns, one-shot receipt. | Any label回流, post-hoc tuning, incomplete fresh provider outputs, or claim broader than declared slice → Stage B FAIL. |

## 5. Agreement and disagreement matrix

| Question | Evidence agreement | Remaining disagreement / non-equivalence | Resolution in v08 |
|---|---|---|---|
| Does v07 describe observed distributions? | v07 review and gate record agree: yes, for fixed viewed cohort and policies. | It is not a prospective target or candidate result. | Label every v08 numeric field as target slot, observed reference, or result; never use one as another. |
| Is natural superiority required? | No source requires it; v07 shows MIRA below Frozen under order averaging. | A generic “online improves natural accuracy” story would conflict with evidence. | Make natural ≤ Frozen an allowed outcome and a boundary result. |
| Is controlled superiority plausible? | Existing controlled MIRA–SGD contrast is positive under the declared intervention; development baselines are mostly non-superior. | Controlled drift is evaluator-side, not empirical deployment drift. | Require candidate-specific controlled replay and scope wording; no generic drift claim. |
| Can public LinUCB/Bayesian numbers set BBB thresholds? | They motivate baseline families. | Tasks, arms, feedback and metrics differ. | Use them only as source IDs/rationale; re-run adapters locally. |
| Can v07 CI/SD/win-rate be inherited? | No; v07 itself says future candidates have unknown distributions. | Reference values remain useful for designing logs and estimands. | Recompute all candidate uncertainty from candidate raw traces. |
| Does majority/positive result establish novelty or truth? | No source supports that inference. | Agreement across agents or baselines is not causal evidence. | Require source-level provenance, matched controls and falsification tests. |

## 6. Decisive tests and kill criteria

1. **Contract replay test:** independent checker rejects any trace that exposes truth, unselected output, or premature feedback. Failure blocks all Gates.
2. **Own-control test:** candidate must have its own Frozen, matched online/replay, permutation, and mechanism ablation. Missing one blocks C2/C3.
3. **Natural boundary test:** report candidate–Frozen paired effect. A negative effect is retained; it kills only an unqualified natural-superiority claim.
4. **Controlled recovery test:** preregister `m_ctrl` and maximum harm/failure increase. Candidate must clear the margin against matched online under the declared stress; otherwise controlled claim is killed.
5. **Attribution test:** full method must beat its own ablation on the mechanism metric. Improvement only against an old comparator is non-attributive.
6. **Cost/utility test:** compute break-even refusal/escalation and verify p50/p95, memory and calls from raw receipts. Accuracy gains after dropping failures kill the utility claim.
7. **Confirmation freeze test:** no labels from E6 may alter code, menu, threshold, epoch or baseline. Any modification invalidates Stage B.
8. **Recovery test:** independent raw-log regeneration must reproduce every primary table within predeclared rounding. Otherwise the result remains HOLD.

## 7. Ranked next actions

1. **Write the v08 machine target schema and freeze the human-chosen `m_ctrl`, harm/failure caps, and utility budget.** This is prerequisite for deciding whether a controlled result is meaningful.
2. **Close Gate A evidence gaps:** truth/split/lineage/family receipts and machine contract checker. Current A=11/16 blocks confirmation.
3. **Close Gate B with one common-direct-prior baseline contract:** re-run or explicitly exclude mismatched historical Replay; preserve public papers as motivation only.
4. **Register candidate-specific E1–E5 configs and raw-log paths before any run.** Include own Frozen, matched controls, permutation and ablations.
5. **Run decisive E2/E3/E4 tests first on development data.** Stop if controlled effect misses `m_ctrl`, mechanism attribution fails, or harm/cost caps fail; do not tune toward v07 values.
6. **Complete E5 resource and failure accounting plus independent recovery.** This determines whether any accuracy effect is usable.
7. **Only after Gates A–C reach thresholds, execute one-shot E6 untouched confirmation.** Keep natural degradation and controlled gain as separate rows and claims.

## 8. Unresolved questions for the human

- What practical controlled uplift `m_ctrl` (percentage points) justifies the method, and over which comparator: matched SGD, Replay, or both?
- What maximum harmed-switch, runtime-failure, unknown/refusal, call, latency and memory increases are acceptable?
- Is the paper claim intentionally limited to evaluator-side controlled correctness drift, or should a natural chronology/empirical lifecycle extension be required?
- Should E6 controlled stress be applied to the same 157 fresh molecules after natural confirmation, or be a separately declared stress cohort?
- Which baseline prior is canonical for Gate B (common-direct-prior), and is historical BT Replay retained only as audit-only?
- Is a candidate allowed to be below Frozen in natural accuracy while still being the main method, and how should that trade-off be described in the abstract?
- What family-level aggregation (macro, exposure-weighted, or both) is primary for the final paper?
