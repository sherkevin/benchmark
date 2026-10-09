# ADR-0014：主 benchmark 的来源与外部 benchmark 的分工

日期：2026-09-24

状态：Accepted（benchmark 组织原则）；PTRB-v0 的正式数据发布、版本和最终题量仍待完成。

## 背景

Task36 已有真实医药工具结果、具体模型回放和信息隔离接口，但它本身是回放环境与工程资产，不是社区已有的公开 benchmark。通用工具调用 benchmark 通常没有“工具正常返回但结果可能错误”的独立科学真值，不能直接回答本项目的核心问题。

## 决定

1. 当前主 benchmark 使用工作名 **PTRB-v0（Pharma Tool Reliability Benchmark）**，明确标为“本项目拟构建的领域 benchmark 草案”。它复用 Task36 的真实结果和回放环境，由本项目补充 episode、信息条件、数据切分、延迟反馈、验证/拒答动作和评分规则。
2. PTRB-v0 只有在数据资格、真值审查、版本清单、可复现实验和独立复核完成后，才可以在论文中作为公开 benchmark 交付物表述；在此之前不称为现成标准或社区 benchmark。
3. `tau2-bench`、Inspect AI、jev-benchmarks、OBP、ASlib 和 LLMRouterBench 分别承担交互运行、日志、概率评估、OPE 或跨域迁移角色。它们的结果不与 PTRB-v0 合并成单一排行榜，也不替代医药真值。
4. 论文必须单独报告三类来源：复用的真实数据/回放环境、我们新增的 benchmark 协议、外部 benchmark 的迁移或 sanity-check 结果。

## 影响

- 主问题与数据真值保持一致，benchmark 的设计责任和新增贡献透明。
- 需要额外完成数据发布、版本锁定、独立复核和跨域验证，不能只依赖已有 Task36 文件宣称 benchmark 已完成。
- baseline 可以先在回放环境中适配；PTRB-v0 的公开状态不影响接口合同开发，但影响论文主张强度。

## 相关文件

- [开源复用与集成路线](../开源复用与集成路线_动态Jev.md)
- [Task46 报告](../../task/task46-open-source-composition/report.md)
- [ADR-0013：开源优先拼装](0013-open-source-first-composition.md)
