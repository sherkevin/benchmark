# SciCode — 采样说明【核心匹配：单 agent + 一次输入 + 子问题级 hard 测试】

## 论文与来源
- 论文：*SciCode: A Research Coding Benchmark Curated by Scientists*（arXiv:2407.13168，NeurIPS 2024 Datasets & Benchmarks）
- 代码/数据仓库：https://github.com/scicode-bench/SciCode（数据另在 HF `SciCode1/SciCode`）
- 本地采样下载通道：GitHub API（gh 认证），2026-07 抓取

## 与需求的匹配度
- 单 agent：是（单模型解题，无协作、无模拟用户）。
- 一次性输入：是（主问题+子问题描述一次给全，无中途交互）。
- hard 中间 checkpoint：**是——每个子问题配一组可执行断言测试（`test_cases`），共 338 个子问题级卡点**。

## Benchmark 是什么
科学家（含相关领域一线研究者）出题的科研编程基准：80 个主问题（物理/化学/生物/材料的真实计算任务），每个主问题被科学家分解为多个子问题（平均 4-5 个，采样的 Berendsen_thermostat 分解为 12 个），子问题按依赖链渐进——后面的函数要调用前面写好的函数。

## checkpoint 机制（本项目关注点）
- 每个子问题（`sub_steps[i]`）含：`step_description_prompt`（子任务描述）、`function_header`（函数签名）、`test_cases`（若干条 `assert np.allclose(f(x), target)` 式可执行断言）；
- 评测分两层：**子问题通过率**（每个子问题的测试全过才算过，就是中间卡点得分）与**主问题解决率**（所有子问题全过）；
- SOTA 模型主问题解决率仅 14%-28.5%，但子问题通过率远高于此——卡点得分精确刻画"走到第几步开始偏"。

## 采样内容
- `first_problem.jsonl`：完整主问题 Berendsen_thermostat（分子动力学恒温器），含 12 个子问题及全部测试用例、`general_tests`（主问题终态测试）。
- `eval_test_generated_code.py`：官方判分脚本（逐子问题执行测试）。
- `REPO_README.md`：官方说明。

## 对我们的参考价值
最贴近需求的范式之一：**由领域专家在出题时就把长任务拆成带可执行测试的子问题链**。checkpoint = 子问题单元测试，完全 hard（Python assert，无 LLM/无人主观判定），且拆分粒度天然（依赖链）。缺点：卡点由人工出题时定义，规模化受限于专家标注（80 主问题）。
