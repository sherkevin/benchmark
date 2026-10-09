# Matched effect matrix v2 (145-row CPU gate)

## Purpose

This is the next development gate after the verified 32-row matrix. It keeps
the method, fit state, provider menu, feature encoder, probe seed, and delayed
feedback contract fixed and expands only the held-out cohort to all 145 fold-0
evaluation molecules. It does not start a Nebula job or use confirmation data.

## Frozen design

- Cohort: fold 0, seed 0, all 145 evaluation rows in their frozen order.
- Methods: `probe_mira_relay`, `probe_independent_residual`, and
  `probe_no_update`.
- Delays: 0, 1, and 4; due tick is `decision_tick + 1 + delay`.
- Nine streams, each with 145 decisions, 145 terminals, and 145 releases.
- The independent baseline copies the family fit mean/variance at cold start;
  its online residual is provider-local. The no-update control keeps the same
  probe and hazard lifecycle but freezes residual and bias updates.

## Readouts and falsification

For each identical molecule and delay, report selected correctness reward from
`feedback.reward`, runtime validity separately, and MIRA's paired rescued and
harmed counts against each control. A family-transfer event is a selected
MIRA residual update that changes a family residual for a family with at least
two providers. The independent control must change only its selected provider
residual. These state diagnostics are contract checks, not effect claims.

The run is rejected if event identity, selected-only feedback timing, reward
binding, state hash chains, roster binding, or method-specific state updates
fail. Zero or negative paired reward is a retained development result; it does
not trigger a seed or cohort change.

## Evidence

`run_mira_matched_effect_matrix_v2.py` writes `precompute_config.json` before
policy construction and `config.json` before the first event. The v2 verifier
uses the same strengthened roster/family checks as v1 and requires 145 rows
per stream. Input, fit-state, candidate, checkpoint, source hashes, exact
commit, raw JSONL events, summary, and verifier output are retained under the
v2 experiment receipt directory.
