# TravelPlanner — 采样说明【核心匹配：单 agent + hard 约束校验】

## 论文与来源
- 论文：*TravelPlanner: A Benchmark for Real-World Planning with Language Agents*（OSU，arXiv:2402.01622，ICML 2024 Spotlight）
- 代码：https://github.com/OSU-NLP-Group/TravelPlanner ｜ 数据：HF `osunlp/TravelPlanner`
- 本地采样下载通道：GitHub API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是（单模型做旅行规划）。
- 一次性输入：是（sole-planning 模式下所有参考信息一次给全，agent 直接产出完整计划，无中途交互）。
- hard 中间 checkpoint：**是——多个可程序化校验的约束，分 hard constraint 与 commonsense constraint 两族，全部由 Python 判定**。

## Benchmark 是什么
1225 个多日旅行规划任务：给定出发地/目的地/天数/预算/偏好，agent 要产出一份逐日计划（交通/住宿/餐饮/景点）。计划必须同时满足大量约束。

## checkpoint 机制（本项目关注点）
虽不叫"checkpoint"，但**约束就是并列的中间卡点**，且全 hard（见采样的两个评测脚本原文）：
- **Hard Constraint**（`hard_constraint.py`）：预算不超标、住宿满足房型规则、交通不冲突、餐厅/景点不重复等——每条都是一个返回 True/False 的函数。
- **Commonsense Constraint**（`commonsense_constraint.py`）：城市连贯、时间合理、信息完整等 8 类常识约束。
- 两个核心指标：**Micro Pass Rate**（通过的约束数 / 总约束数，即卡点命中率）与 **Macro Pass Rate**（约束全过的计划比例）。GPT-4 Final Pass Rate 仅 0.6%。

## 采样内容
- `evaluation_hard_constraint.py` / `evaluation_commonsense_constraint.py`：全部约束校验函数原文（就是"卡点判定器"合集）。
- `std_travelplanner_sample.jsonl`：2 条完整任务（含参考信息与标准计划）。
- `example_evaluation_sample.jsonl`：2 条"query + plan"评测样例（`idx/query/plan`，plan 是逐日结构化行程）。
- `REPO_README.md`：官方说明。

## 对我们的参考价值
把"过程正确性"形式化为**一组可独立判定的约束谓词**，用 Micro Pass Rate 度量"满足了几条"——这与 checkpoint 命中率数学上等价，且约束由规则库统一定义（非逐条人工），规模化友好。是"planning 类长程任务 hard 卡点"的标杆。
