# DiscoveryWorld — 采样说明【科学发现全循环虚拟环境：scorecard 原生过程分】

## 论文与来源
- 论文：*DiscoveryWorld: A Virtual Environment for Developing and Evaluating Automated Scientific Discovery Agents*（AI2，NeurIPS 2024 D&B **Spotlight**，OpenReview cDYqckEt6d）
- 代码：https://github.com/allenai/discoveryworld
- 本地采样下载通道：GitHub Contents API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是。
- 一次性输入：是（任务描述给定后 agent 在环境内自主探索，无用户介入）。
- hard 中间 checkpoint：**是——每个任务自带 oracle scorecard：子目标清单逐项判 completed（环境状态判定），`scoreNormalized` 即 0–1 过程进展分**。

## Benchmark 是什么
首个覆盖**完整科学发现循环**（假设→实验→分析→结论）的虚拟环境：120 个任务、8 个主题（含材料学 topic：测未知材料性质并推断规律），agent 通过 API 在 2D 模拟世界中行动（对话/使用仪器/记笔记）。

## checkpoint 机制（本项目关注点）
scorecard 三层判分（本地采样 `example_scorecard.json` 为官方真实示例）：
- **任务完成**：`completedSuccessfully`（0/1）；
- **过程进展**：`scoreCard` = 子目标列表，逐项 `completed`/`score`（如"已与对方对话 1/1"→"到达对话树第 3 层 0/1"），汇总为 `scoreNormalized`（示例 = 0.5）——**这就是逐卡点的 progress rate**；
- **发现知识**：`criticalQuestions` 二元问答（对 agent 笔记判定，此层非纯 hard）。
关键设计：agent 只能看到任务描述与是否完成，**看不到细粒度 scorecard**——防止按卡点作弊。

## 采样内容
- `example_scorecard.json`：官方 scorecard 真实示例（Dialog 单元测试任务，含逐子目标得分与 scoreNormalized）。

## 对我们的参考价值
**"scorecard = checkpoint 调色板"的工程范本**：子目标由环境状态机判定（hard）、进展分归一化、卡点对 agent 隐藏——三条设计可直接搬进我们的材料长程 benchmark；其材料主题任务亦可改造复用。
