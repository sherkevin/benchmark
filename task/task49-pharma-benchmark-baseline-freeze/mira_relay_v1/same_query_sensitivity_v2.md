# Same-query sensitivity audit v2

**Status:** completed post-hoc mechanism audit; not an effectiveness result.

## Why v2 replaces v1

The first audit (`experiments/logs/task49_mira_relay_same_query_20261003/v1/`) used the score vector recorded at the original decision as its pre-action state. With delayed feedback, other releases can occur before that feedback is applied, so that comparison did not isolate the update being audited. It also used the mutable runtime hazard state for the fallback reference and included unavailable-arm sentinel scores in a norm. `v1/CORRECTION.json` records these defects and is retained.

## Estimand

For each feedback event, keep the released molecule feature and provider menu fixed. Recompute the exploitation action twice:

1. `exploit(state_before)`, where `state_before` is the state immediately before that feedback release;
2. `exploit(state_after)`, where `state_after` is the state immediately after the release.

The fallback hazard is the immutable fit-only beta reference. Challenger hazards use fit counts plus runtime counts from the corresponding state. Unavailable arms are excluded from the action argmax; no norm is computed over their `-1e300` sentinel. This isolates whether the selected feedback update can change the ranking for the same query. It does not measure the next molecule's reward.

## Receipt

The audit was configured before parsing the event stream. The source-bound authoritative receipt is:

`experiments/logs/task49_mira_relay_same_query_20261003/v3/`

The earlier corrected v2 receipt is retained; v3 adds the audit, fit-diagnostic, and encoder source hashes to `config.json`. Each receipt contains `config.json`, `same_query_events.jsonl`, and `summary.json`. The input, replay events, fit-state archive, encoder checkpoint, family registry, and source hashes are recorded in the authoritative config.

The receipt contains 288 feedback records: 3 methods × 3 delays × 32 molecules. The contract remains the fixed fold0/seed0 development replay and has no effectiveness estimate.

## Observed mechanism signal

| method | delay | feedback | residual updates | same-query action changes |
|---|---:|---:|---:|---:|
| ProbeMIRA-Relay | 0/1/4 | 32 each | 22 each | 1 each |
| ProbeNoUpdate | 0/1/4 | 32 each | 0 | 0 |
| NoProbeShield | 0/1/4 | 32 each | 0 | 0 |

The single changed query is `z002750` in all three delay replicas. Before the release its exploitation action is `fpadmet`; after the selected feedback update it is `lgbm_bbb`. The selected `fpadmet` feedback has reward 0. The family-mean and provider-bias changes for this event have L2 norms 1.0328 and 0.0792. The same sequence is replayed for each delay, so the three occurrences are not independent samples.

## Interpretation and boundary

This is evidence that the current MIRA-Relay implementation can alter a fixed-query exploitation ranking after a selected update, and that the no-update controls do not alter their residual parameters. It is not evidence of improved accuracy, transfer across family siblings, calibration, or delay robustness. One changed query in a 32-query development slice is too small for a performance claim. The earlier adjacent-decision counts (`24/23/22`) are not causal evidence and must not be used as a result.

The next scientific gate is a matched development matrix with an independent-residual update control and query-level rescued/harmed counts, followed by unseen-sibling transfer. Full replay and confirmation remain closed until that matrix is preregistered and independently checked.
