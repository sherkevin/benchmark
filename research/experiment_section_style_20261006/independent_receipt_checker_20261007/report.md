# Independent fresh-receipt contract audit (2026-10-07)

## Purpose

This audit is a read-only, event-level check of the two fresh-provider
receipts used in the paper.  It does not import
`run_provider_fresh_confirmation_v1`, any policy class, a checkpoint, or the
evaluator.  It therefore tests the receipt contract independently of the
runner implementation.  It is a contract/reproducibility check, not a new
effect estimate.

## Implementation

- Checker: `src/probe_headless/audit_fresh_receipt_contract_v1.py`
- Tests: `src/probe_headless/tests/test_audit_fresh_receipt_contract_v1.py`
- Output: `natural_audit.json`, `stress_audit.json` in this directory.
- The current old schema uses exact per-event allowlists.  A missing or extra
  field fails closed; decision records explicitly reject evaluator labels,
  truth, provider status, and reward.

For each stream the checker independently verifies:

1. exact event cardinality and unique event identities;
2. decision followed immediately by its bound terminal, and one feedback for
   every decision;
3. terminal due clocks, feedback due/release clocks, release ordering, and the
   final queue flush semantics;
4. decision menu, selected-provider membership, and the sealed read set;
5. observable state-hash/version continuity across the raw event order;
6. raw counts, selected-provider counts, accuracy, and the canonical per-stream
   event digest against `summary.json`.

The checker compares state-hash commitments across events but does **not**
claim to rehash a learned policy state.  The receipt contains opaque hashes,
not complete serialized policy state, so an independent learned-state hash is
not identifiable from this artifact.

## Results

| receipt | streams | events | decision/terminal/feedback | status |
|---|---:|---:|---:|---|
| natural v2 state-hash | 270 | 127,170 | 42,390 / 42,390 / 42,390 | verified |
| controlled stress v3 | 150 | 70,650 | 23,550 / 23,550 / 23,550 | verified |

The source event-file hashes are preserved in the JSON outputs.  They match
the previously sealed receipt hashes: natural
`69598343f9254ff9ea1b46a7ae5c3fb41acbf3aed82333aebe1447780d488aab` and stress
`5e3744ce8ac1af04d3e8d9d81d215d4a1655e792e517377473f1cd4358e42a0e`.

## Mutation suite

The nine local tests include a clean smoke and eight fail-closed mutations:

| mutation | injected defect | expected rejection |
|---|---|---|
| H1 | duplicate event identity | `identity` |
| H2 | missing terminal/cardinality | `cardinality` |
| H4 | incorrect terminal due tick | `due_clock` |
| H5 | release before due | `release_clock` |
| H7 | evaluator label in decision | `extra_fields`/decision leak |
| H8 | state hash chain break | `hash_chain` |
| H9 | summary count tampering | `summary_count` |
| H12 | unrecognised decision field | `extra_fields` |

Command:

```text
python3 -m pytest -q src/probe_headless/tests/test_audit_fresh_receipt_contract_v1.py
```

Observed result: **9 passed**.  Full receipt commands were:

```text
python3 -m src.probe_headless.audit_fresh_receipt_contract_v1 \
  --run experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3 \
  --output research/experiment_section_style_20261006/independent_receipt_checker_20261007/natural_audit.json
python3 -m src.probe_headless.audit_fresh_receipt_contract_v1 \
  --run experiments/logs/task49_provider_fresh_drift_stress_20261006/v3_bayesian_statehash_run1 \
  --output research/experiment_section_style_20261006/independent_receipt_checker_20261007/stress_audit.json
```

## Boundary and next step

This closes the event-level receipt contract and exposes the remaining
evidence gap cleanly: the verifier cannot independently recompute a learned
state hash or provider output from the current receipt alone.  A future receipt
version can close that gap by recording canonical state bytes (or a separately
sealed state transition trace) per feedback, with a bounded size budget.  Until
then, paper claims should describe this check as **cross-event commitment
continuity**, not independent policy-state re-execution.
