# Stream drift audit v1

## What was checked

This audit asks whether the frozen BBB development evaluation order contains a signal that an online selector could exploit. It uses the already sealed `fingerprint_diagnostic_v1/inputs.json` panel (722 molecules, 33 providers, five scaffold-disjoint folds). No new provider call, model training, label, or confirmation sample was used.

For each fold, the evaluation rows were kept in their recorded arrival order and split into blocks of 16, 32, and 64. Within every block we recorded the best provider, the gap to the second provider, and the regret to the per-molecule oracle. We then repeated the same calculation after 500 fixed-seed within-fold permutations. The permutation keeps the same molecules, labels, provider count, fold, and block sizes, but removes the original order.

The executable audit is `src/probe_headless/audit_pharma_stream_drift.py`. The complete machine-readable result is `experiments/logs/task49_stream_drift_audit_20260927/analysis_v2.json`.

## Result

| block size | observed winner changes (mean/fold) | permutation mean | observed minus null | observed mean margin | permutation mean margin |
|---:|---:|---:|---:|---:|---:|
| 16 | 7.20 | 7.48 | -0.28 | 0.0156 | 0.0171 |
| 32 | 4.00 | 3.63 | +0.37 | 0.0175 | 0.0170 |
| 64 | 1.60 | 1.73 | -0.13 | 0.0094 | 0.0166 |

All five folds have at least one observed winner change at every block size. That fact by itself is weak: the observed counts are close to the order-destroying null at all three resolutions. The observed winner margin is also not larger than the null margin; at block size 64 it is smaller. At block size 16 the observed number of changes is actually below the null mean.

The original block-size-32 sequence is therefore compatible with finite-sample winner noise among 33 providers. It does not establish a provider regime switch, temporal nonstationarity, or an online-policy advantage.

## Decision for the method

Do not introduce a change-point or regime-switching dynamic JEV based on this stream order. The current benchmark stream does not provide enough evidence that order alone contains a stable state to track. A method that assumes such a state would add machinery without a demonstrated source of information.

The matched-SGD development run already showed that selected-only updates can improve a weak Frozen warm start (`0.8913` to `0.8991`) while remaining below train-only Best Fixed (`0.9155`). Together with this audit, the next method must obtain an information or representation advantage from the molecule/provider interaction itself. Ordinary selected-only SGD, replay, and an order-only change-point layer are controls, not the contribution.

## Limits

This is a development diagnostic, not a final benchmark result. It uses full-panel labels only to test whether the fixed arrival order is informative; an online policy is still restricted to the selected provider's released feedback. The permutation comparison is a noise audit, not a causal test of stationarity. It also does not rule out a context-conditioned method whose signal is in molecular/provider interactions rather than in the row order.
