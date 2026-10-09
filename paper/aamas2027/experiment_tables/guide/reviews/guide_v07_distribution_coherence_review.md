# Guide v07 distribution and coherence review

Date: 2026-10-08. Scope: read-only check of current `guide.pdf`/source, v07 manifest, arrival audit/anchors, conditional requirements, prior scientific reviews, and master manuscript table mapping. No Guide edits and no new experiments.

## Verdict

**Overall: PASS with two interpretation repairs recommended; no numeric inconsistency found in the checked distribution summaries.** The key estimands and denominators align between PDF, anchors, arrival audit report, and manifest. The report properly distinguishes observed order heterogeneity, Monte Carlo uncertainty in the mean, and a binomial interval for win frequency. Future candidate/deployment uncertainty remains unestimated. The audit's existing trajectory verification is relied upon; this review did not replay policies.

## Prioritized checks

| Check | Status | Evidence / action |
|---|---|---|
| Statistical unit | PASS | One complete uniform permutation of the fixed 157-molecule cohort is the unit; n=128 orders. Shared order across methods/folds/seeds/delays/conditions. Correctly not treating 61,440 streams or 9,646,080 decisions as independent n. |
| Per-order denominator | PASS | 4,710 records/order = 157 molecules × 5 fixed fit folds × 2 fixed seeds × 3 delays; primary contrast averages these 30 configurations. Explicit in PDF, manifest, anchor JSON and audit report. |
| Arrival law / scope | PASS | Original identity order excluded from 128 Monte Carlo orders. Inference concerns uniform orders on already-viewed cohort and fixed policies. Controlled stress begins at tick 78, and reordering changes both histories and which molecules are exposed to flipped labels; Guide says so. |
| Primary contrast and statistics | PASS | Controlled pooled MIRA−matched SGD mean +4.7363 pp, SD 2.3988, MCSE .2120, mean MC interval [4.3167, 5.1558], empirical q2.5–q97.5 [0.4490, 8.6125], wins 125/128, exact win-probability interval [0.9330,0.9951]. Values round consistently across PDF and anchors/report. |
| Mean vs distribution intervals | PASS | Page labels order quantiles as not mean CIs, and labels MC interval and exact win-frequency interval separately. Range includes 3 negative orders, so a positive central quantile is not presented as universal per-order success. |
| SD labels | PASS | Page 1 identifies descriptive spread across five fit-fold means, not SE. Page 5 calls figures order SD and distinguishes them from fit SD. Fixed natural Frozen is order-constant; its 0 SD is compatible with natural MIRA order SD .49 pp. |
| Covariance / paired variance | PASS | Table 16 uses common-order covariance and the variance identity. Controlled MIRA−SGD paired SD 2.40 vs independence counterexample 3.22; MIRA−Frozen 2.46 vs 2.06; Local−permutation 4.39 vs 4.00. Anchors and audit report agree. |
| Conditional requirements scope | PASS | `conditional_requirements.json` says same alpha/beta within each recorded stratum and across conditions, no binary draws or altered histories. Guide calls these assumption-driven budgets and says they apply only to original manifest order; manifest repeats that they are not transported to new trajectories. |
| Original-order E1 and arrival distribution | PASS, clarify in prose | Page 1 original-order natural MIRA/Frozen/SGD/LinUCB/Bayesian values are 89.81/89.81/89.21/89.72/89.53, with MIRA tied in raw accuracy. Page 5 natural order means are 89.52 vs Frozen 89.81, SGD 89.05, LinUCB 89.70, Bayesian 89.72, permutation 89.86. The changed comparison is expected: original fixed order versus averaged random orders. Page 5 explicitly says natural MIRA mean is below Frozen. Recommend one short sentence explicitly stating Table 1 is a single original order and Table 14 averages randomized orders, so rank changes are not a contradiction. |
| Method naming / ranking | NEEDS REPAIR | Page 5 uses “MIRA-Relay” in Table 14, but paired contrast labels say “MIRA–Frozen/SGD” while the anchor primary is “MIRA query minus matched online SGD.” Existing Table 1 distinguishes MIRA-Relative and MIRA-Relay and says their seed vectors are identical; however labels could still imply an algorithm or query variant mismatch. Specify that “MIRA” in order summaries means the executed MIRA-Relay/query policy and state this is the existing policy, not a new candidate. |
| Body table crosswalk | PASS | Manifest main mapping matches `main.aux`: E1 tab:main-results=Table 3; E2 credit-ablation=4; E3 learned-prior=5; E4 centering-diagnostic=6; E6 postcall=7; E5 cost-validity=8. Guide page headings map to these labels, including E5 Table 8 and E6 Table 7. |
| Controlled vs natural mechanism | PASS, bounded | Original E1 paired errors show natural MIRA has 82 rescues and 82 harms vs Frozen (net zero); controlled MIRA has 780 rescues vs 112 harms (net +14.18 pp). Arrival-order means preserve a natural disadvantage to Frozen (−0.287 pp), while controlled means give +11.207 vs Frozen and +4.736 vs SGD. This is coherent with an adaptive recovery-under-drift story, not a general natural-accuracy dominance claim. |
| Oracle/fixed controls | PASS with caveat | Fixed-provider best is explicitly not per-molecule oracle; joint native provider vectors preserve correlation. But the controlled intervention is a fit-best-provider label flip at tick 78, not empirical pharmaceutical degradation. Do not read controlled advantage as general deployment benefit. |

