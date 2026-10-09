# Hierarchical residual sensitivity: fold0 paired metrics

Generated: 2026-10-04T04:40:59.368792+00:00

## Purpose

Compare the completed fold0/seed0 hierarchical residual candidate with the already matched v3 MIRA, Independent, and NoUpdate streams. This is a mechanism gate diagnostic; it is not a confirmation result.

## Definitions

- `action_changes`: number of molecules whose selected provider differs.
- `rescued`: candidate reward 1, baseline reward 0.
- `harmed`: candidate reward 0, baseline reward 1.
- `net = rescued - harmed`.
- `accuracy_difference`: candidate accuracy minus baseline accuracy.

## Results

| delay | baseline | candidate correct | baseline correct | action changes | rescued | harmed | net | accuracy difference |
|---:|---|---:|---:|---:|---:|---:|---:|---:|
| 0 | probe_mira_relay | 127/145 | 127/145 | 3 | 0 | 0 | +0 | +0.000000 |
| 0 | probe_independent_residual | 127/145 | 126/145 | 2 | 1 | 0 | +1 | +0.006897 |
| 0 | probe_no_update | 127/145 | 126/145 | 37 | 2 | 1 | +1 | +0.006897 |
| 1 | probe_mira_relay | 127/145 | 127/145 | 3 | 0 | 0 | +0 | +0.000000 |
| 1 | probe_independent_residual | 127/145 | 126/145 | 2 | 1 | 0 | +1 | +0.006897 |
| 1 | probe_no_update | 127/145 | 126/145 | 36 | 2 | 1 | +1 | +0.006897 |
| 4 | probe_mira_relay | 125/145 | 126/145 | 4 | 0 | 1 | -1 | -0.006897 |
| 4 | probe_independent_residual | 125/145 | 126/145 | 2 | 0 | 1 | -1 | -0.006897 |
| 4 | probe_no_update | 125/145 | 126/145 | 37 | 1 | 2 | -1 | -0.006897 |

## Interpretation

- Against MIRA: the candidate changes 3, 3, and 4 actions at delays 0, 1, and 4. It is tied at delays 0 and 1, and loses one correct selection at delay 4.
- Against Independent: the candidate changes only 2 actions at each delay. It rescues one at delay 0/1 and harms one at delay 4.
- Against NoUpdate: the candidate changes 36–37 actions, but the net gain is only +1 at delays 0/1 and -1 at delay 4.
- The candidate therefore has action-level consequences, but no consistent improvement over MIRA; the preregistered “continue to full folds only if the mechanism shows meaningful action-level consequence” is technically met by the action changes, while the result does not support an effectiveness claim. An independent event/state verifier is still required before scientific interpretation.

## Provenance

- Candidate summary: `experiments/logs/task49_mira_hierarchical_sensitivity_20261004/fold0_seed0/summary.json` (SHA-256 `f2a4f8168feb700f9ba87f25bb95a721bf0c033c43af2524435b9d0362975a69`).
- Matched v3 summary: `experiments/logs/task49_mira_matched_effect_matrix_20261004/v3_fold0_seed0_r2/summary.json` (SHA-256 `fee6859b29c1df17d1f2bd979f61d23cb11b031fd2aa22195619d1c525beb6d3`).
