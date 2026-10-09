# Provider-fresh natural MIRA-Context confirmation v1

- 157 scaffold-disjoint BBB candidates; four sealed checkpoints; 5 folds × 2 seeds × 3 delays.
- 270 streams and 127,170 event rows; one selected call per molecule; selected-only feedback released at `decision_tick + 1 + delay`.
- Contract/replay verification: `270/270` streams, no forbidden decision fields, no replay mismatch.

MIRA-Context-Reliability obtains **89.81%**, exactly the Frozen selector's **89.81%**. The paired event delta is `0.00 pp`, with scaffold bootstrap 95% CI `[-1.24,+1.40] pp`. It changes 164 actions relative to Frozen (82 rescued and 82 harmed), so the null utility result is not caused by a static action sequence. The natural fresh menu therefore supports stability and information-boundary claims, but not a generic online improvement claim.

The result is a completion of the method-consistency check: the same MIRA-Context method is now evaluated on both the stationary fresh menu and the preregistered controlled correctness-drift stress. The stress result remains a separate mechanism result (`+14.18 pp`, CI `[+9.61,+19.03]`); it is not reclassified as natural drift.

## 2026-10-06 state-hash rerun addendum

The original v1 receipt was rerun after serializing the MIRA-Context fields that directly determine future scores: `fit_rates`, `successes`, `counts`, and `prior_count`. No data, candidate menu, prior, hyperparameter, seed, delay, or aggregation rule changed.

- Receipt: `experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/`
- Streams/events: 270 streams; event count remains 127,170.
- Contract/replay verification: 270/270; forbidden decision fields 0; replay mismatches 0.
- MIRA-Context: 0.898089 (89.81%); Frozen: 0.898089 (89.81%); paired delta 0.000000; scaffold bootstrap CI `[-0.012393,+0.013964]` (rounded `[-1.24,+1.40]` pp).
- Receipt hashes: events `69598343f9254ff9ea1b46a7ae5c3fb41acbf3aed82333aebe1447780d488aab`; summary `93454fa5372aea61045f01f208bfae74c872ad2761c3a34964d657096a8979d1`; config `46be8e926adc40871415436771b6c10c2123dc6e9885414f7dff6376a693d3bf`.

This rerun closes the state-hash audit gap; it does not change the natural null result or broaden the claim beyond a stationary fresh menu.
