# AAMAS 2027 论文版式、图表和实验矩阵审查 v1

- 审查日期：2026-10-04
- 审查范围：`paper/aamas2027/` 当前草稿、图表源与嵌入结果、有效的三份生效评价标准、当前 scorecard。
- 审查方式：只读检查；未改论文正文、未联网、未启动 GPU。
- 当前源码快照：`paper/aamas2027/main.tex`，SHA-256 `d92b451251172816dd7059cbad43a14f959d7c88d6c2416419a0b6ef37f2409c`（审查期间该文件已有未提交改动，后续必须重新取 hash）。
- 参照的最后有效 PDF：`paper/aamas2027/build/verify_figures_v7_20261003/main.pdf`，SHA-256 `0b639699238f8b5a4fd706385a5e5f6b62531d8f54e76dbc8e185068883b6069`；它对应旧的 HEAD 源 SHA `f80973e5db417e66c87db5931dba7c1d9737f7be743d828fc46f3b912c97db23`，不是当前源码的编译证明。

## 一、结论

**当前不能把论文说成已通过三份标准，也不能把当前 `build/main.pdf` 作为可交付 PDF。**

上一版隔离构建的 7 页 PDF 在官方 AAMAS 双栏版式下没有发现越界，Figure 1 和表格位置基本可读；但当前源码在该 PDF 之后发生了修改，当前 `build/main.pdf` 只有 2,555 bytes，`pdfinfo` 报 xref/stream 损坏，不能作为有效产物。`build/main_preview.png` 仍是 2026-09-26 的旧稿，标题为 `Learning to Select Scientific Tools from Outcome Feedback`，与当前题目和正文不一致。这个源文件—PDF—预览不一致是当前最高优先级的交付缺陷。

科学内容仍是内部 pre-results draft：主实验表、机制表和矩阵的结果格全部为 `TBD`，E6 为 `BLOCKED`。这符合“先留位置、禁止编造结果”的开发纪律，但不满足三份标准的投稿门槛。当前 scorecard 已明确为：故事线 `47.5/100 FAIL`、方法论 `37/60 HOLD/FAIL`、Benchmark+Baseline `41/60 HOLD`，Gate A/B/C 为 `11/16`、`11/16`、`10/16 HOLD`。

## 二、官方格式核对

官方本地留存的 `paper/aamas2027/reference/official/submission_instructions.html:491-496` 要求英文 PDF、双盲、官方模板、主文最多 8 页（参考文献另计），并要求 LaTeX；`reviewer_guidelines.html:509-524` 将超页、严重技术不足、格式违规和匿名性问题列为 desk-reject 风险，并要求正文自足。项目格式审计 `paper/aamas2027/AAMAS_2027_FORMAT_AUDIT.md` 的保守口径也要求附录不能承载核心内容。

| 项目 | 证据 | 状态 | 判断 |
|---|---|---|---|
| 官方 class 和双栏 | `main.tex:3`, `reference/template/aamas.cls` | 通过（旧有效 PDF） | 当前源码仍使用官方 class，未发现改边距/字号；须在当前源上重建 |
| 页数 | `verify_figures_v7_20261003/main.pdf` 为 7 页 | 仅对旧源通过 | 7 页不是必须目标；最终要求是不超过 8 页。当前源没有有效 PDF |
| 标题/作者匿名 | `main.tex:32-37` | 内部稿可用 | 投稿前必须移除 `INTERNAL PRE-RESULTS REVIEW`、副标题和 banner，并重新查 PDF metadata/源码路径 |
| 摘要 | `main.tex:39-41`，约 177 个英文词 | 内部稿通过 | 在 100–300 词范围内；最终摘要必须回填已确认 headline result 和边界，不能保留“results await” |
| 参考文献 | 旧有效 PDF 第 7 页，另计 | 通过（旧源） | 最终仍需从干净目录重建，不能依赖旧 `.aux/.bbl` |
| 图题/表题/可访问描述 | 两图均有 `caption` 和 `\Description{}` | 通过（旧源） | 真实结果图加入后逐图重查 |
| 当前 PDF 可交付性 | `build/main.pdf` 2,555 bytes，`pdfinfo` 报 `Invalid XRef`/`Missing endstream` | **不通过** | 先完成单进程干净构建，再进行任何最终排版判断 |
| 当前预览一致性 | `build/main_preview.png` mtime 2026-09-26，标题与当前正文不同 | **不通过** | 预览必须由同一 PDF 生成，和 PDF 一起记 hash |

