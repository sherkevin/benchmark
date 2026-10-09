# Fresh MIRA context-state replay — experiment card

## Purpose

Close the current fresh confirmation's learned-state replay gap without changing
the benchmark, provider checkpoints, method parameters, or reported outcomes.
The historical receipt records only an opaque production state hash. This card
freezes an import-free replay of the state that can affect a later MIRA
decision.

## Frozen contract

- Four ordered providers: `ecfp_logreg`, `ecfp_mlp`, `ecfp_random_forest`,
  `graph_message_passing`.
- The paired `Frozen` stream is the immutable base score surface. The replay
  applies the sigmoid to its logged provider logits; it never imports the
  production selector or runner.
- For provider $a$, released state is $(n_a,s_a)$ and $n_0=20$:
  $q_a=(n_0\bar r_a+s_a)/(n_0+n_a)$ and
  $S_a(x)=\operatorname{sigmoid}(w_a^\top x)+(q_a-\bar r_a)$.
- Only a scored selected feedback event increments that provider's counters;
  the release clock is `decision_tick + 1 + delay`, with final queue flushes
  allowed after the last decision.
- Delays are $D\in\{0,1,4\}$, 157 decisions per stream, and the fresh event
  order is treated as the authoritative lifecycle order.

## Verification and falsification

The verifier must reconstruct every MIRA score, argmax action, selected-only
counter transition, version, terminal binding, and release clock. It must fail
if one reward or one score is changed. The verifier must not import
`run_provider_fresh_confirmation_v1`, `MIRAContextReliabilityPolicy`, or
`run_stream`.

This artifact is a replay and audit closure. It is not a new effectiveness
estimate, provider training run, optimizer rerun, or claim about natural drift.
The historical production hash includes the immutable weight array; the new
canonical trace hashes the dynamic learner state and records the paired Frozen
surface as its sealed static input.
