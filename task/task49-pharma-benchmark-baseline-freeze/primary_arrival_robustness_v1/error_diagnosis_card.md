# Post hoc error decomposition card

2026-10-08. Frozen before this analysis is executed, **after** viewing the full study's aggregate outcomes. Descriptive diagnosis only; it is not a preregistered confirmatory test, a new experiment, or permission to tune on the 157 molecules.

## Question

The completed runner reports natural pooled MIRA−Frozen as negative but its equal-scaffold counterpart as positive. Which recorded molecule/scaffold contributions account for the sign difference? The comparator remains Frozen, with all 128 orders and the same 30 configurations retained. No positive subset will replace the primary pooled metric.

## Inputs and analysis

Use only the sealed `full_v1` inputs, shared orders, chunk manifests and selected-action/correctness arrays. Verify the full-run manifest hash and each consumed chunk hash. No new provider forward pass, fitting, API call, sampling or model update.

For both natural and controlled conditions, pair MIRA and Frozen by fold, seed and delay. Map each decision tick back to its molecule using the saved order. Save all 157 molecules' correct counts, rescued/harmed counts, action-change counts and net difference; each molecule has 128×30=3,840 paired exposures.

For every scaffold save size, all constituent IDs, summed rescued/harmed/net counts, mean molecule difference, contribution to the pooled effect, and contribution to the equally weighted scaffold effect. Report both the full scaffold table and two fixed size strata: singleton scaffolds and scaffolds containing more than one molecule. Sorting the full table by contribution is a diagnostic display, not a new subgroup claim.

The pooled effect must equal the sum of scaffold contributions under molecule weighting; the scaffold mean must equal their sum under equal-scaffold weighting. Reconcile both against the frozen full-result aggregate within 1e-10 pp. Check net=rescued−harmed, exposure conservation and exact paired identities. A failed identity stops interpretation and preserves the failed run.

Do not compute new CIs, p-values, favorable-subset accuracy or an alternative primary score. Differences can locate where the recorded harm occurs; they do not identify a chemical mechanism, calibration failure or cause of the learned ranking.

## Artifacts and authorization

Phase GO under existing research authorization: one deterministic analysis, bounded to two minutes wall time, 1 GiB memory and 50 MiB additional artifacts. Save config/source snapshot/hashes before execution, JSONL progress/failures, per-molecule and full scaffold CSV/JSON, aggregate reconciliation and a short interpretation. Keep output outside immutable `full_v1`.

Await full independent replay verification before labeling the resulting diagnosis verified or merging it into the paper. The primary study's effect decision and the broader scientific gate are unchanged by this analysis.
