# MatTools — 采样说明【材料工具使用 benchmark：每题自带 properties 断言 + 单元测试，原生 checkpoint 三件套】

## 论文与来源
- 论文：*MatTools: Benchmarking Large Language Models for Materials Science Tools*（HKU 等，arXiv:2505.10852）
- 代码/数据：https://github.com/Grenzlinie/MatTools
- 本地采样下载通道：GitHub Contents API（gh 认证），2026-07 抓取

## 与需求的匹配度（材料方向工具调用 + hard 判分）
- 单 agent：是。
- 一次性输入：是（问题陈述 + 性质清单一次给全，agent 自主写代码调用 pymatgen 求解）。
- hard 中间 checkpoint：**是——每题带 `properties.json`（逐条期望值断言）+ `new_unit_test.py`（可执行单元测试），Docker 沙箱执行判分，无 LLM judge**。

## Benchmark 是什么
考察 LLM/agent 使用**材料科学工具（pymatgen / pymatgen-analysis-defects）**的能力，两个子集：
- **real-world tool-usage**：49 题 / 138 个任务——给一段真实材料计算需求（如谐振缺陷 HarmonicDefect 的缺陷带结构），agent 生成调用 pymatgen 的代码求出各性质；
- QA benchmark：约 6.9 万条 pymatgen 代码/文档 QA（辅助集）。

## checkpoint 机制（本项目关注点）
每题一个文件夹，**天然三件套**（本地采样即为真实原文）：
- `question.txt`：问题陈述；
- `properties.json`：**性质级断言清单**——每个性质给 description/format/期望 value（如 `defect_band = [(138,0,1),(138,1,1)]`、`spin_index = 1`）——一条性质 = 一个卡点，命中数即过程分；
- `new_unit_test.py`：可执行单元测试，Docker（`grenzlinie/mat-tools`）内跑代码验证。
判定完全 hard：断言 + 测试执行。

## 采样内容
- `case_question.txt` / `case_properties.json` / `case_new_unit_test.py`：`test_HarmonicDefect` 一题的完整真实三件套。

## 对我们的参考价值
**"性质清单即卡点"的材料版实证**：把一道材料计算题拆成 N 条可断言性质，与我们「多阶段 pipeline 每阶段埋可执行桩点」的构建法同构；且直接补上材料领域"工具调用"能力格。
