# 实验矩阵与结果段落风格审查（2026-10-07）

## 审查范围

只读检查：

- 正文：paper/aamas2027/main.tex
- 已编译提交版：paper/aamas2027/build/submission_20261007_084607/main.pdf
- 顶会实验结构调研：research/experiment_section_style_20261006/report.md、topconf_experiment_literature.md
- 之前的只读审查：final_paper_readonly_review_20261006.md

本审查不修改正文、代码或实验结果，也不新增结果。

## 结论

当前版本已经从“预注册清单/项目台账”改成了论文式的四列表格，主体结构也基本符合 ICLR、NeurIPS、ICCV 高质量论文的共同模式：

实验设置（短） → compact map → 主结果表 → 机制/成本解释 → 限制与复现。

因此，问题不再是矩阵整体失控，而是三个会影响审稿人阅读和证据对应关系的细节：

1. **矩阵没有覆盖正文中的全部实验结果块。** Table 5（centering diagnostic）和 Table 6（learned-prior diagnostic）在正文中有独立结果，但 Table 2 只有 Headroom/Natural/Drift/Cost 四行。读者无法从 map 预期这两张表，容易感觉结果块是事后追加的。
2. **Cost / validity 行更像审计任务描述，而不是实验问题。** “All confirmation streams / calls, failures, replay checks, p95 update time” 混合了测量指标、验证状态和 artifact 检查；其数字不在同一张表中，且没有明确 failure denominator。
3. **结果表的比较基准和重复信息还可以更明确、更紧。** 两张主表的 Paired Δ 没有在表头中直接写出“相对于 Frozen”；Table 4 的 N=4710 对每个方法都相同；结果段落又逐行复述多个 baseline，略像报告而不是“问题—结论—解释”。

这些问题都能用正文最小改动修复，不需要新实验。

## 按优先级的问题与修改建议

### P0：让实验 map 覆盖所有正文结果块

**证据。**

- Table 2（main.tex:224--239）只有四行：Headroom、Natural adaptation、Drift recovery、Cost / validity。
- Table 5（main.tex:317--335）报告 score scale/centering 的 factorial diagnostic。
- Table 6（main.tex:346--362）报告 BCE/BT offline objective 的 static selection diagnostic。
- main.tex:300--344 把这两组结果称为 development diagnostic、headroom 和 offline objective 结果，但 map 没有对应行。

**为什么影响论文风格。**

顶会论文的实验地图通常先告诉读者正文将回答哪些问题；后续每张主表都能回到一个问题。当前正文主结果表能对应 map，但 Table 5/6 没有对应的 study 标签，会破坏“问题 → 结果”的导航。

**最小改动。二选一。**

- **推荐：**在 Table 2 增加一行 Static diagnostics，例如：

  Static diagnostics | 722 BBB development molecules; five scaffold folds; no confirmation reuse | corrected/raw Frozen, marginal/query residual, BCE+CLS, BT+CLS | OOF/stream accuracy; selection headroom

  这样 Table 5 与 Table 6 都归入同一个明确的 diagnostic block。
- 若必须维持四行，则把 Table 5/6 移至 appendix/supplement，并在正文保留一句“development diagnostics are reported in Appendix X”。不要在正文保留未出现在 map 中的完整诊断表。

不需要改变任何数值或 claim。

### P1：把 Cost / validity 行改成已测量的结果，而非审计清单

**当前写法。**

Cost / validity | All confirmation streams | All one-call methods | calls, failures, replay checks, p95 update time

这一行同时承诺 calls、failures、replay checks 和 timing，但正文随后分散给出这些内容；“failure”没有清楚的分母，replay check 也不是方法比较指标。

**推荐最小改动。**

将行改为：

Cost / validity | 270 natural + 150 stress streams; one call/molecule | all deployable methods | policy update p95; calls; receipt pass rate

并在该行后或结果段落中保留一句：

“Each stream uses 157 calls; 270/270 natural and 150/150 stress receipts pass the contract/replay checks.”

如果确实要报告 runtime failures，再给出明确的 failed selected calls / total selected calls；否则不要在矩阵里写 failures。这只重命名指标，不创造结果。

### P1：让主结果表直接表达比较对象

**当前问题。**

- Table 3（main.tex:252--264）列名为 Paired Δ (95% CI)；读者需要读段落才知道它是相对于 Frozen。
- Table 4（main.tex:280--288）同样使用未限定的 Paired Δ。
- Table 4 的 N=4710 在所有方法行重复，caption 已经给出 pooled calls。

**最小改动。**

- 两张表的列名都改为 Δ vs Frozen (95% CI)。
- Table 4 删除重复的 N 列，在 caption 中保留 N=4,710 paired calls。
- Frozen 行的参考值写成 0.00 (ref.) 或 0.00，避免破折号与数值混用。
- Table 3 caption 可以缩为：“Success is prequential system success; paired deltas are against Frozen with 95% scaffold-cluster bootstrap CIs. Update p95 excludes I/O and provider inference.” 数据规模和 157 分子在上一段保留即可。

这些修改会让表在几秒内可读，符合调研中“方法 × 指标”的主表习惯。

