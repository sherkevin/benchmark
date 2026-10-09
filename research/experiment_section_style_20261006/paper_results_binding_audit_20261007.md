# Paper result binding audit — 2026-10-07

## Purpose

Check that every number newly presented in the main tables is traceable to the
sealed baseline receipt or the new mechanism-ablation receipt. This is a
presentation integrity check; it does not create a new estimate or broaden
the scientific claim.

## Result

`src/probe_headless/audit_paper_results_binding_v1.py` returned `verified`.
It independently summed the old natural/stress summaries, summed the new
Local/Shared mechanism summaries, imported rescue/harm only from the matching
analysis JSON, and checked that the main source contains the intended
mechanism numbers (`+14.18`, `+7.13`, `+7.05`, and `1,656`).

The old primary receipt counts are unchanged: natural `4,230/4,710` for
Frozen and MIRA-Relay, `4,217` BayesianRouter, `4,226` Greedy LinUCB, and
`4,202` matched SGD; controlled Frozen is `3,456`, MIRA-Relay `4,124`,
BayesianRouter `3,781`, Greedy LinUCB `3,822`, and matched SGD `3,958`.

The mechanism receipt independently binds MIRA-Shared to `4,218/4,710`
natural decisions and `3,792/4,710` controlled decisions. Rescue/harm are
`114/126` natural and `486/150` controlled for Shared-Credit, versus
`82/82` and `780/112` for Local. The derived Local−Shared controlled
scaffold interval remains `[+3.11,+9.98]` pp.

The verified output is
`experiments/logs/task49_paper_results_binding_20261007/results_v2.json`,
SHA256 `165be45d18ccb014e2afb9e6725df4d7f2969796262197557bda86d1dc72bc54`;
the checker source SHA256 is
`500beb99ea412da01471d312d0ca646bd6c1bf2b49084691cceed7b5f43e771e`.

## Boundaries

This audit does not re-run provider inference or the offline fit. The original
fresh receipts have the v2 full-state replay, and the new Shared-Credit
streams now have a separate import-free replay with score/reward mutation
tests. Other standard baseline streams remain covered by the original
contract audit.