不要为了“正好 7 页”增加无关文字。最终策略是：填入真实结果后主文不超过 8 页，保留读者能独立判断主张所需的图、表、定义和限制。

## 三、图表检查

### Figure 1

- 源和嵌入：`paper/aamas2027/figures/figure1/v4/figure1_v4.{svg,pdf,png}`，`main.tex:75-81`。
- PDF 尺寸：504 × 202.08 pt，约 7.00 × 2.81 in，符合跨栏图的目标高度。
- 语义：输入/菜单/历史 → 固定 selector 与 relay → 单一 selected provider → runtime ledger/judge → delay queue → released update → next action；caption 和 `\Description{}` 都明确 selected-only、release 和 fallback ledger-only。
- 旧版图审计给出 15/16；在旧有效 PDF 第 3 页，箭头、caption 和边界没有裁切。
- **状态：版式和信息层级通过。** 最终结果填入后只需做 100% 尺寸、黑白打印和 caption 与方法公式的一致性复核。

### Figure 2

- 源和嵌入：`paper/aamas2027/figures/figure2/v8/figure2_v8.{svg,pdf,png}`，`main.tex:179-185`。
- 实际 PDF 尺寸：240 × 270 pt，即 3.33 × 3.75 in；单栏嵌入时图中文字约 8–9 pt，旧有效 PDF 第 4 页没有裁切。
- 语义：decision → call → terminal/judge → pending delay → release → next action；未选输出以 crossed-out ghost 标出，caption 明确 release 前不更新。
- **计划冲突：** `paper/aamas2027/FIGURE_LAYOUT_AUDIT.md` 和 `figures/FIGURE_SET_PLAN.md` 仍把 Figure 2 目标写为约 1.8–2.2 in 高、横向 2.1–2.3 宽高比；v8 实际是纵向 3.75 in 高、宽高比约 0.89。v8 的可读性审查虽给出 15/16，但它已经偏离已记录的图表计划。
- **状态：REVISE。** 必须二选一并留下版本记录：
  1. 保留 v8，明确把 Figure 2 的规范改成“单栏纵向生命周期图、3.33 × 3.75 in”，重新检查页预算和图表计划；或
  2. 画一个不低于 8 pt 的紧凑横向 v9，使其回到原计划的 1.8–2.2 in 高，并重新做语义/黑白检查。

不要在没有真实结果前增加第三幅结果图。只有在填入 E1–E5 后矩阵仍无法在两栏中读懂，才按 `FIGURE_SET_PLAN.md` 的条件增加 protocol figure。

## 四、实验矩阵和表格检查

### 已做到的部分

1. `main.tex:250-280` 按 RQ/E1–E6 组织，而不是按代码执行顺序；覆盖任务有效性、在线比较、机制消融、反馈压力、错误/成本和确认。
2. `main.tex:225-246` 给出 Random、Best Fixed、Oracle、Frozen、Online SGD、Replay、LinUCB、PILOT、BayesianRouter、GraphRouter、任务控制和 MIRA-Relay 的角色解释。
3. 旧有效 PDF 第 5–6 页的表格没有越出栏宽；表题在表上，图题在图下。
4. `main.tex:251` 明确 raw JSONL、config/input/source hash、逐事件可见性和 recovery；这与三份标准要求的可复核方向一致。

### 当前不合格项

#### 1. E2 把不同比较资格层混在同一行

`main.tex:270-275` 的 E2 同时列 strict matched、Replay separate-control、adapted PILOT/BayesianRouter 和候选 MIRA-Relay。正文后面虽然解释 Replay 是 separate-control，但表格没有独立的 `stratum/primary contrast/status` 列。审稿人容易把它读成一张可直接排序的总榜，违反当前 Benchmark 标准 Gate B 对 stratum、prior、likelihood 和 warm-start 的分离要求。

**修复：** 将 E2 拆为至少三行或增加一列：`strict main (Frozen/Matched/MIRA)`, `separate control (Replay)`, `adapted modern (LinUCB/PILOT/BayesianRouter)`；每行写清是否允许主排名、共同 prior、反馈合同和 evidence status。Table 3 也要使用同一分层，不要只在 caption 里提醒。

