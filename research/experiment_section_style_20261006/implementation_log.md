# 实验章节重排与当前构建记录

日期：2026-10-06

## 目的

把正文实验部分从“运行说明清单”改成顶会常用的短 setup、紧凑实验地图、主结果表和机制解释。所有数字仍来自已保存的 raw receipt；这次没有重跑策略，也没有改动确认输入。

## 采用的写法

- 用四行实验地图回答 Headroom、Natural adaptation、Drift recovery、Cost / validity。
- 用两张主表回答自然 fresh 与 controlled drift；表中只保留方法、分母、效果、切换和成本字段。
- 每张表后只保留“设置 → 数字 → 差值/区间 → 解释/边界”的短段落。
- Random、Best Fixed、Retrospective Best Fixed、Frozen、matched SGD、Greedy LinUCB、BayesianRouter、MIRA-Relay 按角色分组；audit-only 方法不进入主榜。
- 自然 fresh 的 null 结果和受控 evaluator-side correctness drift 分开写，避免把 controlled stress 外推为自然 provider drift。

## 证据绑定

- Natural v2：`experiments/logs/task49_provider_fresh_context_natural_20261006/v2_statehash_run3/`，270 streams、127,170 events，270/270 contract/replay；MIRA-Relay/Frozen 均 89.81%，paired delta 0.00 pp，scaffold CI `[-1.24,+1.40]`。
- Stress v3：`experiments/logs/task49_provider_fresh_drift_stress_20261006/v3_bayesian_statehash_run1/`，150 streams、70,650 events，150/150 contract/replay；MIRA-Relay 87.56%，Frozen 73.38%，paired delta `+14.18 pp`，CI `[+9.61,+19.03]`；BayesianRouter 80.28%，delta `+6.90 pp`，CI `[+3.50,+10.41]`。
- 逐 delay 量化审计：`research/experiment_section_style_20261006/quantitative_audit/quantitative_audit_report.md`。自然 delta 为 `-0.13,-0.13,+0.25` pp；stress delta 为 `+13.63,+14.52,+14.39` pp（D=0,1,4）。
- 成本审计为 CPU policy-operation p95，不是端到端服务延迟；MIRA-Relay p95 为 2.813 μs，硬件和计时范围写在表题与审计中。

## 图和投稿合规

当前正文使用 Figure 1 v08 与程序绘制的 Figure 2 v13。AAMAS 规则对生成式 AI 图有额外限制，因此 v13 是提交构建中的正式版本；AI 候选保留在 archive/review 目录，不作为投稿图。

## 当前 PDF

- 源码 SHA-256：`55eb99725c03b1b87b5b32056b663787ba65d5a3a85f82636ac07d2564fff513`
- PDF：`paper/aamas2027/build/clean_submission_20261006_matrix_rewrite.pdf`
- PDF SHA-256：`05a5a4173e14417f83ae43ef4e5069b4e09736a09c58be1ad34d7f3d9b2af346`
- 总页数 9；正文 8 页，参考文献从第 9 页开始。
- 最终日志没有 `Overfull`、`Undefined`、`Missing $`、`Fatal` 或 `Emergency`；有模板常见的 `Underfull \vbox` 提示。
- 已目视检查第 5、6、7、8 页：图、矩阵和结果表可读，表格没有截断，结果表先于解释段落出现。

## 仍未通过的科学 gate

这次排版不能替代科学验收。三份标准下仍需补：最近方法的统一合同矩阵（PILOT、GraphRouter、RouterDC、RouterBench 等）、所有 adapted baseline 的统一 state/cost/unknown/refusal 字段，以及 E3 中 independent residual/interface necessity 的更强反事实证据。当前 Stage-B 只对声明的 one-call dynamic scope 通过，不能写成全文科学 gate 已打开。
