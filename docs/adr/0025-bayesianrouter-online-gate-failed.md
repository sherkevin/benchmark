# ADR-0025: Selected-only BayesianRouter update fails the online-effect gate

- Status: Accepted
- Date: 2026-09-27
- Scope: Task49 pharmaceutical direct provider selection
- Supersedes: none

## Context

The benchmark needs to distinguish a useful online adaptation mechanism from a frozen selector. The pre-registered diagnostic used the same Morgan fingerprint representation and the same offline BT+CLS prior for both policies. The only difference was whether the selected provider's released binary outcome updated a Bayesian posterior. Delays were 0, 1, and 4. The confirmation split was closed.

## Evidence

The fixed A800 follow-up task `01a0e0c65614a23dee31880441509c29` completed with 60 streams, 8,664 decisions, and 8,664 delayed feedback events. Independent recovery checked input rewards, selected statuses, feedback binding, version conservation, and source hashes.

Mean accuracy over ten paired fold-seed streams was:

- delay 0: frozen 0.89136, online 0.88859, delta -0.00277;
- delay 1: frozen 0.89136, online 0.88927, delta -0.00208;
- delay 4: frozen 0.89136, online 0.89068, delta -0.00068.

The pre-registered gate required a positive mean delta at two of three delays. It was positive at zero delays.

## Decision

Treat selected-only BayesianRouter updating as a negative baseline. It is not the proposed contribution and will not be described as a successful dynamic JEV. Do not post-tune this diagnostic.

The next method must explain how representation and online state interact under delayed, selected-only feedback. Before inventing a new update rule, implement matched Frozen/Online-SGD/Replay-SGD controls under the same event contract. Keep confirmation closed until that comparison has a pre-registered method card.

## Consequences

This removes a tempting but unsupported story: a standard Bayesian posterior update is not enough for this task. It preserves a clean baseline and a concrete failure mode for the paper. The result also exposes a systems cost: the online kernel's mean p50 selection time was about 256 microseconds versus 22 microseconds for the frozen prior in this run.

Evidence and recovery details are in `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/online_bayesianrouter_effect_result.md`.
