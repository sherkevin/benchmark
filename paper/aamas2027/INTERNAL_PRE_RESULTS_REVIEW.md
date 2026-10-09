# INTERNAL PRE-RESULTS REVIEW

状态：内部工作稿，不是投稿包；2026-10-05 更新。

## 目的

这版稿子先把题目、单一故事线、最小可实现的方法规格、benchmark/baseline 合同和完整实验矩阵闭合。当前正文保留 8 页版式，摘要、主表、机制表和 one-step evidence 表都已经有固定位置；实验尚未完成，因此正文所有最终效果单元都保留为 `TBD`。已有 development receipt 不能直接回填，只能作为范围受限的负结果或任务难度证据保留在内部记录中。逐项回填规则见 `EXPERIMENT_RESULT_FILL_MAP.md`。

## 不得直接投稿的条件

- MIRA-Relay 仍处于候选方法规格，尚未通过 CPU state-to-action stop test、同 schedule 对照和独立 recovery。
- Benchmark+Baseline 当前为 `41/60 HOLD`；Gate A/B/C 为 `11/16`、`11/16`、`10/16 HOLD`。
- 多端点资格、provider lineage、完整 E1--E5 统一主表和冻结 confirmation 尚未闭合。
- 所有 `TBD` 必须由 raw JSONL、配置/输入/source hash、逐事件 recovery 和作者复核逐表回填；不能用开发诊断或预期值替换。
- `make submission` 保持 fail-closed，不能把本内部稿复制成投稿 artifact。

## 结果回填顺序

1. 完成 MIRA-Relay CPU stop test、NoProbe/ProbeNoUpdate/MIRA 同 schedule receipt 和独立 verifier。
2. 完成统一 baseline fairness/cost/unknown/refusal 合同，重建 E1--E5 coverage matrix。
3. 方法与 baseline 冻结后执行一次 confirmation；标签不得回流选 epoch、阈值、菜单或 claim。
4. 按 `EXPERIMENT_RESULT_FILL_MAP.md` 逐表、逐句将 TBD 与 raw receipt 对齐，并保留差异审计。
5. 最后用干净目录重编译，确认正文正好 8 页、没有未解析引用、没有 overfull box，再移除内部 banner 并运行投稿门禁。
