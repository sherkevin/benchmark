# Conditional second-call v2: state-basis development diagnostic

The frozen five-fold experiment completed without changes after smoke. The primary repair gate failed. None of the four added-basis models exceeded the strongest existing control, confidence_cascade. All prespecified attribution intervals cross zero. These are previously inspected development data; this is not independent confirmation.

| Method | Utility/input | Calls | Wrong | Refused | Unchanged-action extra calls |
|---|---:|---:|---:|---:|---:|
| stop_only | -0.20252078 | 722 | 60 | 316 | 0 |
| confidence_cascade | -0.19594183 | 897 | 74 | 234 | 93 |
| pair_mean_gain | -0.20286704 | 872 | 66 | 287 | 121 |
| base_confidence | -0.20407202 | 884 | 65 | 294 | 136 |
| signed_only | -0.20110803 | 895 | 66 | 281 | 134 |
| refusal_only | -0.19668975 | 901 | 63 | 280 | 141 |
| signed_refusal | -0.19806094 | 900 | 63 | 284 | 142 |
| magnitude_polynomial | -0.20477839 | 885 | 65 | 296 | 139 |

| Prespecified contrast | Original-order difference | Bootstrap 95% interval |
|---|---:|---|
| signed_refusal_minus_stop_only | +0.00445983 | [-0.00537962, +0.00957710] |
| signed_refusal_minus_confidence_cascade | -0.00211911 | [-0.01278270, +0.00245828] |
| signed_refusal_minus_pair_mean_gain | +0.00480609 | [-0.00704905, +0.00638672] |
| signed_refusal_minus_base_confidence | +0.00601108 | [-0.00649986, +0.00671759] |
| signed_refusal_minus_signed_only | +0.00304709 | [-0.00809809, +0.00487608] |
| signed_refusal_minus_refusal_only | -0.00137119 | [-0.00238481, +0.00489198] |
| signed_refusal_minus_magnitude_polynomial | +0.00671745 | [-0.00587903, +0.00729659] |
| signed_only_minus_base_confidence | +0.00296399 | [-0.00205215, +0.00533165] |
| refusal_only_minus_base_confidence | +0.00738227 | [-0.00800771, +0.00601782] |
| interaction | -0.00433518 | [-0.00552181, +0.00429503] |

## Contract and interpretation

- Reused v1 A verifier, first selector, original B paired targets, splits and model coefficients. No A refits, provider inference, API, GPU or confirmation reads.
- Every added basis was projected against the old 16-column B design without gain labels, using pinv rcond=1e-12. Inference reused the saved B transform.
- Maximum change in the original 16 coefficients across all new models: 3.24e-16. Fold 1 signed rank fell from 4 to 3 and combined signed/refusal rank from 8 to 7; other residual ranks retain their nominal added dimension.
- All 2,888 old policy and evaluator payloads match exactly after mapping base_confidence to the v1 conditional_gain name. Original token states, visible evidence, terminal decisions and outcomes match. All four old bootstrap utility columns match exactly.
- All 10,000 scaffold draw arrays/maps were copied byte-for-byte from v1. Each of the eight policies reran its controller for every resampled sequence. Models were held fixed.
- Block resampling changes both sampled molecules and their order. The bootstrap mean can differ from the original-order point estimate; these intervals include this order effect and exclude training uncertainty. Reusing draws does not remove adaptive feature-selection bias.
- Signed score restores direction discarded by absolute confidence. Refusal is a threshold function of existing confidence, not additional information. Neither feature gained reliable attribution support here.
- The full model made 178 extra calls, of which 142 left the terminal action unchanged. Its apparent point improvement over base confidence is insufficient for the prespecified gate.

## Execution and artifacts

11 focused tests passed; fold 0 smoke completed in 0.694 s; all five folds plus bootstrap completed in 9.382 s. No failed experimental attempts or parameter search occurred.

- `tests_attempt01/`: test command, source/card hashes, stdout and result.
- `smoke_v1/`, `full_v1/`: pre-computation config, source/card snapshots, execution traces, model parameters and artifact manifests.
- `full_v1/fold*_models.json`: immutable inherited weights, new coefficients/transforms, B state support, rank/norm diagnostics.
- `full_v1/B_features_and_targets.jsonl`: every original X16, gain target, raw added basis and residualized design.
- `full_v1/policy_decisions.jsonl`: legal observed evidence and requests/selected calls/token states.
- `full_v1/evaluator_outcomes.jsonl`, `C_evaluator_panel.jsonl`: independent-truth scoring and full-panel diagnostics, separated from policy receipts.
- `full_v1/old_control_audit.jsonl`: rowwise exact payload hashes for inherited controls.
- `full_v1/bootstrap_*`: exact reused draws/maps, 10,000 per-replicate utilities and numerical array.

## Reuse and next step

Reuse the B-only orthogonal-basis implementation and exact inherited-policy audit as controls for representation claims. Preserve this negative result. Follow the frozen stopping rule: return to feedback and terminal-rule mechanism review rather than searching further bases or thresholds. Independent root verification remains separate from runner checks; no method promotion is made.

Runner SHA256: `1b8498e990bb777e8c8e49580241933ebb6b1212d2a847bdaedd518cd6108def`.
Card SHA256: `8ae92f309b9412a6947468b2e3d07c2bb7f7b231fd61a4c53815c46a17da5f6e`.
