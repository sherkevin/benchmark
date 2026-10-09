# SUPER — 采样说明【核心匹配：单 agent + 一次输入 + landmark hard checkpoint】

## 论文与来源
- 论文：*SUPER: Evaluating Agents on Setting Up and Executing Tasks from Research Repositories*（AI2，arXiv:2409.07440，EMNLP 2024）
- 代码：https://github.com/allenai/super-benchmark ｜ 数据：HF `allenai/super`
- 本地采样下载通道：ModelScope 镜像 `allenai/super` + GitHub API，2026-07 抓取

## 与需求的匹配度
- 单 agent：是（单个 LLM agent 在沙箱里配置并跑通研究仓库）。
- 一次性输入：是（任务 query 一次给出，agent 自主执行，无用户中途介入）。
- hard 中间 checkpoint：**是——每个任务标注 `landmarks`（里程碑），用正则匹配 agent 执行历史中的确定性输出**。

## Benchmark 是什么
考察 agent 能否从 GitHub 上"冷门"研究仓库把 ML/NLP 实验跑起来：Expert 45 题（专家手写）、Masked 152 题、AutoGen 602 题。任务如"在我的数据上训练 ColBERT 模型，报告最后一个 loss 值，格式 {"last_loss": 0.0}"。agent 要 clone 仓库、装依赖、改配置、跑训练、抽取结果。

## checkpoint 机制（本项目关注点）
每个任务两类判分信号：
- **landmarks（中间卡点）**：一组正则，对应执行过程中必然出现的关键输出。采样的 colbert 任务 landmarks 例：`"Some weights of ColBERT were not initialized..."`（模型正确加载的标志）、`"0 \\d+\\.\\d+"`（训练开始打印 loss 的标志）。评测器 `evaluate_checkpoints()` 逐条正则搜索 agent 的 observation 历史，命中率 = 过程分。
- **answer（终态）**：最终数值答案精确比对（`last_loss` 浮点值）。

判定完全 hard：正则匹配 + 数值比对，无 LLM judge。这与需求高度一致，且 landmark 的定义方式（"执行必经的确定性 stdout"）对我们设计 checkpoint 极有参考价值。

## 采样内容
- `expert_full.jsonl`：Expert 全 45 题（含 query / landmarks / answer / solution）。
- `sample_3cases.json`：前 3 题格式化。
- `evaluate_dataset.py`：官方判分脚本，含 `evaluate_checkpoints()` / `evaluate_entrypoint()` / `evaluate` 三个函数原文。
- `DATASET_README.md`：官方数据卡。

## 对我们的参考价值
**landmark = 与实现路径无关的"执行痕迹谓词"**，用正则捕获 stdout 里必然出现的关键行。相比 AgentBoard 的环境状态匹配，SUPER 证明了"从执行日志抽 checkpoint"在真实 coding/科研任务上可行，且完全 hard。是本项目 coding/科研方向的一号参考。