#### 2. 当前生效方法与论文方法名未做映射

生效故事/方法文档的目标名是 **MIRA**，当前候选中还有 FamilyGuard、ContextProbe、MIRA-Relay 等阶段性实现。论文把 `MIRA-Relay` 写成 proposed method，但 E2/E3 没有说明它与 MIRA 主方法的关系，也没有在矩阵中标出哪些是停止的负候选、哪些是当前待检验方法。若投稿时仍保留这些名字，审稿人会不知道结果是同一方法的版本、消融还是不同方法。

**修复：** 在矩阵增加 `method identity / version` 字段，给出一条固定映射（例如“论文方法 MIRA；MIRA-Relay 是本文冻结实现；FamilyGuard/ContextProbe 是独立消融或停止候选”），并为每个 E2/E3 条目绑定 proposal/preregistration/commit。若最终方法仍未冻结，继续标为 pre-results，不能把不同候选合并成一个结果。

#### 3. 结果表目前无法满足 Benchmark 标准的统计和分母要求

Table 3 (`main.tex:285-305`) 只有 `System success / Δ vs Frozen / Runtime fail / Update p95`；Table 4 (`main.tex:309-324`) 只有 `Probes / Release change / Rescued / Harmed`。三份标准要求的 grouped/paired 95% CI、seed/scaffold/family 分层、calls、unknown/refusal、failure 分母、selection/update cost 和确认状态没有列位。表 2 虽然列了部分指标，但不会自动把指标带进结果。

**修复：**

- 主比较表至少显式放入：方法角色/stratum、attempt 数、system success 与 95% grouped CI、与 Frozen/Best Fixed 的 paired delta、runtime failure、unknown/refusal、calls、selection/update p50/p95；长成本字段可放一个紧邻的 cost table，但不能只放补充材料。
- 机制表至少按 delay `{0,1,4}` 报 release-after action change、paired system-success delta/CI、harmed/rescued、switch-away、runtime failure 和 probe exposure；family permutation 不能只写一个 pooled 行。
- 所有数值必须从对应 raw receipt/recovery 生成；不能把已知 development diagnostics 回填成 E2/E3 主结果。

#### 4. E1–E6 状态全部是占位，不能误称已完成

Table 2 的 E1–E5 为 `TBD`，E6 为 `BLOCKED / TBD`；Table 3/4 全部为 `TBD`。这在内部设计稿阶段是合规的 fail-closed 表达，但按当前 scorecard 只能记为 `PARTIAL/MISSING`，不能满足故事标准的“每条核心主张有独立证据”、方法标准的 realtime/effect gate 或 benchmark 标准 Gate C。

**修复：** 先按当前统一 receipt schema 完成 E1/T1、E2/T2、E3/T4、E5/T5，并给每格写 `PASS/PARTIAL/BLOCKED` 和 receipt pointer；只有 E1–E5、baseline strata、公平合同和 paired uncertainty 关闭后，才做 E6。结果返回后逐句回填摘要、贡献和结论，保留负结果和停止规则。

#### 5. 当前矩阵没有把“控制事件”与“自然动态”分开到表格字段

E4 将 transient/persistent failure、family-balanced exposure、delay、runtime block 与 arrival/retire/version 放在同一 Setting 单元，文字虽写“only if separately frozen”，但没有独立 scope/status。当前验收只支持 `feedback-adaptive`；自然 provider arrival/retire/version drift 仍未完成。

**修复：** 加 `dynamic scope` 与 `evidence status` 两列：`feedback-adaptive replay`、`controlled outage`、`natural lifecycle` 分开。没有独立 trace 的 lifecycle 行必须明确 `NOT RUN/BLOCKED`，不能与受控压力结果共享分母或结论。

#### 6. 结果段落和表格层级还会造成“placeholder 看起来像结果”的风险

`Results: Pre-results Tables` 和 `Known development diagnostics` 已声明范围，但表题仍使用 “Development comparison” 且方法列表混排；最终回填时若不改 caption，读者会把静态 Best Fixed/Oracle 或 old receipt 当作在线主结果。

