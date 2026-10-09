# AI 图最终候选审计（2026-10-08）

本审计只覆盖论文中的 AI 架构图和流程图，不覆盖 Figure 3 数据图，也不宣称已满足 AAMAS 官方 AI 图投稿政策。

## 结论

当前推荐配对为：

- Figure 1：`generated/figure1/v34_ai.png`
- Figure 2：`generated/figure2/v27_ai.png`

两张图均完成独立 16/16 审查，均无硬错误，并通过同一份 7-inch paired QA。主版 `figures/qa/main.pdf` 尚未替换，避免在用户最终选择前静默同步。

## 逐项核验

| 要求 | 当前证据 | 结果 |
|---|---|---|
| 充分参考样例库 | `gallery_style_study_20261007.md` 记录实际查看的 TableDART、CRITIC、CollabLLM、GenAL 图；v34/v27 审查报告再次对照本地样例 | 通过 |
| 使用 AI 生成 | `v34_manifest.json`、`v27_manifest.json` 的 `manual_pixel_editing=false`，均记录 image-gen 源路径和参考图 hash | 通过 |
| Figure 1 讲架构 | v34 exact graph：Query→Frozen scorer→q⁰；Released state→Residual；二者→plus→Scores→Select；Eligible tools→Select；feedback→Update→高亮行 | 通过 |
| Figure 2 讲流程 | v27 exact graph：decision→selected action→call→terminal→judge→chemical feedback→pending→release→released state→first later decision | 通过 |
| 两图不重复 | F2 无 scorer、Residual、counter register、Scores、Eligible tools、q=q⁰+Δ；F1 不展开 call/judge/pending/release 时间链 | 通过 |
| 失败路径正确 | 两份审查均核验 runtime error 只进入 runtime ledger，不进入 chemical feedback 或 pending | 通过 |
| 风格一致 | 配对 QA 与审查核验共同白底、深蓝/灰/琥珀/紫色、统一线宽与图标族，同时保持不同布局 | 通过 |
| 纸面可读性 | `qa/build_v24/ai_structures_7inch_20261008_v34_v27.pdf` 两次 pdflatex 无 fatal、overfull、undefined-reference、missing-font warning；两页实际查看 | 通过 |
| 历史留痕 | gallery 58 条记录；F1 37 条、F2 21 条；原 PNG、prompt、manifest、generation request、审查报告和 before 快照保留；图片 hash 校验 missing=0、bad=0 | 通过 |

## 独立审查证据

- Figure 1：`external_review/20261008_figure1_v44_independent_review.md`，16/16。
- Figure 2：`external_review/20261008_figure2_v28_independent_review.md`，16/16。
- 配对 QA：`qa/build_v24/ai_structures_7inch_20261008_v34_v27.pdf`，SHA-256 `e668d5aa47c49fa65f1aacb940de6dedc2cfde66e43f5b87028def5e5f39b8e9`。
- Figure 1 QA 页：`25598f4e2470dbd9a4b4ff517d9a98914c2caa7b89f0e220ee18e42669c0ad15`。
- Figure 2 QA 页：`ab03df4ac747ba1b9568bbe2c13b279319976f145e4911654fe75c4f8044ddb8`。

## 仍未完成的动作

“生成并验证高质量 AI 架构图/流程图”的目标证据已齐全；用户最终审美选择和 `figures/qa/main.pdf` 的同步是后续发布动作，不在本审计中自动执行。任何进一步修改都必须从 v34/v27 新建版本，不能覆盖当前候选或历史文件。
