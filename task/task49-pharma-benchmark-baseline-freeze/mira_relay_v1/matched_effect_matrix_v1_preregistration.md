# Matched effect matrix v1

## Scope

This is a CPU development gate for the direct-provider task. It is not the
confirmation campaign and it is not a significance test. The matrix uses the
sealed fold-0/seed-0 fit state, frozen encoder, provider menu, and the first 32
evaluation molecules. Every method sees the same molecule order and receives
only selected terminal feedback after the declared release delay.

## Methods and matched controls

| Method | Online residual | Probe/safety lifecycle | Purpose |
|---|---|---|---|
| `probe_mira_relay` | one residual shared by provider family | epsilon 0.20, same hazard shield | proposed family relay |
| `probe_independent_residual` | one residual per provider, initialized from the same family fit | epsilon 0.20, same hazard shield | isolates family sharing |
| `probe_no_update` | frozen residual and bias | epsilon 0.20, same hazard shield | isolates all residual learning |

The independent baseline is warm-started by copying each provider's family fit
mean and variance. Thus the initial score rule is matched; only the online
sharing path differs. Each method is run at delays 0, 1, and 4. The seed is
derived once and reused across methods and delays so the development receipt
does not compare different random probe schedules by accident.

## Primary development readouts

1. Selected correctness reward: the `feedback.reward` attached to the selected
   terminal event. Runtime success (`terminal.valid_return`) is recorded as a
   separate readout; it is not substituted for chemical correctness.
2. Paired rescued/harmed counts for MIRA versus each control, matched by the
   same molecule and delay: rescued means MIRA reward 1/control reward 0;
   harmed means MIRA reward 0/control reward 1.
3. Transfer diagnostics: family residual changes for MIRA and provider-local
   residual changes for the independent baseline. These diagnose whether the
   declared sharing structures actually execute; they are not effectiveness
   claims.

## Falsification and stop rules

The receipt must not be reported as evidence of a method effect if any of the
following occurs: event identity or delayed-feedback conservation fails; the
reward is not bound to the selected terminal event; the independent update
changes an unselected provider; the no-update residual changes; or paired
rescued/harmed results are not reproducible from raw events. A non-positive
paired reward delta is retained as a negative development result and triggers
method analysis before any remote run.

## Reproducibility

The runner writes `precompute_config.json` before policy construction and
`config.json` before the first event. It records input, fit-state, candidate,
checkpoint and source hashes, Python/NumPy/platform versions, exact rows,
seeds, delays, and the code commit. Raw `events.jsonl`, `summary.json`, and an
independent verifier receipt are retained. The runner never reads unselected
provider outcomes at decision time.
