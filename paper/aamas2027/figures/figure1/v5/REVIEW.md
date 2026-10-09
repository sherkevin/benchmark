# Figure 1 v5 review

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
