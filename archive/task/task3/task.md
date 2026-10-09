# Task 3 — 从 MolBench 种子重建「自主性度量」benchmark 的原型(MVP)

## 1. 由来

task2 结论:MolBench 释放的数据集是「单轮、只评最终答案」的 QA 题库,**不能直接满足**我们 PRD
(`prd/AI4S_agent_autonomy_metrics.md`)对**过程量 / 人类介入度量**的需求;但它的分子、靶点、
数值真值(尤其 MS-3 的 `answer_score`)是**合格的种子原料**,且 MolClaw 的 `skills/L2_workflows`
提供了可复用的「金标准过程模板」。

本任务把"能不能"推进到"做出来":**用真实下载到的数据当种子,产出两个可落地的原型,验证"重建"路线走得通。**

## 2. 目标(GOAL)

> 用**真实 MolBench 数据**当种子,做出两个最小可用产物,证明我们能把"最终答案题库"重建成"带过程量的自主性 benchmark":
> A. 单个 seed → 完整的 **L0–L3 prompt + 带阶段 checkpoint 的金标准轨迹 + 分级介入卡 + 反事实变体**;
> B. 把 MS-3 的 25 个排序任务,做成一个**带数值锚的「筛选阶段」过程评分原型**(可运行)。

## 3. 两个交付物

### A. 端到端任务样例(seed → 自主性任务规格)
- **种子**:取自下载数据 `MS-3 index=1` —— 靶点 **CK2α(Casein kinase II subunit alpha, CHEMBL3629)**,
  含真实活性分子与实验 pChEMBL(6–8)。
- **产物**:`A_worked_example_CK2alpha.md`,含
  1. 任务规格(目标、起点分子、可被工具判定的成功标准);
  2. L0–L3 prompt(目标明确性恒定、路径指导性递增,按 PRD §2.3);
  3. 4 阶段金标准轨迹 + 每阶段 checkpoint(任务分解 / 实验设计与基线 / 迭代执行 / 结果分析),
     映射到 `skills/L2_workflows/00,01,02,05,08`;
  4. 分级介入卡(每阶段 × D0–D3 四档深度,按 PRD §2.5,取材自 L2 的 "Common Failures & Recovery");
  5. 反事实变体 ≥3(换起点 / 改目标(选择性)/ 换靶点变体),用于抗污染(PRD §2.2)。

### B. MS-3「筛选阶段」过程评分原型(可运行)
- **产物**:`B_ms3_screening_prototype/`
  - `ms3_screening_eval.py`:把 MolBench 的「只看 Hits@3 最终答案」升级为**带数值锚的分级评分**
    (Hits@k、Recall@k、富集因子 EF、以 pChEMBL 为分级相关度的 NDCG、AUROC、BEDROC),
    并给出可插入对照裁判的「筛选 checkpoint 判定」。提供 `evaluate_ranking(...)` 公共 API 供接入真实 agent 排序。
  - `results.md`:在真实 25 个任务上用参考排序器(oracle / random / reversed / noisy-oracle)跑出的结果,
    证明指标正确且对排序质量敏感(可区分强弱)。
  - `README.md`:用法与设计说明。

## 4. 约定与真实性标注

- 种子分子、靶点、pChEMBL 均来自下载数据(真实);成功阈值(如 ΔDock ≥ 1.5 kcal/mol)、金标准**路径**、
  介入卡为**作者撰写的模板**,文中明确区分「真实数据锚定」与「作者撰写」。
- 拉取文件在 `prd/`,调研结论在 `search/`,本任务交付在 `archive/task3/`。
- 不臆造工具运行结果:凡需工具实算的数值(对接分、ADMET)以"由确定性工具在评测时计算"标注,不编造具体数字。

## 5. 与 goal 的关系

完成后即证明:**MolBench 数据可经"种子复用 + L2/L3 脚手架 + 过程量重建"转化为我们要的自主性 benchmark**,
为后续扩成 30–50 题的 MVP 核心集(PRD §5.1)提供可复制的模板与评分件。
