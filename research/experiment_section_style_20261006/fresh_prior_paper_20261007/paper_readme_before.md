# AAMAS 2027 论文草稿

当前工作题目：**After the Decision: Learning to Adapt from Delayed Outcomes**。

## 当前生效构建（2026-10-07：六格机制诊断表）

- [最新 PDF](build/submission_20261007_043532/main.pdf)：正文 **8 页**，参考文献从第 9 页开始；第 7 页六格结果表和第 8 页已目视核对，无溢出或裁切。
- 英文源码 SHA-256：`fd4fff41cdad26b7f735d3905630344d9cdfcbab28cd7d17ff4fcd09b84b2223`；PDF SHA-256：`d76952790290d39268971dc225de6554325ca7f1c0a013ff92da79546afcc119`。
- 新增分数修正×在线中心化的六格表，180条开发流、25,992次决策经过独立核验。分数质量改善，但新中心化未达到预登记1pp推进线；旧确认结果和方法保持原定义。完整报告见 `task/task49-pharma-benchmark-baseline-freeze/calibration_centering_v1/report.md`。
- `make submission` 每次重新检查原始事件；当前 420 streams 审计及编译预检通过。三份完整论文标准仍为 HOLD，机制/校准和 baseline 比较继续推进。
- 以下编译记录是历史快照；以上述源码 hash 对应的 PDF 为本轮依据。中文阅读版仍须与此 hash 对齐。

截至 2026-10-07，主稿已经回填一次调用范围内的真实 fresh confirmation 和 controlled correctness-drift 结果。MIRA-Relay 是当前一次调用方法：冷启动严格等于 Frozen，释放后只更新被选 provider 的相对可靠性。post-call verification、拒答、hERG/AMES、自然 lifecycle drift 和完整 33-provider model-fresh 仍不在本稿主张内。

源码是 `main.tex`、`references.bib` 和 `WRITER_REPORT.md`；运行 `make draft` 编译到 `build/main.pdf`，运行 `make submission` 做声明范围内的科学状态、双盲、引用、日志和 8 页 preflight。它只生成本地投稿候选 PDF，不向 OpenReview 上传。

### 2026-10-07 阅读稿 Figure 1 恢复

默认阅读稿采用 **AI Figure 1 v09**：[`最新阅读 PDF`](build/reading_ai_20261007_v09/main.pdf)。
用户的 `MIRA-Relay延迟反馈架构图 (1).png` 与仓库 `figures/figure1/v07/figure1_v07.png`
字节相同，原图没有丢失；之前正文引用程序 v08，导致阅读 PDF 看不到 AI 图。

本轮按实际查看的 TableDART、CRITIC、CollabLLM 样例展开 frozen scorer、计数寄存器、
residual vector、合成算子和选择模块。所有原图、失败版和完整 prompt 保留。
详见 [`图库拆解与设计规格`](figures/gallery_style_study_20261007.md)、
[`最新完整提示词`](figures/prompts/figure1/v17.txt) 和 [`图版本索引`](figures/SELECTED_VERSIONS.md)。

阅读构建与投稿构建显式区分：默认 Figure 1 为 AI v09；`make submission` 定义
`MIRASubmissionFigures` 后选择程序 Figure 1 v08，Figure 2 仍为程序 v13。
这是按 AAMAS 官方 AI 图片条件选择构建源，不代表双方已确认待议决议 0011。
Figure 2 的统一风格 prompt v23 已保存但尚未生成，数据图继续程序绘制。
下方 2026-10-06 构建记录为历史版本，不覆盖本节的当前阅读图选择。

当前正式英文稿的中文阅读版为 [`main_zh.md`](main_zh.md)。该文件用于中文通读与审查；只有其中记录的英文源码 hash 与当前英文稿一致时，才视为已同步。引用键沿用 `references.bib`，提交与数字核对以英文稿为准。

