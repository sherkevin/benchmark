# Real BBB replay contract smoke

- Date: 2026-09-29 (Asia/Shanghai)
- Scope: interface and information-boundary validation only; not an effectiveness experiment.
- Frozen input: `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json` (SHA-256 recorded in `experiments/logs/task49_composition_20260929/bbb_replay_contract_v1/result.json`).
- Replay components: repository `PredictionCache`, `ReplayEpisode`, and `TruthStore`.
- Cohort: first 8 rows of the frozen BBB development input, round 1, delay 1.
- Learner input: 1024-dimensional `on_bits` feature only; no provider panel and no truth label at decision time.

## Checks

The independent runner `src/probe_headless/truth_recovered_bbb_runner.py` was executed against the real replay cache. It produced 8 decisions, 8 selected-provider terminal events, and 8 delayed feedback events. All 8 feedback events were scored and released; all 8 updates were applied after release. The selected-only cache access trace and source hashes are in the raw result.

Terminal and decision records contained no `chemical_correct`, `recovered_label`, `truth`, or `tool_feedback`. The only feedback records carrying correctness were emitted after the release tick. No full provider panel was read by the runner. The run ended with zero pending feedback and policy version 8.

## Interpretation boundary

This run verifies that the selected-only delayed-feedback contract can execute on the real BBB replay environment. It does not compare selectors, estimate accuracy, establish a dynamic JEV effect, qualify a provider checkpoint, or open confirmation. The next scientific step is the pre-registered CPU comparison under the same information contract.

## Evidence

- Raw event stream, configuration, command, and hashes: `experiments/logs/task49_composition_20260929/bbb_replay_contract_v1/`
- Unit and contract tests: `src/probe_headless/tests/test_truth_recovered_bbb_runner.py`, `src/probe_headless/tests/test_truth_recovered_selection_stream.py`, `src/probe_headless/tests/test_truth_recovered_structured_kernel.py`
