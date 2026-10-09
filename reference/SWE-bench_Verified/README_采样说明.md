# SWE-bench Verified — 采样说明

## 论文与来源
- 论文：*SWE-bench: Can Language Models Resolve Real-World GitHub Issues?*（arXiv:2310.06770，ICLR 2024）；Verified 子集由 OpenAI 与作者团队人工核验发布（2024-08）
- 数据：HuggingFace `princeton-nlp/SWE-bench_Verified`；本次经 ModelScope 镜像 `AI-ModelScope/SWE-bench_Verified` 下载（500 条全量 parquet，与官方数量一致）
- 官网：https://www.swebench.com/
- 本地采样下载通道：ModelScope resolve 通道，2026-07 抓取

## Benchmark 是什么
Coding 长程任务的事实标准：给 Agent 一个真实 GitHub issue + 完整代码仓库，要求产出修复 patch。Verified 子集 = 500 条经人工确认"issue 描述清晰、测试公平"的样本。

## checkpoint 机制（对照组：纯结果验证）
SWE-bench 是 **outcome-only** 构建法的代表：
- 评测只跑两组单元测试：`FAIL_TO_PASS`（修复后应由挂转过）与 `PASS_TO_PASS`（不能引入回归）；
- 全部通过 = resolved，否则 0 分——**没有任何中间 checkpoint**，Agent 中途探索、改错文件、绕远路都不影响评分；
- 因此社区又在其上派生了过程级研究（如 TRAIL 对 SWE-bench trace 做错误标注、Who&When 对失败轨迹做归因），恰好证明"只看结果"的不足。

## 采样内容
- `test.parquet`：Verified 全量 500 条。
- `sample_3cases.json`：抽取 3 条（字段：`repo`、`problem_statement`（issue 原文）、`patch`（金标准修复）、`test_patch`、`FAIL_TO_PASS`/`PASS_TO_PASS`（判分测试列表）、`difficulty`）。
- `README.md`：官方数据卡。

## 对我们的参考价值
代表"**可执行测试作为终态 oracle**"的构建法：客观、可自动化、无标注偏差，但对长程过程完全盲。我们的 checkpoint 设计可以把这种"可执行 oracle"思想搬到中间节点上（每个 checkpoint 配一个可执行验证器），兼得两者优点。
