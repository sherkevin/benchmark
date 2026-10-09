# Figure 2 v3 — natural-language specification

A compact single-column timeline with three horizontal lanes and five short stage headings. The figure keeps only the labels needed to answer the information-boundary question: `DECIDE`, `CALL`, `QUEUE`, `RELEASE`, `NEXT`; the detailed semantics appear inside the boxes. The selector lane shows `choose a_t` from `A_t`; the provider/verifier lane shows one call and the runtime return and independent frozen endpoint judgment; a crossed-out unselected output is marked `not observed`; the updater lane shows `queue / wait D`, `release y_t / at tau_t`, and `update / next action`. A dashed vertical line before release is labelled `no feedback before tau_t`. Solid arrows mean completed events and dashed arrows mean waiting/control flow. Use blue for selector/state, gray for provider/frozen, amber for queue, and purple for verifier/release. No red/green semantics, shadows, gradients, or equations. The geometry is deliberately sparse so 42px headline labels map to approximately 7pt when the 1500-unit canvas is placed at single-column width.


## v4 revision

The stage geometry is unchanged from v3. Lane labels are reduced slightly to avoid colliding with the first box, and the no-feedback boundary is drawn only through the waiting/update region so it no longer cuts through the terminal-event panel.


## v5 revision

The selector lane label is shortened to `SELECT` so it remains fully visible at actual single-column size; all earlier versions remain untouched.


## v7 revision

The crossed-out unselected output no longer has a downward arrow. It remains a non-observation marker; only the selected terminal event flows into the selected-only queue. This removes a possible reading that an unselected output contributes to the terminal event.
