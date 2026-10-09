# ProcessBench — 采样说明

## 论文与来源
- 论文：*ProcessBench: Identifying Process Errors in Mathematical Reasoning*（Qwen 团队，arXiv:2412.06559，ACL 2025）
- 数据：HuggingFace `Qwen/ProcessBench`；本次经 ModelScope 镜像 `Qwen/ProcessBench` 下载
- 代码仓库：https://github.com/QwenLM/ProcessBench
- 本地采样下载通道：ModelScope API，2026-07 抓取

## Benchmark 是什么
数学长链推理的**过程级错误定位**基准：3,400 个 case（GSM8K / MATH / OlympiadBench / OmniMath 四个难度层），每个 case 是一条模型生成的分步解答，由人工标注**最早出错的步骤编号**。

## checkpoint 机制
把推理链的每一步当作隐式 checkpoint：
- `steps`：解答按步切分（paragraph 级）；
- `label`：最早错误步的下标；如果全对则为 -1；
- `final_answer_correct`：终态是否正确——特意包含"**答案对但过程错**"的样本，用于揭穿只看结果的评测盲区。
评测任务 = 模型（作为 critic/PRM）找出最早错误步，指标为对错样本上的 F1。

## 采样内容
- `gsm8k_full.json`：GSM8K 子集全量（400 条）。
- `sample_5cases.json`：抽取 5 条。每条含 `problem`、`steps`（分步解答）、`label`（最早错误步 index）、`final_answer_correct`。
- `DATASET_README.md`：官方数据卡。

## 对我们的参考价值
math 类长程任务的 checkpoint 构建法是"**步级切分 + 最早错误步标注**"。其"答案对但中间步骤错"的设计理念（outcome 正确 ≠ 过程无偏）正是我们区分"结果评测 vs 过程评测"的关键证据。
