# TravelPlanner 怎么测（事前规划）

**一句话：给出发地/天数/预算/偏好，让 agent 一次性产出一份逐日行程，再用一组 Python 约束函数逐条判断它满足了几条。**

## 一个真实 case（sole-planning 模式）

- **任务**：从 Washington 出发去 Myrtle Beach，3 天（3/13–3/15），预算 $1400。
- **产出**：逐日结构化计划——每天的交通（航班号）、早/午/晚餐、景点、住宿及花费。

（完整 2 条 case 见 [`reference/TravelPlanner/case_示例.jsonl`](./case_示例.jsonl)）

## 判分机制（怎么算对）

约束就是并列的"卡点"，全部由 Python 判定（True/False）：

- **Hard Constraint**（`hard_constraint.py`）：预算不超标、住宿满足房型规则、交通不冲突、餐厅/景点不重复等。
- **Commonsense Constraint**（`commonsense_constraint.py`）：城市连贯、时间合理、信息完整等 8 类常识约束。
- **两个指标**：**Micro Pass Rate** = 通过的约束数 / 总约束数（= 卡点命中率）；**Macro Pass Rate** = 约束全过的计划比例。GPT-4 的 Final Pass Rate 仅 0.6%。

## 属于哪种考法

**约束/规则库（并列卡点）**：把"计划对不对"拆成一组可独立判定的约束谓词，用命中比例度量规划质量，全 hard。

> 完整数据与判分脚本：[`reference/TravelPlanner/`](./)（`evaluation_hard_constraint.py` / `evaluation_commonsense_constraint.py` 全部约束函数、`std_travelplanner_sample.jsonl`）
