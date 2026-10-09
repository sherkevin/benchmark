# Quantitative audit of natural and controlled-drift receipts

日期：2026-10-06（只读重聚合；没有重新执行策略，没有修改回执）

## 数据边界与输入证据

- Natural: `experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/`; 270 streams, 127,170 events; 9 methods × 5 folds × 2 seeds × 3 delays.
- Stress: `experiments/logs/task49_provider_fresh_drift_stress_20261006/v3_bayesian_statehash_run1/`; 150 streams / 70,650 events; 5 methods × 5 folds × 2 seeds × 3 delays; evaluator-side label flip starts at tick 78 only when fit-best provider is selected.
- Candidate manifest: 157 confirmation molecules, 124 Bemis--Murcko scaffold clusters.
- Machine-readable result: `quantitative_audit.json`; analysis code: `audit_receipts.py`. Input and analysis SHA-256 values are recorded in the JSON.

## Hardware and timing semantics

- Both runs are CPU-only macOS arm64 receipts (`macOS-26.3.2-arm64-arm-64bit`, `gpu_used=false`); these are not A800 measurements. Natural Python is 3.9.6, NumPy 1.26.4, RDKit 2025.09.2; stress config records Python 3.9.6.
- `selection_ns_*` and `update_ns_*` are `perf_counter_ns` around the policy decision/release operation per event. They exclude file I/O, provider inference, evaluator scoring, and external service latency. Each stream computes p50/p95 over its event calls; reported timing is the arithmetic mean of those stream-level quantiles.
- Update p95 should therefore be cited as CPU policy-operation overhead, not end-to-end tool latency or GPU throughput.

### Mean stream-quantile timing (microseconds)

These values are the arithmetic mean of per-stream quantiles; they are reported for auditability and are not hardware-normalized.

| Run / method | Selection p50 | Selection p95 | Update p50 | Update p95 | Streams |
|---|---:|---:|---:|---:|---:|
| Natural / BayesianRouter | 107.835 | 133.651 | 72.063 | 88.960 | 30 |
| Natural / Best Fixed | 3.574 | 5.819 | 0.386 | 0.678 | 30 |
| Natural / Frozen | 6.582 | 15.885 | 0.504 | 1.314 | 30 |
| Natural / Greedy LinUCB | 101.663 | 128.627 | 71.988 | 90.896 | 30 |
| Natural / Online SGD | 6.279 | 14.081 | 8.707 | 19.367 | 30 |
| Natural / MIRA-Relay | 13.018 | 27.209 | 1.233 | 2.813 | 30 |
| Natural / MIRA-Relative | 24.294 | 42.016 | 41.597 | 73.297 | 30 |
| Natural / Retrospective Best Fixed | 3.528 | 4.273 | 0.379 | 0.543 | 30 |
| Natural / Random | 39.887 | 67.140 | 2.965 | 5.444 | 30 |
| Stress / BayesianRouter | 112.818 | 137.339 | 75.074 | 92.550 | 30 |
| Stress / Frozen | 6.797 | 17.875 | 0.536 | 1.499 | 30 |
| Stress / Greedy LinUCB | 105.276 | 128.361 | 74.169 | 92.325 | 30 |
| Stress / Online SGD | 6.737 | 14.291 | 9.347 | 20.021 | 30 |
| Stress / MIRA-Relay | 14.455 | 32.468 | 1.472 | 3.464 | 30 |

## Overall selected correctness

Each method has 4,710 scored decisions in natural and stress (30 streams × 157). Percentages are pooled event-row correctness; the JSON also stores mean stream accuracy.

### Natural fresh menu

| Method | Streams | Correct / decisions | Success | Mean stream accuracy |
|---|---:|---:|---:|---:|
| BayesianRouter | 30 | 4217/4710 | 89.5329% | 89.5329% |
| Best Fixed | 30 | 4152/4710 | 88.1529% | 88.1529% |
| Frozen | 30 | 4230/4710 | 89.8089% | 89.8089% |
| Greedy LinUCB | 30 | 4226/4710 | 89.7240% | 89.7240% |
| Online SGD | 30 | 4202/4710 | 89.2144% | 89.2144% |
| MIRA-Relay | 30 | 4230/4710 | 89.8089% | 89.8089% |
| MIRA-Relative | 30 | 4230/4710 | 89.8089% | 89.8089% |
| Retrospective Best Fixed | 30 | 4260/4710 | 90.4459% | 90.4459% |
| Random | 30 | 3945/4710 | 83.7580% | 83.7580% |

