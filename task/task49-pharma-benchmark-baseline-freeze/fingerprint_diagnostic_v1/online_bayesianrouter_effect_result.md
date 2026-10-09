# BayesianRouter online effect diagnostic — verified result

## Run

The first submission is documented separately as an invalid infrastructure run. The fixed follow-up was a valid A800 run:

- Nebula task: 01a0e0c65614a23dee31880441509c29
- Application: xdl-6ab884b1ecbd
- Code commit: ffc5fd2f43113da83497b57cdc07a13b82f07428
- Input SHA-256: a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf
- Hardware: 1 × NVIDIA A800-SXM4-80GB
- Protocol: 5 scaffold folds, 2 seeds, 3 delays (0, 1, 4), frozen BT+CLS prior versus selected-only BayesianRouter update
- Confirmation split: closed

## Evidence checks

The complete worker log was downloaded and the application JSON events were independently checked.

- config: 1
- epoch: 2,000
- stream_complete: 60
- decision: 8,664
- feedback: 8,664
- complete: 1
- Every stream conserved its decisions and delayed feedback.
- Rewards and selected statuses were recomputed from the frozen input rows.
- Frozen streams made zero posterior updates; online streams made exactly one update per selected result.
- The three logged source hashes match the local files at the submitted commit.

Recovery output: `experiments/logs/task49_fingerprint_online_20260927/recovered_v2/summary.json` and `events.jsonl`. Sanitized platform logs: `experiments/logs/task49_fingerprint_online_20260927/full_download_sanitized_v3/`.

## Result

The numbers below are the mean over 10 paired fold-seed streams at each delay. The pair is evaluated on the same molecules and the same frozen BT prior.

| Feedback delay | Frozen | BayesianRouter online | Online − frozen |
| ---: | ---: | ---: | ---: |
| 0 | 0.89136 | 0.88859 | −0.00277 |
| 1 | 0.89136 | 0.88927 | −0.00208 |
| 4 | 0.89136 | 0.89068 | −0.00068 |

Only 4 of 10 paired streams had a positive online delta at each delay. The pre-registered gate required a positive mean at at least two of the three delays; it failed at all three delays.

The online update was also slower in this implementation: mean stream p50 selection time was about 256 microseconds versus 22 microseconds for the frozen prior. This is a descriptive systems measurement, not a tuned speed comparison.

## Decision

Do not use this standard selected-only Bayesian posterior update as the proposed method or as evidence that online adaptation improves the benchmark. Keep it as a strong, correctly implemented negative baseline. Do not tune the prior variance, noise, or delay after seeing this result. The result does not open the confirmation split.

The result does establish that simply adding real-time Bayesian updates to the current fixed Morgan BT representation is insufficient. The next method work must address the representation/update interface and delayed-feedback stability, with a matched frozen/online SGD or replay baseline before proposing a new algorithm.
