# RE-Bench — 采样说明【★核心匹配：单 agent + 连续 scoring function 随时可测的过程分】

## 论文与来源
- 论文：*RE-Bench: Evaluating Frontier AI R&D Capabilities of Language Model Agents against Human Experts*（METR，arXiv:2411.15114）
- 代码/任务：https://github.com/METR/ai-rd-tasks ｜ 官方 blog：metr.org/blog/2024-11-22
- 本地采样下载通道：GitHub API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是（单 agent 做 ML 研究工程）。
- 一次性输入：是（任务描述+起始代码一次给定，agent 在环境内自主迭代）。
- hard 中间 checkpoint：**是——每个环境自带一个可执行的 scoring function，agent 随时可调用，返回连续分数**。

## Benchmark 是什么
7 个开放式 ML 研究工程环境（如 optimize LLM foundry 训练速度、triton 写 cumsum 核、restricted MLM、小规模 scaling law），每个都是"给定起点、优化某个可量化目标"的长程任务。核心是与人类专家在相同时限下的表现对比。

## checkpoint 机制（本项目关注点）
RE-Bench 的判分哲学最贴合"过程量评测"：
- **每个环境定义一个 scoring function**（论文原文："A scoring function, which defines the goal of the environment, and **can be run by the agent at any time**"）——不是终态 0/1，而是一个**连续分数**（如训练损失、运行速度、准确率）；
- agent 在长程优化过程中可**反复调用 scoring 查看当前进展**，评测取其达到的最佳分数；
- 分数经过归一化（相对 starting solution=0、human expert reference=1），可刻画"走到哪个水平"；
- 完全 hard：scoring function 是可执行代码，无 LLM。

采样的 `ai_rd_triton_cumsum`：写高效 Triton GPU 核实现前缀和，scoring = 正确性约束下的运行速度。`manifest.yaml` 定义环境，`build_steps.json` 定义构建。

## 采样内容
- `example_ai_rd_triton_cumsum/`：一个完整环境——`ai_rd_triton_cumsum.py`（含 scoring 逻辑）、`README.md`（任务说明）、`manifest.yaml`、`build_steps.json`。
- `REPO_README.md`：官方说明（7 个环境总览与评测协议）。

## 对我们的参考价值
**"随时可调用的连续 scoring function"是 checkpoint 的一种高级形态**——把"过程有没有偏"量化成"当前分数离目标多远"，且 agent 自己也能看到（可用于过程引导）。对我们做"过程量而非终答"的评测极具启发：卡点不必是离散通过/失败，可以是连续进展信号。METR 出品，与其时间跨度指标配套。
