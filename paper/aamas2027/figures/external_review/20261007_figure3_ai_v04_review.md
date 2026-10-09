# Figure 3 AI generation review — v04

**历史视觉自检，非数值精确证明。** 2026-10-07 根据用户澄清和决议 0012，
AI 数据图退出候选。当前数据图见 `figure3/v02_data/`；下文原评分保留供追溯。

**Asset:** `paper/aamas2027/figures/generated/figure3/v04_ai.png`  
**Prompt:** `paper/aamas2027/figures/prompts/figure3/v07.md`  
**Image SHA-256:** `316cf46c019b216bd70f957e0a7bed34bdc7d27f3e7e3e7221c77d7cee980eac`  
**Canvas:** 2087×753 RGB PNG  
**Receipt manifest:** `generated/figure3/data_binding_v1.json`  
**Manifest SHA-256:** `ac467b303775e83b737ba9e807e806552dc4414651ce8d476aa081e1817c20fc`

## Data and semantic check

The image was generated from the v03 layout with a complete prompt that
contains only the values in the receipt manifest. The visible bar labels in
panel (b), the two feedback series and their axes in panel (a), and the five
method points and axes in panel (c) were checked against the manifest. No
ProbeNoUpdate, `Other`, confidence interval, significance star, ranking arrow,
or invented service-latency claim appears. Panel (b) reports paired action
counts, not accuracy; panel (c) reports policy-update overhead and serialized
state.

## 16-point gate

| Dimension | Score | Review |
|---|---:|---|
| Semantic contract | 4/4 | The three panels use the current result roles: natural versus controlled feedback effect, rescued versus harmed action counts, and update-cost/state scatter. Method names and comparison roles match the receipt manifest. |
| Causal/data flow | 4/4 | The figure does not add a causal arrow, ranking, frontier, or unobserved comparison. Axes and legends distinguish natural fresh, controlled label flip, rescued/harmed, and matched baselines. |
| Gallery style | 4/4 | Compact three-panel composition, short labels, navy/gray/amber semantics, restrained light grid, rounded header tabs, and white background match the audited Top-Conf results style. |
| Paper readability | 4/4 | The 7-inch full-width PDF keeps panel titles, axes, legends, method names, and the numeric bar labels readable. |

**v04 score: 16/16.** No hard semantic, data-role, or visual-integrity error
was found in the candidate or in the independent full-width QA render.

## QA evidence

The full-width QA source is
`paper/aamas2027/figures/qa/ai_candidates_fullwidth_20261007_v04.tex`.
The resulting PDF is
`paper/aamas2027/figures/qa/build_v04/ai_candidates_fullwidth_20261007_v04.pdf`
with SHA-256
`9e7ed89b5df6524fd07565f11cfcc2d33f12c9a9914dbd6f41e0564614c94b1f`.
Page 3 was rendered and inspected at 180 dpi. The previous v03 layout remains
preserved; v04 is the current data-bound candidate, not yet a formal-paper
replacement.
