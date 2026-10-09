# Baseline receipt map v1

This is a candidate-only manuscript iteration. It makes the baseline table expose
the role and fidelity boundary that is already locked in
`task/task49-pharma-benchmark-baseline-freeze/baseline_scope_lock_and_receipt_join_v1/matrix.json`.

The candidate does not change data, methods, results, claims, figures, or the
master PDF. It only replaces the compact comparator table with role/fidelity
tags:

- `[native]` means task-native under the common one-call contract;
- `[adapted]` means a published family adapted to the single-step contract;
- `[eval-only]` means evaluator-only and not a deployable ranking policy;
- `[replay]` and `[audit]` are diagnostic boundaries.

Build output, page checks, and visual review are kept under
`paper/aamas2027/build/baseline_receipt_map_v1/`. Promotion is intentionally
not performed in this iteration.
