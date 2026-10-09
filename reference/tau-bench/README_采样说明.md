# tau-bench (τ-bench) — 采样说明

## 论文与来源
- 论文：*τ-bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains*（Sierra AI，arXiv:2406.12045，ICLR 2025）
- 代码/数据仓库：https://github.com/sierra-research/tau-bench（后继版本 tau2-bench：https://github.com/sierra-research/tau2-bench）
- 本地采样下载通道：GitHub Contents API（gh CLI 认证），2026-07 抓取

## Benchmark 是什么
客服场景（retail / airline）的长程多轮工具交互基准：Agent 一边与 LLM 模拟用户对话、一边调用数据库 API（改订单、退款、订机票），需遵守领域政策（policy）。任务平均需要多轮对话+多次写操作。

## checkpoint 机制（对照组：无显式 checkpoint）
τ-bench 代表另一种构建哲学——**终态数据库比对**：
- 每个任务标注一组 ground-truth 写操作（`actions`），执行后得到唯一目标数据库状态；
- 评测 = 对话结束后比对实际数据库状态 与 目标状态（外加输出内容必须包含的信息）；
- 提出 **pass^k 指标**：同一任务跑 k 次全部成功的概率，度量长程行为的**稳定性/一致性**（结果显示 gpt-4o pass^8 <25%，长程可靠性远低于单次成功率）。
- 没有中间 checkpoint：过程自由，只卡终态。这是与 checkpoint 型设计互补的路线。

## 采样内容
- `retail_tasks_test.py`：retail 域全部测试任务定义（每个任务含 user 指令、ground-truth `actions` 序列与期望输出）。
- `few_shot_sample.jsonl`：2 条官方 few-shot 轨迹（含完整多轮对话+工具调用 trace）。
- `REPO_README.md`：官方说明。

## 对我们的参考价值
两点：(1) "以终态状态比对代替过程检查"是 checkpoint 的替代方案，构建成本低但无法定位偏差发生点；(2) **pass^k** 是度量"长程执行是否容易跑偏"的统计学指标，可与 checkpoint 机制正交组合。
