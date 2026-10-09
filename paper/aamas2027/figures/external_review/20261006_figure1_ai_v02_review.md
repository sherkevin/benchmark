# Figure 1 AI generation review — v02

**Asset:** `paper/aamas2027/figures/generated/figure1/v02_ai.png`  
**Prompt:** `paper/aamas2027/figures/prompts/figure1/v08.md`  
**SHA-256:** `6ac7e5d247d076b14ae590fb9db4fba15d85989d495d344934f93adb12fd01cb`

## Findings

- **Semantic contract: pass.** Candidate A/B/C appear only in the eligible menu; the selected-provider module is one card with one provider icon. Current labels use provider residual + released counters.
- **Data flow: pass pending PDF check.** Input/menu → selector → one selected provider → one call → terminal record → judge/correctness; runtime failure and correctness converge at terminal outcome; only selected outcome enters the delayed queue.
- **Gallery style: pass.** The figure is compact, module-led, uses a restrained palette, and avoids the text-heavy manual v08 layout.
- **Paper readability: pending.** Must be checked after the figure is placed at the final full-width size.

**Current decision:** keep v02 as the Figure 1 AI candidate; retain v01 and manual v08 for audit.
