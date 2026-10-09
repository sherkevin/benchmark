# Arrival replay implementation review

Date: 2026-10-08. Read-only code review by `arrival_replay_engine`, recorded by the coordinator. No experiment was run. This supplements the draft card; it does not freeze or authorize the full replay.

> **Historical snapshot notice (added after execution).** The paragraphs below record the
> pre-freeze review state. They are not the final execution status. The run was later bound
> to the frozen card `experiment_card.md` (SHA-256
> `9fc008e3b56c894929f71d83b1369007f4f5ce38285a30ee7856e293d1b652f1`), approved by
> `resource_go.json`, and completed in `full_v1/`. The final results and independent
> `full_v2` verification are in `report.md`; no new experiment is implied by this note.

## Reusable inputs

- Cached confirmation probabilities: 157 molecules × 4 providers; SHA-256 `14c05b6485f48abdcf5c922685a12c7526d5a31c8cf668313cb7d9a51776d06e`.
- Confirmation manifest: 157 rows; SHA-256 `c0af602bbc4ec49df448b916fb1e54dfce183926965d61c184cb736e68402805`.
- Prior cache: five sets of weights `(4,65)` and fit rates `(4,)`; SHA-256 `7e76743f5258df4714b23528acce55bdcb926ad429bb40b36bfc9c281a475206`.
- No standalone confirmation feature array was found. Generate it once using the existing `selector_feature`: Morgan radius 2, 64 bits, chirality, L2 normalization and intercept. Save the resulting `(157,65)` array and its hash. The development `(722,65)` cache is not the confirmation data.

Paths and hashes are recorded in `experiment_card_draft.md`. This route requires no provider inference or refitting.

## Preserve the actual algorithms

Use `run_provider_fresh_confirmation_v1.make_policy` for main-table methods. Frozen, Online SGD and the Gaussian controls begin with the original logit weights. SGD uses learning rate .05; LinUCB uses precision 50 and alpha .5; BayesianRouter uses prior variance .02 and observation variance 1. Preserve the existing MIRA-Relative parameters.

The main MIRA computes its residual directly from counts on every decision. Mechanism-v2 Local accumulates posterior increments. These are algebraically equivalent but can differ at floating-point precision; do not silently substitute one implementation and claim byte-identical states. Likewise, main Frozen returns logits and mechanism-v2 Frozen returns sigmoid scores. The old action traces and scores need separate, appropriately stated comparisons.

The fixed credit permutation is the existing cyclic provider mapping; its seed does not create a random permutation. The retrospective fixed control is the natural full-cohort best fixed provider, not a per-question oracle. Retain its evaluator-only role.

Release due feedback before each decision at `decision_tick + 1 + delay`. The old main runner drains remaining feedback at tick 163; an exact-due tail drain changes only post-decision timestamps, provided update order and final states remain identical. The controlled flip remains tied to arrival ticks 78–156, not to a fixed subset of molecule IDs.

## Compact evidence without changing policy behavior

The old runner repeatedly serializes complete Gaussian covariance arrays for state hashes. This logging occurs outside its decision/update timers. It is unnecessary for independent replay from the initial state.

The final card should explicitly replace per-event full-state hashes with:

1. Frozen inputs, source snapshots, parameters, versions and exact order arrays.
2. Every decision's stream coordinates, molecule/tick, selected provider, four float64 scores, feature reference and state version.
3. Separate evaluator records for original/returned labels, flip flags and correctness.
4. Ordered releases with event/provider/reward, due/actual ticks and versions before/after updates.
5. Every stream's complete final numerical state, initial/final RNG state, pending-empty check, consumed-event count and canonical hash.
6. Chunk hashes, completion flags and a decoder, followed by independent replay of every decision, release and final state.

This is sufficient to check replay from the beginning, not a claim that arbitrary intermediate restart checkpoints were saved. The draft currently requests per-event state hashes; revise that contract before execution rather than silently omitting them.

BayesianRouter calls `rng.normal(size=4)` once per decision. Record NumPy version, bit-generator identity, seed and initial/final state. Normal sampling need not consume exactly four underlying PCG64 draws. An independent checker must preserve the normal-sampling interface and call order while implementing the update equations independently.

## Remaining resource gate

The full card requires 480 streams per order, 61,440 random-order streams and 9,646,080 decisions. Gaussian final arrays alone are approximately 2.1 GB uncompressed. Stream and compress artifacts rather than accumulating all events or states in memory.

Recorded Gaussian decision/update medians imply roughly seven minutes of algorithm time for their share of this workload; this is not a full-run runtime estimate. The original-order smoke must still measure exact inheritance, elapsed time, memory and compressed output size. Retain the draft's fixed limits and full comparator roster. No methods or order count may be dropped after seeing effects.

Next: revise and freeze the card, implement the compact runner and independent checker, then execute the original-order reproduction/resource smoke. Full 128-order execution depends on that check; none has run under this card yet.
