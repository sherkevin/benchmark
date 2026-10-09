# Commit0 — 采样说明【核心匹配：单 agent + 分单元测试渐进 hard 卡点】

## 论文与来源
- 论文：*Commit0: Library Generation from Scratch*（arXiv:2412.01769，ICLR 2025）
- 代码/数据：https://github.com/commit-0/commit0 ｜ 官网 https://commit-0.github.io/
- 本地采样下载通道：GitHub API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是（单 agent 从零写库）。
- 一次性输入：基本是（给定完整 spec 文档 + 单元测试骨架，agent 自主实现；属"规格一次给全"型长程任务）。
- hard 中间 checkpoint：**是——库被拆成多个模块/函数，每个都有独立单元测试，通过数即渐进式卡点**。

## Benchmark 是什么
从 commit 0 开始造 Python 库：给 agent 一份 spec（含接口签名与文档字符串）和一整套隐藏单元测试，要求它把 57 个真实核心 Python 库（如 minitorch、jinja、requests 类）从空实现补全到通过测试。单库常含上百个待实现函数，是典型的长程编码。

## checkpoint 机制（本项目关注点）
- 每个库自带**分层单元测试套件**：agent 每实现一部分，就能跑对应测试，通过数随实现推进单调上升；
- 核心指标是**测试通过率**（pass rate over unit tests），而非 0/1 的"整库是否完成"——这正是渐进式 hard 卡点：每个单元测试就是一个卡点，精确刻画"实现到哪、哪里开始不达标"；
- 判定完全 hard：pytest 执行，无 LLM。`harness/evaluate.py`（采样）是判分入口，`get_pytest_ids.py`/`run_pytest_ids.py` 负责按测试 ID 逐个跑。

## 采样内容
- `harness_evaluate.py`：官方评测 harness 入口原文（如何编排分测试执行与打分）。
- `configs_base.yaml`：benchmark 配置（库清单、split 等）。
- `REPO_README.md`：官方说明（含 57 库列表与评测协议）。
- 注：各库源码与测试体量大（1000+ 文件），此处采样机制文件而非全部库，库清单见 README。

## 对我们的参考价值
"**一套单元测试 = 一批天然 hard 卡点**"的最纯粹形态，且卡点数量大（单库上百）、粒度细、完全自动。相比 SWE-bench 只看终态全过，Commit0 的测试通过率天然给出过程分。是"从零构建"类长程编码卡点的代表。
