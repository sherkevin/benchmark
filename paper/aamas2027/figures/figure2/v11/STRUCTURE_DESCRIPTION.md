# Figure 2 v11 structure description

Compact editable redraw of the accepted v09 AI layout. The upper band is a single action spine: state and menu context, selector, one selected provider, call, terminal record, frozen endpoint judge, and valid-return chemical correctness. Runtime failure is a separate branch. Both terminal outcomes meet at `terminal outcome`.

The lower band is an explicit selected-only release chain: `terminal outcome → selected outcome → selected-only queue → released outcome → relay update → S_τt → first action at or after τ_t`. The feedback is hidden until `τ_t=t+1+D`; the long blue return arrow is a future-state read, not current-action feedback. Fallback bookkeeping is omitted because Figure 1 owns that path.