### Controlled evaluator-side drift

| Method | Streams | Correct / decisions | Success | Mean stream accuracy |
|---|---:|---:|---:|---:|
| BayesianRouter | 30 | 3781/4710 | 80.2760% | 80.2760% |
| Frozen | 30 | 3456/4710 | 73.3758% | 73.3758% |
| Greedy LinUCB | 30 | 3822/4710 | 81.1465% | 81.1465% |
| Online SGD | 30 | 3958/4710 | 84.0340% | 84.0340% |
| MIRA-Relay | 30 | 4124/4710 | 87.5584% | 87.5584% |

## Delay-stratified selected correctness

Each delay contains 10 streams per method and 1,570 scored decisions.

### Natural fresh menu

#### Delay $D=0$

| Method | Correct / decisions | Success |
|---|---:|---:|
| BayesianRouter | 1406/1570 | 89.5541% |
| Best Fixed | 1384/1570 | 88.1529% |
| Frozen | 1410/1570 | 89.8089% |
| Greedy LinUCB | 1408/1570 | 89.6815% |
| Online SGD | 1398/1570 | 89.0446% |
| MIRA-Relay | 1408/1570 | 89.6815% |
| MIRA-Relative | 1410/1570 | 89.8089% |
| Retrospective Best Fixed | 1420/1570 | 90.4459% |
| Random | 1315/1570 | 83.7580% |
#### Delay $D=1$

| Method | Correct / decisions | Success |
|---|---:|---:|
| BayesianRouter | 1406/1570 | 89.5541% |
| Best Fixed | 1384/1570 | 88.1529% |
| Frozen | 1410/1570 | 89.8089% |
| Greedy LinUCB | 1410/1570 | 89.8089% |
| Online SGD | 1396/1570 | 88.9172% |
| MIRA-Relay | 1408/1570 | 89.6815% |
| MIRA-Relative | 1410/1570 | 89.8089% |
| Retrospective Best Fixed | 1420/1570 | 90.4459% |
| Random | 1315/1570 | 83.7580% |
#### Delay $D=4$

| Method | Correct / decisions | Success |
|---|---:|---:|
| BayesianRouter | 1405/1570 | 89.4904% |
| Best Fixed | 1384/1570 | 88.1529% |
| Frozen | 1410/1570 | 89.8089% |
| Greedy LinUCB | 1408/1570 | 89.6815% |
| Online SGD | 1408/1570 | 89.6815% |
| MIRA-Relay | 1414/1570 | 90.0637% |
| MIRA-Relative | 1410/1570 | 89.8089% |
| Retrospective Best Fixed | 1420/1570 | 90.4459% |
| Random | 1315/1570 | 83.7580% |

### Controlled evaluator-side drift

#### Delay $D=0$

| Method | Correct / decisions | Success |
|---|---:|---:|
| BayesianRouter | 1259/1570 | 80.1911% |
| Frozen | 1152/1570 | 73.3758% |
| Greedy LinUCB | 1288/1570 | 82.0382% |
| Online SGD | 1334/1570 | 84.9682% |
| MIRA-Relay | 1366/1570 | 87.0064% |
#### Delay $D=1$

| Method | Correct / decisions | Success |
|---|---:|---:|
| BayesianRouter | 1261/1570 | 80.3185% |
| Frozen | 1152/1570 | 73.3758% |
| Greedy LinUCB | 1262/1570 | 80.3822% |
| Online SGD | 1328/1570 | 84.5860% |
| MIRA-Relay | 1380/1570 | 87.8981% |
#### Delay $D=4$

