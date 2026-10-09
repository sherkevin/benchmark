# Matched effect matrix v1 report

## Receipt

- Output: `experiments/logs/task49_mira_matched_effect_matrix_20261003/v4/`
- Scope: fold 0, seed 0, first 32 evaluation molecules; CPU only.
- Methods: `probe_mira_relay`, `probe_independent_residual`,
  `probe_no_update`.
- Delays: 0, 1, 4. There are 9 streams and 864 raw events.
- Independent verifier: `verification_v4.json`, status `verified`.
- The run used the sealed fit-state v7 and the source commit recorded in
  `config.json`. No confirmation data or Nebula job was used.
- v1--v3 are retained as superseded provenance attempts; v4 is the only
  authoritative receipt.

## Observed development result

All three methods returned 28/32 selected correctness rewards at every delay.
All selected calls were runtime-valid (32/32) in this cohort. MIRA versus the
provider-independent baseline had 0 rescued and 0 harmed molecules at each
delay. MIRA versus `probe_no_update` also had 0 rescued and 0 harmed molecules;
there was one provider action change per delay, but its reward was unchanged.

The lifecycle diagnostics did execute the intended parameterization. MIRA made
22 residual updates, with 2 updates changing a shared family residual. The
independent baseline made 22 provider-local updates and no family transfer.
The no-update control changed neither residual state. These are mechanism
checks, not an effect estimate.

## Interpretation

This receipt falsifies a claim that the current 32-row development stream
already demonstrates an accuracy gain from MIRA family sharing. It does not
falsify the method as a whole: the cohort is small, every method starts from
the same fit state, and the observed action divergence is too rare to test
transfer. The next experiment must increase the held-out stream and expose
matched sibling-family cases before a remote run is justified.

The result is retained as a negative gate. It should not be replaced by a
different seed or cohort merely to obtain a positive number.
