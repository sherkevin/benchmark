# Figure 1 AI assets

**编号说明：**图片资产版本与 prompt 修订版本独立编号；以 `version_gallery_20261007/manifest.json` 的映射为准。

Current reading-draft image: **AI v09**, generated from complete prompt v17.
The user's original is `../../figure1/v07/figure1_v07.png`, byte-identical to
`Downloads/MIRA-Relay延迟反馈架构图 (1).png`. The separate program-rendered
`../../figure1/v08/figure1_v08.png` is the submission-build source under the
AAMAS image policy; it is not the user's AI image. Both remain preserved.

| Raw image | Prompt | Status |
|---|---|---|
| v01_ai.png | v07 | preserved; selected-provider ambiguity |
| v02_ai.png | v08 | preserved; input-to-feedback arrow |
| v03_ai.png | v09 | preserved; input-to-feedback arrow remained |
| v04_ai.png | v10 | preserved; disconnected terminal handoff |
| v05_ai.png | v11 | preserved; prior visual pass superseded by direct handoff audit |
| v06_ai.png | v13 | rejected; B/A mismatch, update bypass, query/menu inputs mixed |
| v07_ai.png | v15 | rejected; bypass and mixed inputs remained |
| v08_ai.png | v16 | rejected; Judge without incoming output, wrong feature notation |
| v09_ai.png | v17 | independent image-level pass; reading draft selected |
| v10_ai.png | v18 | generated readability revision; failed independent review (11/16): Release/update path ended at counter border |

AI v09: 2060×763 RGB PNG, SHA-256
`c793d1d37c98133513d662317a7c9770fd5788d3816d4993be335d819c2ae97c`.

New versions retain generation requests and raw-image manifests. Tool outputs
are copied without pixel editing. Full prompts v12–v17 remain separate files,
including prompt-only revisions. Independent failures and current paper-scale
review are saved under `../../external_review/`.

See `../../gallery_style_study_20261007.md` for actual reference-image analysis,
graphic grammar, causal requirements and reading/submission output roles.
Architecture diagrams use AI; data plots remain code-based. The prior README
is retained at `../../archive/20261007_gallery_style/figure1_README.before.md`.

| v11_ai.png | v19 | preserved; separate v19 review not saved; visual audit found the same Update-edge risk |
| v12_ai.png | v20 | preserved; failed independent review (10/16): Release bypassed Update |
| v13_ai.png | v21 | independent review 13/16, no hard error; still uses a long purple Release→Update bend |
| v14_ai.png | v22 | independently reviewed 14/16, no hard error; Update→B still uses an L-shaped route |
| v15_ai.png | v23 | rejected on visual precheck; Update route and queue wiring drifted |
| aux_20261007_unchanged_v12_ai.png | inline copy request | preserved auxiliary output; not a numbered candidate |

AI v10 is retained separately (2060×763 RGB, SHA-256
`d579f1f1e3bd0021ae8058447fe0e96fc453ca9afb3390f94e91e9a934e37f67`) with prompt
v18 and its generation request. It is not connected to the paper until the
independent review and user choice are complete.


AI v11–v13 and the auxiliary output are kept as separate raw files with their manifests and generation requests. v13 is not connected to the paper or master PDF until independent review and user choice.

| v16_ai.png | v24 | review pending | Current-method architecture redrawn in the compact v07 visual language; no standalone A/B/C labels; one queue→release→update→highlighted-row path. |
| v17_ai.png | v25 | review rejected in precheck | Update enters the highlighted row, but the eligible-tools arrow was misrouted to released state. |
| v18_ai.png | v27 | review pending | Integrates the eligible-tools tray beside Selector and keeps the sole Update→highlighted-row write path. |
| aux_20261007_exec-6db8_ai.png | v26 | rejected auxiliary | Preserved intermediate output; eligible-tools edge still entered released state. |
| aux_20261007_exec-34b1_ai.png | v28 | rejected auxiliary | Same-height attempt retained; Update visually passed through residual. |
| v20_ai.png | v30 | independent review pass 15/16 | Direct same-height Update→highlighted-row path; residual separate; retained prior candidate. |
| v19_ai.png | v29 | precheck rejected | Same-height chain still routed Update through residual; kept for comparison. |
| v21_ai.png | v31 | independent review pass 15/16; recommended | Same v20 topology with explicit purple dashed waiting and blue solid update edges. |
| v22_ai.png | v32 | visual redesign retained | Compact central-hub redraw matching Figure 2 card grammar; retained as an intermediate. |
| v23_ai.png | v33 | visual candidate; review pending | Removes large panels and floating symbols; central-hub architecture with one delayed return loop. |
| v24_ai.png | v34 | semantic cleanup candidate; review pending | Quiet group captions and one leftward queue→Release waiting edge. |
| v25_ai.png | v35 | independent review 10/16; rejected | Eligible-tools tray feeds Selector only, but Update stopped at the released-state card border; review retained in `../../external_review/20261007_figure1_role_v35_independent_review.md`. |
| v26_ai.png | v36 | independent review pending; current candidate | Update arrow is explicitly drawn into the highlighted released-state row; v25 review retained as a failure record. |
| v27_ai.png | v37 | independent review pending; current candidate | Two-row released-state register and centered Update write; intended to remove the v25/v26 border-edge ambiguity. |
| v28_ai.png | v38 | independent review pending; current candidate | Horizontal queue→Release→Update→state chain; Update enters state from the right. |
| v29_ai.png | v39 | independent review 15/16; no hard error; 7-inch paper check passed | Horizontal Update edge ends on an unlabeled interior write port in the highlighted row; secondary labels remain small at paper scale. |


| v30_ai.png | v40 | 中间版；中心选择器架构重绘，但 selector 文本出现渲染错误 |
| v31_ai.png | v41 | 当前候选；修正 selector 文本并保留中心 hub 与局部回写环；独立审查待完成 |
| v32_ai.png | v42 | 预检拒绝；局部修补仍保留 state→Selector 旁路；原图保留 |
| v33_ai.png | v43 | 全量架构重绘；中心计算结构与 Figure 2 接口分离；独立审查待完成 |

| v34_ai.png | v44 | AI micro-refinement; independent review 16/16, no hard error | Preserves v33 architecture and adds a small molecule glyph in Query; removes the formula annotation arrowhead. v34 is the recommended current candidate; user selection and master sync remain pending. |
