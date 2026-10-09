# Space allocation after observed results

Status: proposed replacements only; no main-file edit or compilation. **This supersedes the v01 suggestion to remove the experimental map. Keep `tab:matrix`.** Preserve the old result numbers and the new complete-roster arrival table.

## 1. Replace repeated validity fields with a compact paragraph

In `main.tex`, replace the text from `\subsection{Validity and reproducibility}` through the end of `\subsection{Artifact scope}` with the following candidate. Its verification statements apply explicitly to **the original-order artifacts**, not to the still-pending arrival audit.

```latex
\subsection{Validity and reproducibility}
Manifests bind splits, source code, checkpoints and features; confirmation
molecules are scaffold-disjoint from development fit and validation.
Truth is evaluator-only. All 270 natural and 150 controlled original-order
streams pass contract checks. Independent receipt closure reconstructs canonical
states and scores and checks release clocks, checkpoint restart and malformed
records. These checks verify the declared policy replay, not a second provider
inference or training implementation. The artifact preserves full event fields
and the 77,976-event development reconstruction.

\subsection{Artifact scope}
Static diagnostics retain 30 initialization/best checkpoints, 4,000 epoch
records and 2,888 held-out predictions. Independent NumPy recovery matches
heads, choices, aggregates and checkpoint selection within a maximum logit
error of $1.93\times10^{-7}$. Original primary streams contain no failures,
unknown truth or refusals; those transitions and integrated multi-call agent
evaluation remain outside their measured scope.
```

The method already defines the ledger and read set. Repeating every receipt field in Discussion does not add a new scientific check. The existing preceding Discussion paragraphs continue to state endpoint, lifecycle and provider-family limitations; their list need not be copied again below this replacement.

The new arrival result must not inherit the old `270/150 pass` statement. Add its verified audit status only after the independent audit closes; do not infer it from the formatter manifest.

## 2. Use targeted replacements, not additional caveat paragraphs

- Replace the old protocol's scaffold-interval sentence with the fixed-trajectory-range definition in `../MERGE_NOTES.md`.
- Use `arrival_insert.tex` as the complete new arrival discussion; do not append an additional paragraph repeating fixed priors, inspected cohort, or non-generalization.
- Preserve `tab:matrix`. Update its E1/E2 evidence links to include the arrival result without adding a new experimental ID or duplicating the whole protocol in the map.
- Preserve the natural pooled negative difference, primary SGD effect/MCSE/mean interval/win interval, every baseline row, and the scope of the time-based flip. These carry scientific information and are not space-saving candidates.
- If prose must shrink further, the old main-result paragraph may stop repeating all three modern controls' gains, because their unchanged values remain in the original table. Keep Relay's point estimate, rescued/harmed counts, and natural null. Do not silently drop inconvenient new estimates or relabel the weighting target.

## 3. Space check still required

The replacement consolidates the existing validity/artifact material; it does not guarantee the float will fit within eight pages. Root should merge after audit, compile once, and inspect the affected pages. Prefer float placement and deletion of duplicated prose to reducing the 9 pt table body or suppressing comparisons. This folder contains no compiled-page claim.