| Method | Correct / decisions | Success |
|---|---:|---:|
| BayesianRouter | 1261/1570 | 80.3185% |
| Frozen | 1152/1570 | 73.3758% |
| Greedy LinUCB | 1272/1570 | 81.0191% |
| Online SGD | 1296/1570 | 82.5478% |
| MIRA-Relay | 1378/1570 | 87.7707% |

## MIRA-Relay paired deltas by delay

Delta is MIRA-Relay terminal correctness minus the baseline terminal correctness for the same fold, seed, delay, and molecule. Positive/negative rescue counts are restricted to action changes. Bootstrap intervals resample scaffold clusters with replacement for 10,000 draws, preserving all event rows in each sampled cluster and using the event-row denominator.

### Natural fresh menu

#### Delay $D=0$

| Baseline | MIRA success | Baseline success | MIRA − baseline (pp) | 95% scaffold CI (pp) | Changes | Rescued / harmed |
|---|---:|---:|---:|---:|---:|---:|
| BayesianRouter | 89.6815% | 89.5541% | +0.1274 | [-1.06, +1.57] | 725 | 42 / 40 |
| Best Fixed | 89.6815% | 88.1529% | +1.5287 | [-0.95, +4.05] | 1228 | 72 / 48 |
| Frozen | 89.6815% | 89.8089% | -0.1274 | [-1.38, +1.21] | 546 | 26 / 28 |
| Greedy LinUCB | 89.6815% | 89.6815% | +0.0000 | [-1.33, +1.49] | 488 | 26 / 26 |
| Online SGD | 89.6815% | 89.0446% | +0.6369 | [-0.67, +2.08] | 832 | 36 / 26 |
| MIRA-Relative | 89.6815% | 89.8089% | -0.1274 | [-1.38, +1.21] | 538 | 26 / 28 |
| Retrospective Best Fixed | 89.6815% | 90.4459% | -0.7643 | [-3.95, +2.41] | 1038 | 54 / 66 |
| Random | 89.6815% | 83.7580% | +5.9236 | [+2.18, +9.69] | 1176 | 130 / 37 |
#### Delay $D=1$

| Baseline | MIRA success | Baseline success | MIRA − baseline (pp) | 95% scaffold CI (pp) | Changes | Rescued / harmed |
|---|---:|---:|---:|---:|---:|---:|
| BayesianRouter | 89.6815% | 89.5541% | +0.1274 | [-1.20, +1.64] | 727 | 39 / 37 |
| Best Fixed | 89.6815% | 88.1529% | +1.5287 | [-1.27, +4.35] | 1302 | 76 / 52 |
| Frozen | 89.6815% | 89.8089% | -0.1274 | [-1.70, +1.51] | 578 | 28 / 30 |
| Greedy LinUCB | 89.6815% | 89.8089% | -0.1274 | [-1.89, +1.73] | 592 | 30 / 32 |
| Online SGD | 89.6815% | 88.9172% | +0.7643 | [-0.65, +2.26] | 840 | 36 / 24 |
| MIRA-Relative | 89.6815% | 89.8089% | -0.1274 | [-1.70, +1.51] | 564 | 28 / 30 |
| Retrospective Best Fixed | 89.6815% | 90.4459% | -0.7643 | [-4.00, +2.48] | 904 | 54 / 66 |
| Random | 89.6815% | 83.7580% | +5.9236 | [+2.04, +9.87] | 1181 | 133 / 40 |
#### Delay $D=4$

| Baseline | MIRA success | Baseline success | MIRA − baseline (pp) | 95% scaffold CI (pp) | Changes | Rescued / harmed |
|---|---:|---:|---:|---:|---:|---:|
| BayesianRouter | 90.0637% | 89.4904% | +0.5732 | [-0.97, +2.32] | 706 | 42 / 33 |
| Best Fixed | 90.0637% | 88.1529% | +1.9108 | [-0.83, +4.67] | 1208 | 76 / 46 |
| Frozen | 90.0637% | 89.8089% | +0.2548 | [-0.90, +1.69] | 532 | 28 / 24 |
| Greedy LinUCB | 90.0637% | 89.6815% | +0.3822 | [-0.98, +2.00] | 478 | 28 / 22 |
| Online SGD | 90.0637% | 89.6815% | +0.3822 | [-0.92, +1.82] | 732 | 34 / 28 |
| MIRA-Relative | 90.0637% | 89.8089% | +0.2548 | [-0.90, +1.69] | 518 | 28 / 24 |
| Retrospective Best Fixed | 90.0637% | 90.4459% | -0.3822 | [-3.78, +3.18] | 942 | 62 / 68 |
| Random | 90.0637% | 83.7580% | +6.3057 | [+2.26, +10.49] | 1178 | 137 / 38 |

