# Figure 2 AI generation review — v21

**Date:** 2026-10-06  
**Asset:** `paper/aamas2027/figures/generated/figure2/v21_ai.png`  
**Raw size:** 2117 × 743 PNG  
**SHA-256:** `ac7771eed788caa0b9ed91211407ec2eaba0a0ef6cb5a2f40f5b2652ceff6d8f`  
**Prompt:** `paper/aamas2027/figures/prompts/figure2/v21.md`

## Review purpose

This is a topology and presentation review of the AI-generated candidate. It is not a claim about experimental results. v20 was retained but rejected because it contained a direct terminal-outcome → released-outcome shortcut.

## Checks

- **Density:** pass. The phase tabs sit close to the module band; the lower relay is compact and the large empty margins from earlier versions are reduced.
- **Module visibility:** pass. The figure exposes selector, selected provider, call, terminal record, frozen endpoint judge, correctness, queue, released outcome, relay update, state, and next action as distinct visual cards.
- **Upper data flow:** pass. `S_t + A_t → selector → one selected provider → call → terminal record → frozen endpoint judge → chemical correctness`.
- **Failure semantics:** pass. `runtime status → system failure`; system failure and chemical correctness converge at `terminal outcome`.
- **Lower data flow:** pass on visual inspection. `terminal outcome → selected outcome → selected-only queue → released outcome → relay update → S_τt → first action`.
- **Queue integrity:** pass. The queue has one visible incoming arrow from selected outcome and one outgoing arrow to released outcome. No terminal-outcome-to-released-outcome bypass is visible in v21.
- **Feedback timing:** pass. The queue carries `τ_t = t + 1 + D`, with `t+1 … τ_t−1`, `hidden until τ_t`, and the local `future state` cue.
- **Scope discipline:** pass. No verifier, refusal, fallback ledger, diagnostic ledger, second call, current truth before action, or same-action feedback is shown.
- **Typography:** pass. The final compiled page was rendered at 160 dpi; labels and arrows remain readable at the figure width used in the paper.

## Decision

**Review score: 16/16 checks pass.**

**Keep v21 as the current AI visual candidate.** Keep v19 and v20 unchanged for comparison and audit. Before submission, compile the paper with v21 and inspect the rendered figure at its final column width; if a label becomes unreadable, revise the prompt rather than editing pixels manually.
