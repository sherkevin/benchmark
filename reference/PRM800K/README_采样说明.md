# PRM800K — 采样说明

## 论文与来源
- 论文：*Let's Verify Step by Step*（OpenAI，arXiv:2305.20050，ICLR 2024）
- 数据/代码仓库：https://github.com/openai/prm800k（数据以 Git-LFS 存放）
- 本地采样下载通道：GitHub LFS media 通道（media.githubusercontent.com），按字节范围截取 `phase2_test.jsonl` 前 57 条完整记录，2026-07 抓取

## Benchmark 是什么
过程监督（process supervision）的奠基数据集：对 MATH 数据集问题的模型解答做 **80 万个步级人工正确性标注**（75K 条解答、12K 问题）。用它训练的过程奖励模型（PRM）在 MATH 上显著超过只看最终答案的结果奖励模型（ORM，78.2% vs 72.4%）。

## checkpoint 机制
每条解答的每一步由人工标注三值评分：
- `rating: 1`（正确且有推进）/ `0`（无实质推进或存疑）/ `-1`（错误步）；
- 数据结构：`question`（问题+参考答案）→ `label.steps[]`（每步的 `completions[{text, rating}]`）；
- 这相当于**最细粒度的 checkpoint**：链条上每一步都是一个有人工裁决的中间节点。

## 采样内容
- `phase2_test_partial.jsonl`：phase2 测试集前 57 条完整记录（原文件较大，按需截断，每行均已校验为合法 JSON）。
- `sample_5cases.json`：其中 5 条的格式化展示。
- `REPO_README.md`：官方说明（含数据 schema 详解）。

## 对我们的参考价值
step 级人工评分是过程评测的"金标准"上限，但标注成本极高（80 万标注）。对我们的启示：checkpoint 粒度需要在"步级（贵、细）"与"里程碑级（便宜、粗）"之间做设计权衡。
