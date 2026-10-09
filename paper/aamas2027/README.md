# AAMAS 2027 论文草稿

当前工作题目：**After the Decision: Learning to Adapt from Delayed Outcomes**。

## 当前生效构建（2026-10-08：到达顺序证据合入）

- 唯一全文入口：[主版 PDF](main.pdf)。来源为 [submission_20261008_114253](build/submission_20261008_114253/main.pdf)，正文 **恰好8页**，References 第9页开始；源码合并到 `main.tex`。
- 主版 SHA：`be5e1f50da9ba5313ef5bd2159c288d2a2e9de84ddd271569900562a5c6b3cdc`；源码 SHA：`0377e70fe9aec39158651b25bff6a898dbc9f4f13957cd478ec2cc1b02226930`。[来源记录](main_provenance.json)已更新，旧主版在 `archive/promotions/main_before_20261008T115550Z.pdf`。
- 主表将原始顺序和128个随机顺序的均值并排，10行、5列、9pt，完整保留各条件的对照。受控 MIRA−SGD +4.74pp，125/128顺序为正；自然 MIRA−Frozen −0.29pp。独立全量复算已通过；[规范实验报告](../../task/task49-pharma-benchmark-baseline-freeze/primary_arrival_robustness_v1/report.md)保留统计对象和负结果。
- 原适应实验的区间改写为固定轨迹 scaffold 重采样范围，数值与详细原表保留；新的均值区间只针对已检查队列的到达顺序分布。静态 post-call 的条件bootstrap定义和绑定未变。
- 编译前后post-call绑定、270/150原轨迹检查、页数/引用/overflow检查通过。根代理看p1及p5–9；[独立审查](build/submission_20261008_114253/independent_visual_review.md)实际看全部9页。无overfull或未定义引用；6条underfull与已知模板ifx诊断保留。图1旧面板文字约9PDFpt越框的小项留在审查记录，未称全稿零瑕疵。
- [合入迭代v01–v05](iterations/arrival_robustness_v1/)、两次超页与一次中断构建全部保留。表格合并后前移post-call小节，解决孤立跨栏表；字号、官方class和页边距未改。前两遍LaTeX使用原生draftmode跳过重复PDF输出，最终遍仍实际输出并接受完整检查。
- 旧的独立表预览是合入前快照；阅读全文以本页主版为准。完整科学gate仍 **HOLD**，没有向OpenReview上传。

## 历史构建（2026-10-08：实验图表重排）

- [主版 PDF](main.pdf) 已从 [最终候选](build/submission_20261008_093454/main.pdf) 提升；正文 **8 页**，References 从第 **9 页**开始。根代理实际查看第 5–9 页，独立视觉审查及页数闭环均通过。
- 主版 PDF SHA-256：`67fcf08e3d4eae094c072428eb3e98a17f5f833d90d2b87a183d6fed76141eec`；源码 SHA-256：`831149e1f05b17a6943dafa8d309ddb98c4643eef5083a1a82e820fc54f47a72`。唯一绑定记录是 [main_provenance.json](main_provenance.json)；旧主版与 provenance 已归档。
- 六组实验对应六张结果表（正文 Tables 3–8），延迟分层改为 Figure 3 的两个数据 panel。9pt 表体、8.5pt 表注，统一行组、精度、表头和图例；删去重复列与整列占位。没有改结果、研究范围、标题或原图资产选择。
- E6 renderer 只改变呈现；原输入/hash/行数/统计绑定检查保持不变。natural/stress 270/270、150/150 receipt audit 和构建前后 post-call binding 通过。没有新实验、API、GPU 或 provider inference。
- 两次超页构建及全部源码保留；最终无 overfull、undefined reference/citation、missing-math 或 fatal error。仍有 4 个 underfull vbox、1 个 underfull hbox 和已知模板 ifx 诊断，实际页面未见相应裁切，不能称 zero-warning。
- [两页独立预览](experiment_tables/main.pdf)、[实际获奖论文参考页及分析](experiment_tables/reference_style_report.md)、[合入记录](experiment_tables/reviews/main_integration_20261008.md)、[最终独立闭环](experiment_tables/reviews/formal_tables_final_closure_20261008.md)、[页面 QA](build/submission_20261008_093454/visual_qa.json)。预览 v01–v05 均保留。
- 完整科学投稿 gate 仍 HOLD；本轮完成的是实验展示整改。参考文献补全仍为独立待办，未修改 references.bib 或向 OpenReview 上传。

## 历史构建（2026-10-08：post-call 证据修正与结果表）

