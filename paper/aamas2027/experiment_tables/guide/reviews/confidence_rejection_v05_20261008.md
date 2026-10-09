# Independent confidence rejection audit — Guide v05

Author: Codex agent `/root/guide_confidence_rejection_audit`  
Date: 2026-10-08  
Scope: read-only inspection of Guide v05, raw reference evidence, and the active benchmark contract. No experiments or canonical artifact edits. This review file was saved at the coordinating agent's explicit request.

**Verdict: LOW confidence as a scientific target distribution.** The measured anchors are useful. The proposed target matrix is a collection of aspirations, with no specified mechanism or joint distribution that makes the six improvements mutually credible. Arithmetic consistency cannot resolve this gap.

| Dimension | Score / 4 | Reason |
|---|---:|---|
| Empirical grounding | 2 | Recorded anchors and menu bounds exist; target magnitudes are handpicked. |
| Joint distribution | 0 | No paired outcomes, covariance, scaffold allocation, or provider error model supports the targets. |
| Adaptive temporal consistency | 1 | Delay contract is stated; the proposed curves lack a causal learning trajectory. |
| Common intervention across tables | 0 | Improved prior, update, ranking, and Relay have no shared candidate definition. |
| Uncertainty calibration | 1 | Avoiding fabricated CIs is appropriate, but no distribution of plausible outcomes replaces them. |
| Attainable error mechanism | 1 | Oracle headroom establishes possibility with truth access; it does not establish learnability from permitted observations. |

These scores assess the target specification, not the integrity of the measured experiments.

## P0 — The question is not identifiable until the candidate is defined

**Evidence:** E3 changes the prior; E4 explicitly retains the old prior; E6 changes a post-call ranking; E1 introduces an undefined improved Relay. See `guide.tex` lines 121, 152, and 194.

**Alternative explanation:** A future E1 gain could arise entirely from better initialization, while attribution and delayed adaptation contribute nothing. A better E6 ranking could succeed independently of routing.

**Recommendation / decision test:** Define one candidate with fixed features, fitted parameters, update rule, and feedback permissions. Evaluate a prior × update factorial, then the combined method. Freeze the post-call gate separately. An online mechanism claim fails if its gain disappears against the candidate's own Frozen version or survives removal of the claimed update mechanism.

## P0 — The matrix lacks the paired error structure scientists need

**Evidence:** Observed natural Local/Frozen rescues 82 decisions and harms 82. Local/permutation rescues 106 and harms 114. Its equal-weight scaffold contrast is −0.305 pp, with the recorded interval [−1.416,+0.762] pp. The guide replaces these with rescue/harm 110/62 and a +1.00 pp attribution target without identifying which errors change or why.

The raw provider panels show strong joint structure. In provider order logreg/MLP/RF/graph, development has 104 `0000` and 471 `1111` molecules; fresh has 7 `0000` and 110 `1111`. Independent provider Bernoulli errors would destroy this structure and overstate routing opportunities.

Read-only reconstruction of the full observed vector frequencies:

| Vector | Development | Fresh natural |
|---|---:|---:|
| 0000 | 104 | 7 |
| 0001 | 8 | 3 |
| 0010 | 1 | 0 |
| 0011 | 15 | 1 |
| 0100 | 3 | 1 |
| 0101 | 2 | 0 |
| 0110 | 0 | 0 |
| 0111 | 9 | 7 |
| 1000 | 20 | 3 |
| 1001 | 0 | 0 |
| 1010 | 2 | 1 |
| 1011 | 8 | 0 |
| 1100 | 13 | 3 |
| 1101 | 1 | 0 |
| 1110 | 65 | 21 |
| 1111 | 471 | 110 |

**Recommendation / decision test:** Build the reference from molecule-level four-provider correctness vectors, paired method actions, scaffold IDs, and score margins. Show rescue/harm, unchanged errors, action changes without correctness changes, and overlap across methods and delays. Reject any scenario that changes natural correctness on an all-correct or all-wrong molecule while retaining the fixed provider menu.

## P0 — Thirty configurations do not supply arrival-order uncertainty

**Evidence:** Read-only inspection found one identical molecule arrival sequence across all 270 natural method streams. Across each method's 30 configurations, Frozen has five distinct action histories, Relay and SGD have fifteen, and Random has two. The configurations vary priors, seeds, and delays; they are not thirty independent arrival trajectories.

The guide's smooth decreasing delay targets replace an observed nonmonotone pattern. Faster feedback need not improve every finite trajectory when updates are noisy.

