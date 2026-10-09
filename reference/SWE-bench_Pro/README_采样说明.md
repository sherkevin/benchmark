# SWE-bench Pro — 采样说明【对照：单 agent 长程编码，终态测试，无中间 checkpoint】

## 论文与来源
- 论文：*SWE-Bench Pro: Can AI Agents Solve Long-Horizon Software Engineering Tasks?*（Scale AI，arXiv:2509.16941）
- 数据：HF `ScaleAI/SWE-bench_Pro`；本次经 ModelScope 镜像 `ScaleAI/SWE-bench_Pro` 下载
- 本地采样下载通道：ModelScope resolve 通道，2026-07 抓取（731 条全量 parquet）

## 与需求的匹配度
- 单 agent：是。
- 一次性输入：是（issue + 需求 + 接口规格一次给全）。
- hard 中间 checkpoint：**否——仍是终态单元测试（fail_to_pass/pass_to_pass），无中间卡点**。作为"更长更难但仍无过程分"的对照组保留。

## Benchmark 是什么
SWE-bench 的长程强化版：更难、跨多文件、企业级、抗污染。采样确认 731 条，字段比原版丰富——除 `patch/test_patch/fail_to_pass/pass_to_pass` 外，新增 `requirements`（结构化需求）、`interface`（接口规格）、`issue_specificity` 等，任务描述更完整，单任务改动横跨多个文件、耗时更长。

## checkpoint 机制
与 SWE-bench 相同的 outcome-only：应用 patch 后跑 fail_to_pass（须由挂转过）+ pass_to_pass（须保持通过），全过才 resolved。**没有中间 checkpoint**——这正是本项目要补的空白：如此长的多文件任务，中途偏差无法定位。它的 `requirements`/`interface` 字段其实为"人工拆中间卡点"提供了现成素材（可据接口规格派生子目标）。

## 采样内容
- `test.parquet`：全量 731 条。
- `sample_3cases.json`：3 条（含 repo / problem_statement / requirements / interface / patch / fail_to_pass / pass_to_pass）。
- `DATASET_README.md`：官方数据卡。

## 对我们的参考价值
双重意义：(1) 作为"长程但无过程分"的最新对照，凸显中间 checkpoint 的缺失；(2) 其 `requirements`+`interface` 结构化字段，是把终态任务改造成"带 hard 中间卡点"任务的良好原料——可据接口规格自动派生子目标与对应测试。
