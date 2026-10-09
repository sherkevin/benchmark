# Figure 1 AI generation review — v05

**Asset:** `paper/aamas2027/figures/generated/figure1/v05_ai.png`  
**Prompt:** `paper/aamas2027/figures/prompts/figure1/v11.md`  
**SHA-256:** `5fd6838daa4b166fcaf5a99a166ba2166c2e2cfa59bcae3ea124f5c5e57c2355`  
**Canvas:** 2079×756 PNG

## 16-point gate

| Dimension | Score | Review |
|---|---:|---|
| Semantic contract | 4/4 | Inputs/menu, fit-only reference, provider residual + released counters, selected provider, terminal judge, terminal outcome, selected-only queue, relay update, and released state match the current MIRA-Relay contract. No verifier, refusal, fallback ledger, or invented metric is present. |
| Causal/data flow | 4/4 | The only feedback handoff is `terminal outcome → selected outcome → selected-only queue → released outcome → relay update → released state`. The input group has no outgoing feedback arrow; system failure and chemical correctness merge before the handoff; no queue bypass is visible. |
| Gallery style | 4/4 | Module-led flat vector, short labels, restrained navy/gray/amber/purple palette, consistent strokes, and grouped data flow match the audited Top-Conf architecture examples. |
| Paper readability | 4/4 | The independent 7-inch full-width QA PDF renders the complete overview without clipped labels, unreadable arrows, or collapsed modules. |

**Current score: 16/16.** No hard semantic error was found.

The paper-scale check is recorded in
`paper/aamas2027/figures/qa/build/ai_candidates_fullwidth_20261006.pdf`
(SHA-256 `aabf5af09e80d0c17a015ce3528216245235ecdfa6fde4a7752b83f6d0853308`),
page 1.

## Version decision

Keep v05 as the current Figure 1 AI candidate. Preserve v01–v04: v01 repeats
candidate providers inside the selected-provider card; v02 and v03 retain an
input-to-feedback arrow; v04 removes that arrow but disconnects terminal outcome
from selected outcome. Do not hand-edit the PNG. The formal paper path remains
unchanged until Figure 3 has its receipt-bound data layer.