**Recommendation / decision test:** Retain observed delay results as an empirical reference. Specify causal signatures: identical update-only actions before first feedback; no drift-specific action response before affected selected feedback arrives; post-release recovery measured by cumulative error and recovery lag. Test new arrival orders by executing the full adaptive policy. Treat fixed-trace reweighting and order sensitivity as distinct from population uncertainty.

## P1 — Headroom is real, but the proposed rescue mechanism is missing

**Evidence:** E3's 591/590 targets imply mixed-molecule success 120/147 = 81.63% and 119/147 = 80.95%, versus Corrected Frozen's observed 108/147 = 73.47%. E4's natural target implies 357/441 = 80.95% mixed success, versus Query's 327/441 = 74.15%.

For Corrected Query, 109 of 114 avoidable development errors already had feedback released from at least one correct provider. This counts provider-level coverage, not relevant local context.

**Prediction:** Merely releasing feedback earlier is unlikely to explain the full proposed +30-record E4 gain. A better conditional representation or use of feedback would be required.

**Recommendation / decision test:** Stratify errors by all-menu-wrong, pending correct-provider feedback, never-selected correct provider, and released correct-provider feedback; then by score margin and context support. A pending-feedback mechanism should preferentially improve its designated stratum. If gains instead come from static ranking, revise the mechanism claim.

## P1 — E1 and E2 cannot be independently filled to convenient means

**Evidence:** E1 uses pooled decision accuracy; E2 uses equal-weight scaffold means. The proposed +1.02 pooled gain and +1.10 scaffold gain could coexist, but no common paired allocation demonstrates that they do.

**Recommendation / decision test:** Derive both from the same candidate outcomes and scaffold weights. Show effect concentration and leave-one-scaffold-out sensitivity. A robust effect must not depend on a small number of large or unusually favorable scaffolds.

## P1 — E6 needs ranking evidence at matched coverage, with dependence preserved

**Evidence:** The current gate accepts 663 records: 627 correct and 36 wrong. Its refused outputs include 65 correct and 57 wrong. Eight favorable swaps are logically available, but permitted features have not been shown to identify them. Accepted counts by fitted policy are 119,134,137,137,136. Matching only the pooled count permits redistribution across conditions.

**Recommendation / decision test:** Compare the proposed gate against fit-only calibration/ranking controls on identical selected outputs. Report paired risk–coverage curves, within-policy coverage, swap identities, and utility sensitivity. Use repeated random acceptance draws for the conditional random null. If routing changes too, recompute gate outcomes on that routing policy; the old E6 records cannot represent the combined intervention.

## Required replacement

The replacement should be a reference-and-design specification containing:

- Observed joint provider vectors; paired method errors/actions; fold, scaffold, delay, release, margin, and time strata; source hashes and one shared aggregation map.
- One candidate contract and its necessary controls, including a common Frozen counterpart.
- Research aspirations expressed as practically meaningful decision thresholds. Remove exact future counts and unsupported narrow seed gaps.
- Observed variability and negative outcomes from comparable interventions; fixed-menu logical bounds shown separately. Keep zero and harmful effects plausible until evidence excludes them.
- Explicit temporal predictions and failure branches.
- An uncertainty contract distinguishing fitted-policy variation, arrival trajectories, scaffold generalization, and conditional randomization.
- A feasibility gate requiring efficacy and kernel budgets jointly, with real timing and memory measurements for new capacity.

Cached CPU replay makes diagnostic controls feasible. The principal constraints are the undefined intervention, only forty fresh mixed molecules, correlated repeated outcomes, and the inspected confirmation cohort. A new candidate needs genuinely untouched confirmation before a generalization claim.

## Evidence inspected and reusable assets

- `paper/aamas2027/experiment_tables/guide/{guide.tex,target_spec.json,README.md}`.
- `docs/研究主线/生效/03-Benchmark与Baseline.md` and its active evaluation standard.
- `experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/{events.jsonl,provider_predictions.json,config.json,summary.json}`.
- `experiments/logs/task49_fresh_mechanism_ablation_v2_20261007/{natural_v2_analysis.json,controlled_v2_analysis.json}`.
- `experiments/logs/task49_headroom_feedback_20261007/v1/{summary.json,molecule_rows.jsonl,decision_rows.jsonl}`.
- `experiments/logs/task49_calibration_centering_20261007/v1/config.json`.
- `experiments/logs/task49_postcall_fresh_confirmation_20261008/{analysis_audit_v3/analysis_audit_v2.json,full_v1/policy_receipt.jsonl,full_v1/evaluator_ledger.jsonl}`.

The raw panels, event traces, headroom strata, and matched mechanism controls are reusable foundations. No new empirical run was performed; descriptive inspections above were computed in memory from existing files.
