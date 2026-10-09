# Final paper read-only review — 2026-10-06

**Scope.** Read-only review of `paper/aamas2027/main.tex`,
`paper/aamas2027/build/clean_submission_20261006_matrix_rewrite.pdf`, and
`research/experiment_section_style_20261006/nearest_work_matrix.md`. No experiment was run and `main.tex` was not edited.

## Verdict

The draft has the right top-level order for an AAMAS paper: task contract and method → baseline/fairness contract → short experimental map → natural confirmation table → controlled-drift table → mechanism interpretation → limitations and reproducibility. The PDF has 8 body pages plus a references page (9 pages total), and the two result tables are numerically consistent with the audited receipts.

It is **not yet a clean final gate**. One figure has a semantic label conflict with the formal label contract; the result tables are set at approximately 5.98 pt; and the cost/validity row promises more evidence than the main tables show. These are small and actionable, but they affect methods/benchmark review directly.

## Findings requiring action

### P0 — fix before submission: Figure 1 label contract conflicts with the text

`main.tex:74–76` includes `figures/figure1/v08/figure1_v08.png`. In the rendered figure, the endpoint branch is labelled `selected y_t` and the failure branch says `failure: y_t = 0; no chemical label`. In the formal contract (`main.tex:84–92`), `y_t` is the independent endpoint truth, while selected chemical correctness is `r_t^{chem}` and system success is `r_t^{sys}`. A runtime failure must not be represented as `y_t=0`. The caption is correct at a prose level, but the visible labels can make a reviewer think failures are relabelled as chemical errors.

**Action:** replace those two visible labels with `selected chemical label / r_t^{chem}` and `runtime failure / no chemical label` (or make the graphic use `r_t^{chem}` and `r_t^{sys}` consistently), then rebuild and inspect the full-width PDF figure. Keep the existing figure-version manifest/hash.

### P1 — improve result-table readability

The two main result tables use `\scriptsize` (`main.tex:250` and `278`); PDF text extraction measures their cells at about **5.98 pt**, while body text is about 8.88 pt. At 130 dpi the numbers are barely readable, especially Table 4's CI column. This is below the visual standard expected for a main result table even though no overflow occurs.

**Action:** raise table text to at least `\footnotesize`/the template's readable table size and reclaim space from prose or the audit table if necessary. Rebuild the exact-eight-page PDF and check both tables at 100% zoom. Do not solve this by shrinking captions or moving the tables to a supplement.

### P1 — align the cost/validity row with displayed evidence

The experimental map (`main.tex:233–236`) promises calls, failures, replay checks, and p95 update time for **all confirmation streams**, but Table 3 reports timing only for the natural confirmation and Table 4 has no timing/failure column. The prose reports 420/420 replayable streams and one call per molecule, but does not give stress update p95 or a failure denominator. This leaves the `Cost / validity` row only partially closed.

**Action:** either (a) add one compact sentence/mini-row with natural and stress timing plus failure counts, or (b) narrow the map's metric label to the fields actually shown and point to the exact receipt for the remaining fields. Preserve the audited semantics: CPU policy-operation timing excludes I/O, provider inference, and evaluator scoring.

### P1 — make the audit-only baseline boundary explicit in the paper

`nearest_work_matrix.md` correctly classifies PILOT, GraphRouter, RouterDC, and RouterBench as adapted/audit-only or protocol references. The main text (`main.tex:219`, `330–334`) says only in general terms that unmatched routers are retained in the artifact audit. A reviewer could read the related-work paragraph as an omission of recent baselines rather than a contract decision.

**Action:** add one compact sentence in Benchmark/Baselines naming the excluded recent rows and their concrete mismatch (full-panel feedback, graph/provider-descriptor semantics, or no selected-only delayed implementation). Keep them out of the numeric ranking unless a paper-faithful receipt is produced.

### P2 — use one public method name in the main tables

The paper introduces `MIRA-Relay` but the proposed row is printed as `MIRA-Relay (MIRA-Context)` (`main.tex:211`), and the artifact alias is explained in `main.tex:61`. This is auditable, but it makes the method look renamed during the submission. Keep the artifact alias in the README/manifest and use `MIRA-Relay` alone in the paper's tables and captions, or add one short parenthetical definition once and remove the repeated alias.

### P1 — mechanism claim still needs one stronger counterfactual or narrower wording

The stress table shows MIRA-Relay above matched SGD, LinUCB, and BayesianRouter, and MIRA-Relative is a useful stopped negative variant. However, `nearest_work_matrix.md` records E3 as not fully closed: there is no clean independent-residual/family/interface ablation in the primary result. The current sentence (`main.tex:303`) that MIRA-Relay's larger recovery is the mechanism result is plausible but can be challenged as a difference in update family/likelihood rather than provider-local centering itself.

**Action:** either add a compact provider-local-vs-global/family counterfactual receipt, or state explicitly that the evidence identifies the declared provider-local adapter under the tested stress but does not uniquely identify every implementation component. Do not broaden the natural-stationary null into a generic online claim.

## Sequence and layout check

- **Setup → table:** passes. The four-row map (Table 2) follows the benchmark and fairness contract and precedes both result tables.
- **Table → delta:** passes. Table 3/4 are immediately followed by paired-difference, rescued/harmed, and delay-stratified prose.
- **Delta → mechanism:** passes. Section 8 interprets the natural null and controlled stress after the tables, then Section 9 gives boundary conditions.
- **Page count:** passes the stated limit: pages 1–8 contain the body; page 9 is references. No `Overfull`, undefined-reference, missing-math-delimiter, fatal, or emergency diagnostics were found. The build log contains only template-style `Underfull \vbox` notices.
- **Whitespace:** page 4 and especially the lower-right of page 8 have visible unused space. This is not a hard failure, but it is the safest budget for enlarging Tables 3/4 or adding the compact cost/timing sentence. Avoid adding unrelated prose merely to fill the page.
- **Figures:** Figure 2 (`figures/figure2/v13/figure2_v13.png`) matches the current release-queue contract and is readable at full width. Figure 1 is the sole semantic inconsistency (P0 above). Both formal figure sources are local SVG/HTML assets with manifests; no AI-generated image is included in the current PDF.

## Three acceptance standards: current status

1. **Storyline / innovation:** mostly passes for the declared one-call selected-only scope. The causal sequence is readable and the natural null is honestly separated from controlled recovery. The remaining gate is mechanism sufficiency (P1) and the Figure 1 label conflict (P0).
2. **Benchmark / baseline:** benchmark truth and split/confirmation separation are clear, and strict versus adapted roles are documented in the nearest-work matrix. The main paper should make audit-only modern baselines explicit (P1) and should state why family correlation limits provider-count claims.
3. **Experimental matrix / results:** RQ order and paired tables are strong, with audited natural/stress numbers and bootstrap intervals. The displayed evidence does not yet fully close the map's cost/validity row, and delay effects are given as pooled values plus a sentence rather than a compact delay table/plot. A supplement or one-line delay panel should preserve the audited $D=0,1,4$ values without adding a new claim.

**Overall:** conditional pass after the P0 figure correction and the two evidence/legibility fixes. No new experiment is required for this read-only review; all proposed actions use existing receipts or make the claim boundary explicit.
