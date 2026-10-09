# Task47：Agent benchmark 与 baseline 发表范式调研

## 目标

回答两个问题：

1. 近期 Agent 论文如何用 benchmark、baseline、evaluator 和诊断实验支撑发表；
2. PTRB-v0 和 B0–B8 当前是否已达到可发表的 benchmark/baseline 标准。

## 交付

- [x] 代表性 benchmark、方法论文和 routing 工作调研。
- [x] ToolBench-X 最新直接邻域核查。
- [x] PTRB-v0 与 B0–B8 的现状审计。
- [x] 主论文定位、外部 sanity benchmark 和 Task47 关闭顺序。

## 结论

我们已有真实医药回放资产、benchmark 协议草案和 baseline 候选，但还没有公开可复现的 benchmark 和已运行冻结的 baseline 套件。论文应先以动态 Jev 在线适应与 Agent 可靠调用为方法主张，PTRB-v0 作为 project-defined domain benchmark；正式把 benchmark 列为贡献要等 evaluator、split、发布包和独立复核完成。

## 主要产物

- `../../research/Agent_Benchmark_Baseline_Publication_Survey_20260924.md`
- `sources.jsonl`
- `verification.json`

## 未完成

需要把报告中的 Task47-01 至 Task47-06 转成可执行工单，先完成合同与静态 baseline，再申请符合资格的最小科学批次。
