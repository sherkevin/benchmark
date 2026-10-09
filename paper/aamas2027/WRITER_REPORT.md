# AAMAS 2027 paper writer report

日期：2026-10-06
状态：声明范围内的投稿候选稿已完成；正文严格为 8 页，仍需作者最终双盲和官网复核。

## 当前论文

- 题目：**After the Decision: Learning to Adapt from Delayed Outcomes**。
- 方法名：MIRA-Relay。正文不使用 JEV 或旧别名作为方法名。
- 主任务：公开 TDC BBB truth、四个 checkpoint-bound fresh providers、一次调用、selected-only delayed feedback，延迟 $D\in\{0,1,4\}$。
- 论文范围：调用前 provider 选择和释放后更新。post-call verifier/refusal、hERG/AMES、自然 lifecycle drift、完整 33-provider model-fresh 不属于当前主张。

## 主结果

- 自然 fresh confirmation：157 个 scaffold-disjoint BBB molecules，270 streams。Frozen、MIRA-Relative、MIRA-Context 均为 89.81%；MIRA-Context 相对 Frozen 为 0.00 pp，95% scaffold CI `[-1.24,+1.40]`，有 82 rescued 和 82 harmed。
- 受控 correctness-drift stress：150 streams（其中新增 30 条 BayesianRouter comparator）。MIRA-Context 为 87.56%，Frozen 为 73.38%，提升 14.18 pp，95% scaffold CI `[+9.61,+19.03]`；BayesianRouter 为 80.28%，相对 Frozen `+6.90 pp`，CI `[+3.50,+10.41]`。这是预登记 evaluator-side label flip 的机制压力，不是自然部署漂移。
- 自然 270/270、stress 150/150 通过 contract/replay verification；状态中的 `fit_rates`、`successes`、`counts`、`prior_count` 已纳入 hash。
- 论文中的 evaluator-only 控制统一称为 **Retrospective Best Fixed**，不称逐分子 Oracle，也不列入可部署排名。

## 论文结构与三份标准

| 部分 | 当前检查重点 |
|---|---|
| Abstract / Introduction | 正常返回不等于可靠；信息边界、delayed selected-only feedback 和 MIRA-Relay 的切口一致 |
| Problem / Method | selector、provider call、release ledger、provider-local residual 和 verifier extension 接口分开 |
| Benchmark / Baselines | TDC truth 与项目 routing protocol 分层；Random、Best Fixed、Frozen、Matched SGD、Greedy LinUCB、Bayesian Router、MIRA 和 evaluator-only control 角色明确 |
| Experiments | 自然 fresh null 与 controlled stress 分开；E3/E4a 明确是同一 stress 的机制视图和结果视图 |
| Discussion / Artifact | 说明 family correlation、成本、失败/unknown 语义和 scope limits，不外推到未验证端点或自然漂移 |

## 实验和证据入口

- 科学门：`task/task49-pharma-benchmark-baseline-freeze/stage_b_scientific_submission_gate_v3/`，状态为 `PASS_FOR_DECLARED_ONE_CALL_DYNAMIC_SCOPE`。
- 自然 receipt：`experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/`。
- stress receipt：`experiments/logs/task49_provider_fresh_drift_stress_20261006/v2_statehash_run2/`。
- BayesianRouter stress receipt：`experiments/logs/task49_provider_fresh_drift_stress_20261006/v3_bayesian_statehash_run1/`；聚合：`task/task49-pharma-benchmark-baseline-freeze/provider_fresh_drift_stress_v2_bayesian/`。
- 聚合报告：`task/task49-pharma-benchmark-baseline-freeze/provider_fresh_context_natural_v1/` 和 `provider_fresh_drift_stress_v1/`。
- 图版本：Figure 1 v08、Figure 2 v13；历史版本保留，不覆盖。

## 当前 PDF

- 文件：`build/clean_submission_20261006_bayesian.pdf`，来自干净提交 `07d88e20`。
- 页数：9 页总计；正文 8 页，参考文献第 9 页。
- SHA-256：`458f804f8750209e9377429b8cf14ad8deba2c94214759eba6ee4a7bf554c04a`。
- 干净日志无 `Overfull`、`Undefined`、`Missing $`、fatal 或 emergency；第 2、5、6、8 页已目视检查。

此前 `c2656260` 的干净构建仍保留作历史证据；当前以 `07d88e20` 的 Bayesian stress 版本为准。

## 投稿前剩余事项

1. 用提交 commit 在干净目录重新构建一次，并核对页数、匿名信息、引用和官方页面最新要求。
2. 作者最终检查双盲、自引、补充材料和标题摘要。
3. 维持 claim 边界：自然菜单结果是 null；正结果只支持声明的 controlled correctness drift 机制。

这份报告记录当前版本，不把历史占位符、开发诊断或未完成扩展当成投稿结果。

## 2026-10-06 top-conference experiment-matrix rewrite

- 参考 ICLR 2024、ICCV 2023、NeurIPS 2024 的公开 PDF 结构审计后，正文实验部分改为短 setup、四行实验地图、两张主结果表和短机制解释；运行状态、哈希和越界内容移出正文。
- 当前源码 SHA-256=`55eb99725c03b1b87b5b32056b663787ba65d5a3a85f82636ac07d2564fff513`。
- 当前 PDF：`build/clean_submission_20261006_matrix_rewrite.pdf`，总 9 页，正文 8 页，参考文献第 9 页；SHA-256=`05a5a4173e14417f83ae43ef4e5069b4e09736a09c58be1ad34d7f3d9b2af346`。
- 当前正文表格直接使用最新自然 v2 与 stress v3 审计数字；没有新增未运行结果。
- 第 5、6、7、8 页已目视检查；结果表在解释段落前，图表没有截断。
- 科学 gate 仍只在声明的 one-call dynamic scope 内通过；最近方法合同、统一成本字段和更强机制反事实仍是后续工作。

## 2026-10-07 投稿 preflight

- `make submission` 已通过：候选 PDF 为 `build/submission_20261006_181518/main.pdf`，9 页总计，正文 8 页；PDF SHA-256=`abf88071fe9e829d1207d354513396533ec8c1379e2fa9eb9001193aac097394`。
- 源码 `main.tex` SHA-256=`ee5264e8819f1f3f2e6c06292105cc3d1c3837ef0f46fe3958d2d59ce81c3503`；日志无 `Overfull`、未定义引用、缺失数学分隔符或 fatal/emergency 错误。
- 独立 event-level receipt checker 已验证 natural 270 streams 和 stress 150 streams，并通过 9 个 CPU mutation/smoke tests。当前证据口径是 opaque hash continuity；不声称旧回执已完成 learned-state 独立重算。

### 2026-10-07 clean-tree final build

- 从提交 `dff33da9` 的 Git archive 完成 BibTeX 与三轮 LaTeX：`build/clean_submission_final/main.pdf`，9 页总计，正文正好 8 页。
- PDF SHA-256=`663d9fcdf779c6043b8c9b7883bf1887f2bba8fff6a556714e57f0fb0c08205f`；日志 SHA-256=`c03a6d0645317fc663fa42d7eec4b4a9b3aaaa5b4163edf76a1694bef376e212`。
- 第 2、3、5、8 页已目视检查；没有 overfull、undefined citation、missing math、fatal 或 emergency diagnostics。
