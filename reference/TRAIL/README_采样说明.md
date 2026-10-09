# TRAIL — 采样说明

## 论文与来源
- 论文：*TRAIL: Trace Reasoning and Agentic Issue Localization*（Patronus AI，arXiv:2505.08638）
- 数据：HuggingFace `PatronusAI/TRAIL`；本次经 ModelScope 镜像 `PatronusAI/TRAIL` 下载（HF 直连受限）
- 代码仓库：https://github.com/patronus-ai/trail-benchmark
- 本地采样下载通道：ModelScope API，2026-07 抓取

## Benchmark 是什么
148 条**带人工错误标注的 agent 执行 trace**（OpenTelemetry 格式 span），来自 GAIA（开放世界信息检索）与 SWE-bench（软件工程）两类长程任务，共标注 841 个错误。提出一套跨领域的 **agentic 错误分类学（taxonomy）**，分三大类：
- **Reasoning 错误**：幻觉、信息误读、输出格式不合规（Instruction Non-compliance）等；
- **System Execution 错误**：工具调用失败、环境/配置错误、资源耗尽等；
- **Planning/Coordination 错误**：任务编排失败、上下文丢失、无效反思等。

## checkpoint 机制
无预设 checkpoint，而是 **span 级错误定位**：每条标注含 `category`（错误类别）、`location`（trace 中的 span id）、`evidence`（原文证据）、`impact`（HIGH/MEDIUM/LOW 影响级别）、`description`。评测任务 = LLM 阅读完整 trace，输出所有错误的位置+类别+影响，与人工标注对比（joint F1）。SOTA 模型（Gemini-2.5-Pro）也只有 ~18% 的联合得分。

## 采样内容
- `GAIA/`、`SWE_Bench/`：各 2 条完整原始 trace（OpenTelemetry span JSON，单条可达数 MB，含每一步 LLM 调用与工具调用）。
- `annotations_gaia/`：3 条对应的人工错误标注（errors + scores）。
  - 例：`0035f455...json` 标注了 "Instruction Non-compliance"（未按要求输出 `<end_plan>` 标签，impact LOW）等多个错误，每个错误精确到 span id。
- `DATASET_README.md`：官方数据卡。

## 对我们的参考价值
给出了"trace 偏差"的**工业级标注 schema**：错误类别 × trace 内位置 × 严重程度。我们构建 checkpoint 型 Benchmark 时，这套 taxonomy 可直接用作"偏差类型"的定义域；其 span 级 location 亦可作为 checkpoint 粒度的参照。
