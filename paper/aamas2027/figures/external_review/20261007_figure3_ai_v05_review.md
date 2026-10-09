# Figure 3 AI generation review — v05

**历史视觉自检，非数值精确证明。** 2026-10-07 根据用户澄清和决议 0012，
AI 数据图退出候选。当前数据图见 `figure3/v02_data/`；下文原评分保留供追溯。

**Asset:** `paper/aamas2027/figures/generated/figure3/v05_ai.png`  
**Prompt:** `paper/aamas2027/figures/prompts/figure3/v08.md`  
**Image SHA-256:** `f019e48a7cdd1d06687bfa30350052f5334362b2ee2a957667396f4ecaa2c1a3`  
**Canvas:** 2087×753 RGB PNG  
**Receipt manifest:** `generated/figure3/data_binding_v1.json`  
**Manifest SHA-256:** `ac467b303775e83b737ba9e807e806552dc4414651ce8d476aa081e1817c20fc`

## Version change

v05 is a complete AI edit of v04. The only intended semantic change was to
spell the fifth method in panel (b) as `MIRA-Relay`, matching Figures 1–2 and
panel (c). The rescued/harmed bars, top labels, feedback curves, scatter
coordinates, axes, legends, and panel geometry were visually checked and
remain unchanged. v04 is retained as the prior candidate.

## 16-point gate

| Dimension | Score | Review |
|---|---:|---|
| Semantic contract | 4/4 | The panel roles and all method names match the current receipt contract; `MIRA-Relay` is used consistently. |
| Causal/data flow | 4/4 | No unsupported causal arrow, ranking, frontier, confidence interval, or service-latency claim was added. The panels remain descriptive result views. |
| Gallery style | 4/4 | Compact three-panel composition, short labels, restrained navy/gray/amber palette, light grid, rounded tabs, and white background match the audited Top-Conf results style. |
| Paper readability | 4/4 | The independent 7-inch full-width PDF keeps the full `MIRA-Relay` label, bar numbers, axes, legends, and point labels readable. |

**v05 score: 16/16.** No hard semantic, data-role, naming, or visual-integrity
error was found.

## QA evidence

The full-width QA source is
`paper/aamas2027/figures/qa/ai_candidates_fullwidth_20261007_v05.tex`.
The resulting PDF is
`paper/aamas2027/figures/qa/build_v05/ai_candidates_fullwidth_20261007_v05.pdf`
with SHA-256
`b24bc653165afee01912781e50e4aa37447d5a6fcb4f290cb48b195b142b823e`.
Page 3 was rendered at 180 dpi and inspected. v05 is the current Figure 3
candidate; formal-paper substitution remains deferred until the joint
three-figure lock review.
