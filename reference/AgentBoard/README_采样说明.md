# AgentBoard — 采样说明

## 论文与来源
- 论文：*AgentBoard: An Analytical Evaluation Board of Multi-turn LLM Agents*（arXiv:2401.13178，NeurIPS 2024 Datasets & Benchmarks，Oral）
- 数据：HuggingFace `hkust-nlp/agentboard`；本次经 ModelScope 镜像 `hkust-nlp/agentboard` 下载（完整包 data.tar.gz 1.4GB，本次按环境采样单文件）
- 代码仓库：https://github.com/hkust-nlp/AgentBoard
- 本地采样下载通道：ModelScope API，2026-07 抓取

## Benchmark 是什么
9 类环境（具身 AlfWorld/ScienceWorld/BabyAI、游戏 Jericho/PDDL、Web WebArena/WebShop、工具 tool-query/tool-operation）、1013 个多轮任务的统一评测板。核心主张：多轮 agent 评测不能只看最终 Success Rate，要看 **Progress Rate（进度率）**。

## checkpoint 机制
**每个任务人工标注 subgoal 序列**（子目标即 checkpoint）：
- 数据字段：`goal` + `subgoals`（如 PDDL 任务 goal="4 个球都搬到 roomb"，subgoals = 每个球各一条）；
- **Progress Rate = 已达成 subgoal 的最大匹配比例**，随交互过程持续更新（agent 每有实质进展，进度就 +1/n）；
- 论文证明 progress rate 比 0/1 成功率信号密集得多，可区分"完全不会"与"走了 80% 但最后失败"的模型。

## 采样内容
- `pddl/test.jsonl`、`alfworld/test.jsonl`、`tool-query/test.jsonl`：三类环境的任务定义与 subgoal 标注（每行一个任务：`id/goal/difficulty/subgoals`）。
- `DATASET_README.md`：官方数据卡。

## 对我们的参考价值
subgoal 注释 + progress rate 是"**里程碑式 checkpoint**"的标准学术范式：把长任务离散化为可判定的子目标序列，用达成比例度量过程健康度。与 TheAgentCompany 的 checkpoint 评分在思想上同源，粒度介于 step 级与结果级之间。
