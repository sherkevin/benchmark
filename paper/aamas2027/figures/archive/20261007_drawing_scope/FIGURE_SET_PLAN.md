# Figure set plan (working version 1)

## Selection rule

We will add a figure only when it resolves a reviewer question that prose and tables cannot answer as quickly. The current paper needs two figures; a third experiment protocol figure is optional and will be added only if the final matrix remains hard to parse at two-column width.

## Core figures

### Figure 1 — Method overview (`figure*`)

**Reviewer question:** What is selected, what is observed, and where does the proposed online state enter the next decision?

**Visual sentence:** A molecule, provider menu, and released history enter a frozen direct selector; MIRA-Relay scores one selected provider, receives only its released outcome after a delay, and uses the updated relay state for the next choice while the fallback remains immutable.

**Structure:** left-to-right pipeline with bounded panels: input and released history → frozen selector/fallback → relay residual and provider bias → selected provider → runtime ledger + frozen endpoint judge → delay queue → released state → next decision. Frozen modules are gray, MIRA-Relay is blue, delayed feedback is amber, endpoint judging/runtime accounting is purple. The selected AI image must use arrows and labels for information boundaries; color is redundant with line style and text.

**Target:** full-width `figure*`, about 2.5–2.8 inches high, aspect ratio 2.5–3:1. No equations beyond short variable labels.

### Figure 2 — Delayed selected-only lifecycle (`figure`)

**Reviewer question:** At what moment can a result influence a later action, and what exactly is updated?

**Visual sentence:** At decision tick $t$, one provider is called; after termination the frozen judge and runtime status form one selected terminal outcome, which remains unavailable during the delay and is released at $\tau_t$ to update only the selected-provider state before the next action.

**Structure:** the user-provided v12 figure uses a horizontal event spine with five stage pills—decision, selected call, terminal, pending, release/next action—and a lower delayed-feedback ribbon. The upper spine is `state → selector → one selected provider → call → terminal record → frozen endpoint judge`; chemical correctness and runtime failure converge at a terminal-outcome chip before the selected outcome enters the queue. The lower ribbon shows `selected-only queue → release → relay update → first action at or after $\tau_t$`. Figure 1 carries the detailed input/state architecture; Figure 2 makes the terminal and release order explicit. Solid arrows indicate observed/completed events; dashed arrows indicate waiting/control flow.

**Target:** wide `figure*` or equivalent full-width placement, about 2.3–2.7 inches high, aspect ratio about 2.7–3.0. The AI image must remain legible at 100% PDF size and in grayscale; final placement is still a layout decision.

## Optional figure

### Figure 3 — Evaluation protocol (conditional)

Add only if the final paper's E1–E6 matrix cannot be understood from Table 2. It would show the common replay contract, fit-only warm start, selected-only feedback, delays, and held-out confirmation as an audit protocol. Do not add it merely to fill pages.

## Shared visual contract

- AI-source-first: each version keeps the prompt, the raw generated image, and a review record. A manual SVG/HTML/PDF may be kept for internal semantic inspection only and is not a formal paper asset.
- Palette: ours `#2E5DA1`, frozen/baseline `#B8C0CC`, terminal/judge `#D99000`, pending/delay `#6B4C9A`, background `#F7F9FC`; no red/green-only semantics.
- Typography: Arial/Helvetica fallback, 9–10 pt equivalent at final size; no text walls.
- Every final AI figure gets a self-contained caption, a `\Description{}` string, a version manifest, a raw image, and a review record.
- Versions are never overwritten. A later revision must live in `v2`, `v3`, etc.; the selected version is recorded explicitly.


## Current selected source versions

- Figure 1: `figure1/v07/figure1_v07.png` — user-provided architecture; four regions explicitly separate input/candidates, relay state, execution/evaluation, and delayed feedback.
- Figure 2: `figure2/v12/figure2_v12.png` — user-provided terminal flow; judge-gated terminal outcome and selected-only release chain are explicit.
- Figure 2 historical formal placeholder: `figure2/v9/figure2_v9.pdf` — retained for comparison only; do not treat the manual redraw as the selected visual asset.
- Historical AI visual prompts remain under `prompts/figure1/` and `prompts/figure2/`; the current paper assets are user-provided PNGs with manifests under `figure1/v07/` and `figure2/v12/`. Later changes must create a new version and preserve the source image.
