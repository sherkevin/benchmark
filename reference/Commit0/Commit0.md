# Commit0 怎么测（反馈应变）

**一句话：给 agent 一份库的规格（接口+文档）和一整套隐藏单元测试，让它从空实现把整个 Python 库写到通过测试；测试通过率随实现推进单调上升，就是过程分。**

## 一个真实 case（配置见 `case_config.yaml`）

- **数据集**：`wentingzhao/commit0_docstring`——57 个真实核心 Python 库（如 minitorch、jinja、requests 类）。
- **给 agent**：spec（接口签名 + docstring）+ 隐藏单元测试骨架；单库常含上百个待实现函数。

## 判分机制（怎么算对）

- **分层单元测试**：每实现一部分就能跑对应测试，通过数随实现单调上升。
- **核心指标 = 测试通过率**（不是 0/1 的"整库是否完成"）：每个单元测试就是一个**渐进 hard 卡点**，精确刻画"实现到哪、哪里开始不达标"。
- 全程 hard：pytest 执行，无 LLM（`harness_evaluate.py` 是判分入口）。
- **反馈应变**体现在：跑测试 → 看哪些没过 → 补实现 → 再跑的迭代闭环。

## 属于哪种考法

**单元测试即卡点（渐进式）**：一套测试 = 一批天然 hard 卡点，数量大（单库上百）、粒度细、全自动，天然给出过程分——相比 SWE-bench 只看终态全过，Commit0 直接产出"实现进度曲线"。

> 完整数据：[`reference/Commit0/`](./)（`harness_evaluate.py` 判分入口、`configs_base.yaml`、57 库清单见 `REPO_README.md`）
