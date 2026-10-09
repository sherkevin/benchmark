# Primary arrival robustness v1 — reuse note

Date: 2026-10-08. This is an implementation handoff, not a new experiment card or a change to the method.

## Current state

The requested original-order smoke runner and independent checker already exist. The saved smoke and full run are complete; this handoff does not rerun them.

| Artifact | Saved evidence |
|---|---|
| Runner | `run_primary_arrival_robustness_v1.py`; SHA-256 `a86ac85db81b198e24bbed76925fde39fbe09442c7552473c12cd37cb0d1c827` |
| Independent checker | `verify_primary_arrival_robustness_v1.py`; SHA-256 `186359ca9a5398b5fa2ef7fb4c397792acc9d2e104769bd3ac532a09a6abb9ab` |
| Original-order smoke | `experiments/logs/task49_primary_arrival_robustness_20261008/smoke_v2/`; 480 streams, 75,360 decisions |
| Smoke verification | `independent_smoke_v1/verification.json`; SHA-256 `6dc46467de61b013823b1234278efe585849b01aae78c95dfa3ba3dd0946dbcc` |
| Full-run verification | `independent_full_v2/verification.json`; SHA-256 `99fb3c3c0d0e9995641bdcf22f4c5e61f5333b9c13c182ae60585dc26e2a87b9` |

All log paths above are relative to the repository root; the verification directories are siblings of `smoke_v2`. The canonical interpretation remains in `task/task49-pharma-benchmark-baseline-freeze/primary_arrival_robustness_v1/report.md`.

## Interfaces that can be reused

- `prepare_inputs` calls the existing cached loader, creates the original 157-row feature matrix once, and binds provider probabilities, priors, truth, identities and orders with hashes. It performs no provider inference or offline fitting.
- `build_policy` uses the original main-table policies and parameters. The selected-feedback entry points remain `original.decide` and `original.release`; the scorer receives feature, event ID and query ID only.
- `replay_stream` releases due events before each decision, at `t + 1 + delay`, drains the tail and requires exact pending/consumed conservation. The controlled label flip follows the new arrival tick, from tick 78 onward.
- `decode_stream` reconstructs decision, terminal and feedback records from compressed receipts. Final numerical states and initial/final RNG states remain recoverable.
- The checker imports neither production runner nor policy classes. It independently rebuilds scores, actions, selected updates, final states and aggregates, and rejects altered release/terminal records.

## Interfaces that must not be substituted

- Do not invoke `run_provider_fresh_confirmation_v1.main`: that entry point loads checkpoints and refits priors. Reuse its policy classes and functions only.
- Do not replace main MIRA's direct-count residual with mechanism-v2's accumulated residual merely because the formulas are algebraically equivalent. The audit explicitly preserves the floating-point inheritance boundary.
- Main Frozen returns logits; mechanism-v2 Frozen returns sigmoid scores. Only the declared historical comparison transforms them. Actions must still match exactly.
- Preserve `rng.normal(size=4)` and its call order for BayesianRouter, with the recorded NumPy/PCG64 state. Do not replace it with full weight sampling.
- The old arbitrary final-drain timestamp is not a decision mismatch. The new exact-due tail still has to retain update order and final state.

## Local invocation requirement

The repository uses a `src/` package layout. From the repository root, set `PYTHONPATH=src`:

```bash
PYTHONPATH=src python3 -m pytest -q tests/test_primary_arrival_robustness_v1.py
```

This handoff's first bare `python3 -m pytest ...` failed during collection with `ModuleNotFoundError: No module named 'probe_headless'`; no tests or replay ran. Adding `PYTHONPATH=src` produced **11 passed in 3.55s** under Python 3.9.6. No source changes were needed. This is a local interface check, not a new scientific result.

The runner's `smoke` mode executes only order 0 and requires a new immutable output directory under its assigned log root. `full` mode requires the separate saved resource GO. Existing output directories are rejected before mutation. No new smoke/full invocation was made in this handoff.

## Preserved failures and limits

- Historical `smoke_v1/failure.json` records the interrupted repeated-NPZ-decompression path. The implemented fix materializes each compressed array once; the regression test remains in the 11-test set.
- Historical `independent_full_v1` failed at a secondary histogram boundary caused by equivalent percentage calculations with different last bits. The independent v2 checker fixes the explicit arithmetic representation and keeps discrete counts exact; it does not relax checks.
- This implementation supports arrival-law robustness on the fixed viewed cohort. It does not create new confirmation data or close the broad scientific gate.

No policy math, experiment settings or paper files were changed during this handoff. The verified master PDF remains SHA-256 `be5e1f50da9ba5313ef5bd2159c288d2a2e9de84ddd271569900562a5c6b3cdc`.
