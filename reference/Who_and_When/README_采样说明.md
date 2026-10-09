# Who&When — 采样说明

## 论文与来源
- 论文：*Which Agent Causes Task Failures and When? On Automated Failure Attribution of LLM Multi-Agent Systems*（arXiv:2505.00212，ICML 2025）
- 数据/代码仓库：https://github.com/ag2ai/Agents_Failure_Attribution（另有 HuggingFace `Kevin355/Who_and_When`）
- 本地采样下载通道：GitHub Contents API，2026-07 抓取

## Benchmark 是什么
**与"多智能体执行过程中出现偏差"最直接对口的数据集。** 收集 184 条多智能体系统（MAS）的**失败任务轨迹**（127 个由算法自动生成的 MAS + Magnetic-One 手工构建系统，任务来自 GAIA/AssistantBench），每条轨迹由人工细粒度标注：
- **Who**（`mistake_agent`）：哪个 agent 造成失败；
- **When**（`mistake_step`）：失败发生在哪一步（decisive error step）；
- **Why**（`mistake_reason`）：失败的自然语言解释。

## checkpoint 机制
它不预设 checkpoint，而是反向定义"**决定性错误步**"：对一条完整多轮 trace，标注出使任务不可能再成功的那一步。评测任务 = 给定 trace，模型能否定位 (agent, step)。论文发现即使最好的模型定位 step 的准确率也很低（<15%），说明"判断 trace 在哪偏了"本身就是当前的开放难题。

## 采样内容
- `Algorithm-Generated/`：3 个算法生成 MAS 的失败轨迹（`history` 为各 agent 逐步发言/动作，含 `mistake_agent`/`mistake_step`/`mistake_reason` 标注）。
  - 例：`1.json` — Excel 数据统计任务，标注 `mistake_agent: Excel_Expert`，`mistake_step: 0`，原因是代码未处理地址字段边缘情况。
- `Hand-Crafted/`：3 个手工构建系统（Magnetic-One）的失败轨迹，标注格式相同。
- `REPO_README.md`：官方说明。

## 对我们的参考价值
提供了"**过程偏差 ground truth**"的直接标注范式：(失败 agent, 失败步, 失败原因) 三元组。若我们的 Benchmark 要度量"执行过程有没有偏、偏在哪"，这是最接近的已有数据资产与任务形式（failure attribution）。
