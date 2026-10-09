# Figure 1 v4 self-review

Date: 2026-10-03

## Checks

- [x] Four main bounded regions; no text wall.
- [x] Cross-column source is 1500×600 CSS px; intended 504pt final width gives approximately 8pt body text (22px CSS at 96dpi), 9pt labels (25px), and 10pt headings (27px).
- [x] The fallback score is explicitly fit-only and immutable online.
- [x] Runtime status and frozen endpoint truth are separate.
- [x] Delay is explicit: `τ_t = t + 1 + D` and no label before release.
- [x] Challenger updates shared family residual/provider bias; fallback is ledger-only.
- [x] Unselected outputs are explicitly not observed.
- [x] No unimplemented escalation/refusal branch is drawn.
- [x] SVG remains editable and PDF is vector.

## Remaining limitations

The figure intentionally does not encode every update equation, hazard-gate detail, or confirmation protocol. Those belong in the method and experiment sections. The paper caption must state that the frozen judge is an evaluation oracle used after termination, not a signal available to the runtime selector.
