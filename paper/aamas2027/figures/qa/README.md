# Figure paper-scale QA

Current drawing scope: two AI structure diagrams and one program-rendered
 data plot, per decision 0012 (2026-10-07).

Figure 1 has now been restored in the default reading draft as AI v09. The full
paper is `../../build/reading_ai_20261007_v09/main.pdf`; the raw AI image and prompt
are preserved. Before/after source snapshots, original-file equality evidence,
and paper-scale checks are under `restore_20261007_v1/`. The prior AI v05 pass is
historical; later direct audit found a terminal-handoff ambiguity.

- Figure 1 AI v05 and Figure 2 AI v22: earlier 7-inch full-width PDFs below
  remain readable-scale evidence; the images are retained.
- Figure 3: current program data version is
  `../figure3/v02_data/figure3_v02.pdf`, natively 7×2.8 inches with embedded
  TrueType fonts. Its PDF render is `../figure3/v02_data/pdf_preview.png`.
- All earlier QA PDFs containing AI Figure 3 are historical only. Their visual
  checks do not certify precise scientific coordinates.

Reading and submission figure sources are explicit in `main.tex`: the default
uses AI Figure 1 v09; `MIRASubmissionFigures` selects program v08. This QA does not
change experiments, methods, numerical results or the AAMAS image policy.

## 主版与独立迭代

按决议 0014，`main.tex` 与 `main.pdf` 位于本目录，是当前通过审查候选的主版入口；本轮主版暂为 Figure 1 v09 + Figure 2 v25，PDF hash 和源文件 hash 见 `main_manifest.json`，渲染页在 `main_render_20261007/`。

每个 `build_vNN/` 都是独立迭代版本。它们保留自己的源文件、编译日志、失败现场、PDF 和 7 英寸渲染页。只有独立图级审查通过并由用户选定后，才同步覆盖主版，并先保存旧主版副本和同步记录。Figure 1 v13 当前处于审查中，因此尚未进入 `main.*`。
