# PaperBench — 采样说明【★重要参考：层次化 rubric 树 = 数百个中间 checkpoint】

## 论文与来源
- 论文：*PaperBench: Evaluating AI's Ability to Replicate AI Research*（OpenAI，arXiv:2504.01848，ICML 2025）
- 代码：https://github.com/openai/preparedness（project/paperbench）
- 本地采样下载通道：GitHub API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是（单 agent 从零复现一篇论文）。
- 一次性输入：是（给论文+说明，agent 自主复现，无交互）。
- hard 中间 checkpoint：**部分——checkpoint 是层次化 rubric 树（数百节点），但叶子的判定由 LLM judge 执行**。机制上是"最细粒度的过程分解"，判定层非纯 hard，需甄别。

## Benchmark 是什么
20 篇 ICML 2024 论文，要求 agent 从零复现（读论文、写代码、跑实验、复现结果）。这是极长程任务。核心贡献是**层次化评分 rubric**：把"复现一篇论文"递归分解成一棵任务树。

## checkpoint 机制（本项目重点关注）
采样的 rice 论文 rubric（`rice_rubric_full.json`，317KB）实测结构：
- **489 个节点的树**，其中 **361 个叶子节点**（最细的可判定 checkpoint），每个叶子是一条明确的 requirement；
- 每个节点有 `weight`（加权，1/2/3…）和 `task_category`，实测分布：**Code Development 178 个、Code Execution 170 个、Result Analysis 13 个**；
- 叶子 requirement 例（真实）："Any version of the (MuJoCo) Hopper environment has been correctly set up and can be initialized"（weight 3）；
- 评分：自底向上按 weight 加权汇总叶子得分 → 整篇 Replication Score（0–100%）。**这就是把长任务拆成数百个中间卡点、逐个判定再加权**。

关键甄别：叶子判定由 LLM judge（论文另做了 JudgeEval 验证 judge 可靠性）执行，**不是纯 hard**。但其中 "Code Execution" 类（能否成功跑起来）本质可执行化。

## 采样内容
- `rice_rubric_full.json`：完整 489 节点 rubric 树（就是 checkpoint 全集）。
- `rice_rubric_top2levels.json`：裁剪到前两层的结构预览（便于快速看树形）。
- `rice_meta/config.yaml`：论文任务配置。
- `REPO_README.md`：官方说明（含 rubric 设计与 JudgeEval）。

## 对我们的参考价值
**"把长程任务递归分解成加权 rubric 树、每个叶子是一个可判定 checkpoint"是过程分解的最高级范式**，粒度（数百节点）和加权机制都值得直接借鉴。对我们的启示：checkpoint 可组织成树、按重要性加权；但要把叶子判定从 LLM judge 替换为可执行断言，才符合"hard"要求（PaperBench 的 Code Execution 类天然可做到）。