### Controlled evaluator-side drift

#### Delay $D=0$

| Baseline | MIRA success | Baseline success | MIRA − baseline (pp) | 95% scaffold CI (pp) | Changes | Rescued / harmed |
|---|---:|---:|---:|---:|---:|---:|
| BayesianRouter | 87.0064% | 80.1911% | +6.8153 | [+4.03, +9.87] | 699 | 152 / 45 |
| Frozen | 87.0064% | 73.3758% | +13.6306 | [+9.02, +18.56] | 622 | 252 / 38 |
| Greedy LinUCB | 87.0064% | 82.0382% | +4.9682 | [+2.05, +8.11] | 506 | 112 / 34 |
| Online SGD | 87.0064% | 84.9682% | +2.0382 | [+0.12, +4.08] | 806 | 72 / 40 |
#### Delay $D=1$

| Baseline | MIRA success | Baseline success | MIRA − baseline (pp) | 95% scaffold CI (pp) | Changes | Rescued / harmed |
|---|---:|---:|---:|---:|---:|---:|
| BayesianRouter | 87.8981% | 80.3185% | +7.5796 | [+4.60, +10.76] | 684 | 159 / 40 |
| Frozen | 87.8981% | 73.3758% | +14.5223 | [+9.77, +19.59] | 646 | 268 / 40 |
| Greedy LinUCB | 87.8981% | 80.3822% | +7.5159 | [+4.24, +11.07] | 534 | 154 / 36 |
| Online SGD | 87.8981% | 84.5860% | +3.3121 | [+1.00, +5.79] | 812 | 86 / 34 |
#### Delay $D=4$

| Baseline | MIRA success | Baseline success | MIRA − baseline (pp) | 95% scaffold CI (pp) | Changes | Rescued / harmed |
|---|---:|---:|---:|---:|---:|---:|
| BayesianRouter | 87.7707% | 80.3185% | +7.4522 | [+4.64, +10.41] | 690 | 156 / 39 |
| Frozen | 87.7707% | 73.3758% | +14.3949 | [+10.00, +19.07] | 616 | 260 / 34 |
| Greedy LinUCB | 87.7707% | 81.0191% | +6.7516 | [+3.84, +9.94] | 486 | 136 / 30 |
| Online SGD | 87.7707% | 82.5478% | +5.2229 | [+2.78, +7.88] | 648 | 106 / 24 |

## Bootstrap and aggregation audit

- Existing receipt aggregation and this audit use scaffold-resampled event-row denominators, not IID event rows and not a policy rerun. For each draw, the 124 observed scaffold clusters are sampled with replacement; all paired deltas in each sampled cluster are concatenated, then `sum(delta)/count(rows)` is computed. The 2.5th and 97.5th percentiles form the interval. Seed is 20261006 and draws are 10,000.
- The audit recomputes deltas directly from terminal events and pairs streams by `(fold, seed, delay)`; it checks terminal counts and `n_correct` against stream summaries. Bootstrap cluster counts are 124 for every reported paired comparison.
- For the natural overall result, MIRA-Relay is 89.8089% versus Frozen 89.8089%; this is a pooled cancellation of delay-specific differences, not evidence that every delay is exactly zero.

## Interpretation constraints

- The stress positive result is a controlled evaluator-side label flip, not a natural provider version trace.
- CPU timing numbers are implementation overhead under the receipt environment and should not be described as A800 or end-to-end latency.
- The current audit gives the exact numerical material for a main table, a delay ablation table, and a delay-effect plot; it does not broaden the claim to post-call verification, refusal, lifecycle drift, hERG/AMES, or a 33-provider model-fresh experiment.
