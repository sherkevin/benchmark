# Figure 2 v1 — natural-language drawing specification

Draw a clean, wide, white-background academic timeline for a two-column AAMAS paper. Use three horizontal swim lanes with left-aligned labels: `SELECTOR`, `PROVIDER / VERIFIER`, and `UPDATER`. Use five vertical stages from left to right: `decision tick t`, `call + terminal`, `delay queue D`, `release at τ_t`, and `next action`.

In the selector lane, show a blue rounded box labelled `choose a_t from menu A_t` and a small clock marker `t`. In the provider/verifier lane, show a gray provider box labelled `call a_t`, followed by a split terminal box with two short lines: `runtime status` and `chemical label | valid return`; place a small amber tag `selected only`. Add a very light, crossed-out ghost box labelled `unselected output` above the terminal lane, connected with a dashed line and the annotation `not observed`.

In the updater lane, show an amber queue box labelled `pending event` and `wait D`, then a purple box labelled `release y_t at τ_t`, then a blue box labelled `update selected state`. Connect the stages with arrows. Solid arrows mean an event has happened; dashed arrows mean the event is waiting or control moves forward. Draw a thin dashed vertical boundary just before the release stage and label it `feedback unavailable before τ_t`.

At the far right, show a small blue box labelled `next action t+1`; connect `update selected state` to it with a solid arrow and a return arrow from `next action` back toward the selector lane. Keep the visual hierarchy flat: no shadows, no gradients, no decorative icons, at most four semantic colors. The labels must remain readable when the figure is reduced to a single-column width and distinguishable in grayscale through fills, borders, and line styles.
