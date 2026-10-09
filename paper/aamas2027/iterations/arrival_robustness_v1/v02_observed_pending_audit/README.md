# v02 — observed results, independent audit pending

This is a proposed paper fragment, generated from the completed `full_v1/results.json`. It is **not approved for main-source integration or submission**. The LaTeX guard remains enabled. The independent experiment audit and scientific gate are separate from the formatter checks; scientific gate remains **HOLD**.

## Files

- `arrival_insert.tex`: one full-roster table and two paragraphs, with observed values populated automatically.
- `cell_ledger.json`: every numerical table/prose field maps to its source JSON pointer, raw value, scale, displayed precision, and source hash. Structural reference/em-dash cells carry explicit reasons.
- `results_source_snapshot.json`: byte-identical snapshot of the source result file.
- `all_reported_statistics.json`: complete primary and secondary statistics, including contrary directions, all delays, distributions, and equal-scaffold contrasts. Nothing is filtered by sign.
- `source_manifest.json`: source/card/renderer hashes, generated-file hashes, and the limited formatter checks.
- `compression_plan.md`: retain the experimental map; replace duplicate validity prose to allocate space.

Renderer: `../render_observed_v02.py`. Run from the repository root:

```bash
python3 paper/aamas2027/iterations/arrival_robustness_v1/render_observed_v02.py
```

Source SHA-256: `dbaf3e64f9f3695b0d0a20ade3124b25b1732e24cacad46e76e8ec0e12044946`.

## Interpretation preserved

The primary controlled comparison is MIRA-Relay minus Online SGD. The prose also reports the **negative natural pooled MIRA−Frozen difference**; the positive equal-scaffold Local−Frozen statistic is a different target and is not used to replace it. The controlled Local−Permutation effect is explicitly equal-scaffold and pointwise.

The original order is excluded. One random order is one independent Monte Carlo unit; priors, delays, and the two fixed policy RNG realizations are averaged inside it. The cohort was viewed before the robustness design. No fresh confirmation, new-scaffold inference, generic stationary benefit, or deployment guarantee is claimed.

Existing original-order main/credit numbers remain unchanged. Their old intervals are fixed-trajectory scaffold-resampling ranges; new order-law MC mean intervals do not replace or retroactively validate them. Apply the scoped renaming checklist in `../MERGE_NOTES.md`, leaving the static post-call inference contract intact.

No compilation, main merge, PDF promotion, commit, or push was performed. The formatter checked expected roster sizes, primary/secondary consistency, preserved negative direction, and populated cells. These checks are not independent reconstruction of the experiment. Root owns final reconciliation after the auditor returns and the eight-page check.