**修复：** 维持三层明确标题：`Static task validity (fit-only/evaluator-only)`、`Strict selected-only online comparison`、`Adapted/separate controls and diagnostics`。每个 caption 写 scope、分母、是否可部署、是否进入 primary ranking。

## 五、与三份标准的对照判断

| 标准 | 当前判断 | 主要依据 |
|---|---|---|
| 故事线与创新 | **FAIL（内容阶段）/版式 PARTIAL** | 结构已覆盖问题→机制→证据→边界，但没有 confirmed headline result；方法名映射和最终 claim–evidence 仍未闭合；当前 scorecard 47.5/100 |
| 方法论 | **HOLD/FAIL（内容阶段）** | 公式、信息边界和图示较清楚；但 MIRA-Relay 的 realtime/action-level evidence、独立 transition replay、最终机制消融未闭合；当前 37/60 |
| Benchmark+Baseline | **HOLD** | truth/routing 两层写清；baseline roles 有；但 strict/adapted/separate strata、完整结果分母、paired uncertainty、provider lineage 和 E6 未闭合；当前 41/60，Gate A/B/C=`11/16`,`11/16`,`10/16` |
| AAMAS 硬格式 | **当前 FAIL，旧隔离 PDF PASS** | 旧有效 PDF 为 7 页；当前 build/main.pdf 损坏且 preview 过期。 |

## 六、建议的修复顺序

### P0：先修交付一致性（不耗 GPU）

1. 暂停并发写 `main.tex/build/`，确定一个源码 commit；记录 `main.tex`、figures、references 和 template hash。
2. 在干净目录单进程构建，产出新的 `build/verify_<date>/main.pdf`；用 `pdfinfo`、`pdftotext`、`pdffonts`、页面渲染和 SHA-256 验证。
3. 从这份 PDF 生成 `main_preview.png`，删除/标记过期预览，不允许旧标题继续存在。
4. 重新检查当前 PDF 页数（≤8）、overfull box、引用、匿名 metadata 和每页图表位置。

### P1：固定图表规范和方法身份

5. 对 Figure 2 v8 做计划对齐决定：保留纵向规格并更新图表计划，或绘制横向 v9；重新编译后检查单栏可读性。
6. 在论文正文和矩阵增加 MIRA/MIRA-Relay/FamilyGuard/ContextProbe 的固定映射与版本指针；停止候选只进机制/负结果，不进 proposed 主行。

### P2：关闭实验矩阵的可审查缺口

7. 把 E2 按 strict/separate/adapted strata 拆开；把 E4 按 feedback-adaptive/controlled stress/natural lifecycle 拆开。
8. 重做 Table 3/4 的字段，使主结果带分母、分组/成对 95% CI、failure/unknown/refusal、calls、latency/memory 和机制 action consequences；保留 static、online、verification 三个 scope。
9. 用统一 recovery receipt 回填 E1–E5；任何 `TBD` 只有在 raw evidence 和独立 verifier 指针齐全后才能替换。当前不打开 E6。

### P3：结果回填后的最终构建

10. 先回填主表，再回填摘要、贡献段、Discussion、Conclusion；结果不满足预登记 gate 时保留负结果，不改分母或换 benchmark。
11. 最后删除 internal subtitle/banner/TBD、检查匿名和元数据、重建不超过 8 页的投稿 PDF；保留内部 pre-results 版本和 hash，避免覆盖历史现场。

## 七、审查出口条件

只有同时满足以下条件，才可以把稿子称为“达到三份标准的投稿审查候选”：

- 当前源码有一份可解析、可重建且 hash 对齐的 PDF，主文不超过 8 页；
- Figure 2 规范与图表计划一致，所有图在 100% 单/双栏和黑白下可读；
- E2/E3/E4 的比较 strata 和 dynamic scope 在表格中显式分开；
- E1–E5 每项有 raw receipt、独立 recovery、主指标和 paired uncertainty；
- MIRA identity、baseline role、endpoint/provider scope 和 claim boundary 在摘要、主表、Discussion 中一致；
- E6 在方法、菜单、阈值、epoch 和 baseline 冻结后一次性执行，且不回流标签。

在这些条件未达到前，当前文档的正确标签是：**版式设计基本成形的内部 pre-results draft；不是可投稿 PDF，也不是已通过三份标准的论文。**

## 2026-10-04 P0 构建修复补充

