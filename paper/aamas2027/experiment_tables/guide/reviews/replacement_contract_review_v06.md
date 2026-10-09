# Independent review of the proposed v06 replacement contract

Author: Codex agent `/root/guide_confidence_rejection_audit`  
Date: 2026-10-08  
Review basis: the coordinating agent's replacement concept, Guide v05, and existing raw evidence. This is a concept review before rendering, not verification of a completed v06 implementation. No experiments were run. Only the two explicitly assigned review files were written.

**Decision: conditionally approve for rendering as an empirical reference and conditional requirements analysis, after the corrections below.** It becomes scientifically useful because the displays share observed paired outcomes and expose the tradeoffs needed to attain aspirations. It does not establish predictive confidence in a future candidate or repair the missing deployable mechanism.

## What can be trusted

- **Evidence:** the observed joint provider vectors, paired method errors, folds, delays, scaffolds, and existing feedback traces are a reliable reference, conditional on the recorded providers, fitted priors, and original arrival order.
- **Conditional design signature:** algebra derived from those records is exact given the declared response assumptions. Its interpretation is a requirements or sensitivity analysis.
- **Future performance:** unresolved. There is no specified candidate, transported response model, or predictive calibration that licenses a success probability, forecast CI, or high posterior credibility.

Use these three labels in the rendered artifact. Distinguish empirical distributions across recorded configurations from distributions over new data or new adaptive trajectories.

## Required correction 1 — Define the response at the row level

For each condition/configuration/molecule row, define observed Frozen correctness F and observed Local correctness L. Define:

- H = 1 if F=1 and L=0; otherwise H=0.
- R = 1 if F=0 and L=1; otherwise R=0.
- L* = L + alpha H − beta R, with alpha,beta in [0,1].

H and R are mutually exclusive. Harm rows receive score alpha, rescue rows receive score 1−beta, and unchanged rows retain their observed correctness. Every value remains in [0,1].

Call L* **fractional conditional correctness**, or expected correctness mass under an explicitly stated response assumption. It is not an observed binary outcome, a counterfactual provider output, or a new policy event. Fractional totals must retain decimals and scenario labels; do not round them into apparent achieved counts.

**Crucial assumption:** global proportions of prevented harms and lost rescues do not determine scaffold, fold, or delay profiles. To derive those profiles by this formula, explicitly assume the same fractional response on every observed H row and every observed R row, across all strata and both conditions. This homogeneity assumption is a sensitivity device, not evidence that a candidate repairs uniformly.

Freeze observed histories, menus, and paired controls. Altering an outcome in a real online policy would alter feedback and later actions; this ledger algebra does not propagate that change. Use no invented state hashes, actions, releases, or recovery traces.

## Required correction 2 — Use one algebra for E1 and E2; abandon independent E2 targets

**Observed Local/Frozen counts:** natural H=82,R=82; controlled H=112,R=780. Thus conditional changes relative to observed Local are:

- Natural: 82 alpha − 82 beta.
- Controlled: 112 alpha − 780 beta.

| alpha | beta | Natural conditional record gain | Controlled conditional record gain |
|---:|---:|---:|---:|
| 0.0 | 0.00 | 0.00 | 0.00 |
| 0.0 | 0.02 | −1.64 | −15.60 |
| 0.0 | 0.10 | −8.20 | −78.00 |
| 0.3 | 0.00 | +24.60 | +33.60 |
| 0.3 | 0.02 | +22.96 | +18.00 |
| 0.3 | 0.10 | +16.40 | −44.40 |
| 0.6 | 0.00 | +49.20 | +67.20 |
| 0.6 | 0.02 | +47.56 | +51.60 |
| 0.6 | 0.10 | +41.00 | −10.80 |

These are analytical scenario outputs, not measured candidate gains. The grid usefully shows that protecting harms while losing even a modest fraction of controlled rescues can worsen the controlled result.

The inverse requirements for the old E1 goals are:

- +48 natural records: alpha − beta = 48/82 ≈ 0.585366.
- +58 controlled records: alpha = 58/112 + (780/112) beta.
- Both exact goals jointly: alpha ≈ 0.5966846794 and beta ≈ 0.0113188258.

This means about 59.7% of observed harmful changes prevented while retaining about 98.9% of observed rescues, under the response assumption. It establishes a requirement, not its feasibility.

**Read-only scaffold reconstruction:** equal-weight scaffold coefficients for H and R are natural 1.267921 and 1.370968 pp; controlled 1.805556 and 16.209677 pp. Keep the observed permutation arm fixed when responding only to Local. At the joint exact E1 requirements above, the derived contrasts are:

| Contrast | Natural pp | Controlled pp |
|---|---:|---:|
| Responded Local − observed Frozen | +0.844078 | +15.297995 |
| Responded Local − observed permutation | +0.436372 | +16.565916 |

