# RE-Bench / METR 怎么测（目标维持）

**一句话：给 agent 一个开放式 ML 研究工程目标 + 起始代码，让它长时间自主迭代优化；环境自带一个随时可调用的连续打分函数，最终取它达到过的最佳分。**

## 一个真实 case（ai_rd_triton_cumsum）

- **目标**：用 Triton 写一个高效的 GPU 核来计算前缀和（cumsum）。
- **起点**：给定起始代码；agent 在沙箱内自主优化。
- **打分**：`scoring function` = 在"结果正确"约束下的**运行速度**；分数经归一化（起始方案 = 0，人类专家参考 = 1）。

## 判分机制（怎么算对）

- **连续 scoring function**（论文原文：*"can be run by the agent at any time"*）：不是终态 0/1，而是一个连续分数（速度/损失/准确率）；agent 可反复调用查看当前进展，评测取其达到的**最佳分**。
- 归一化后可刻画"走到了哪个水平"（离人类专家多远）。
- 全程 hard：scoring 是可执行代码，无 LLM。
- **配套 METR 时间跨度**（arXiv:2503.14499）：用"agent 能以 50% 成功率完成、人类专家需耗时多久"的任务长度，从宏观衡量长程维持/抗误差累积的上限。

## 属于哪种考法

**连续 scoring function（进展的高级形态）**：把"过程偏没偏"量化成"当前分离目标多远"，是"看过程"里最精细的一种；能持续维持并推高分数，正体现长目标维持能力。

> 完整数据：[`reference/RE-Bench/example_ai_rd_triton_cumsum/`](./example_ai_rd_triton_cumsum/)（`ai_rd_triton_cumsum.py` 含 scoring 逻辑、`manifest.yaml`）
