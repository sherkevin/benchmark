# Open-source Jev candidate evaluation

## Decision

Use **AnyJev** as the implementation scaffold for the dynamic JEV selector. Pin the downloaded source to commit `a59a69eaa91e62dc9b42924f8491f76d1b1972ea` until we choose a release. Treat the online method in our paper as a new wrapper/head update algorithm; do not present AnyJev's existing `observe` as our contribution.

The source snapshots are under `reference/`. The exact source metadata is in `raw/source_manifest.json`. No model weights were downloaded.

## Why AnyJev is the best fit

Our feedback is one selected action with a binary outcome. AnyJev already exposes the matching typed primitive (`Question.noul`), a frozen model/backbone path, a labelled hidden-state head (`fit_head`), an online label intake (`observe`), and artifact export/load. Its built-in loop is therefore the shortest path to a working prototype and a credible comparator.

The binary task is encoded as one fixed question:

```text
Question.noul("Is the selected tool result correct?", name="tool_correct")
```

The state is a canonical JSON record containing the molecule, target property, tool ID/version, tool input, and returned tool result. For every executed tool call, `correctness=1` maps to AnyJev's `Yes` option index `0`; `correctness=0` maps to `No` option index `1`. The selector scores the same question on one state per candidate tool and chooses the candidate with the largest predicted correctness probability.

A local smoke test exercised exactly this mapping with 16 selected-only observations. AnyJev solved its first head at 8 labels, refit at 16, and produced the same prediction after export/load. The raw JSON is `raw/binary_observe_smoke.json`; the upstream fake-backend test suite passed 24 tests.

## Candidate comparison

| Candidate | Binary decision | Frozen backbone + light head | Native online label intake | Dynamic menu/version support | Role in our work |
|---|---:|---:|---:|---:|---|
| **AnyJev** | Yes (`noul`, p_true) | Yes (L2 hidden-state head) | **Yes (`observe`, refit at 30/60/120...)** | Wrapper needed | **Primary scaffold and fixed-JEV comparator** |
| Laya | Yes (`noul`) | Yes (encoder + typed decision head) | No; hooks only, no label fit/update API | Wrapper needed | Static strong baseline or alternative backbone |
| SemIf-OpenJev | K-way direct option logits | No exposed hidden-state head/update loop | No; offline temperature scaling only | Input options are dynamic, update wrapper needed | Direct-logit baseline |

Source licenses are Apache-2.0 for AnyJev and Laya, MIT for SemIf-OpenJev.

## What AnyJev already solves

- Prompted typed decision readout with `choice`, `noul`, and `score`.
- L0 position/prior correction and L1 temperature calibration.
- L2 closed-form per-question hidden-state head with no base-model weight update.
- `observe(question, state, label)`, thresholded fitting, refitting, observation storage, and artifact persistence.
- Routing/adaptation for question wording and option order.

## What AnyJev does not solve

`observe` is not a per-event constant-time learner. It stores all labels and refits a closed-form head on the full history at 30, 60, 120, ... labels. It also assumes option-index labels and has no delayed-feedback queue, arm identity protocol, dynamic candidate masking, new-tool cold start, explicit propensity, or drift-aware forgetting. These are the parts our method must add.

## Proposed integration boundary

Keep the AnyJev model and prompt/readout frozen. Add a separate `DynamicToolSelector` wrapper with four operations:

```text
score(context, candidate_menu) -> {tool_id: p_correct, uncertainty, propensity}
choose(context, candidate_menu) -> tool_id, propensity, state_version
observe(event_id, correctness, observed_at) -> updated_state_version
snapshot()/restore()
```

The first implementable online learner should be a residual adapter on the frozen AnyJev score, updated only when the executed tool's label arrives. Use exponential forgetting and a rank-one online update so each feedback is O(d²) or lower, depending on the chosen diagonal/low-rank approximation. Keep a slow full `fit_head` refit as a scheduled checkpoint and ablation.

The state must include a versioned candidate identity (`provider`, `model`, `version`, `configuration`) so a new tool is a cold-start arm rather than silently sharing a stale label distribution. Log the chosen arm, candidate menu hash, score vector, propensity, feedback delay, update version, and checkpoint hash.

## Baselines to freeze before implementation

1. AnyJev raw/L0/L2 with no online update.
2. AnyJev `observe` with its existing 30/60/120 full refit schedule.
3. Laya static `noul` scoring.
4. SemIf direct logits with fixed offline temperature.
5. A standard contextual bandit using the same feature/state contract (for example diagonal LinUCB or Thompson sampling).
6. Random and best-global-tool policies.

The first paper claim should be narrow: **selected-only 0/1 feedback can update a frozen JEV-style decision model online and improve tool-choice reward under drift at bounded update cost**.

## Reproduction checks

- AnyJev fake-backend tests: 24 passed.
- SemIf core/calibration tests: 12 passed.
- Laya training checks passed 61 checks, but its `test_training.py` exits during pytest collection by design; the combined pytest command therefore returned a collection error. A separate Laya model test could not run in the current Python 3.9 environment because installed `huggingface-hub==1.8.0` conflicts with the installed Transformers requirement. This is an environment mismatch, not evidence against the source implementation.

## Next implementation step

Implement the thin AnyJev adapter on the existing Task36 replay schema, first with a static score plus an online residual state. Before any large run, freeze the state schema, selected-only feedback timing, train/validation/test split by molecule, candidate-version cold-start rule, update cadence, and drift generator.