## MIRA win/loss coherence

**Evidence.** In the original arrival order, MIRA-Relay ties Frozen at 89.81% natural accuracy, with 82 rescues and 82 harms; it is behind SGD and BayesianRouter numerically. In the 128-order natural distribution, MIRA averages 89.522%, below Frozen 89.809%, SGD 89.053%? (MIRA is above SGD), LinUCB 89.700%, BayesianRouter 89.718%, and permutation 89.863%. Natural MIRA−Frozen averages −0.287 pp and wins only 36/128 orders. Under controlled drift, MIRA averages 86.961%, versus Frozen 75.753%, SGD 82.224%, and permutation 71.395%; the primary MIRA−SGD contrast is positive in 125/128 orders, with 3 explicit losses.

**Interpretation.** The intended claim that feedback/credit helps under this specified drift is consistent with the measured large rescue excess under control. A blanket claim that MIRA improves natural performance is contradicted by the order-averaged Frozen contrast and only 36/128 wins. Natural equal-scaffold contrast is positive (+0.211 pp) while pooled is negative, demonstrating weighting sensitivity; the Guide correctly warns against swapping estimands post hoc.

**Recommendation.** Keep the natural result as a limitation and frame the strongest evidence as conditional robustness to the specified tick-triggered stress law. Preserve the matched pooled contrast as primary; show equal-scaffold as a separately named sensitivity result. Avoid saying MIRA “wins” without naming comparator, condition and aggregation.

## Remaining actionable changes / decision tests

1. Add the two labeling clarifications above: (a) Table 1 single original order vs Table 14 order-average; (b) define MIRA summary shorthand and whether “query” equals the executed Relay policy.
2. Before any broader mechanism claim, test on a preregistered natural chronology or externally justified drift law; current controlled stress jointly changes adaptive histories and molecule allocation to corruption.
3. For a future candidate, evaluate its own matched Frozen, SGD, permutation and rescue/harm tables on common complete histories. Do not transfer v07 SD, interval, win-rate or conditional alpha/beta budget to that candidate.
4. If asserting a useful win probability, choose a minimum practical margin in pp before new data. Current criterion is strictly positive contrast; it does not test meaningful benefit. Report P(Delta > margin) with an interval under the fixed order law.
5. Keep any exploratory secondary MC intervals labeled pointwise. The anchors mark these as not simultaneous; do not select a best method/stratum based on them without multiplicity control or a confirmatory design.

## Evidence, prediction, recommendation

- **Evidence:** the fixed-cohort, fixed-policy 128-order study supports a positive average controlled MIRA−SGD contrast under its stated law; it also records three losing orders and a negative natural MIRA−Frozen mean. Cross-artifact values and body-table mapping checked here agree.
- **Prediction:** no calibrated prediction is available for new molecules, new fitted priors, new seeds, deployment chronology, or unspecified improved candidates. The manifest marks future forecast unestablished.
- **Recommendation:** retain scoped wording and paired metrics; fix the two minor label ambiguities before using the Guide as a standalone reviewer-facing artifact.
