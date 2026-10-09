# DevBench — 采样说明【匹配：单 agent + 软件开发生命周期分阶段 hard 卡点】

## 论文与来源
- 论文：*DevBench: A Comprehensive Benchmark for Software Development*（arXiv:2403.08604）
- 代码/数据：https://github.com/open-compass/DevBench
- 本地采样下载通道：GitHub API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是（单 agent 走完整开发流程；仓库也含多智能体 baseline，但任务本身不要求多 agent）。
- 一次性输入：是（给定 PRD/设计文档等规格，agent 自主完成各阶段产物）。
- hard 中间 checkpoint：**部分是——阶段产物用可执行方式校验（编译通过、单元测试、集成测试、覆盖率），是显式的阶段性卡点**。

## Benchmark 是什么
按软件开发生命周期（SDLC）分阶段评测：software design → environment setup → implementation → acceptance testing → unit testing。覆盖 Python/C++/Java/JS 多语言、22 个真实仓库级任务。每个任务提供 PRD、UML、架构设计等规格文档。

## checkpoint 机制（本项目关注点）
**阶段 = 天然中间卡点，且大部分阶段 hard 判定**：
- environment setup：环境能否成功搭建（可执行判定）；
- implementation：代码能否编译/运行（可执行）；
- unit/acceptance testing：单元测试与验收测试通过率（可执行，hard）；
- 覆盖率等量化指标。
少数阶段（如 design 文档质量）用 LLM/相似度辅助，但核心的实现与测试阶段是 hard 的。整体是"分阶段渐进卡点"的工程化实现。

## 采样内容
- `example_area_calculation/`：一个 C++ 任务的完整规格——`PRD.md`（需求）、`UML_class.md`、`architecture_design.md`（设计）、`repo_config.json`（仓库结构与各阶段测试配置）。
- `benchmark_data_README.md`：数据组织与各阶段评测说明。
- `REPO_README.md`：官方说明。

## 对我们的参考价值
展示了**把长任务按 SDLC 阶段切成卡点**的思路：每个阶段有明确的可执行验收标准（编译/测试/覆盖率）。对我们设计"阶段性 checkpoint + 每阶段配 hard 验收"有直接借鉴。注意甄别：design 类阶段并非完全 hard，采用时应只取可执行验收的阶段作为硬卡点。