The old independent natural targets +1.10 and +1.00 pp cannot be retained under this response. Derive all E2 numbers from the shared rows and weights. Specify whether permutation stays observed or receives its own response; the proposed contract should use the former consistently.

**Interpretation limit:** responded Local versus the unchanged permutation is a conditional contrast against an observed control. A future candidate's source-attribution claim still requires its own matched cyclic intervention and real trajectories.

## Required correction 3 — Preserve empirical dependence

Use one observed four-provider correctness vector per molecule, repeated consistently wherever that molecule appears. Preserve method pairing, scaffold membership, fitted-prior identity, and delay/configuration identity. Do not independently regenerate provider correctness or duplicate the same molecule as independent evidence.

The thirty configurations contain five fitted priors × two seeds × three delays; all natural streams use one arrival order. Deterministic methods duplicate seed histories. Display empirical configuration ranges with these labels; they are not arrival-order robustness or population uncertainty.

The response specification defines fractional first moments on the observed ledger. It does not define a future joint stochastic law or candidate covariance. Avoid calling it a predicted joint distribution.

## Required correction 4 — E3/E4 must retain cohort and intervention boundaries

For natural development, the shared fixed-menu partition is 471 all-correct, 147 mixed, 104 all-wrong. E3 Corrected Frozen's mixed skill is 108/147=73.47%. E4 Corrected Query's natural mixed skill is 327/441=74.15%; 441 is three delayed appearances of the same 147 mixed molecules.

An aspirational +1 pp overall requires about +4.91 pp mixed skill at this mixture: 7.22 expected additional correct molecules in E3, or 21.66 expected additional correct records in E4. An actual integer-count acceptance rule requires at least 8 molecules or 22 records, respectively. Keep a rate aspiration and a realized-count decision threshold distinct.

Each score/prior scale needs its own Frozen counterpart. Compare update benefit within that scale and against the strongest matched update. Different prior scales change provider exposure and therefore the stress response. Separate main effects and interaction; do not add independent E3 and E4 gains into an invented combined method.

**Controlled E4 correction:** the native natural partition cannot be reused unchanged after the evaluator-side provider label flip. Reconstruct post-flip joint correctness by fold, decision tick, and flipped provider, then obtain controlled all-correct/mixed/all-wrong strata. A native all-wrong molecule can become correct under the flip, and a native mixed molecule can lose its only correct provider. If this reconstruction is unavailable, restrict the partition-based response to natural E4 and state the controlled limitation.

The development mixture also differs from fresh natural 110/40/7. Applying development mixed skill to fresh performance requires an additional transport assumption.

## Required correction 5 — E6 should expose a within-fit swap budget

Join the current confidence gate to its always-accept arm by fit and molecule to obtain correctness of refused outputs. In the refusal ledger, `answer_correct=0` for a refused action does not mean its unaccepted provider prediction was wrong.

The verified matched cells are:

| Fit | Accepted correct | Accepted wrong | Refused correct | Refused wrong | Accepted total |
|---:|---:|---:|---:|---:|---:|
| 0 | 114 | 5 | 26 | 12 | 119 |
| 1 | 127 | 7 | 11 | 12 | 134 |
| 2 | 129 | 8 | 9 | 11 | 137 |
| 3 | 129 | 8 | 9 | 11 | 137 |
| 4 | 128 | 8 | 10 | 11 | 136 |

Within fit j, a favorable swap count k_j must satisfy 0≤k_j≤min(accepted_wrong_j,refused_correct_j). Preserve each fit's coverage and show sum(k_j)=8 as the old target requirement. The aggregate error count then decreases from 36 to 28 and utility increases by 8/785≈0.01019 at fixed calls and refusals.

This is an oracle-informed opportunity budget, not an implemented gate or evidence that fit-only ranking can realize the swaps. The same molecule/provider outputs recur across fits; eight records are not necessarily eight independent errors. A future gate needs paired risk–coverage evaluation and fit-only ranking controls.

## Rendering and decision gates

Approve the replacement renderer only if:

1. Every observed panel, action/error mask, denominator, and stratum binds to existing source hashes and stable row IDs.
2. Pooled, scaffold, fold, and delay views derive from the same conditional ledger; no old target is independently filled.
3. Response parameters, homogeneity, frozen-history scope, fractional output, and fixed comparator assumptions are printed beside the scenarios.
4. Zero and adverse grid cells remain visible; no future CI, p-value, posterior confidence, fabricated trace, or deployability claim appears.
5. Native and controlled error partitions are distinguished, and E6 coverage is matched within fit.
6. Research aspirations remain reviewable thresholds. Unknown future performance remains explicitly unresolved.

The full scientific method gate remains unchanged: a candidate must be specified, isolated against legal matched controls, evaluated through real feedback trajectories, and confirmed on genuinely untouched data. The proposed reference is useful preparation for that work.
