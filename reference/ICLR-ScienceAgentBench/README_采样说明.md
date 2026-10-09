# ScienceAgentBench — 采样说明【数据驱动科学发现，rubric 多维评分】

## 论文与来源
- 论文：*ScienceAgentBench: Toward Rigorous Assessment of Language Agents for Data-Driven Scientific Discovery*（OSU，arXiv:2410.05080，ICLR 2025）
- 代码：https://github.com/OSU-NLP-Group/ScienceAgentBench ｜ 数据 HF `osunlp/ScienceAgentBench`
- 本地采样下载通道：GitHub API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是。
- 一次性输入：是（给任务指令+数据集信息，agent 产出分析代码，无交互）。
- hard 中间 checkpoint：**部分——分多维度评分，其中 CBS（可执行成功率）等是 hard，rubric 维度部分依赖 LLM/规则混合**。

## Benchmark 是什么
102 个数据驱动科学发现任务，来自 4 个学科（生物信息学、计算化学、地理信息、心理神经）的真实研究工作流。agent 要写代码完成数据分析，产出图表/结果文件。

## checkpoint / 评分机制
多维评分（见采样的 `grading.py` / `compute_scores.py` / `calculate_metrics.py`）：
- **VER / Valid Execution Rate**：产出代码能否成功执行（hard，可执行）；
- **SR / Success Rate**：结果是否达到任务目标（对照 gold 输出，部分 hard）；
- **CBS / Code-Based Score**：基于 rubric 对代码质量分维度打分；
- 分阶段评分体现了"过程分解"思想，但整体粒度到"任务级多维"，非细粒度逐步卡点。

## 采样内容
- `grading.py`：评分核心逻辑（多维度判定）。
- `compute_scores.py` / `calculate_metrics.py`：指标汇总脚本。
- `REPO_README.md`：官方说明。
- 注：任务与数据集在 HF（gated），此处采样评分机制文件。

## 对我们的参考价值
科学发现领域"多维度评分（可执行成功率 + 结果正确性 + 代码质量）"的代表。其 VER（可执行成功率）是 hard 的、可作为中间卡点；对我们设计"科学任务的分维度 hard 卡点"有借鉴。与 SciCode（纯子问题测试）互补：ScienceAgentBench 覆盖更真实的完整分析工作流，但卡点粒度更粗。
