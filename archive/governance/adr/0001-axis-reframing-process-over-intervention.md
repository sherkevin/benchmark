# ADR 0001 · 项目核心技术轴还原:从"人类介入度"到更一般的"过程量度量"

| 字段 | 值 |
|---|---|
| **状态** | Accepted |
| **日期** | 2026-06-08 |
| **决议人** | jingwu |
| **影响范围** | 项目级(所有 task + 论文叙事 + 测量机器定位)|

---

## 1. 上下文(为什么有这次还原)

项目早期 PRD(`materials/prd/AI4S_agent_autonomy_metrics.md`,2026-06-02 前)把核心技术轴写成「**人类介入程度**」,以此为差异化向量。这个表述当时合理——具体、可量化、对应"agent 系统报告里最缺的那根轴"。

执行 5 个 task(尤其 task2 MolBench 评估、task3 原型搭建、task4 全景调研)之后,需求的形状更清楚了:

1. **介入度量在因果上以过程量为前提**——没有 checkpoint / GT,谈不上"在哪里、什么深度的介入能救回";"介入"是从"过程量"派生出来的二阶量,不该是母题。
2. **真实空白其实更大**——task4 实证:整个 AI4S 方向 benchmark 几乎全部只评最终答案(MolBench / MedAgentBench / Biomni-Eval1 / LAB-Bench / ScienceAgentBench / DiscoveryBench 同构)。"过程量度量"本身就是供给侧空白,介入度只是其中一种部署相关的切面。
3. **论文叙事经不起未来扩展**——以"介入度"作母题,后续若想覆盖化学 / 生物 / 医学 agent 的其他过程量需求(可信轨迹、行为审计、错误归因),框架就不够用了;以"过程量"作母题则天然容纳。

## 2. 决策

把项目级核心技术轴由「**人类介入程度**」**还原为更一般的**「**中间 checkpoint / 过程量度量**」。「人类介入度」降为该轴的**核心操作化之一**,不再是唯一形态。

权威定义见 `goal.md` v1.0(2026-06-08 锁定)。

## 3. 不动的部分(防止过度解读)

- **领域范围**(AI4S = 药物 + 材料)与 PRD §1 一致,**不变**
- **PRD §2 方法学**(L0–L3 prompt 分级 / 分级介入卡 / 对照裁判 / 介入-成功率曲线 / 介入守恒 / 许可校准)**全部保留**,定位从"项目主轴"调整为"过程量度量的一套具体工具箱"
- **task1-5 的结论 / 数据 / 代码** 全部保留并继续可用
- **`src/` 测量机器** 本就是过程量度量的实现,介入指标只是它输出指标之一,不需要重写

## 4. 触发的文档动作(已完成)

| # | 文件 | 动作 |
|---|---|---|
| 1 | `goal.md` v1.0(新建)| 三层 GOAL + DRBEκP 工作流 + 红线 + 重审条件 |
| 2 | `PROJECT_RESULT.md` | 标题 + header 表 + §1 背景 / 差异化措辞修订;加 `goal.md` 指针 |
| 3 | `materials/prd/AI4S_agent_autonomy_metrics.md` | 顶部加 superseded 横幅(正文不动)|
| 4 | `src/README.md` | 开篇定义还原 + 加 `goal.md` 指针 |
| 5 | `src/ACCEPTANCE.md` | 顶部加 2026-06-08 后注 + 目标段措辞 |
| 6 | `task{2,3,4}/result.md` + `task4/benchmark_landscape_report.md` | 顶部加 2026-06-08 框架注(正文不动)|

**未动**(原则:历史快照不重写):task1/5、task2/3 的 goal.md、`research/search/*`、`materials/prd/molbench_repo/*`——这些是某时点的判断快照或一手原始资料,改写会丢失"我们什么时候发现什么"的脉络。

## 5. 后续影响

- 新 task 的 `goal.md` 必须以本 ADR + 项目级 `goal.md` 为参照系,**不再以 PRD §2 为唯一框架**
- 论文写作时,methods 章节以"过程量度量"为母题,「介入度 / autonomy@0 / 介入-成功率曲线」作为该框架下的一组具体指标呈现,而非论文的全部
- 若未来发现"过程量度量"仍不够一般(如需进一步还原为"agent 行为度量 / 可信度度量"等),需写**新 ADR supersede 本 ADR**,并把本 ADR 状态改为 `Superseded by NNNN`——**不静默改 `goal.md`**

## 6. 相关

- 上游:`materials/prd/AI4S_agent_autonomy_metrics.md`(本 ADR 修订其 §2 主轴定位,§1 范围保留)
- 决定了:项目级 `goal.md` v1.0 的核心轴表述
- 触发了:`PROJECT_RESULT.md` v0.2 修订 + 6 个 task / src 文档的框架注
