# ToolBench (ToolLLM) — 采样说明【专门评测工具调用能力：长链多工具 + Pass/Win Rate】

## 论文与来源
- 论文：*ToolLLM: Facilitating Large Language Models to Master 16000+ Real-world APIs*（OpenBMB，arXiv:2307.16789，ICLR 2024 Spotlight）
- 代码/评测器：https://github.com/OpenBMB/ToolBench ｜ ToolEval：`toolbench/tooleval`
- 本地采样下载通道：GitHub Contents API（gh 认证），2026-07 抓取（repo 内 `data_example/`）

## 与需求的匹配度（工具调用能力评测）
- 单 agent：是。
- 一次性输入：是（query + 候选 API 列表一次给全，agent 自主多步调用，无用户交互）。
- checkpoint 形态：**软——ToolEval 用 ChatGPT 机器评审**（Pass Rate + Win Rate），非 hard 断言。作为"专门测工具调用能力"的代表收录。

## Benchmark 是什么
覆盖 RapidAPI 16000+ 真实 API（49 类）的工具调用基准。指令按难度分三档：
- **G1（单工具）**、**G2（同类多工具）**、**G3（跨集合多工具）**——G2/G3 需在一个任务里连续选调多个 API。
- 每条 query 附一个候选 `api_list`（含各 API 名称、描述、必填/可选参数、方法），agent 要判断用哪些、按什么顺序、填什么参数，并串联结果。
- 解法采用 DFSDT（深度优先决策树）或 ReACT。

## 评分机制（本项目关注点：对照组 = LLM 评审）
ToolEval（仿 AlpacaEval）两个指标：
- **Pass Rate**：在有限 API 调用预算内成功完成指令的比例。
- **Win Rate（Preference）**：与参考答案（ChatGPT-ReACT）成对比较，由 ChatGPT 按预设标准判哪个更优，胜出比例即 Win Rate。
- 评审器与人工一致性：Pass Rate 87.1%、Win Rate 80.3%。
- 注意：**判定用 LLM 评审，非可执行 hard 断言**——这是它与 SUPER/τ-bench 的关键区别，但它直接考"工具选调对不对"，是工具调用能力的正面评测。

## 采样内容
- `G1_query.json` / `G2_query.json` / `G3_query.json`：三档难度的真实指令（含候选 `api_list`）。
- `example_answer_G2_10.json`：一条真实 DFSDT 解法树（多步工具调用轨迹）。
- 采样自 repo 的 `data_example/`；完整 16000+ API 与全量指令在 HF / Google Drive。

## 对我们的参考价值
"**给候选工具集 + 任务，直接考多步工具选调**"的代表；G2/G3 的长链多工具最贴长程。局限：评审依赖 LLM（非 hard），若要纳入 hard 体系需替换为可执行的工具调用正确性判据（如 BFCL 的 AST / 执行匹配）。
