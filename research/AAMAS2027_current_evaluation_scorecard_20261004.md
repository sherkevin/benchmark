# AAMAS 2027 当前验收核对表增量（2026-10-04）

性质：内部开发审查增量；不是 AAMAS 官方评分，也不保证录用。完整评分沿用 `research/AAMAS2027_current_evaluation_scorecard_20260930.md` 和三份生效评价标准。

## 当前分数与状态

| 项目 | 当前分数 | 状态 |
|---|---:|---|
| 故事线与创新 | 47.5/100 | FAIL |
| 方法论 | 37/60 | HOLD/FAIL |
| Benchmark + Baseline | 41/60 | HOLD |
| Gate A：Benchmark | 11/16 | HOLD |
| Gate B：Baseline | 11/16 | HOLD |
| Gate C：矩阵与结果 | 10/16 | HOLD |

本次只吸收一条已完成、已独立复核的候选机制 gate；没有用负结果给分，也没有降低目标 claim。三门分数保持不变，confirmation 继续关闭。

## 本次新增证据

Provider-Graph Relay v1 针对 80% singleton-family update 的覆盖瓶颈，按预注册合同完成 fold0/seed0 CPU gate：4 methods × 3 delays × 145 molecules，12 streams、5,220 raw events。独立 verifier 从冻结 provider cell 重算 selected output、reward、反馈时钟、state hash、fallback invariant 和 paired metrics，状态为 `verified`。

Graph Relay 相对 MIRA 的 molecule-paired `(rescued/harmed/net)`：

| delay | rescued | harmed | net | action changes |
|---:|---:|---:|---:|---:|
| 0 | 1 | 3 | -2 | 28 |
| 1 | 1 | 3 | -2 | 26 |
| 4 | 4 | 4 | 0 | 32 |

相对 degree-matched random graph 的 net 分别为 `-1/-1/-1`。因此图关系确实造成了动作变化，但没有在任一 delay 产生正净效用，触发预登记停止条件。该候选不扩展 fold1--4、seed1、A800 或 confirmation，降为已核验的负机制诊断/对照。

## 对三门的影响

- **Gate A：** 不变。Graph Relay 不提供 hERG/AMES 真值、provider training membership 或 confirmation lineage，因此不增加 benchmark 资格分。
- **Gate B：** 不变。它增加一个有完整回放合同的候选/对照，但不能替代严格 baseline 的 feature、warm-start、likelihood、预算和成本公平合同，也不能把图 relay写成现代 baseline 的正向结果。
- **Gate C：** 不变。5,220 条事件已经可复核，但只是一条 fold0 mechanism gate；没有跨 fold、跨 seed 的不确定性，也没有补齐 E1--E5、dynamic scope、unknown/refusal 和冻结 confirmation。

## Replay-SGD 资格审计补充

历史 Replay-SGD 收据已完成只读 provenance 审计：旧 BT `score.weight`、64 维 feature、输入
哈希和独立 recovery 均有证据，但旧 prior 与当前 strict common-direct-prior 不同，且旧收据
没有 state bytes。因此矩阵把 Replay 从 `blocked` 收敛为 `separate_control`，保留历史
paired control 价值，不纳入 strict 主表。该角色收敛不增加任何分数，三门状态保持不变。

Adapted baseline 的 raw field audit 也已完成：四条 receipt 的 event counts、state hash、
runtime status 和 failure feedback 齐全，state bytes 与逐事件 unknown/refusal reason 仍缺。
因此只提高字段定位性，不把 adapted 行提升为 strict baseline，分数保持不变。

Benchmark 端点资格也已收敛成机器矩阵：BBB 是 `development_primary`，AMES 是
`appendix_candidate`，hERG 是 `blocked_semantics`。这一步明确了当前主 benchmark 边界，
没有把低覆盖 AMES 或语义未对齐 hERG 写成已通过的多端点主结果；Gate A 分数不变。

## 当前最重要的结论

当前最明确的失败模式不是“没有关系信息”，而是“关系信息改变了 action，却没有稳定提高 utility”。静态 fit-only correctness graph 不能保证 delayed release 时 recipient residual 与 selected residual 同向，也不能保证排序变化跨过 margin 后仍然选对。后续任何新机制都必须在预登记中说明如何避开这个失败模式，否则不新增回放。

## 证据入口

- 结果报告：`task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/provider_graph_relay_fold0_v1_report.md`
- 候选卡：`task/task49-pharma-benchmark-baseline-freeze/mira_relay_v1/provider_graph_relay_v1_candidate.md`
- 独立验收：`experiments/logs/task49_provider_graph_relay_fold0_20261004/receipt_v3/verification_v2.json`
- 原始事件哈希：`experiments/logs/task49_provider_graph_relay_fold0_20261004/receipt_v3/raw_manifest_v1.json`

## 2026-10-04 新增审计结论

### Provider lineage

新增 `task/task49-pharma-benchmark-baseline-freeze/provider_lineage_audit_v1/`，对 BBB 33 个
具体 provider 逐一审计来源、回放身份、训练成员和 checkpoint 绑定。结果为 G3=1、G2=30、
G1=2，model-fresh confirmation 资格为 0。24 个 B3clf 和 3 个 DeepPurpose 分别归入共享
family/source，不能按动作数当成独立训练来源。该结果明确了 Benchmark 的 claim boundary，
不增加 Gate A 分数。

### E1--E5 coverage

新增 `task/task49-pharma-benchmark-baseline-freeze/e1_e5_coverage_audit_v1/`，将当前收据与
三份生效标准逐项对齐。E1--E5 均为 `partial`，E6 为 `missing`：统一控制收据、分层 E2
主表、普通任务机制消融、lifecycle 事件协议、unknown/refusal/cost 主表仍未闭合。下一步
顺序固定为 E5 → E2 → E1 → E3 → E4；不跨 scope 填数字，不在 E1--E5 闭合前打开确认集。

### 论文构建

当前源码已在 `paper/aamas2027/build/verify_current_20261004/` 单进程构建出可解析的 7 页
PDF；源码、PDF、预览和警告见 `task/task49-pharma-benchmark-baseline-freeze/
paper_format_review_v1/build_provenance.json`。这修复了构建产物一致性问题，不改变三门分数：
故事线 `47.5/100 FAIL`、方法 `37/60 HOLD/FAIL`、Benchmark+Baseline `41/60 HOLD`，Gate
A/B/C=`11/16`、`11/16`、`10/16 HOLD`。