### P1：结果段落减少逐行复述，保留一个 headline 和一个边界

当前 Natural 段落（main.tex:272）已经有正确的结论，但在一个段落中又列出多个 baseline 的区间；Controlled 段落（main.tex:293）也逐个列出三个控制方法的增益。

建议采用统一的五句以内结构：

1. 一句说明问题和设置；
2. 指向表；
3. 报告 MIRA-Relay 与 Frozen 的绝对值和差值/区间；
4. 用一句话概括其他控制（“all matched controls improve under this stress; see Table 4”）；
5. 一句写清证据边界。

例如：

On the stationary fresh menu (Table 3), MIRA-Relay and Frozen both obtain 89.81% success (paired Δ 0.00 pp, 95% CI [−1.24,+1.40]); its 164 switches split evenly into 82 rescued and 82 harmed decisions. Thus the current panel gives a stability result, not a generic online gain. Under the declared midpoint flip (Table 4), MIRA-Relay reaches 87.56% versus 73.38% for Frozen (+14.18 pp, 95% CI [+9.61,+19.03]); matched controls also recover, so the result identifies response to this stress rather than superiority on all online streams.

不需要改变结果或降低 claim；只是把表承担数字、正文承担结论。

### P2：Baseline 角色表目前可读，但略有重复

Table 1（main.tex:200--217）和 Table 2、Table 3/4 重复列出大部分方法。角色分组本身合理，也符合 anchors / matched updates / modern controls / proposed 的论文写法；它不是硬问题。

如果需要节省版面，最小方案是把 Table 1 的三列压成两列：

Role | Methods

并用上一句解释每组的作用。详细的 adapter 语义、PILOT/GraphRouter/RouterDC/RouterBench 的 audit-only 原因继续留在正文一句话和 artifact。不要在主文放完整 baseline contract、hash 或执行状态。

### P2：把“未做/扩展”从结果段落移到限制或 artifact

main.tex:296--298 的 “post-call utility decomposition is reserved for the out-of-scope evidence extension” 和 “Post-call verification is outside scope” 是项目状态语言。顶会结果段落通常只报告当前可测量的指标；未覆盖的 verification/refusal/lifecycle/hERG/AMES 已在 Benchmark 与 Limitations 有位置。

建议：

- 在“Decision changes and costs”中只保留当前测量：selected success、rescued/harmed、calls、policy p95、receipt pass rate；
- 将“verification/refusal/lifecycle are separate contracts”保留在 Benchmark/Limitations/Artifact scope 一次即可；
- 结果段落不要出现 reserved、out-of-scope evidence extension 这类计划语句。

这会让正文更像已完成的论文，而不是实验管理文档。

## 建议的 compact map（不改结果）

如果需要一个可以直接替换 Table 2 文案的最小版本：

| Study | Setting | Methods | Metrics |
|---|---|---|---|
| Static headroom | BBB development, 722 molecules; scaffold OOF | Best Fixed, Frozen, BCE+CLS, BT+CLS | selected correctness; oracle/headroom gap |
| Natural stream | 157 fresh molecules; 4 providers; D∈{0,1,4}; stationary | Frozen, MIRA-Relay, MIRA-Relative, Online SGD, LinUCB, BayesianRouter | prequential success; paired scaffold CI |
| Controlled drift | same menu; fit-best label flip after t=78 | Frozen, MIRA-Relay, Online SGD, LinUCB, BayesianRouter | success; rescued/harmed; paired Δ |
| Static diagnostics | 722 development molecules; 5 scaffold folds; no confirmation reuse | corrected/raw Frozen, marginal/query residual, BCE+CLS, BT+CLS | OOF/stream accuracy; headroom |
| Cost / validity | 270 natural + 150 stress streams; one call/molecule | deployable methods | update p95; calls; receipt pass rate |

若 5 行影响版面，可将第一行和第四行合并为 Static diagnostics。

## 已通过的部分

以下结构已经符合调研得到的顶会规律，不建议为“填满 8 页”而扩写：

- 主结果顺序是自然 fresh → controlled drift → mechanism/cost，科学问题顺序清楚。
- Table 3/4 是方法 × 核心指标的紧凑数值表，不再把 hash、状态、执行状态放进矩阵。
- 结果段落确实先指向表，再解释差值、救回/伤害和 claim 边界。
- MIRA-Relay、Frozen、matched Online SGD、LinUCB、BayesianRouter 的共同输入和反馈时钟已在设置段统一说明。
- 现有 PDF 的正文页数和编译错误 gate 已通过；本审查不建议再扩大文字量。

## 最终判定

- **风格：条件通过。** 目前已经是论文实验章节，而不是原先的审计台账。
- **必须处理：** map 与 Table 5/6 的对应关系；Cost / validity 指标与证据对齐；Δ vs Frozen 明示。
- **可选优化：** 压缩 baseline 表和结果段落，将 reserved/out-of-scope 计划语言移出结果段。
- **不需要：** 新实验、虚构数字、重写 benchmark 或扩大 claim。

本报告是风格/组织审查，不替代三份科学验收标准，也不把当前 scientific submission gate 从 HOLD 改成 PASS。
