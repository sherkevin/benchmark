# LongProc 怎么测（事前规划）

**一句话：给一道要"严格按规程一步步生成"的任务，让 agent 一次性产出完整的长过程，再用确定性规则逐步校验每一步对不对。**

## 一个真实 case（countdown 凑数）

- **输入**：`nums = [21, 16, 17, 26]`，`target = 46`。
- **标准解**：`["21+16=37", "26-17=9", "37+9=46"]`（另附完整搜索轨迹 `demonstration`）。
- 要求 agent 输出规定格式的分步解。

（3 条真实 case 见 [`reference/LongProc/case_示例.json`](./case_示例.json)）

## 判分机制（怎么算对）

- **逐步确定性校验**：每一步算术是否正确、是否合法使用了剩余数字、搜索/推理链是否自洽——全部由确定性程序解析中间步 + 规则判定，不是只看最终等式。
- 6 类任务（countdown / path_traversal / pseudo_to_code / tom_tracking / html_to_tsv / travel_planning）各有专门 evaluator；按输出长度分 0.5K/2K/8K 三档。
- 全程 hard：可解析结构 + 规则，无 LLM。

## 属于哪种考法

**约束/规则库（逐步校验）**：把"长过程对不对"拆成"每一步是否合规"，中间步结构化可解析——考察长程生成中能否每一步都不跑偏。

> 完整数据：[`reference/LongProc/`](./)（`countdown_*.json` 样例、`*_prompts.yaml` 输出格式规范）
