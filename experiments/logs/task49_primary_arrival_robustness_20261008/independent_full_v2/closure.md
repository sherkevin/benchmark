# Independent arrival robustness audit — closed

Status: **verified**, 2026-10-08. The checker imports no production runner or
policy implementation. It independently reconstructs each selected-feedback
trajectory from frozen inputs. The successful run is `independent_full_v2/`;
`independent_full_v1/failure.json` is retained as a real failed audit attempt.

## Exact coverage

- All 128 sampled orders, with order 0 excluded from Monte Carlo statistics.
- 61,440 streams, 9,646,080 decisions, 38,584,320 float64 scores.
- All 17 receipt fields, including exact actions, labels, drift, pending
  counts, due/release sequence, updates, versions, credit and source delta.
- 61,440 complete numerical and instance final states, including constants
  and all Relative decision payloads; 23,040 initial/final RNG states.
- 422,400 state arrays, all referenced and checked; consumed IDs/count/hash
  and empty final pending queues; all input/source/chunk manifests.
- Per-stream and per-order counts, rescues/harms, action changes, provider
  counts, before/after-78 counts, all delays and equal-delay aggregates.
- All 168 secondary Monte Carlo summaries and the complete primary summary,
  including standard deviations, MCSE, t intervals, quantiles, histograms,
  integer primary wins/ties, and exact binomial limits.

The maximum absolute error is **0** for scores, states and metrics in this
successful complete replay. Discrete fields match exactly. The malformed
release and terminal-label mutation checks both reject the changed field.
The separate original-order audit checked all 480 new streams against the
420 main and 180 credit historical streams; its version/score representation
differences and historical final commitments are documented in
`../independent_smoke_v1/verification.json`.

## Preserved failure and repair

The first full replay passed every trajectory but rejected exact secondary
histogram counts: equivalent percentage/scaffold formulas differed below
1.42e-14 and moved data-derived bin edges. The diagnostic covers all 168
histograms, identifies 35 affected ones, and confirms every primary statistic
and all non-histogram-count fields. Nothing in the production results changed.

The checker now explicitly applies the archived floating-point representation
contract to independently rebuilt integer counts: multiply by 100 before
division; for scaffold effects, average sorted block fractions then multiply
by 100. The concrete boundary counterexample is tested in
`../independent_tests_v2/verification.json`. The tolerance remains
atol = rtol = 1e-10; integer comparisons remain exact. The successful second
audit recomputed every trajectory from the beginning and reused no trajectory
cache. See `../independent_histogram_diagnosis_v1/repair_note.md`.

## Resource accounting

Successful pass: monotonic wall 511.025249083 s, process CPU 510.798452 s,
peak RSS 578,682,880 bytes. Including both targeted-test runs, smoke audit,
the failed full audit and histogram diagnosis, cumulative recorded audit CPU
is **1,040.777607 s**, below the declared 1,800 s allowance. There were no
provider forward passes, fits, APIs or GPU calls.

## Verified primary result and limits

Controlled MIRA-Relay minus Online SGD: mean **4.736265924 pp**, MCSE
0.212027691 pp, approximate 95% mean interval [4.316701387, 5.155830460] pp.
There are **125/128 positive orders, zero ties**, determined from integer
correct-count differences over denominator 4,710. The exact positive-frequency
interval is [0.933033698, 0.995140296]. Both preregistered directional conditions
hold. The natural equal-scaffold Local-minus-Permutation mean is only
0.051768313 pp, with pointwise mean interval [-0.087758426, 0.191295052] pp.

These results concern uniform arrival orders of the same viewed 157-molecule
cohort, the five frozen priors, and the two fixed policy seeds. They do not
establish new-scaffold, training/checkpoint, full policy-randomness or deployment
uncertainty. This audit closes the numerical evidence check, not the broad
scientific gate or manuscript promotion.

## Binding

- Verification SHA-256: `99fb3c3c0d0e9995641bdcf22f4c5e61f5333b9c13c182ae60585dc26e2a87b9`.
- Full run manifest SHA-256: `c3ff33a5923de655f6a900afbfbdbf196c49909a3aa9d507e83dce0def8b88f2`.
- Full run config SHA-256: `7d2b3985522a5a7822fab40270ca1866afafbedefadfcded6216697f836674d4`.
- Checker source SHA-256: `186359ca9a5398b5fa2ef7fb4c397792acc9d2e104769bd3ac532a09a6abb9ab`.

Machine-readable evidence: `verification.json`, `independent_statistics.json`,
`order_001_metrics.json` through `order_128_metrics.json`, `events.jsonl`,
`config.json`, and the exact `verifier_source.py` snapshot.
