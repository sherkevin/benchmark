# task7 · POC 索引

> **3 个 retrofit POC**:把 task6 调研结论(`../task6/report.md`)真跑出来,验证我们的测量机器(`../../src/`)能不能在真 benchmark + 真 LLM 上输出有意义结果。

## 顶层

| 文件 | 职责 |
|---|---|
| [`goal.md`](goal.md) | POC 章程(目标 / 不做什么 / 与项目关系) |
| [`README.md`](README.md) | 本页:导航 |
| [`result.md`](result.md) | 3 POC 综合判定(⏳ POC 跑完后写) |

## 3 个 POC

| # | 目录 | 验证模式 | 优先级 |
|---|---|---|---|
| **POC 1** | [`poc1_do_challenge/`](poc1_do_challenge/) | 自定义预算约束 cp + 集合 grader(药物虚筛) | 主线 1 |
| **POC 2** | [`poc2_matbench_mptrj/`](poc2_matbench_mptrj/) | 物理真值 cp + 弛豫帧 oracle(材料 discovery) | 主线 2 |
| **POC 3** | [`poc3_physicianbench/`](poc3_physicianbench/) | 现成 670 cp + 升加权得分(医疗,**最快验证机制**) | 起步 |

## 共享 evaluator

[`evaluator/`](evaluator/) —— grader / LLM agent harness 共享实现(若需扩 src,改动放这)。

## 依赖

- **测量机器**:[`../../src/`](../../src/)(已 21 单元测试通过,本 POC 直接接进)
- **调研结论**:[`../task6/report.md`](../task6/report.md)(每 POC 的 retrofit 步骤都来自对应卡)
- **方法学**:[`../../prd/AI4S_agent_autonomy_metrics.md`](../../prd/AI4S_agent_autonomy_metrics.md)(superseded by goal.md,但方法学保留)

## 用户决策项

见 [`../todo_user.md`](../todo_user.md):LLM API key / PhysicianBench DUA / CHGNet vs fairchem / 预算上限。

## 当前状态

⏳ 骨架已搭(2026-06-10),三 POC 子目录就绪;**等用户确认 API key + 预算后开干 POC 3 起步**。
