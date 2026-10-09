# Figure 1 v07 — MIRA-Relay delayed-feedback architecture

- Source file: `/Users/jingwu/Downloads/MIRA-Relay延迟反馈架构图.png`
- Paper asset: `figure1_v07.png`
- Dimensions: 2120 × 742 px, RGB PNG
- SHA-256: `f29cb6865b85f601e1969e8e145b2ca9052fbdcf20cf6f15191a6f8df8520f4c`
- Role: full-width method overview
- Supersedes in the working paper: `figure1/v6/figure1_v6.pdf` (retained)

## Semantic contract

The image has four regions: inputs/candidates, MIRA-Relay state, tool execution/evaluation, and delayed feedback/state update. The fit-only reference is immutable; family residual and provider bias are the only online relay state. A selected call creates a terminal record, then the frozen endpoint judge supplies chemical correctness after termination. Runtime failure follows a separate ledger path. The selected outcome enters a delay queue and becomes part of the released state only at $\tau_t=t+1+D$; decisions continue while feedback is pending. The fallback outcome is diagnostic-ledger-only.

The diagram does not imply that unselected outputs are observed, that chemical correctness is available before termination, or that fallback feedback updates the relay.
