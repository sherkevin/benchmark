# Matched effect matrix v2 addendum

The first receipts (`experiments/logs/task49_mira_matched_effect_matrix_20261003/v1/`
through `v3/`) are retained as history. They are superseded because the
strengthened verifier binds the provider roster and family map explicitly and
the earlier invocations contained manually supplied commit strings. The
authoritative rerun is:

`experiments/logs/task49_mira_matched_effect_matrix_20261003/v4/`

It was run with `declared_commit == local_head == git rev-parse HEAD` recorded
in `config.json` and verified by `verification_v4.json` (`status: verified`). It
has 9 streams and 864
events. The result is unchanged: for delays 0, 1, and 4, all three methods
obtain 28/32 selected correctness rewards and 32/32 runtime-valid returns;
MIRA versus both matched controls has 0 rescued and 0 harmed molecules. MIRA
executes 22 residual updates and 2 family-transfer events, while the
provider-independent control executes 22 provider-local updates.

This remains a negative development gate. The matrix verifies that the three
sharing/update contracts execute, but it does not support an effectiveness
claim or justify a confirmation run by itself.
