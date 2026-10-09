# τ-bench 怎么测（用对工具）

**一句话：给 agent 一个客服任务让它调数据库 API 去办，对话结束后比对数据库的最终状态是否等于标准答案，并要求同一题跑 k 次都对。**

## 一个真实 case（retail 域，摘自官方任务文件）

> 用户指令：*"You are Yusuf Rossi in 19122. You received your order #W2378156 and wish to exchange the mechanical keyboard for a similar one but with clicky switches and the smart thermostat for one compatible with Google Home instead of Apple HomeKit..."*
>
> 标准写操作 actions（部分）：`find_user_id_by_name_zip(first_name="Yusuf", last_name="Rossi", zip="19122")` → 定位订单 → 查可选商品 → `exchange_delivered_order_items(...)`

即：任务 = **用户指令 + 一串 ground-truth 写操作**（见 [`reference/tau-bench/case_示例.py`](./case_示例.py)）。

## 判分机制（怎么算对）

- **终态比对**：每题标注的标准 actions 执行后得到唯一目标数据库状态；对话结束后比对 agent 实际造成的数据库状态是否与之一致（外加回复须包含指定信息）。全中=1，否则=0。
- **pass^k**：同一题独立跑 k 次，**全部成功**才算过——专门度量长程行为的稳定性（论文中 gpt-4o 的 pass^8 < 25%，远低于单次成功率）。
- 全程 hard：数据库状态比对，无 LLM 裁判。

## 属于哪种考法

**终态状态比对（state-based oracle）+ 一致性指标**：不设中间卡点，只卡结果；工具用错会直接反映在最终数据库状态上。

> 完整任务与轨迹：[`reference/tau-bench/`](./)（`retail_tasks_test.py` 全部任务+actions、`few_shot_sample.jsonl` 完整对话轨迹）