2026-10-06 的当前隔离构建 `build/review_20261006_v16/main.pdf` 已完成多轮 LaTeX/BibTeX/LaTeX。正文正好 8 页，参考文献单独从第 9 页开始；PDF SHA-256=`6de7eb6673c2899f03fe44a95b87f8313384107ad1c8ffe5cd18be1e6f5a50f3`。最终日志没有 undefined citation/reference、overfull box 或 fatal error；第 8 页的 artifact scope 已排满，保证不是空白凑页。

官方模板由 AAMAS 2027 投稿说明页下载，压缩包和解压原件保存在 `reference/`。`aamas.cls`、引用样式及版式没有修改，来源与 SHA-256 见 `reference/sources.json`。使用当前项目的目标届次 2027，而不是另取 2026 旧模板。

当前候选领域是 Learning and Adaptation (LEARN)：论文要贡献的是工具选择智能体如何从有限反馈持续改善决策。多个预测器是工具候选，不因数量多就称作多智能体。最终领域选择仍取决于完成的方法与证据。

摘要目前已回填 MIRA-Relay 的真实 headline result、comparator、分母和 controlled-stress 边界。不能只用“MIRA + 在线头”作为创新句，也不能把 BBB development panel 写成最终 confirmation。投稿前仍要做方法名、匿名信息和引用复核。

作者、OpenReview 投稿号和真实结果均未编造。Owner 此前已确认作者 OpenReview 账号完成，不重复核问。本文没有向 OpenReview 提交。

## 编译记录

2026-09-26 已成功生成并目视检查 [一页 PDF](build/main.pdf)：题目、摘要、关键词与官方版式可正常阅读。初次缺少 hyperxmp 和字体的失败日志保存在 build 目录；依赖补齐后的构建成功记录见 `build/build_attempt_04.log`。

当前本机使用 BasicTeX/latexmk。libertine、inconsolata 由 `tlmgr --usermode install libertine inconsolata` 安装；hyperxmp 包不能由用户模式 tlmgr 安装，原包从 `https://mirrors.hit.edu.cn/CTAN/systems/texlive/tlnet/archive/hyperxmp.tar.xz` 取得，SHA-256 为 `ef25309cb2ac16ddbd0788fe26c5b029b9dfc0dd8c87d26c4b90dbd90a2a73e2`。将包的 `texmf-dist/` 内容解压到 `build/texmf/` 后，Makefile 通过 TEXINPUTS 使用本地依赖。完整 TeX Live 环境也可直接提供这些包，无需该补丁目录。

当前环境会给出 `end occurred when ifx ... was incomplete` 条件警告；未修改的官方 sample 在同环境也出现此警告。PDF 构建退出码为 0，未修改官方 class 来隐藏警告；投稿前仍需在完整 TeX 环境再核验。文件哈希和构建依据见 `build_provenance.json`。

## 官方入口

