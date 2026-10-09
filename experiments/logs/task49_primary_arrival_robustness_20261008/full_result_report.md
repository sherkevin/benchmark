# Primary arrival robustness v1: full execution

**Closure, 2026-10-08:** full independent verification v2 has passed all 128 orders, 61,440 streams and 9,646,080 decisions, including every score/state and aggregate. Verification SHA: `99fb3c3c0d0e9995641bdcf22f4c5e61f5333b9c13c182ae60585dc26e2a87b9`. See `independent_full_v2/closure.md` for the preserved v1 histogram-representation failure and complete fresh recheck. The execution-time status below is retained as history. No production result was changed; broad scientific gate remains HOLD.

Status: all 128 preregistered random orders completed. Independent full verification is pending; the original-order smoke independently passed before coordinator resource GO. No API call, provider forward pass, offline refit, or GPU was used. Online selected-feedback updates were actually replayed.

## Primary result

Controlled-drift MIRA-Relay minus the preregistered Online SGD control: **+4.73627 percentage points**, sample SD 2.39882 pp, MCSE 0.21203 pp; approximate mean Monte Carlo 95% interval **[4.31670, 5.15583] pp**. Positive on **125/128** orders (zero ties), with exact Clopper–Pearson 95% interval **[0.93303, 0.99514]** for positive-order probability. Both preregistered directional conditions pass on the saved results.

The 2.5%–97.5% empirical order quantiles are [0.44904, 8.61253] pp; minimum −2.67516 pp and maximum +10.27601 pp. These quantiles describe individual orders and are distinct from the mean interval. Three negative orders are retained: [{"order": 69, "integer_correct_difference": -126, "difference_pp": -2.6751592356687897}, {"order": 86, "integer_correct_difference": -34, "difference_pp": -0.721868365180467}, {"order": 90, "integer_correct_difference": -40, "difference_pp": -0.8492569002123143}].

## All fixed-roster mean pooled accuracies

| Method | Natural (%) | Controlled drift (%) |
|---|---:|---:|
| Frozen | 89.80892 | 75.75338 |
| Online SGD | 89.05288 | 82.22432 |
| Greedy LinUCB | 89.70044 | 76.89922 |
| BayesianRouter | 89.71785 | 79.60473 |
| MIRA-Relay | 89.52196 | 86.96059 |
| MIRA-Relative | 89.80295 | Not in frozen controlled roster |
| Fixed-permutation credit | 89.86299 | 71.39530 |
| Fit-only Best Fixed | 88.15287 | Not in frozen controlled roster |
| Retrospective Best Fixed (evaluator-only) | 90.44586 | Not in frozen controlled roster |
| Random | 86.46746 | Not in frozen controlled roster |

Natural pooled MIRA-Relay minus Frozen is −0.28696 pp. There is no natural pooled-gain result. All per-delay comparisons and all preregistered modern-control contrasts are retained in `full_v1/results.json`.

## Equal-scaffold source-credit contrasts

Each order first averages paired correctness within each of 124 scaffolds, then gives each scaffold equal weight. This is distinct from pooled molecule accuracy.

| Condition | Contrast | Mean (pp) | Pointwise mean MC 95% interval (pp) |
|---|---|---:|---|
| natural | mira_context_reliability__minus__frozen | 0.21103 | [0.12296, 0.29910] |
| natural | mira_context_reliability__minus__mira_relay_fixed_permutation | 0.05177 | [-0.08776, 0.19130] |
| natural | mira_relay_fixed_permutation__minus__frozen | 0.15926 | [0.06816, 0.25036] |
| controlled_drift | mira_context_reliability__minus__frozen | 10.70824 | [10.26847, 11.14800] |
| controlled_drift | mira_context_reliability__minus__mira_relay_fixed_permutation | 15.61236 | [14.81464, 16.41009] |
| controlled_drift | mira_relay_fixed_permutation__minus__frozen | -4.90413 | [-5.44703, -4.36123] |

Natural Local−Permutation remains unresolved by its mean interval. Its equal-scaffold Local−Frozen contrast is positive despite the negative pooled contrast; these are separate weighting targets and must not be conflated.

## Reproduction and resource evidence

- 11 targeted tests passed; `tests_v1`, `tests_v2`, and `tests_v3` preserve their own source/config and stdout/stderr.
- `smoke_v1` was interrupted during inefficient archive access, with the actual traceback retained. `smoke_v2` fixed only repeated decompression and passed 480-stream original-receipt reproduction; independent smoke verification passed before the coordinator’s GO binding source, card, orders, and verification hashes.
- Full: 61,440 streams; 9,646,080 selected decisions, each with output/evaluator/release evidence, all four scores, conservation/version fields, complete final model/RNG/consumed state, and bound chunks.
- Replay plus analysis 530.51045 s; compressed chunks 3,145,050,307 bytes; peak RSS 323,665,920 bytes. The fixed one-hour / 10-GiB / 4-GiB limits were met.
- Frozen original order is excluded from the 128-order statistics. Primary signs use integer correct-count differences divided by 4710, so ties cannot become wins through mean subtraction.

## Scope and reusable result

This is retrospective evidence about uniform arrival permutations of the same viewed 157 molecules, conditional on the five cached priors, four providers, two fixed policy seeds, and frozen delay mixture. It does not estimate new-scaffold, offline-training, checkpoint, or deployment uncertainty, establish novelty, or close a broad method/scientific gate.

The reusable artifact is the complete cached-policy replay and independent verification interface: the saved 129 orders, frozen source/input/card hashes, documented binary receipts, decoder, complete final states, and order-level statistical outputs can support future audits without provider inference or reconstructing hidden intermediate events.

## Artifact identities

- Runner source SHA-256: `a86ac85db81b198e24bbed76925fde39fbe09442c7552473c12cd37cb0d1c827`.
- `full_v1/results.json` SHA-256: `dbaf3e64f9f3695b0d0a20ade3124b25b1732e24cacad46e76e8ec0e12044946`.
- `full_v1/manifest.json` SHA-256: `c3ff33a5923de655f6a900afbfbdbf196c49909a3aa9d507e83dce0def8b88f2`.
- Independent verification will be appended as a separate closure record; the completed run directory remains immutable.
