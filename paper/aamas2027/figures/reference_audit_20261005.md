# Figure reference audit — 2026-10-05

## Scope

This audit records the local references consulted before writing the AI-generation prompts. It is a style audit, not a claim about the source papers' scientific content.

## Local references

### TopConf Figure Gallery

- Checkout: `/Users/jingwu/Documents/Codex/2026-09-27/new-chat/topconf-paper-figure-gallery`
- `data/figures.json` currently contains 3516 figure records.
- High-scoring patterns inspected in the local metadata: `architecture`, `framework`, `pipeline`, `results`, and `comparison`.
- The gallery is used for layout and visual grammar only. Its images are not copied into the paper.
- Relevant local records inspected for the feedback/agent framing were `iclr2024-17` (CRITIC),
  `acl2024-2024.acl-long.570` (LLMs in the Imaginarium), `aaai2025-32038` (GenAL), and
  `iclr2026-0082` (In-the-Flow Agentic System). They suggest a compact feedback loop and
  bounded module groups; they are not content templates for this paper.

### Editable figure library

- `framework_pipeline/DPO_fig6_1.pdf`: bounded left/right comparison, one visual claim per panel, short labels.
- `attention_mechanism/FlashAttention2_fig4_1.pdf`: nested boxes, directional arrows, explicit inner/outer loops, sparse palette.
- `model_architecture/Neural_ODE_fig7_1.pdf`: repeated aligned panels, restrained typography, captions that explain the visual takeaway.
- `comparison_benchmark/Rethinking_ImageNet_fig4_1.pdf`: small multiples with consistent geometry and a single comparison dimension.

## Reusable grammar for this paper

1. Use one dominant reading direction per figure.
2. Put the information boundary on the diagram itself; do not rely on the caption to explain hidden outputs or delayed release.
3. Use four semantic colors at most: navy for MIRA-Relay, grey for frozen/baseline components, amber for delayed feedback, muted purple for the independent judge/audit path.
4. Use line style as redundant encoding: solid for observed data, dashed for waiting or control flow, ghosted/crossed elements for unavailable observations.
5. Keep labels short and exact. Under ADR 0010, the reviewed AI generation is the formal visual asset; label or arrow errors are corrected by a new prompt/image version, not by manual redraw.
6. Do not add decorative icons, 3-D effects, gradients, fake numbers, or extra branches that are not in the paper contract.

## Mapping to our figure set

- Figure 1 follows the DPO-style bounded overview plus FlashAttention-style explicit loop: context → selector → one call → delayed release → next decision.
- Figure 2 follows the small-multiple/timeline discipline: one vertical lifecycle with explicit phase boundaries and no unselected feedback arrow.
- Figure 3 is reserved for verified results. It will use aligned panels and confidence intervals rather than decorative bar charts.

## What we deliberately do not copy

We do not use teaser/poster layouts, dashboard cards, or photorealistic molecular illustrations. The paper's claim is an information and feedback boundary, so the figure must make that boundary visible through panel structure and arrows instead of decoration.
