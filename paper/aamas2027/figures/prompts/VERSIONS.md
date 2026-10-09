# AI figure prompt versions

**编号说明：**历史 prompt 版本号与生成图片资产的 `vNN_ai.png` 编号不是同一条序列（例如图片资产 v06 使用 prompt v13；图片资产 v13 使用 prompt v21）。画廊 `manifest.json` 是图像资产与 prompt 的唯一映射，避免把两种编号混为一谈。

2026-10-07 生效分工见决议 0012：只有架构图、流程图和概念图采用 AI。
Figure 3 是数据图，本表中该图的全部 prompt 均为历史探索；当前版本转到
`../figure3/v02_data/` 程序图。

| Figure | Version | Status | Purpose |
|---|---|---|---|
| Figure 1 | v01 | preserved | Initial four-panel MIRA-Relay overview prompt. |
| Figure 1 | v02 | preserved | Tightened release semantics and prohibited extra calls/feedback leakage. |
| Figure 1 | v03 | preserved | Added explicit terminal judge, delayed queue, and immutable-reference wording. |
| Figure 1 | v04 | preserved | Moves fallback ledger bookkeeping out of the selector, makes selected-only release explicit, and shows decisions continuing while feedback is pending. |
| Figure 1 | v05 | preserved | Rebuilds the prompt as a compact modular architecture figure with candidate cards, a visible family-sharing inset, and short labels. |
| Figure 1 | v06 | preserved | Removes the duplicate pre-selected provider and separates fallback diagnostic bookkeeping from runtime failure. |
| Figure 1 | v07 | preserved | First true AI overview generation; compact four-region composition, but the selected-provider card still visually contained A/B/C candidates. |
| Figure 1 | v08 | preserved | Removes candidate duplication inside the selected-provider card; exactly one selected provider is emitted. |
| Figure 1 | v09 | preserved | Reasserts input isolation, but the generated image still repeated an input-to-feedback arrow. |
| Figure 1 | v10 | preserved | Removes the input-to-feedback arrow; the generated image left terminal outcome disconnected from selected outcome. |
| Figure 1 | v11 | preserved | Single vertical terminal handoff; later visual audit found an ambiguity. |
| Figure 1 | v12 | preserved; prompt reviewed | Gallery-derived internal computation and selected-only counter loop. |
| Figure 1 | v13 | preserved; AI v06 rejected | Adds identity/componentwise residuals; image had wiring and identity errors. |
| Figure 1 | v14 | preserved; correction draft | Specifies query/menu inputs, B identity and update destination. |
| Figure 1 | v15 | preserved; AI v07 rejected | Local feedback grouping; generator retained input and update branches. |
| Figure 1 | v16 | preserved; AI v08 rejected | Complete graph and adjacent menu; Judge had no input and feature notation was wrong. |
| Figure 1 | v17 | current; AI v09 | Repairs Return→Judge→feedback and φ(x), keeps input menu neutral; independent graph review passed. |
| Figure 1 | v18 | generated as AI v10; review pending | Removes duplicate labels and enlarges secondary paper-scale labels while preserving v09 graph. |
| Figure 1 | v19 | generated as AI v11; visual audit pending | Attempts a direct Update→B-row edge; retained for comparison with the border-edge risk. |
| Figure 1 | v20 | generated as AI v12; rejected after review | Moves Update left and docks a horizontal edge to B, but retained a Release→counters bypass. |
| Figure 1 | v21 | generated as AI v13; independently reviewed 13/16, no hard error | Makes Release→Update→B-row a single chain and forbids every Release→counters bypass; Release→Update remained a long purple bend. |
| Figure 1 | v22 | generated as AI v14; independently reviewed 14/16, no hard error | Places Release and Update on one baseline with short horizontal Release→Update; Update→B still bends around the left. |
| Figure 1 | v23 | generated as AI v15; rejected on visual precheck | Tried to dock Update to B directly but moved feedback into Update and disturbed the event graph. |
| Figure 1 | v32 | generated as AI v22; visual redesign retained | Removes the large panel treatment and uses a compact central-hub architecture; preserved as an intermediate because it remained close to v21. |
| Figure 1 | v33 | generated as AI v23; visual candidate | Removes giant panel headings and floating symbols; central hub and bottom delayed loop are compact and visually aligned with Figure 2. |
| Figure 1 | v34 | generated as AI v24; semantic cleanup candidate | Keeps v23 composition, replaces bracket headings with quiet captions, and makes queue→Release a single leftward dashed edge. |
| Figure 1 | v35 | generated as AI v25; independent review pending | Moves the eligible-tools tray to Selector and removes its misleading connection to released state; current review candidate. |
| Figure 1 | v36 | generated as AI v26; independent review pending | Forces the Update write arrowhead into the highlighted released-state row; preserves the v25 composition. |
| Figure 1 | v37 | generated as AI v27; independent review pending | Simplifies released state to two rows and aligns Update with the single highlighted row to eliminate the border-edge ambiguity. |
| Figure 1 | v38 | generated as AI v28; independent review pending | Changes the lower write path to a horizontal queue→Release→Update→state chain. |
| Figure 1 | v39 | generated as AI v29; independent review pending | Adds an unlabeled interior write port so the horizontal Update edge visibly terminates inside the highlighted row. |
| Figure 1 | v40 | generated as AI v30; visual redesign retained | Replaces the two long serial pipelines with a dominant central selector hub and a local delayed write-back loop; the generated selector label was malformed, so the image is retained as an intermediate. |
| Figure 1 | v41 | generated as AI v31; independent review pending | Performs a targeted text cleanup of v30 while preserving its central architecture hub, shared style with Figure 2, and distinct non-temporal role. |
| Figure 1 | v42 | generated as AI v32; precheck rejected | Attempts to remove the state-to-Selector shortcut and connect eligible tools; the generator still preserves a semantically wrong shortcut, so the raw output is retained. |
| Figure 1 | v43 | generated as AI v33; independent review pending | Full architecture rebuild: central score/state computation is dominant, Figure 2 is represented only as a feedback lifecycle interface, and the valid graph is explicitly constrained. |
| Figure 1 | v44 | generated as AI v34; independently reviewed 16/16, no hard error | Micro-refinement of v43: adds a small molecule glyph inside Query and changes the equation annotation line to have no arrowhead; all modules, edges, labels, and Figure 2 boundary are preserved. |
| Figure 2 | v01 | preserved | Initial selected-only lifecycle prompt. |
| Figure 2 | v02 | preserved | Explicit action/release phase boundary and post-release state semantics. |
| Figure 2 | v03 | preserved | Added explicit release clock, selected-only queue, and no same-action feedback wording. |
| Figure 2 | v04 | preserved | Separates intermediate decisions from the release tick and removes the incorrect generic `next decision t+1` label for positive delay. |
| Figure 2 | v05 | preserved | Rebuilds the prompt as a three-lane, five-column selected-only lifecycle with short labels and visual state rails. |
| Figure 2 | v06 | preserved | Replaces the ambiguous fallback/failure label and keeps chemical correctness, runtime failure, and fallback bookkeeping separate. |
| Figure 2 | v07 | preserved | Replaces the table-like swimlane with a free-form horizontal event spine and delayed feedback ribbon; fixes the generated draft's pre-terminal queue placement and removes full-width grid lines. |
| Figure 2 | v08 | preserved | Keeps the no-grid event spine, gates selected outcome on the post-termination judge, merges valid and failed terminal outcomes explicitly, and removes fallback bookkeeping from Figure 2. |
| Figure 2 | v09 | preserved | Compresses the layout into two close bands and explicitly enforces terminal outcome → selected outcome → queue → released outcome with no shortcut or whitespace-heavy separation. |
| Figure 2 | v10 | preserved | Adds compact phase tabs and module glyphs, attaches the unavailable-output marker to the input, and removes most vertical whitespace. |
| Figure 2 | v11 | preserved | Removes the long global return loop and replaces it with a local future-state glyph. |
| Figure 2 | v12 | preserved | Forces terminal outcome → selected outcome → queue as the only lower-chain handoff. |
| Figure 2 | v13 | preserved | Uses an explicit centered feedback-junction topology and restores the complete terminal outcome → selected outcome → queue chain. |
| Figure 2 | v14 | preserved | Tightens the terminal/selected handoff and aligns the central outcome cards. |
| Figure 2 | v15 | preserved | Removes the ambiguous terminal-record shortcut so terminal outcome receives chemical correctness and system-failure branches. |
| Figure 2 | v16 | preserved | Aligns terminal outcome and selected outcome on one vertical centerline. |
| Figure 2 | v17 | preserved | Corrects the runtime-failure arrow direction. |
| Figure 2 | v18 | preserved | Moves system failure beside terminal outcome and uses a horizontal merge arrow. |
| Figure 2 | v19 | preserved | Preserves the correct causal handoff while shortening the central elbow; terminal outcome is the sole source of selected outcome, which alone enters the delayed queue. |
| Figure 2 | v20 | preserved | Tightens the cropped layout and reduces whitespace; rejected after the generator reintroduced a terminal-outcome-to-released-outcome shortcut. |
| Figure 2 | v21 | preserved | Retains v20 density while enforcing terminal outcome → selected outcome → queue → released outcome with no bypass arrow. Its old residual label is not current. |
| Figure 2 | v22 | current generated asset | Keeps v21 topology and current provider residual + released counters label. |
| Figure 2 | v23 | next full prompt; not generated | Gallery-derived compact event cards; removes misleading giant stage ribbons. |
| Figure 2 | v24 | generated as AI v23; rejected | Single full prompt; waiting path still had bidirectional arrows and reversed time order; Menu B remained highlighted. |
| Figure 2 | v25 | generated as AI v24; candidate 13/16 | One leftward waiting edge and one update register; Menu B remained highlighted. |
| Figure 2 | v26 | generated as AI v25; candidate 14/16 | Neutralizes Menu A/B/C before Select; preserves the reviewed event graph. |
| Figure 2 | v27 | generated as AI v26; role review 15/16 | Redraws Figure 2 as a temporal event strip only; removes architecture internals and provider letters, with selected-only pending and delayed release. |
| Figure 2 | v28 | generated as AI v27; independent review pending | Micro-refinement of v27: enlarges only secondary paper-scale timeline labels and legend text; preserves the single-event graph and Figure 1 role boundary. |
| Figure 3 | v01 | preserved | Initial results layout; retained because it exposed comparison-role ambiguity. |
| Figure 3 | v02 | preserved | Separates the primary ProbeNoUpdate contrast from the secondary Frozen reference and defines rescued/harmed and cost effects. |
| Figure 3 | v03 | preserved | Empty results layout; forbids invented marks and leaves effect signs/data to verified receipts. |
| Figure 3 | v04 | preserved | AI-generated data-free three-panel results layout; all marks remain reserved for verified receipt data. |
| Figure 3 | v05 | preserved | Rewrites the empty results shell to match the current natural-vs-controlled, rescued/harmed, and update-cost receipt contract. |
| Figure 3 | v06 | preserved | Removes the ambiguous cost-panel “Other” legend and names the matched-baseline role explicitly. |
| Figure 3 | v07 | preserved; used for v04 | Adds the receipt-bound marks to the AI v03 layout; v04 is retained as the pre-consistency candidate. |
| Figure 3 | v08 | historical; AI data plots superseded | Replaces the panel (b) MIRA shorthand with MIRA-Relay; retained only as history. |

## Generation policy

- Prompts can be copied to an external image tool or passed to the built-in tool. Raw images are under `../generated/`; reference images and attribution are under `../style_references/`. No API keys are stored here.
- Every revision is kept in its own file; a later prompt must not overwrite an earlier version.
- Under decision 0012, AI structural diagrams keep their raw images, prompts, and reviews; errors are fixed through new prompt/image versions. Data plots and data tables are generated with code or native LaTeX and can be styled in code. No further AI Figure 3 data versions are planned.
- The shared visual contract is navy ours, grey frozen/baseline, amber terminal/judge, muted purple pending/delay, white background, flat vector, no gradients, no 3-D, no decorative icons.
