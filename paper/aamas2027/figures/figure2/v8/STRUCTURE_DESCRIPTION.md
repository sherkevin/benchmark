# Figure 2 v8 structure description

## Intended reading

A single vertical timeline shows the selected-only lifecycle at one decision tick. It starts with `choose a_t`, then one provider call, terminal/runtime verification and independent frozen endpoint judging, a pending delay queue, release at `τ_t = t + 1 + D`, and the first subsequent action that can see the released relay state.

## Information boundary

The ghosted crossed-out box is an unselected output: it is not observed and cannot enter the queue. Runtime status and the chemical truth label are separated inside the terminal/judge block. The queue makes the unavailable interval explicit; no label or online state update occurs before release.

## Visual contract

The vertical timeline is designed for a single AAMAS column. Blue marks selector/state, gray marks the call, amber marks terminal judging, and purple marks delayed visibility. No second call, refusal, or escalation path is shown.
