# Fresh MIRA full-state replay — experiment card

## Purpose

Strengthen the independent replay audit for the already sealed fresh
confirmation receipts. The target is the exact state that determines a later
MIRA decision: the learned prior weights, fit-rate anchors, selected-only
posterior counters, version, and delayed lifecycle. This is an audit of an
existing run; it does not change the benchmark, checkpoints, hyperparameters,
or reported outcome numbers.

## Frozen inputs

- Four ordered providers: `ecfp_logreg`, `ecfp_mlp`, `ecfp_random_forest`,
  `graph_message_passing`.
- Sealed five-fold prior cache and the confirmation manifest are read-only
  inputs. Morgan radius 2, chiral Morgan64, L2 normalization, and an intercept
  are rebuilt directly from the manifest with RDKit.
- For provider $a$, the replay computes
  $q_a=(20\bar r_a+s_a)/(20+n_a)$ and
  $S_a(x)=\operatorname{sigmoid}(w_a^\top x)+(q_a-\bar r_a)$.
- Only selected feedback increments $(n_a,s_a)$; the release tick is
  `decision_tick + 1 + delay`, with the recorded final queue flush.
- Delays are $D\in\{0,1,4\}$ and every stream has 157 decisions.

## Independent checker

`verify_fresh_context_replay_v2.py` uses Python standard library, NumPy and
RDKit only. It does not import the production runner, policy, model loader, or
stream implementation. It reconstructs the full production state before and
after every event, checks every available production state hash, recomputes
all scores and argmax actions, checks terminal/reward binding and delayed
release order, then writes a compressed canonical full-state trace.

The historical terminal records contain no state hash because they do not
mutate policy state; they are checked through lifecycle binding. The summary
records also contain no final-state hash, so the audit reports that final-hash
availability is absent rather than treating absence as a mismatch.

## Falsification conditions

The checker must reject a receipt if an input score, reward, provider action,
state hash, terminal binding, release clock, or summary aggregate is changed.
The verifier is not allowed to silently repair a malformed receipt.

This is an implementation and evidence audit. It is not a new effectiveness
estimate, an offline optimizer rerun, provider inference rerun, or a claim
about natural provider drift or end-to-end agent gains.