- [主版 PDF](main.pdf) 是唯一生效的 paper-level PDF，位于迭代目录 `build/submission_*/` 上方；完成的修改合入 `main.tex` 后，经预检再提升到主版。每次替换保留旧 PDF 和来源记录。
- [本次通过预检的构建](build/submission_20261008_082437/main.pdf)：正文 **恰好 8 页**，References 从第 9 页开始；第 6、7、8、9 页已实际渲染检查，无截断/越界。第 8 页左栏到正文底线，右栏完整结束于结论，比上版空白明显减少。
- `main.tex` SHA-256：`429c6ca2a8f07b50433a7933a4e34579f1635bf54728b9f11defc8cc3df96486`；主版 PDF SHA-256：`90deeeeda2bd15892a7d0d74df8cd1d99d12c8dfa4802c9e22a3b51494d9f1d9`；构建来源见 [main_provenance.json](main_provenance.json)。
- 新增紧凑 post-call 结果表和效用分析；修正 random 对照的 CI 为 `[+2.77,+9.29]` pp，并补齐相对 always-accept 的 utility CI `[+0.0088,+0.0609]`。这轮复用 157 个先前检查过的分子，明确记作 **secondary analysis**，不是新的 untouched confirmation；random 是 evaluator-side conditional null。
- `bind_postcall_results.py` 在构建前后检查表格与 CI 和审计原始文件的绑定；独立 verifier 已验证 3,140+3,140 行及 232 个汇总字段，26 个测试通过。原始结果、错误报告快照和失败构建均保留。
- 原 direct-selection、controlled drift 和 v2 mechanism 数据不变。声明范围内的构建/receipt gate 通过，**完整科学投稿 gate 仍 HOLD**：当前没有 integrated online verification/agent gain 的完整证据，不能用 secondary refusal observation 替代它。
- 详细修正及下一步：[本轮记录](../../research/experiment_section_style_20261006/postcall_correction_and_master_20261008.md)、[修正报告](../../task/task49-pharma-benchmark-baseline-freeze/postcall_fresh_confirmation_v1/report.md)、[第二次调用条件价值候选](../../task/task49-pharma-benchmark-baseline-freeze/conditional_second_call_v1/design_review.md)。

本次未改变标题或当前图资产。LaTeX 无 overfull、undefined reference/citation、missing-math 或 fatal error；官方模板环境的已知 conditional warning 和部分 underfull 仍存在，不能称完全 zero-warning。OpenReview submission ID 仍空，未向网站上传。

源码是 `main.tex`、`references.bib` 和 `WRITER_REPORT.md`；运行 `make draft` 编译到 `build/main.pdf`，运行 `make submission` 做声明范围内的科学状态、双盲、引用、日志和 8 页 preflight。它只生成本地投稿候选 PDF，不向 OpenReview 上传。

维护主版时先运行 `make submission`，确认新目录中的 PDF 通过 9 页（正文 8 页 + 参考文献）和日志检查后，再运行 `make promote-main SOURCE=build/submission_<timestamp>/main.pdf`。不要直接把 `build/main.pdf`（阅读稿）复制成主版。

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

### 2026-10-07 mechanism ablation main.pdf

- 主稿新增 MIRA-Shared wrong-credit control，并把 Local/Shared/Frozen 机制结果回填到实验矩阵和 drift 表；没有改变 benchmark 或 provider checkpoint。
- 来源构建：`build/submission_20261007_122046/main.pdf`；9 页总计，正文正好 8 页，参考文献第 9 页。
- 当前 paper-level 主版：`main.pdf`；PDF SHA-256=`8faa50ed36cc4080ff70822f050ebaa73499e8bcd08735b3d1b5a7bc37cf0ae4`。
- `main.tex` SHA-256=`93a5d40c309c7d11ceb3898d4683882983a5f45614f8c447914bcab06b47dc8d`；构建日志 SHA-256=`4bc8b3cf80431b3c3cb9717b2b3ff787fb620e09d6187684809f29bbd7292f30`。
- 最终 warning gate 无 `Overfull`、`Undefined`、`Citation`、`Missing $`、`Fatal` 或 `Emergency`；`main_provenance.json` 是主版页数和哈希的唯一记录。

### 2026-10-07 Shared-Credit correction (historical)

The preceding mechanism-ablation entry is historical. An independent audit found
that the pooled term in the implemented Shared-Credit score cancels from the
provider `argmax`, and that its initial ranking differs from Frozen. Its
Local--Shared result therefore cannot isolate selected-provider credit. The
primary rows and causal wording were removed from `main.tex`; raw results and
the superseded PDF remain in `paper/aamas2027/archive/20261007_shared_credit_correction/`.
That historical master was promoted from
`build/submission_20261007_140604/main.pdf` after the 9-page and warning gates.

### 2026-10-08 fresh mechanism v2 integration

The invalid v1 Shared-Credit interpretation remains archived. The current
source adds the preregistered v2 matched mechanism control: Frozen, MIRA-Relay,
and fixed cyclic permutation share the same cold-start scores and actions; only
the released residual destination changes. The paragraph reports the separate
equal-weight scaffold-mean estimand and keeps the pooled primary tables intact.
The source was rebuilt and promoted only after the 9-page (8 body + references)
and warning gates passed. The previous paper-level PDF is preserved through
`main_provenance.json` and `archive/promotions/`.
