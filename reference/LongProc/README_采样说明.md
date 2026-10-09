# LongProc — 采样说明【核心匹配：单 agent + 一次输入 + 确定性中间步校验】

## 论文与来源
- 论文：*LongProc: Benchmarking Long-Context Language Models on Long Procedural Generation*（Princeton PLI，arXiv:2501.05414）
- 代码/数据：https://github.com/princeton-pli/LongProc
- 本地采样下载通道：GitHub API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是（单模型执行长程序化生成）。
- 一次性输入：是（信息一次给全，模型一次性生成完整的长过程，无交互）。
- hard 中间 checkpoint：**是——中间步骤用确定性规则逐步校验（可解析、可验证的过程输出），非 LLM 判定**。

## Benchmark 是什么
6 类需要"严格按规程一步步生成"的任务，按输出长度分 0.5K/2K/8K token 三档：countdown（凑数搜索）、path_traversal（路径遍历）、pseudo_to_code（伪代码转代码）、tom_tracking（心智状态追踪）、html_to_tsv（结构抽取）、travel_planning。核心考察：能否在长程生成中保持每一步都符合规程、不中途跑偏。

## checkpoint 机制（本项目关注点）
以采样的 countdown 为例：`nums:[21,16,17,26], target:46`，标准解 `solution:["21+16=37","26-17=9","37+9=46"]`，且带完整 `search_steps` 搜索轨迹。评测不是只看最终等式，而是**逐步校验中间过程**：每一步的算术是否正确、是否合法使用了剩余数字、搜索/推理链是否自洽。所有校验都是确定性程序（解析中间步 + 规则判定），无 LLM。每类任务有专门的 deterministic evaluator。

## 采样内容
- `countdown_countdown_0.5k_sample3.json`：3 个 countdown case（含 `nums/target/solution/search_steps/demonstration/solution_text`——`demonstration` 是完整的分步搜索过程示范）。
- `countdown_prompts.yaml` / `travel_planning_prompts.yaml`：任务提示与输出格式规范（定义了中间步的可解析结构）。
- `REPO_README.md`：官方说明。

## 对我们的参考价值
证明了**"长程生成的中间步可被规则化解析并逐步 hard 校验"**——中间卡点 = 过程中每一步的确定性正确性检查。对我们设计"一次性输入、需逐步判断 trace 有没有偏"的评测最直接：它的过程本身是结构化可解析的，卡点由规则自动施加，无需逐条人工标注。
