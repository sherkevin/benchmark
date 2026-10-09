# Figure 1 v1 review

## Scope

This is the first editable framework draft for the paper's method overview. It is a layout candidate, not a final submission figure. The main paper should replace the placeholder in `main.tex` only after a later reviewer pass.

## AAMAS and top-conference checks

| Check | Status | Evidence |
|---|---|---|
| One-sentence takeaway | PASS | The figure shows selected tool choice, delayed selected-only feedback, and the state used by the next choice. |
| Left-to-right reading order | PASS | Public context → selector → provider → verifier → delay queue → update. |
| Information boundary explicit | PASS | The top band separates `BEFORE ACTION` from `AFTER SELECTED CALL`; the queue marks `release at τₜ`. |
| Unselected outcomes excluded | PASS | The public-context and provider panels state that unselected outputs are hidden/not called. |
| MIRA-Relay visibly distinct | PASS | Blue outline/fill is reserved for the selector state and update state; the fit-only head is gray. |
| Probe/exploit and selected-only label | PASS | Orange chips and annotation show that feedback comes from the selected call. |
| Runtime and chemical semantics | PASS | Provider runtime status and end-to-end `system_success` are shown separately from the verifier. |
| Delayed release | PASS | Purple dashed queue and release arrow make the visibility clock explicit. |
| No empirical claim in Figure 1 | PASS | No accuracy, speed, or improvement value is shown. |
| Color-blind-safe redundancy | PASS | Color is paired with panel names, line styles, and text; no red/green encoding. |
| Vector source | PASS | `figure1_v1.svg` is the editable source; `figure1_v1.pdf` is generated from the HTML. |
| Figure accessibility description | PASS | The SVG contains a title/desc; the LaTeX integration can use the matching `\\Description{}`. |
| Caption placement | PASS | The intended LaTeX caption remains below the figure, per the AAMAS template. |
| Single-column legibility | NOT YET | This is a `figure*` candidate. It should not be placed in a single column. |

## Render checks

- Canvas: `1800 × 560` SVG units, target aspect ratio 3.21:1.
- Browser render: Chromium headless via `render.py`.
- PNG: `figure1_v1.png`, `1800 × 560`, RGB.
- PDF: `figure1_v1.pdf`, one page, vector text and paths; page size `1350 × 420 pt` before LaTeX scaling.
- `pdftotext` confirms labels and semantic symbols are present.
- `pdffonts` confirms embedded font resources; the browser may substitute a platform sans font for a few Unicode subscript glyphs.
- At 100% PNG preview there are no visible overlaps, clipped boxes, or crossing main-flow arrows.

## Known issues for v2

1. The figure is intentionally wide. At final AAMAS `\\textwidth` scaling, secondary labels may fall below the preferred 8 pt reading size. A v2 should either increase the SVG text sizes or reduce the number of words in secondary lines.
2. The top information-boundary band is useful but may be too dense once the final caption is added. Keep it only if it still reads at two-column print size.
3. The molecule glyph is schematic and carries no scientific meaning. It can be replaced by a tiny SMILES/molecule icon only if that improves the public-context cue without adding clutter.
4. The provider panel intentionally shows one selected provider. A later version may add a very small muted menu strip (`a₁ … a_K`) if reviewers think the menu constraint is not obvious enough.
5. The final paper should use the same names and symbols as the method text (`system_success`, `τ_t`, `S_{t+1}`) after the notation is frozen.

## Decision

Keep v1 as the baseline layout. Do not overwrite it when making v2 or v3; copy the directory and preserve the rendered artifacts and review notes for comparison.