- [主轨征稿与领域定义](https://warwick.ac.uk/fac/sci/dcs/aamas2027/calls/call-for-main-track/)
- [投稿说明与模板](https://warwick.ac.uk/fac/sci/dcs/aamas2027/guidelines-and-policies/instructions/)
- [审稿说明](https://warwick.ac.uk/fac/sci/dcs/aamas2027/guidelines-and-policies/reviewer-guidelines/)

2026-09-26 核对：摘要截止 2026-10-01，全文截止 2026-10-08，均为 AoE；摘要约 100–300 词，主文最多 8 页，要求 LaTeX 和双盲。最终提交前再核对官网更新。


## 2026-10-06 state-audit and figure-consistency addendum

- MIRA-Relay receipt state now serializes `fit_rates`, `successes`, `counts`, and `prior_count`; the natural 270-stream and controlled-stress 150-stream receipts both pass contract/replay verification. The additional 30-stream BayesianRouter stress comparator is recorded separately.
- Current gate record: `task/task49-pharma-benchmark-baseline-freeze/stage_b_scientific_submission_gate_v3/`.
- Current submission figures: Figure 1 `figures/figure1/v08/figure1_v08.png`; Figure 2 `figures/figure2/v13/figure2_v13.png`, an editable SVG/HTML-derived rendering. The AI-generated Figure 2 candidates remain archived for internal comparison only and are not used in the submission build.
- Current PDF: `build/review_20261006_v16/main.pdf`, 8 body pages plus references on page 9, SHA-256 `6de7eb6673c2899f03fe44a95b87f8313384107ad1c8ffe5cd18be1e6f5a50f3`; final `main.log` has no undefined citation/reference, overfull, missing-math, fatal, or emergency diagnostics.

Clean-tree submission rebuild from commit `07d88e20`: `build/clean_submission_20261006_bayesian.pdf`, 9 pages total with exactly 8 body pages, SHA-256 `458f804f8750209e9377429b8cf14ad8deba2c94214759eba6ee4a7bf554c04a`; its clean log has no undefined citation/reference, overfull, missing-math, fatal, or emergency diagnostics. Pages 2, 5, 6 and 8 were visually checked.

## 2026-10-06 实验矩阵重排后的当前构建

当前主稿按顶会常见的“短 setup → 紧凑实验地图 → 主结果表 → 机制解释”重排。使用 Figure 1 v08 和程序绘制 Figure 2 v13；生成式 AI 候选只保留为内部审查材料。

- 源码 `main.tex` SHA-256：`55eb99725c03b1b87b5b32056b663787ba65d5a3a85f82636ac07d2564fff513`
- PDF：`build/clean_submission_20261006_matrix_rewrite.pdf`
- 9 页总计，正文 8 页，参考文献第 9 页
- PDF SHA-256：`05a5a4173e14417f83ae43ef4e5069b4e09736a09c58be1ad34d7f3d9b2af346`
- 第 5、6、7、8 页已目视检查；结果表先于解释段落；日志没有 overfull、undefined citation、missing math、fatal 或 emergency。

实验章节重排依据和科学 gate 缺口见 `research/experiment_section_style_20261006/implementation_log.md`、`scientific_gate_review_20261006.md` 与 `nearest_work_matrix.md`。

## 2026-10-07 投稿 preflight 与独立回执审计

- `make submission` 已从硬编码失败改为声明范围内的本地 preflight；运行成功只生成候选 PDF，不上传 OpenReview。
- 当前源码 `main.tex` SHA-256：`ee5264e8819f1f3f2e6c06292105cc3d1c3837ef0f46fe3958d2d59ce81c3503`。
- 当前候选 PDF：`build/submission_20261006_181518/main.pdf`；9 页总计，正文正好 8 页；PDF SHA-256=`abf88071fe9e829d1207d354513396533ec8c1379e2fa9eb9001193aac097394`；日志 SHA-256=`303e9d53dde259e5fd8a9a9d401793db97b64dd0d48a82eb48104fc34b42cffc`。
- 独立 event-level receipt checker 通过 natural v2 的 270 条流/127,170 条事件和 stress v3 的 150 条流/70,650 条事件；9 个 CPU smoke/mutation 测试通过。它验证身份、顺序、绑定、时钟、泄漏、摘要和 opaque hash continuity，不声称从旧回执重算 learned policy state。
- 当前剩余边界：旧回执缺少 canonical state bytes；若要关闭 learned-state 独立重算缺口，需要新 schema 版本和新 raw receipt，不回写本轮科学结果。

### 2026-10-07 clean-tree final build

- 来源：当前提交 `dff33da9` 的 Git archive；不使用工作区的 `.aux`、`.bbl` 或历史构建缓存。
- PDF：`build/clean_submission_final/main.pdf`；9 页总计，正文正好 8 页；PDF SHA-256=`663d9fcdf779c6043b8c9b7883bf1887f2bba8fff6a556714e57f0fb0c08205f`。
- 源码 SHA-256=`ee5264e8819f1f3f2e6c06292105cc3d1c3837ef0f46fe3958d2d59ce81c3503`；日志 SHA-256=`c03a6d0645317fc663fa42d7eec4b4a9b3aaaa5b4163edf76a1694bef376e212`。
- clean build 完成 BibTeX 和三轮 LaTeX；无 `Overfull`、`Undefined`、`Citation`、`Missing $`、`Fatal` 或 `Emergency`。第 2、3、5、8 页已目视检查。
