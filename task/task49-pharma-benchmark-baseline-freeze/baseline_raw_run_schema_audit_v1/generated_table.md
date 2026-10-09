# Baseline raw-run schema audit v1

This report is generated from existing JSONL only. It does not rewrite logs or make missing fields look present.

| run | status | events | decision missing | terminal missing | feedback missing | state-hash coverage |
|---|---:|---:|---|---|---|---|
| matched_online_sgd | ok | 28094 | event_id, menu, scores, propensity, feature_hash, released_feedback, decision_reads, state_hash_before, state_hash_after | event_id, entity_id, tick, provider_id, due_tick, output_status, selected_output, correctness, unknown_truth, state_hash_before, state_hash_after | unknown_truth, state_hash_before, state_hash_after | none |
| pilot_adapted | ok | 26054 | released_feedback, decision_reads, state_hash_before, state_hash_after | unknown_truth, state_hash_before, state_hash_after | unknown_truth, state_hash_before, state_hash_after | none |
| bayesianrouter_adapted | ok | 19390 | event_id, menu, scores, propensity, feature_hash, released_feedback, decision_reads, state_hash_before, state_hash_after | event_id, entity_id, tick, provider_id, due_tick, output_status, selected_output, correctness, unknown_truth, state_hash_before, state_hash_after | unknown_truth, state_hash_before, state_hash_after | none |
| greedy_linucb | ok | 26145 | none | none | none | decision:state_hash_before, decision:state_hash_after, terminal:state_hash_before, terminal:state_hash_after, feedback:state_hash_before, feedback:state_hash_after |
| mira_safeswitch_v2 | ok | 78156 | menu, propensity, feature_hash, released_feedback, decision_reads, state_hash_before, state_hash_after | correctness, unknown_truth, state_hash_before, state_hash_after | unknown_truth, state_hash_before, state_hash_after | none |
| random | ok | 12996 | released_feedback, decision_reads, state_hash_before, state_hash_after | unknown_truth, state_hash_before, state_hash_after | unknown_truth, state_hash_before, state_hash_after | none |

## Interpretation

- An alias match records what the old run actually used; it is not a claim that the run satisfies every contract field.
- Missing terminal events mean an independent verifier cannot reconstruct output-status and feedback-release semantics from that run alone.
- Missing `decision_reads`/released-feedback fields mean the log does not independently prove the pre-choice information boundary, even if the implementation contract says it was respected.
- Missing state hashes mean the verifier cannot check every state transition byte-for-byte.
