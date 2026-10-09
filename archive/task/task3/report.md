# Task Result · TASK-3 —— 从 MolBench 种子重建过程量评测 benchmark MVP

> ⚠️ **2026-06-08 框架注**:本文写作时项目级核心轴表述为"自主性 / 介入度量",已由 `../../goal.md` v1.0 **还原为更一般的"中间 checkpoint / 过程量度量"**(介入度降为操作化之一)。本文产出的 A/B/C 三件 MVP 与"测量仪器"原型**完全保留并继续可用**——读"自主性 benchmark"为"过程量评测 benchmark"即可。

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | TASK-3 / `task3/` |
| **Goal 文件** | `task3/goal.md` |
| **负责人** | jingwu |
| **起止** | 2026-06-02 ~ 2026-06-03 |
| **状态** | ✅ 完成(三件 MVP 全部跑通且数值正确) |
| **上游来源** | `task2/result.md`(把"能不能"推进到"做出来")|
| **下游消费方** | 项目级汇总(`PROJECT_RESULT.md`)的"已证明 / 仅模板 / 真正难"三档证据 |

---

## 一句话结论

> **✅ 收敛型 checkpoint 上的"测量仪器"(分级评分 + 偏离检测 + 分级介入回路 + 介入-成功率曲线)已在真实 MS-3 数据上跑通且对模拟 agent 强弱有区分;长程开放任务的金标准轨迹/介入卡有模板但**未对真实 LLM 实测**;开放式 L0 路径偏离判定仍是研究级难点。一个"有适用边界(收敛型核心)的版本现在就可行"。**

---

## 1. Goal 对照清单(★)

- [x] **A 端到端任务样例**(seed → L0–L3 + 4 阶段金标准轨迹/checkpoint + 分级介入卡 + 反事实变体)
  → `A_worked_example_CK2alpha.md`(基于 MS-3 真实 CK2α 种子 + 下载的 L2-05 模板)
- [x] **B MS-3「筛选阶段」过程评分原型**(可运行 + 真实数据结果 + 验证正确性)
  → `B_ms3_screening_prototype/`;`random hits@3=0.40` = 解析期望、`auroc=0.49`≈0.5,实现正确
- [x] **(额外)C 介入回路可行性演示**(回答"重建路线真的可行吗")
  → `C_feasibility_demo/`;3 种模拟 agent autonomy@0 = 100%/92%/64%,显著区分

---

## 2. 关键产出 / 发现

| # | 发现 | 证据 | 置信度 |
|---|---|---|---|
| 1 | 用真实 CK2α + 下载的 L2 模板可"填出"完整 L0–L3 + 4 阶段 checkpoint + 介入卡——专家从"创作者"降为"审核者"的路径成立 | `A_worked_example_CK2alpha.md` | 高(已写出);**未对真实 LLM 实测**(中) |
| 2 | MS-3 自带的 pChEMBL 可把官方单一 `Hits@3` 升级为带数值锚的分级评分 + 阶段 checkpoint 判定 | `B_ms3_screening_prototype/results.md` | 高(数值匹配理论值)|
| 3 | 介入-成功率曲线 + autonomy@0 + 达标介入数 在 25 真实任务上**单调区分**强/中/弱模拟 agent | `C_feasibility_demo/results.md` | 高(模拟 agent);真实 LLM 未测(中)|
| 4 | 便宜对照裁判可行的前提(手握 ground truth)在**收敛型 checkpoint** 上成立;开放式 L0 路径不成立——研究级难点 | `C_feasibility_demo/C_feasibility_assessment.md` §3 | 高 |

---

## 3. 下一步建议

| # | 动作 | 谁来做 | 何时 | 优先级 |
|---|---|---|---|---|
| 1 | 取 1 个任务(如 CK2α)用**真实 LLM**(Claude Code 技能版 vs vanilla)各跑 L0,k 次重复 | 项目维护者 | MVP 核心集发布前 | P0 |
| 2 | 收敛 checkpoint 上裁判与人工标注一致率 κ 校准(目标 κ>0.6) | + 1 名领域审核者 | 同上 | P0 |
| 3 | A 模板套到剩余 24 个 MS-3 靶点 / 33 个 MS-2 靶点 / MO 源分子,扩成 30–50 题核心集 | 维护者 + 专家审核 | 阶段 1(4–6 周)| P1 |
| 4 | 把 A 的 Stage 3(迭代优化)也接一个像 B 那样的可运行评分件 | 后续任务 | 阶段 2 | P2 |

---

## 4. 阻碍 / 受阻情况

| 阻碍 | 类型 | 何时发现 | 当前状态 | 待决方 |
|---|---|---|---|---|
| 开放式 L0 路径偏离判定的廉价裁判前提不成立 | 方法论 | 2026-06-02 | **已规避**(限定收敛型 checkpoint;开放任务只统计主动求助 + 过程 rubric;κ 公开) | 限定边界,不假装解决 |

---

## 5. 交付物指引

| 类别 | 路径 | 说明 |
|---|---|---|
| 子产物 A | `task3/A_worked_example_CK2alpha.md` | 端到端任务规格(132 行)|
| 子产物 B | `task3/B_ms3_screening_prototype/` | `ms3_screening_eval.py`(可运行)+ `results.md` + `README.md` |
| 子产物 C | `task3/C_feasibility_demo/` | `intervention_loop_demo.py`(可运行)+ `results.md` + `C_feasibility_assessment.md` |
| 上游依据 | `task2/result.md` | task2 的"改造方案"在此被部分验证 |

---

## 6. 对项目的轻量观察

- **L2-05 模板做"金标准轨迹的脚手架"是有效的**——把任务类型级模板填成 per-instance 任务规格,比从零标注省得多。这印证了 task2 §3 的"价值在脚手架不在数据"判断。
- **收敛 vs 开放任务的二分**应当显式写进 PRD:核心集主指标只测收敛 checkpoint;开放任务用主动求助 + 过程 rubric 兜底——这是最快出 MVP 的路径,且诚实可发表。
