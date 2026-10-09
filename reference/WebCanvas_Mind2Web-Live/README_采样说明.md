# WebCanvas / Mind2Web-Live — 采样说明

## 论文与来源
- 论文：*WebCanvas: Benchmarking Web Agents in Online Environments*（arXiv:2406.12373，ICML 2024 Workshop）
- 代码仓库：https://github.com/iMeanAI/WebCanvas
- 数据：Mind2Web-Live（HuggingFace `iMeanAI/Mind2Web-Live`；本次因 HF 直连受限，从 GitHub 仓库自带的 `data/example/mind2web-live_test_20241024.json` 抓取，内容为同一测试集）
- 本地采样下载通道：GitHub Contents API，2026-07 抓取

## Benchmark 是什么
真实在线网站上的 Web Agent 任务（Mind2Web 的"活"版本），542 个任务、2439 个中间评估状态。因为真实网站随时变化，无法用固定的完整动作序列做对比，所以提出 **key node（关键节点）** 评估。

## checkpoint 机制
**key node = 任务完成过程中不可绕过的关键中间状态/动作**（论文原文："critical intermediate actions or states necessary for task completion"）：
- 每个任务的 `evaluation` 字段是一串 key node 匹配器，按序验证；
- 匹配函数包括 `url_included_match` / `url_exactly_match` / `element_value_exactly_match` / `element_path_exactly_match` 等（见仓库 `evaluate/step_score.py`）；
- 指标分两层：**step score**（命中了多少个 key node，即过程分）与 **task success**（所有 key node 全部命中）。

## 采样内容
- `mind2web-live_test_20241024_full.json`：完整测试集（179KB，含全部任务与 key node 标注）。
- `sample_3cases.json`：抽取前 3 个 case。例：任务"Find the store location ... set as home store on gamestop"，`reference_task_length: 8`，key nodes = [到达 gamestop 域名（URL 包含匹配）、URL query `store=2630` 精确匹配]。
- `example_130.json`：官方单任务全轨迹示例（含每步动作记录）。

## 对我们的参考价值
key node 是"**与实现路径无关**的中间状态谓词"：不管 Agent 走哪条路径，只要经过这些必经状态就算过程正确。这正是"判断 trace 有没有偏"的一种可操作定义，且对动态环境鲁棒。
