# Figure 2 AI generation review — v22

**Asset:** `paper/aamas2027/figures/generated/figure2/v22_ai.png`  
**Prompt:** `paper/aamas2027/figures/prompts/figure2/v22.md`  
**SHA-256:** `d23b23d14fb06ca8509f484f383a4555ee1fe8b717f231ce25794fcb3692e31c`

## 16-point gate

| Dimension | Score | Review |
|---|---:|---|
| Semantic contract | 4/4 | The lower update reads provider residual + released counters, matching the current MIRA-Context specification. |
| Causal/data flow | 4/4 | Terminal outcome → selected outcome → selected-only queue → released outcome → relay update → released state; no queue bypass is visible. |
| Gallery style | 4/4 | Compact flat-vector layout, clear phase grouping, limited colors, short labels, and module/data-flow priority match the audited Top-Conf architecture examples. |
| Paper readability | 4/4 | The independent 7-inch full-width QA PDF renders the lifecycle without clipped labels, unreadable arrows, or collapsed modules. |

**Current score: 16/16.** No hard semantic error was found.

The paper-scale check is recorded in
`paper/aamas2027/figures/qa/build/ai_candidates_fullwidth_20261006.pdf`
(SHA-256 `aabf5af09e80d0c17a015ce3528216245235ecdfa6fde4a7752b83f6d0853308`),
page 2.

**Current decision:** keep v22 as the Figure 2 AI candidate; retain v07–v21 for audit and comparison.