上面的审查记录了并发构建期间的损坏 `build/main.pdf`。随后已停止并发写入，使用当前源码在
`paper/aamas2027/build/verify_current_20261004/` 做单进程隔离构建。新的构建已正常退出，
`pdfinfo` 可解析，主文 7 页，PDF SHA-256 为
`b5eea8d0326e026273094727e8224b9dd9ddd1f59add796b0268a221b414dadd`，对应源码
`main.tex` SHA-256 为
`e9eab8db7dd273dcd6d8b5378c1aa395914fb4493136adec4df2fb76218e410b`。由同一 PDF 生成的预览
为 `build/verify_current_20261004/main_preview.png`。完整命令、hash、页数和最终编译警告保存在
`build_provenance.json`，因此“当前 PDF 损坏”这一 P0 项已关闭；旧的 `build/main.pdf` 和旧预览
仍保留为历史现场，不作为交付物。

最终 pdflatex 轮次没有 undefined citation/reference；官方模板仍产生 incomplete-ifx 警告，
以及少量 overfull/underfull box，需在结果回填后再做一次版面清理。科学内容的 E1--E5、方法
身份映射、stratum 分离和 Table 3/4 字段缺口不因构建成功而改变，论文仍是内部 pre-results
draft，不能称为已通过三份标准。

## 2026-10-05 表格分层与公式排版补充

本轮把论文正文中的比较资格层显式拆开：E2a 是 strict common-prior logistic（Frozen、Matched
Online-SGD、MIRA-Relay），E2b 是历史 Replay separate-control，E2c 是 Gaussian/现代路由的
adapted stratum；E4a 是受控 feedback-adaptive stress，E4b 是没有冻结轨迹就保持
`BLOCKED / TBD` 的 natural lifecycle。结果表同时写明 `N/calls`、system success、paired
95% CI、Fail/U/R 和 update p95 的列位，机制表写明 scope/delay、probe、action change、
rescued/harmed 和 paired interval。所有经验格仍为 `TBD`，没有把开发诊断回填成结果。

修正了状态更新公式的单行超宽问题，使用分行的 release-state 定义；没有改模型含义、指标、
数据或实验结果。源码当前 hash 为
`90450db411e41b631d8574c63522ae5d38f0e3cb694bb3664dfe352f5da955ba`。

在隔离目录 `paper/aamas2027/build/verify_current_20261005_v3/` 单进程重新编译：PDF 可解析、
letter、8 页，PDF hash 为
`8e7366bc61c3ab47b23d0ab6ad2d33413c622f4af389b4a7a5b0b9d711fb71a1`。最终 pdflatex 轮次没有
undefined citation/reference，也没有 overfull hbox；仍有官方模板 `ifx` 警告、underfull
排版提示和 1.333pt 的 page-balance overfull vbox。第 2 页公式和第 7 页两张占位表已目视检查，
没有裁切或越栏。完整命令、hash、页数和警告见
`build_provenance_20261005_v3.json`。这只关闭了当前源码的排版交付问题，不改变三门评分或
E1--E6 的科学状态。

## 2026-10-05 方法身份与 baseline 命名补充

对照 `baseline_eligibility_matrix_v2/matrix.json` 后发现，正文还需要把候选实现的层级写死：
已在 `main.tex` 的实验矩阵前明确 MIRA 是框架、MIRA-Relay 是本文冻结实现、FamilyGuard
是其机制条件，ContextProbe 和 Provider-Graph Relay 是停止的诊断，不进入 proposed-method
行；结果表也恢复为与 roster 一致的 `Greedy LinUCB (adapted)`。这修的是审稿可读性和
baseline 身份一致性，没有改变实验数据、比较资格或 claim。

重新隔离构建 `paper/aamas2027/build/verify_current_20261005_v4/`：8 页 letter，PDF hash
`aa93bfeb30d3e97140f39541dd6a4ab024117dbfb9e6100752326648d30b0210`，源码 hash
`169d54af7f0f09d1e735a0395691575b21b14ee8c866dd7229a0de8e2c6394e7`。最终轮次没有
undefined citation/reference、overfull hbox 或 overfull vbox；保留官方模板 `ifx` 和
underfull 提示。第 7 页的主表、机制表和 Related Work 目视通过。凭据见
`build_provenance_20261005_v4.json`。
