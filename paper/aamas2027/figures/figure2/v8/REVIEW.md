# Figure 2 v8 self-review

Date: 2026-10-03

## Checks

- [x] Vertical sequence fits a single-column figure without five narrow horizontal columns.
- [x] Source is 800×900 CSS px; output is vector PDF and editable SVG.
- [x] Main labels are 29px and body labels 26px CSS; at the intended 3.33in column export this is approximately 8–9pt.
- [x] Explicit lifecycle: decision → call → terminal/judge → pending D → release → next action.
- [x] Release is `τ_t = t + 1 + D`; the next action sees the state only after release.
- [x] Runtime status is visually and verbally separate from the frozen endpoint label.
- [x] Unselected output is crossed out and marked not observed; no ghost feedback arrow is drawn.
- [x] No unimplemented escalation/refusal/second-call branch is shown.

## Remaining limitations

The figure does not encode family residual/provider bias or fallback ledger semantics; Figure 1 and the method text own those details. The caption should identify the frozen judge as an evaluation-time source of the label after termination.
