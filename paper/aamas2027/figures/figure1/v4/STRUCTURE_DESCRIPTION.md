# Figure 1 v4 structure description

## Intended reading

Read left to right in four bounded regions. The first region combines the public context and the selector. It separates the immutable fit-only fallback score from the trainable online relay state. The second region contains exactly one selected provider call. The third region separates runtime verification from the independent frozen truth judge and makes delayed visibility explicit. The fourth region shows the two release branches: a selected challenger changes the shared family residual and provider bias; a fallback leaves learnable state unchanged and records only diagnostic ledgers. A dashed loop returns the released state to the next visible decision.

## Information boundaries

- Before action: molecule, endpoint, provider menu, and released history are visible.
- At the call: only the selected provider returns an output and runtime status.
- The frozen endpoint judge supplies the selected label after termination; it is hidden until `τ_t = t + 1 + D`.
- An unselected provider creates no label.
- Fallback feedback is ledger-only for the learnable residual and bias.

## Visual contract

Gray panels are fixed/public or execution; blue panels are MIRA-Relay state; amber/purple panels are judged and delayed events. Solid arrows are the current event path; the dashed blue loop is the released-state control path. The diagram has no second-call, refusal, or escalation branch; those extensions are described in the paper text.
