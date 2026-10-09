# Figure 2 v9 review

## Purpose

This revision preserves v8 geometry and changes `runtime verifier` to `terminal record`, matching the paper contract that the direct runner records runtime status and obtains endpoint correctness from the frozen evaluation judge after termination.

## Checks

- [x] Decision → selected call → terminal record/judge → pending queue → release → next action is explicit.
- [x] The unselected output remains ghosted and has no feedback arrow.
- [x] D=0 is still next-tick visibility, never same-action feedback.
- [x] No post-call verifier or second call is drawn.
- [x] SVG/PDF/PNG remain editable/vector/review artifacts.

## Independent review

2026-10-05 score: **15/16** under `FIGURE_REVIEW.md`. Information, direction, boundary, method mapping, restraint, accessibility, and editable evidence each pass; one point is withheld for small auxiliary text at final single-column size (especially the unavailable-output ghost).
