# SWE-bench 怎么测（反馈应变）

**一句话：给 agent 一个真实 GitHub issue + 整个代码仓库，让它反复"改代码 → 跑测试 → 看报错 → 再改"直到产出修复补丁；最后只看两组单元测试过不过。**

## 一个真实 case（Verified 集）

- **仓库**：`astropy/astropy`
- **实例**：`astropy__astropy-12907`（version 4.3）
- **给 agent**：issue 原文（problem_statement）+ 完整代码库；产出一个修复 patch。

（真实字段见 [`reference/SWE-bench_Verified/case_示例.json`](./case_示例.json)）

## 判分机制（怎么算对）

- **FAIL_TO_PASS**：修复后，原本失败的测试应转为通过（证明修好了）。
- **PASS_TO_PASS**：原本通过的测试不能被改挂（证明没引入回归）。
- 两组测试**全部通过 = resolved（1 分）**，否则 0。纯 pytest，无 LLM。
- **只卡终态**：中途改错文件、绕远路都不扣分——"反馈应变"体现在 agent 自己"测试报错→定位→再修"的迭代闭环里，是完成任务的必经过程。

## 属于哪种考法

**可执行测试作为终态 oracle（outcome-only）**：客观、可自动化、无标注偏差，但对中间过程不设卡点（社区因此又派生了 TRAIL/Who&When 等对其 trace 做过程标注的研究）。

> 完整数据：[`reference/SWE-bench_Verified/`](./)（`test.parquet` 全 500 条含 FAIL_TO_PASS/PASS_TO_PASS 测试列表、`sample_3cases.json`）
