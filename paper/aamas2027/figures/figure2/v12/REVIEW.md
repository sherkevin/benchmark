# Figure 2 v12 semantic review

## Result

Accepted as the current terminal-flow figure for the internal paper draft.

## Checks

- Decision precedes provider output and terminal record.
- Terminal record precedes the frozen endpoint judge.
- Runtime failure is separate from chemical correctness and joins only at terminal outcome.
- Only the selected outcome enters the pending queue.
- The release time is explicit: `$\tau_t=t+1+D$`.
- Relay update and released state occur before the first action at or after the release tick.
- Unselected output is explicitly unavailable.
- No arrow gives the selector access to current truth or unselected outputs.

## Remaining layout check

The image is wide and should be placed as a full-width `figure*`. Final acceptance still requires the clean AAMAS build to confirm readable labels at 100% PDF size and no page-count regression.
