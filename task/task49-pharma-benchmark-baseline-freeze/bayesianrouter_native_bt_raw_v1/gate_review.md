# Native BT+CLS BayesianRouter receipt gate review

**Reviewer:** independent verifier review
**Date:** 2026-09-30
**Receipt under review:** `experiments/logs/task49_bayesianrouter_native_bt_raw_20260930/`
**Verifier:** `src/probe_headless/verify_bayesianrouter_native_bt_raw_v1.py`

## Decision

**Smoke receipt:** PASS. The strict verifier replayed both methods, one fold, one seed, one delay, and all 10 decision/terminal/feedback triples.

**Full-matrix prerequisite:** **PASS for protocol integrity.** The same verifier completed on `full_v1` and retained `verification_strict_v2.json`: 60 streams and 8,664 decision/terminal/feedback triples. This is a CPU audit, not a scientific rerun; the A800 full matrix still needs its own receipt and effectiveness analysis.

## What changed in the independent check

- Source hashes are checked against blobs in `code_identity.declared_commit` using `git show`; the current worktree is not accepted as provenance.
- The verifier requires the exact stream roster and rejects missing, extra, duplicate, reordered, or early feedback events. It enforces decision → terminal adjacency, contiguous cohort order, exact menu, exact read set, provider event IDs, method/episode/run identities, and release timing.
- Every terminal reward is recomputed from the frozen input panel and selected output. Feedback reward/status, state transitions, versions, and aggregate stream/root summaries are checked independently.
- Every manifest checkpoint is restricted to `recovered_v5`, and `score.weight` plus `encoder.0` are loaded and recomputed with NumPy against the stored snapshots.
- The receipt scope is explicit: this is the recovered-v5 native BT+CLS checkpoint, **not an exact migration claim for the old online A800 checkpoint**.

## Remaining scientific boundary

1. This receipt is a recovered-v5 native BT+CLS warm start/control and remains separate from claims about the previous online A800 checkpoint.
2. Preserve `verification_strict_v2.json` with the full receipt and do not overwrite it with a verifier using current-worktree hashes.
3. Treat this as protocol-integrity evidence; its descriptive accuracies are not confirmation or final effectiveness evidence. The A800 full matrix, provider lineage, and held-out confirmation remain separate gates.

## Tamper tests

Against copied smoke receipts, the verifier rejected all six mutations: `menu_before_choice`, `decision_reads`, terminal `system_success`, stream summary `n_correct`, duplicate decision, and feedback released before its stream tick. These are protocol-integrity checks only; they do not establish effectiveness.
