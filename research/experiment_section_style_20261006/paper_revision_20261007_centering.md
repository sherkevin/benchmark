# 六格结果表与正文 8 页复核

日期：2026-10-07。目的：把已核验的机制诊断变成正常论文结果表，减少重复说明，同时保持正文恰好8页。

## 修改与证据

- `main.tex` 的 Mechanism Interpretation 新增 Table 5：raw/corrected × Frozen/marginal/query 的自然与controlled accuracy。数据取自已独立验证的25,992次真实回放决策，没有填入预期数字。
- 用短段落说明分数逆变换、决策时分数中心化、相对对应Frozen的小增量和stress暴露差异；旧157分子confirmation及主方法没有改变。
- 缩短重复artifact说明，保留原始事件、独立重放和确认集边界。

## 最终构建

`paper/aamas2027/build/submission_20261007_043532/main.pdf`：总计9页，正文恰好8页；References标题在第9页，Conclusion在第8页。三遍LaTeX、BibTeX、420条旧确认流独立事件审计和 `make submission` 全部通过，退出码0。第7、8页渲染目视通过，六格表可读、无裁切；源码和结果hash见同名JSON。

此前正文8页版本 `submission_20261007_034632` 保留。本轮首次编辑误用了相对路径，源文件未改；随后误启动的旧源构建 `submission_20261007_043141` 已主动终止，现场和原因在 `paper_revision_20261007_centerring_edit_attempt01.json`。最终PDF检查第一次把References标题错误设为全大写，断言失败；改成独立标题的不区分大小写匹配后通过，记录在最终构建目录 `qa_attempt01.json`。这些都不是科学结果失败，未改数据或判据。

## 科学状态

OOF分数质量改善成立；corrected query相对marginal的自然/stress增量仅+0.508/+0.369pp，未达到事先1pp资源线。该结果用于定位问题，没有把新变体升为主方法或变更确认结果。三份完整科学标准仍HOLD，长程目标继续。
