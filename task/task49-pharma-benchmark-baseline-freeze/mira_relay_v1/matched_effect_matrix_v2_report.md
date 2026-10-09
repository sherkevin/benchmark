# Matched effect matrix v2 report

## Receipt and verification

- Receipt: `experiments/logs/task49_mira_matched_effect_matrix_20261004/v2/`
- Fold 0, seed 0, all 145 evaluation molecules; CPU only.
- Three methods × three delays, 9 streams and 3,915 raw events.
- The declared code commit is recorded in `config.json` and was obtained by
  `git rev-parse HEAD` immediately before the run.
- `verification.json` reports `status: verified`.
- The 32-row v1 receipt remains unchanged and is not merged into this result.

## Results

All 145 selected calls were runtime-valid for each method and delay. Selected
correctness and paired effects were:

| Delay | MIRA correct | Independent correct | NoUpdate correct | MIRA vs Independent rescued/harmed | MIRA vs NoUpdate rescued/harmed |
|---:|---:|---:|---:|---:|---:|
| 0 | 127/145 | 126/145 | 126/145 | 1 / 0 | 2 / 1 |
| 1 | 127/145 | 126/145 | 126/145 | 1 / 0 | 2 / 1 |
| 4 | 126/145 | 126/145 | 126/145 | 0 / 0 | 2 / 2 |

MIRA therefore has net paired correctness delta +1 at delays 0 and 1 against
both controls, and 0 at delay 4. This is a single fold, single seed,
development result; it is not a statistical claim.

## Mechanism checks

MIRA executed 85, 86, and 85 residual updates at delays 0, 1, and 4, with 20,
20, and 19 family-transfer events respectively. The provider-independent
control executed 84, 85, and 83 provider-local updates and zero family
transfer events. NoUpdate changed neither residual state. The selected-only
event and delayed-release invariants were independently verified.

## Interpretation and next gate

The expanded cohort now produces a small, directionally positive signal at
delay 0/1 and a null signal at delay 4. The effect is too small and too
correlated (the same 145 molecules and one seed) to support the paper claim.
It does establish that the family-sharing path can change actions on a larger
cohort and that its state transition is distinguishable from the matched
provider-local baseline. The next gate is multiple scaffold-disjoint folds and
seeds with the same preregistered matrix, followed by confirmation only if the
paired interval and transfer diagnostics remain stable.
