# Known-cost isolation diagnostic

The cost-isolation algebra and inherited trajectories passed. Moving known cost outside the fitted response changed threshold decisions, but did not produce a promising solution against confidence_cascade. No model settings or features changed after the smoke run.

| Original-order method | Utility/input | Calls | Wrong | Refused | Unchanged-action calls |
|---|---:|---:|---:|---:|---:|
| stop_only | -0.20252078 | 722 | 60 | 316 | 0 |
| confidence_cascade | -0.19594183 | 897 | 74 | 234 | 93 |
| pair_mean_gain | -0.20286704 | 872 | 66 | 287 | 121 |
| net_gain_ridge | -0.20407202 | 884 | 65 | 294 | 136 |
| known_cost_ridge | -0.20387812 | 895 | 65 | 293 | 146 |

Known-cost minus old net-gain ridge is +0.00019391/input on the original order; known-cost minus confidence_cascade is −0.00793629/input. Calls increase from 884 to 895. The candidate is not promoted.

## Exact source of the original-order difference

| Call group | Episodes | Old gain sum | New gain sum | Utility difference sum |
|---|---:|---:|---:|---:|
| neither | 539 | 0.00000 | 0.00000 | +0.00000 |
| retained_same_provider | 152 | -1.27000 | -1.27000 | +0.00000 |
| cancelled | 10 | 0.15000 | 0.00000 | -0.15000 |
| added | 21 | 0.00000 | 0.29000 | +0.29000 |
| switched_provider | 0 | 0.00000 | 0.00000 | +0.00000 |

The ten cancelled calls include nine negative gains and one positive gain, producing a net loss of 0.15. Twenty-one added calls produce 0.29. Retained calls and provider switches contribute zero, so total improvement is exactly 0.14/722. Counting only saved call costs would misdescribe the result.

Top candidate changed on 0 episodes; the positive request decision changed on 33. Across 2,166 C candidate rows, 34 crossed zero. Score shifts range from -0.00440002 to 0.00168930; 524 are positive and 1642 negative. The effect is not a uniform negative score offset.

## Descriptive arrival-order sensitivity

Each fold keeps exactly the same molecules and uses 128 uniform permutations from seed 20261009 + fold. Methods share every order and replay separate causal token controllers. The original order is excluded from these statistics. These are descriptive statistics, not confidence intervals or additional independent molecular samples.

| Known-cost contrast | Mean | Sample SD | 5th percentile | Median | 95th percentile | Positive orders |
|---|---:|---:|---:|---:|---:|---:|
| vs stop_only | +0.00265127 | 0.00175471 | -0.00029086 | +0.00281856 | +0.00528116 | 120/128 |
| vs confidence_cascade | -0.00554341 | 0.00301338 | -0.01017521 | -0.00546399 | -0.00018629 | 7/128 |
| vs pair_mean_gain | -0.00106930 | 0.00134288 | -0.00309765 | -0.00105956 | +0.00105679 | 24/128 |
| vs net_gain_ridge | -0.00025753 | 0.00086153 | -0.00150970 | -0.00047091 | +0.00124654 | 37/128 |

Only 7/128 permutations exceed cascade and 37/128 exceed the old net-gain ridge. The small original-order improvement over old ridge is not a consistent descriptive order effect.

## Reproducibility

Seven focused tests passed. Fold 0 smoke completed in 0.526 s; full five-fold processing of all 129 orders completed in 2.069 s. There were no failed experiment attempts. All 2,888 inherited original-order policy/evaluator payloads match after method-name mapping.
Maximum algebra identity error across B/C paired rows: 3.24e-16, below the frozen 1e-10 tolerance. No A refits, provider inference, API calls, GPU work or confirmation reads occurred.

- `full_v1/config.json`, source/card snapshots and `artifact_manifest.json`: pre-computation provenance and artifact hashes.
- `full_v1/fold*_models.json`: old coefficients, theta_delta, h = H^{-1} X^T W 1 and Gram matrices.
- `B_training_targets.jsonl`, `score_shifts.jsonl`: every B response and B/C score-shift identity.
- `policy_decisions.jsonl`, `evaluator_outcomes.jsonl`, `original_call_changes.jsonl`: original-order legal evidence, terminal results and exact utility decomposition.
- `foldN/orders.npy`: shape (129, N), with row 0 original and rows 1–128 permutations.
- `foldN/receipts.npz`: every actual request, selection, terminal action, token state, loss and utility, shape (129, N, 5). `receipt_schema.json` describes axes and codes. Together the arrays preserve 465,690 episode/method outcomes.
- `fold_order_results.jsonl`, `pooled_order_results.jsonl`, `pooled_order_utilities.npy`: every order summary; descriptive statistics exclude row 0.

## Reuse

The known-cost control is reusable for any later immediate-state or delayed-feedback comparison, so changes in cost treatment cannot be misattributed to a new structure. Current evidence does not justify further basis or threshold search. Preserve the negative result and return to mechanism review; no scientific gate or independent-confirmation claim follows.

Source SHA256: `d06160604d47ed30c88615cdc3b572ff77a3ceb799af952af81e6ede66af6479`.
Card SHA256: `9d1cacac95d1730e0a3034d4f80bb9effb9ae51cfc8b4a1f27c70d5660a2400d`.
