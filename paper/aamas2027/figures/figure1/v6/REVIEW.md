# Figure 1 v6 review

## Purpose

This revision preserves the geometry of v4 and fixes the direct-runner terminology. The figure now says `runtime ledger: output + status`; the frozen endpoint judge remains an evaluation-time source of correctness after termination.

## Checks

- [x] Four bounded regions and left-to-right reading are unchanged.
- [x] Selected-only and delayed-release semantics remain visible.
- [x] Direct lifecycle contains no post-call verifier or second-call branch.
- [x] Runtime status and chemical correctness are separate.
- [x] SVG/PDF/PNG remain editable/vector/review artifacts.

## Remaining limitation

The E5 post-call verifier is intentionally not drawn in the direct runner. If it is added later, it must appear as a dashed extension labelled `E5 extension (not in direct runner)`.

## Independent review

2026-10-05 score: **15/16** under `FIGURE_REVIEW.md`. Information, direction, boundary, method mapping, restraint, accessibility, and editable evidence each pass; one point is withheld for auxiliary labels rendering below the 8 pt target at final cross-column size.

## v6 change

Replaces the unsupported “safe cold start” phrase with “anchors the cold start”. Geometry is unchanged. Labels remain small at final size; the AI prompt requests shorter labels and a minimum effective 8 pt for the next visual iteration.

## v6 semantic revision

The subtitle now says `immutable cold-start reference`, avoiding a safety guarantee. Released challenger feedback is explicit in the description. Geometry and paper contract are unchanged.
