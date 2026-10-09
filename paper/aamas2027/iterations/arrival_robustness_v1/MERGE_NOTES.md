# Arrival robustness — merge instructions

2026-10-08. Internal template only. No new numerical results read, inserted, or inferred. Scientific gate: **HOLD**. No main source/PDF, preview table, effective document, or TODO is changed by this iteration. No Git mutation or compilation was run.

## Proposed addition: one compact table and two short paragraphs

`arrival_insert.tex` supplies the complete condition-specific roster: ten natural rows and six controlled rows. Every empirical cell remains `TBD`; em dashes mark only the methods excluded by the frozen roster. No automatic winner bolding or success sentence is included.

The two paragraphs separate (1) the order-law design from (2) the primary comparison and source-credit summary. The primary comparison is **controlled MIRA-Relay − Online SGD**, fixed before these orders. Mean accuracy/gain for every other comparator remains visible even if its result is inconvenient. Online SGD is called the strongest *previously recorded* strict control, not presumed to remain the strongest after this study.

The template inherits the current 9 pt table body / 8.5 pt note, two-level condition headers, horizontal group rules, and aligned numerical columns. This reuses the local reference-style report without borrowing a new external claim or citation. Keep the same precision as the present tables: percentage accuracy and pp effects to two decimals; report sufficient digits for MCSE and positive-order probability to avoid hiding threshold crossings. Use unrounded quantities for decisions.

## Correct the old interval interpretation in place

Preserve all original point estimates, interval endpoints, rescued/harmed counts, and sealed receipts. The audit found an inference-scope limitation, not a numerical error or an order-replay defect in these old calculations. Apply these small replacements before any new result is discussed:

| Existing location / anchor in `main.tex` | Replacement |
|---|---|
| `MiraMainBody`: `95\% CI` column | `95\% range` |
| `MiraMainBody` note | Append `Ranges are central 95\% scaffold-resampling ranges of fixed recorded trajectories.` |
| `MiraCreditBody`: both `[95\% CI]` headers | Both `[95\% range]` |
| `MiraCreditBody` note | Append the same fixed-record interpretation; retain equal-scaffold versus pooled weighting distinction. |
| Abstract: `with a 95\% scaffold bootstrap interval of` | `with a fixed-trajectory scaffold-resampling range of` — retain `[+9.61,+19.03]` and `+14.18`. The main-table note supplies the central 95% definition. |
| Natural paragraph: `95\% CI $[-1.24,+1.40]$` | `fixed-trajectory range $[-1.24,+1.40]$` |
| Controlled paragraph: `95\% CI $[+9.61,+19.03]$` | `fixed-trajectory range $[+9.61,+19.03]$` |
| Mechanism paragraph: `its natural interval crosses zero` | `its natural fixed-trajectory range crosses zero` |
| Method falsification: `scaffold-cluster intervals` | `fixed-trajectory scaffold-resampling ranges` |

Replace the final sentence of Evaluation protocol with:

```latex
Tables~\ref{tab:main-results} and~\ref{tab:credit-ablation} report central
95\% scaffold-resampling ranges of fixed recorded trajectories, preserving
each scaffold's repeated configurations. These describe reweighting sensitivity,
not coverage for new adaptive streams.
```

Do not globally replace every `CI` or `bootstrap` in the paper. The static post-call comparison has a different conditional inference contract; retain its fitted-gate / fixed-random-draw conditioning. Do not relabel the new order-mean MC interval as an individual-order range.

## Eight-page integration plan

**Preferred:** preserve the current main/credit point-result panels, place the arrival table immediately after their discussion, and replace existing low-value duplication:

1. Remove the separate experimental-map table (`tab:matrix`), replacing its introductory reference with a short sentence naming the progression: adaptation, attribution, development diagnostics, cost, secondary refusal. Nearby subsection headings already provide the map. Do not cut actual comparator rows to preserve this navigation table.
2. Consolidate `Validity and reproducibility` into its input/split paragraph plus a single receipt sentence. The long field-by-field event-record paragraph repeats the method's ledger and existing contract checks; retain full details in the artifact. Preserve the observed pass counts and the distinction between policy replay and provider inference.
3. Remove the repeated scaffold-bootstrap sentence from Validity after putting the corrected definition in Evaluation protocol. Keep the development 77,976-event verification evidence in the artifact if necessary. Do not remove natural null, the primary SGD comparison, or the post-call conditioning to make room.

