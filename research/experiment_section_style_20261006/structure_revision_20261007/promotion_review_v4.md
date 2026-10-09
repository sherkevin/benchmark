# 7 节结构候选 v4 提升记录

日期：2026-10-07

## 目的

把正文从碎片化的 13 个顶层节收为 AAMAS 常见的 7 节结构，并保持已核验结果、负结果和 claim 边界不变。该提升只处理行文结构和版式，不把本地 preflight 当成科学 gate。

## 修订点

- 顶层结构固定为 Introduction / Related Work / Problem Setting / MIRA-Relay / Experiments / Discussion and Limitations / Conclusion。
- Related Work 前置；实验 setup、主结果、机制诊断和有效性检查集中在 Experiments。
- 复现性与 artifact 合并到 Discussion and Limitations，删除重复的 verifier/interface 和 delay 说明。
- 将 `r^{chem}`、`r^{run}` 同时写入 released-state 更新输入，明确 chemical residual 只使用非 null chemical label，修复运行状态与化学正确性语义冲突。
- Related Work 不再声称已经完成 family-sensitivity 结果；改为把 family/training-membership 写成有效性边界。
- 删除未定义的 “MIRA adaptation framework”，直接定义 MIRA-Relay；verification/refusal 作为范围外扩展。
- 在 Artifact Scope 末尾补一段已有方法直接支持的 agent design implication，用于利用第 8 页原有留白，不新增实验结果或扩大 claim。

## 编译和目视检查

- 源码：`paper/aamas2027/main.tex`，SHA-256 见 `revision.json`。
- PDF：`paper/aamas2027/build/structure_candidate_20261007_v4/main_structure_candidate_20261007.pdf`。
- 结果：9 页总计，正文 8 页，参考文献从第 9 页开始。
- 最终 LaTeX pass 无 `Overfull`、`Fatal`、`Emergency`、未定义引用或缺失数学；存在若干 `Underfull` vbox/hbox，属于两栏最后一页的松弛排版提示，未造成裁切。第 8 页已渲染并目视检查。
- 失败现场保留：v2 的 BibTeX/编译失败和 v3 的 10 页结果均保存在同一 `build/` 目录及日志中；v1 源码/PDF 保存在 `archive_v1/`。

## 科学边界

本次提升不改变三份科学标准的判定：声明范围内 one-call dynamic scope preflight 可以通过；完整 Benchmark/Baseline/机制标准仍为 HOLD。controlled stress 只支持 action-dependent evaluator-side correctness-drift recovery，不支持自然 provider lifecycle、通用实时收益或端到端 agent 能力上限。

## 正式 submission preflight

- `make submission` 于 2026-10-07 成功；natural 270/270 与 stress 150/150 回执审计均 verified、0 issues。
- 投稿 PDF：`paper/aamas2027/build/submission_20261007_084607/main.pdf`，9 页总计（正文 8 页、参考文献第 9 页），PDF SHA-256=`cd52a061362619b964ba66cbb993dbaa010a55c103529d696fe1bc5cbdcd77d1`。
- submission log SHA-256=`8aeb76f3e8737ac814eea1ac3d6fd4b19aee8bed157fc5acb6dbff7359034866`；最终日志无 Overfull、Undefined、Citation、Missing `$`、Fatal、Emergency。第 8 页已目视检查。
