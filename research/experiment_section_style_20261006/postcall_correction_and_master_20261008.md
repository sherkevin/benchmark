# Post-call 证据修正与主版同步

日期：2026-10-08。目的：修复正文中的事实与区间错误，把次级实验用可复算结果表呈现，并维护唯一 paper-level 主版。上轮属于有实质进展：留下了修正源码、报告和效用区间；但源码未同步到主版，因此本轮继续收尾。

## 修正内容

1. 随机同覆盖率对照的 accepted-accuracy CI 是 `[+2.77,+9.29]` pp；`[+3.34,+9.72]` 属于 always-accept 对照。原 report、recheck 的错误版本单独保留。
2. 157 个分子已在先前 routing 工作中检查，本轮只能称已有留出 cohort 的 secondary analysis，不是另一次 untouched confirmation。阈值 fit-only 与“未读过测试结果”是两件事。
3. random 对照依赖完整 cohort 的接受数，属于 evaluator-side conditional null。五个 fit 共用 157 分子，不是五组独立测试样本。所有 CI 保留同分子/同 scaffold 的重复记录一起重采样。
4. 固定效用点的增益 +0.0338 现在有 paired scaffold CI `[+0.0088,+0.0609]`。收益随拒答代价变化，不把 accepted accuracy 的升高单独当作能力提升。

## 完成的证据工作

- `strict_audit_v3`：完整 receipt/evaluator 3,140+3,140 行、232 个原始 fit/summary 字段、split/source-input hash 校验通过；26 个测试通过。verifier 不导入 runner，拒绝截断、重复、NaN、标签/动作/调用数/状态篡改、泄漏、fit rate/split 变化及结果篡改。
- 原始 fit threshold coverage 经包含 ties 的重算与历史结果一致；最初以为它有错误的测试失败也保留，不能按预期写成真实 bug。
- `analysis_audit_v3`：重算原始点估计、两条 accepted-accuracy CI、两条 utility CI；保存完整 bootstrap draws、执行前配置、源码快照、环境与输入 hash。
- 原始 full_v1 缺少同期 source hash/运行前配置与流式日志。当前复算只证明给定 evidence 的可重算性，不补造历史 provenance。

## 正文与主版规则

主版始终是 `paper/aamas2027/main.pdf`，迭代留在 `paper/aamas2027/build/submission_*/`，旧主版归档到 `archive/promotions/`。源码更新后只有通过 8 body + references page 9、日志检查、数字绑定与页面检查的 build 才能提升。

本轮新建 `bind_postcall_results.py`：从审计结果生成正文的数值表与 CI 段，检查 raw 文件和独立 verifier 的绑定。`make submission` 在构建前后检查该块，防止再次把比较对象与 CI 抄串。新段落定义 utility，并给出固定策略下拒答代价的 break-even；删除重复的过程说明，为有意义的结果留位置。

## 构建留痕

- `submission_20261008_080828`：10 页，结论末尾 59 words 溢到第 9 页，未提升。
- `submission_20261008_081812`：编辑命令相对路径错误导致源码未改变；停止本人构建，保留现场。
- `submission_20261008_081854`：第一次渲染仍报 10 页；停止本人构建并继续删重复说明，未提升。
- 当前待验构建见本文件后续记录；没有通过前，主版仍是上一已发布版本。

## 三标准与下一项科学工作

这些修正提升 S11/S12、M7/M14 和 Benchmark v006 的 C4/R4 的可信度，但不自动补齐 M5/M8 的整体创新及 integrated agent gain，也不恢复 extension 的 E6。最终科学 gate 保持 HOLD。

独立方法审查指出最值得检验的是“第二个工具对当前证据的条件净价值”，先固定终局 verifier，只学习是否值得继续调用，再检查 delayed update 的附加收益。候选推导、真实负结果依据、近邻重叠、paid paired-feedback 的标签与停止规则已写入 `task/task49-pharma-benchmark-baseline-freeze/conditional_second_call_v1/design_review.md`。Cascade Routing 已覆盖多项相关能力，后续必须加入相同 delayed updater 的强对照，并检查 Online Cascade Learning 的反馈合同；此时尚不能宣称新机制已成立。

## 最终构建与提升完成

- `submission_20261008_082437` 完成三轮 LaTeX/BibTeX；PDF 共 9 页，References 第 9 页，正文恰好 8 页。natural/stress audit 分别 270/270、150/150 verified；post-call binding 构建前后通过。
- 第 6、7、8、9 页以 1.8× 渲染并实际检查，表格、公式、结论和引用无截断。p8 左/右栏正文底分别约 y=709.1/625.5 pt；没有把剩余正常留白说成排满每寸。
- 主版已提升为 `paper/aamas2027/main.pdf`，SHA `90deeeeda2bd15892a7d0d74df8cd1d99d12c8dfa4802c9e22a3b51494d9f1d9`；旧主版保存在 `archive/promotions/main_before_20261008T083109Z.pdf`，旧 provenance 同目录。
- 源码 SHA `429c6ca2a8f07b50433a7933a4e34579f1635bf54728b9f11defc8cc3df96486`。旧版 CI/数据状态的错误不再出现在主版；历史快照可供复核。
- LaTeX 预检没有 overfull/undefined/citation/missing-math/fatal；已知 template conditional warning 与 underfull 不计作 zero-warning。
- 可复用产物：严格 receipt verifier、真实数据 mutation tests、保存 draws 的统计重算器和论文数字绑定脚本。下一项科学工作按候选卡先做近邻/label 合同核对，再进行 development 条件调用价值测试，不把版面或审计通过等同于科学目标完成。