These are space-reallocation proposals, **not a checked page budget**. Root must merge, compile, inspect, and establish eight pages. No new section is needed. If still too long, use the panel option below rather than shrinking the table body.

**Alternative panel integration:** replace the original main table with one two-panel float: (a) the unchanged original-order numerical table, with renamed range column; (b) this new all-roster arrival table. Share one caption defining original-order versus order-averaged estimates; keep panel-specific headers and source-credit weighting separate. Move the old table's repeated prose/numerical restatement into the shared note. This keeps one main comparative object, but does not itself guarantee less height. Do not make a nine-column table mixing old ranges and new MC uncertainty.

## Evidence-dependent conclusion: select only after independent verification

The card requires both the lower bound of the approximate mean interval to exceed zero **and** the lower bound of the exact positive-order probability interval to exceed 0.5. Neither a positive mean alone nor a majority alone closes this joint directional test. It is not a practical-effect margin or broad scientific acceptance gate.

| Verified pattern | One-sentence interpretation for the second paragraph |
|---|---|
| Both required lower bounds pass | `The prespecified mean and majority-of-orders criteria support an incremental benefit under this fixed-cohort arrival law.` |
| Positive estimated direction, one or both intervals cross their thresholds | `The observed direction is positive, but the prespecified joint criterion remains unresolved.` Only use “positive direction” if both the mean and observed win fraction have the required direction. |
| Estimated mean is nonpositive or positive fraction is at most one half | `The sampled orders do not support the proposed mean-and-majority advantage over Online SGD.` Report which property fails; do not imply statistical equivalence. |
| Evidence incomplete or independent verification fails | Do not integrate a completed-result paragraph; preserve the template and failed artifacts. |

If Relay beats Frozen but not SGD, preserve the former measured recovery while stating the failed incremental comparison. A nonpositive equal-scaffold Local−Permutation mean does not support source-credit robustness. Natural results are reported for every row regardless of sign. Never tune or remove a method to improve this story.

## Required numerical binding before removal of the template guard

- Original order `0` excluded; exactly 128 sampled orders, paired across the full roster and both conditions. Each order averages five priors × two fixed policy seeds × three delays. No extra independent-sample credit for molecules, priors, delays, or deterministic seed replicas.
- Table means come from the rerun adaptive trajectories. Pooled gain columns compare each row against Frozen on the same order/configuration. The source-credit prose uses equal-scaffold weighting and must not copy pooled values into that slot.
- Bind primary mean, MCSE, sample SD, Student-t/CLT mean interval, positive count, and Clopper–Pearson interval to verified aggregates. For a probability interval use proportions (or explicitly `%`); do not mix it with count units.
- The complete accompanying analysis must include every method's two-condition accuracy and Frozen contrast; Relay versus all three modern controls; all three equal-scaffold credit contrasts; per-delay summaries; SD, median, quantiles, minimum, maximum, and histogram. Secondary intervals stay pointwise exploratory. If these are only in local logs, replace “supplementary analysis” with a genuine provided appendix/artifact reference before submission.
- Keep the intervention at ticks 78–156 under each new order. It changes affected molecules as well as feedback ordering; do not describe this as only permuting feedback for a fixed set of corrupted outputs.
- No claim of new-scaffold, training, checkpoint, new-policy-seed, or deployment uncertainty. The 157 molecules were inspected before this robustness design; this is not fresh confirmation.
- Preserve provider-local mechanism and scientific target. This study evaluates arrival sensitivity; it introduces no new method, no new novelty, and no automatic promotion of the global HOLD gate.

## Sources read

- `paper/aamas2027/main.tex` (current original-order results and layout macros).
- `research/experiment_section_style_20261006/primary_uncertainty_scope_audit_20261008.md`.
- `task/task49-pharma-benchmark-baseline-freeze/primary_arrival_robustness_v1/experiment_card.md` (frozen estimand, roster, inference, and stopping rules).
- `paper/aamas2027/experiment_tables/reference_style_report.md`.
- `TODO_LOG.md` latest entries; `paper-writer` and `academic-writing` skills.

Parent should record this narrowly scoped writing handoff in the project log; this worker was explicitly instructed not to edit the shared TODO.
