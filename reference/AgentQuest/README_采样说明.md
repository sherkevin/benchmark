# AgentQuest — 采样说明【核心匹配：单 agent + progress rate 里程碑框架】

## 论文与来源
- 论文：*AgentQuest: A Modular Benchmark Framework to Measure Progress and Improve LLM Agents*（NEC，arXiv:2404.06411，NAACL 2024 Demo）
- 代码：https://github.com/nec-research/agentquest
- 本地采样下载通道：GitHub API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是（单 agent 解推理/搜索类任务）。
- 一次性输入：任务目标一次给定（交互式任务在环境内多轮，但无第二个 agent、无人类中途干预）。
- hard 中间 checkpoint：**是——progress rate 基于一组可程序化判定的 milestone，driver 用确定性逻辑计算距目标的进展**。

## Benchmark 是什么
一个可扩展的 benchmark 框架，统一封装多个任务（Mastermind、MasterMind、Cipher、Sudoku、GSM8K、ALFWorld、Lateral Thinking 等），核心贡献是提出**通用 progress rate 指标**：不只看成功/失败，而是持续度量 agent 离目标还有多远。

## checkpoint 机制（本项目关注点）
- 每个任务实现一个 `driver.py`，里面定义 milestone 与 progress 的**确定性计算**。以采样的 Mastermind 为例：目标是猜出 4 位密码，`data.json` 存了成百上千个待猜密码（如 `"5918","8607"...`）；progress = 已猜对的数字位数比例（可精确程序化计算），随每次猜测更新；
- progress rate 完全 hard：由 driver 里的规则/匹配逻辑算出，不依赖 LLM judge；
- 框架化设计让"milestone + progress"可复用到多个任务。

## 采样内容
- `mastermind/data.json`：Mastermind 全部谜题实例（4 位密码列表）。
- `mastermind/driver.py`：该任务的目标定义与 progress rate 计算逻辑原文（就是卡点判定器）。
- `mastermind/README.md` + 顶层 `REPO_README.md`：框架与任务说明。

## 对我们的参考价值
提供了**"progress rate 作为通用过程指标"的框架级实现参考**：每个任务用一个 driver 封装 hard 的进展计算。它与 AgentBoard 思想同源，但更强调"模块化、可插拔到任意任务"，对我们想做一套跨任务统一的过程度量框架有工程借鉴价值。局限：内置任务偏推理/游戏，真实工程任务较少。
