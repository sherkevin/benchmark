# Independent post-call receipt audit v3

Date: 2026-10-08. Scope: the preserved `full_v1` cached replay. No provider inference, API call, GPU run, or new scientific effect experiment occurred.

## Outcome

- `receipt_verification.json`: **verified**, exactly 3,140 policy rows and 3,140 evaluator rows, covering five folds, 157 candidate IDs and four policies without duplicate or missing keys.
- `original_result_verification.json`: **verified** for 232 fit/summary fields, the five input hashes and paths, fold identities, confirmation count, random seed and version. Bootstrap and prose/time metadata are explicitly outside this verifier's checks.
- `mutated_result_verification.json`: intentionally corrupted fit acceptance metadata is **rejected** (`receipt_verified_result_mismatch`; CLI exit 2). This is a mutation artifact, not a discrepancy in the original result.
- `runs/002/mutation_tests.stdout.log`: **26 passed**. The 22 parametrized corruption cases cover receipt, evaluator, prior and result failures; additional tests cover the unmodified full replay, actual boundary-tie counting and explicit metadata failure.

## Exact checks

The verifier imports no replay runner. It independently computes fit correctness for all four providers from the 722 frozen development rows, selects the best provider with the specified tie order, and reconstructs the confidence threshold. Prior fit and evaluation indices must exactly match their definitions in development `inputs.json`; the fit/validation/evaluation lists must partition the development rows.

For each policy row it checks a strict field whitelist, field types, finite bounded probabilities, one call, successful status, selected provider, selected probability, confidence, predicted class, threshold, action and acceptance flag. It reconstructs the same-coverage random subset with each fold's fixed seed. Recursive evaluator/truth fields and any additional fields, including unselected provider outputs, are rejected. JSON duplicate fields and non-finite constants are rejected.

Evaluator rows must bind to every policy field and to the manifest's corresponding truth. Correctness and terminal status are recomputed, including null correctness on refusals. All fold and pooled counts, coverage, accepted accuracy, wrong/refusal counts, call counts and rejected-error rates are recomputed. Result fits are compared with independently reconstructed fits; input hashes prevent a result from silently referring to a different source file.

The mutation suite rejects joint receipt/ledger truncation, duplicate policy and evaluator keys, NaN, incorrect call count/status/predicted label/action/random seed, nested truth leakage, unselected outputs, correctness/terminal flips, changed prior fit rates, changed fit or evaluation membership, duplicate result folds, changed result threshold/rate/rejected-error-rate/seed/input hash, and changed result fit acceptance metadata.

## Threshold ties: corrected expectation

The historical runner uses `floor(0.8*n)` both as the threshold rank and as its reported acceptance count. That formula can undercount when the boundary value has ties below the selected rank. The verifier now counts every fit confidence satisfying `confidence >= threshold`.

On these actual frozen inputs, the tie-inclusive count equals the historical count in every fold:

| Fold | Fit n | Threshold | Accepted with ties | Actual coverage |
| --- | ---: | ---: | ---: | ---: |
| 0 | 444 | 0.625 | 355 | 0.7995495495495496 |
| 1 | 483 | 0.6735942572664859 | 386 | 0.7991718426501035 |
| 2 | 492 | 0.5674746816408054 | 393 | 0.7987804878048781 |
| 3 | 483 | 0.5598068809911607 | 386 | 0.7991718426501035 |
| 4 | 480 | 0.5781029995462263 | 384 | 0.8 |

An initial test incorrectly assumed an actual discrepancy in fold 0. It failed: `runs/001` preserves the configuration, hashes, complete test failure, original successful audit, and the incorrect expected CLI exit. The revised test follows the observed data. A separate counterfactual mutation of one real development probability creates a boundary tie while preserving its predicted class and all fit rates; the verifier counts the extra accepted row. Original `full_v1` artifacts have not been changed.

## Reproducibility and limits

Run `python3 experiments/logs/task49_postcall_fresh_confirmation_20261008/strict_audit_v3/run_audit.py` from the repository. Every attempt gets a new `runs/NNN` directory. Configuration, exact commands, input/source SHA256 values, Python/numpy/pytest versions, machine/platform, git commit and zero-call budget are saved before execution. Subprocess stdout/stderr stream to files; `audit_raw.jsonl` preserves timestamped events and per-test outcomes. `config.json` and `summary.json` identify the successful attempt. A targeted `git diff --check` also completed with exit 0.

This verifies arithmetic and receipt consistency relative to the frozen sources. It does not establish prospective holdout status, training-data non-overlap, provider checkpoint provenance, or scientific novelty. Bootstrap and utility uncertainty are owned by the separate statistical audit. The original runner's general tie-reporting limitation remains in its historical source; the independent verifier detects such a mismatch if it occurs in a future artifact.

Reusable output: the standalone import-free verifier, the real-data mutation tests, and the versioned audit driver can be used as submission gates for the same frozen protocol. There are no remaining verifier repairs required for the declared frozen replay.
