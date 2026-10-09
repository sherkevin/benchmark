# Figure 1 v2 review

## What changed from v1

- Main panel headings and key labels were enlarged for full-width AAMAS placement.
- Long secondary labels were shortened (`fit-only head`, `others not called`, `label hidden until τₜ`) so they do not clip at the larger type size.
- The verifier now shows all three contract outcomes: accept, escalate, and refuse.
- v1 remains intact in its own directory and is not overwritten.

## Target-size calculation

The browser PDF has an intrinsic page width of 1350 pt. The expected AAMAS `figure*` width is approximately 504 pt (full 7-inch text width), so the PDF is scaled by about `504/1350 = 0.373`.

- v1 22 px heading → about 6.2 pt at final width.
- v2 27 px main heading → about 7.6 pt at final width.
- v2 23–24 px section labels → about 6.4–6.7 pt.
- v2 18–21 px supporting labels → about 5.0–5.9 pt.

The main semantic labels therefore meet the 7–8 pt target; the small explanatory lines are intentionally subordinate and should be checked in the final paper PDF. If the camera-ready figure must make every line 7 pt or larger, the next iteration should delete supporting lines rather than shrink the panel or alter the AAMAS template.

## Checks

| Check | Status | Evidence |
|---|---|---|
| One-sentence takeaway | PASS | Selection, selected-only feedback, delayed release, and next-state update are visible in one pass. |
| AAMAS two-column geometry | PASS | 1800×560 canvas, one-page vector PDF, target full-width `figure*`. |
| Main-flow arrows | PASS | Six stages read left to right; no crossing main arrows. |
| Information boundary | PASS | Top band separates pre-action inputs from post-call observations and release. |
| Selected-only semantics | PASS | `aₜ only`, `others not called`, and selected-only `yₜ` are explicit. |
| Verifier outcomes | PASS | accept / escalate / refuse shown after provider return. |
| Color and redundancy | PASS | Blue = adaptive state, gray = fixed/public, orange = event/branch, purple = delay; labels and line styles repeat the distinction. |
| Accessibility | PASS | SVG title/desc present; integration should retain a matching LaTeX `\\Description{}`. |
| No empirical overclaim | PASS | No result number or performance claim in the figure. |
| Print legibility | CONDITIONAL | Main labels pass the estimate; secondary lines need final PDF inspection. |

## Render evidence

- `figure1_v2.png`: 1800×560 RGB PNG.
- `figure1_v2.pdf`: one-page PDF, 1350×420 pt before paper scaling.
- `pdftotext` extracts all panel headings and labels.
- `pdffonts` reports embedded sans-serif resources.
- At 100% preview, no text is clipped and no boxes overlap.

## Recommendation

Use v2 as the candidate for the next reviewer pass. Keep v1 as the compact baseline. Before integrating into `main.tex`, render the complete AAMAS PDF and inspect Figure 1 at 100% and at the actual printed two-column size. If secondary text is too small, remove it rather than reducing the figure width or changing the template.
