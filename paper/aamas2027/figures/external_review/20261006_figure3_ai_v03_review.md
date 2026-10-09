# Figure 3 AI generation review — v03

**Asset:** `paper/aamas2027/figures/generated/figure3/v03_ai.png`  
**Prompt:** `paper/aamas2027/figures/prompts/figure3/v06.md`  
**SHA-256:** `9086f83e626bc8fbc100c697fc6816470778fe2ae68c1b8768c2a6061296ad8a`  
**Canvas:** 2087×753 PNG

## 16-point layout gate

| Dimension | Score | Review |
|---|---:|---|
| Semantic contract | 4/4 | The panels match the current paper result roles: natural versus controlled feedback effect, rescued versus harmed actions, and update cost. The stale ProbeNoUpdate comparator is removed; the cost legend names Frozen, MIRA-Relay, and matched baselines. |
| Causal/data flow | 4/4 | Each panel is an empty results shell with no implied data path, ranking, or dominance claim. No generated mark can be mistaken for an empirical observation. |
| Gallery style | 4/4 | The compact three-panel layout, short labels, restrained palette, rounded headers, and light grid structure match the audited Top-Conf results figures. |
| Paper readability | 4/4 | The independent 7-inch full-width QA PDF keeps all three panels, legends, axes, and method labels readable on page 3. Exact marks must be checked again after data binding. |

**Current layout score: 16/16.** No hard semantic or data-integrity error was found.

The layout QA is recorded in
`paper/aamas2027/figures/qa/build_v03/ai_candidates_fullwidth_20261006_v03.pdf`
(SHA-256 `29a764c12cd49da4f20cfca196bd95d6291ae964a1477c962b656f6131c5e281`),
page 3.

## Version decision

Keep v03 as the current Figure 3 AI layout candidate. Preserve v01 (stale
ProbeNoUpdate comparator) and v02 (ambiguous “Other” cost legend). The next
step is to bind exact points, bars, and labels from the receipt manifest
`generated/figure3/data_binding_v1.json` (with no unverified intervals);
the AI image itself contains no empirical values and must not be presented as a
result figure until that layer is independently checked.
