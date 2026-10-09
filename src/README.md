# src —— 过程量评测基准的「测量机器」MVP

> AI4S(药物 + 材料)agent **过程量 / 中间 checkpoint** 评测基准(对齐 `../goal.md` v1.0)的可运行内核;"人类介入度量"是其中一种主要操作化。
> 在 task3 已验证的**单 checkpoint 原型**(`../task/task3/B_ms3_screening_prototype` + `C_feasibility_demo`)之上,**加性扩展**为**多阶段金标准轨迹 + 完整指标套件**。纯标准库,无依赖。
>
> 📌 **项目级 GOAL 看 `../goal.md`(权威);上游 PRD `./prd/AI4S_agent_autonomy_metrics.md` 已加 superseded 横幅,其方法学保留**。

## 这是什么(一句话)

把"agent 需要多少人类介入才能完成长程科学任务"做成一台**可运行、可复现的测量机器**:
输入一个(模拟)agent + 一组任务,输出 **autonomy@0 / 介入-成功率曲线 / intervention-conditioned pass^k / 介入守恒账 / prompt 详细度敏感性 / 许可校准诊断**。

## ⚠️ 诚实边界(必读)

- 本仓的 "agent" 全是**带技能参数的模拟器**(`core/sim_agents.py`),用于证明**测量机器良定义、正确、对自主性有区分度、且不奖励莽撞**。
- **不产出任何真实 LLM 排行榜**。真实 agent 评测需把真 agent 接进 `harness`(见 draft 的"实验协议"与下方"接真实 agent")。
- 科学数值锚点复用**真实数据**;阈值/介入卡/prompt 为**作者撰写**(每个任务 `provenance` 标注)。

## 怎么跑

```bash
cd benchmark/src
python3 tests/test_core.py                       # 单元测试(21 项断言)
python3 run_validation.py --write results.md     # 在合成 fixture 上跑全套指标 → results.md
python3 run_validation.py --tasks tasks --write results_real.md   # 在真实锚定任务集上跑
```

## 五个签名输出(见 `results.md`)

1. **autonomy@0** + 介入-成功率曲线:oracle→weak 单调递降;`never_asks`==`weak`(不奖励莽撞);random/reversed≈0(下界)。
2. **intervention-conditioned pass^k**:随 k 降、随预算升(把 pass^k 让给 τ-bench 并推广为介入条件化)。
3. **介入守恒**(固定 cohort、逐实例):绑定区间内 `pre + runtime ≈ 常数` → 前置 prompt 详细度与运行时求助同币种;过量提示=浪费(`binding占比`塌缩)。
4. **prompt 详细度敏感性**:L0→L3,autonomy@0 升、运行时介入降。
5. **许可校准诊断**(与自主性正交):well-calibrated≈1.0 vs never_asks≈0.5。

## 布局

```
src/
  core/        ranking_metrics(复用 task3) tasks judge interruption metrics conservation sim_agents harness
  tasks/       真实锚定任务集(drug/materials;reuse MolBench/task5 GT)
  tests/       test_core.py + fixtures/(合成 fixture,仅验证机器)
  run_validation.py   报告生成器
  results.md   合成 fixture 上的验证报告
  schema.md    任务规格 schema
```

## 接真实 agent(下一步,需平台)

把真实 agent 包成一个对象,实现与 `SimAgent.produce(stage, ...)` 等价的"给定阶段→产出该阶段输出"的接口,
并由真实运行轨迹喂入 `judge.judge_stage` 与 `harness`。其余指标/曲线/记账原样复用。

## 复用与来源

- 排序指标 + checkpoint:移植自 task3 `ms3_screening_eval.py`(已在真实 MolBench-MS-3 验证)。
- 介入回路 + autonomy@0 + 曲线:概念承自 task3 `intervention_loop_demo.py`,泛化到多阶段。
- 设计依据:PRD `benchmark/src/prd/AI4S_agent_autonomy_metrics.md`。
