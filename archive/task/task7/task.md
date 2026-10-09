# Task 7 — POC:3 个真实 benchmark × 真 LLM × 我们的测量机器

> **上游**:task6 调研结论(`../task6/result.md`)+ 已有测量机器(`../../src/`,21 单元测试通过,五签名输出齐,**只差接真 LLM × 真 benchmark**)。
> **本文职责**:task7 POC 章程(目标 / 子任务 / 验证什么 / 不验证什么)。

## 1. 目标(GOAL)

**用 3 个 retrofit POC 验证调研结论真的能落地**——即把 task6 卡里写的"怎么改造"真跑出来,接真 LLM,看:

1. **我们的 cp 设计能不能跑通**(三档:硬校验 / AI 判官 / 人工)
2. **src 的指标管线**(autonomy@0 / 介入-成功率曲线 / pass^k / 介入守恒 / L0–L3 敏感性 / 许可校准)在真 benchmark + 真 LLM 上**输出是否有意义、有区分度**
3. **过程量 vs 最终答案的相关性**(论文核心实验之一)—— cp 通过率与最终正确率的 Pearson/Spearman 相关
4. **借的方法学技术栈跨域是否一致**(AgentBoard 进度公式 / TheAgentCompany 部分得分 / HealthBench 负分 rubric)

## 2. 3 个 POC(选自 task6 result § "能用什么")

| # | POC | 验证哪类 retrofit 模式 | 工作量(基于已有 src) |
|---|---|---|---|
| **POC 1** | [DO Challenge](poc1_do_challenge/) | 自定义 cp(预算约束硬校验)+ 集合交集 grader + budget oracle 接口 | ~3-4 天 |
| **POC 2** | [Matbench·MPtrj](poc2_matbench_mptrj/) | 物理真值 cp(零标注)+ 弛豫帧轨迹作 oracle + 数值/结构/排序硬校验 | ~1 周(含装 CHGNet) |
| **POC 3** | [PhysicianBench 轻改](poc3_physicianbench/) | 现成 670 cp + 升加权部分得分 + Apache repo 直接 fork | ~半天(最快验证机制) |

**先后顺序建议**:POC 3 → POC 1 → POC 2(最快验证机制 → 跨域 → 装环境最重的)。但 3 个互相独立,可并行。

## 3. 不做什么(诚实边界)

- ❌ **不造新评测器**——直接用 `../../src/`(它已 21 单元测试通过,五签名输出齐)
- ❌ **不造新 benchmark 题**——直接 retrofit 现有 3 个(POC 不是论文 MVP 题集,是机制验证)
- ❌ **不做完整论文实验**(N 个模型 × N 任务大规模跑)——POC = 1-3 道任务跑通即可
- ❌ **不接 PhysicianBench 的真患者数据**(DUA-gated,POC 用 Synthea 合成 demo)

## 4. 交付物

| # | 文件 | 状态 |
|---|---|---|
| 1 | `task7/poc1_do_challenge/results.md` —— DO Challenge × Claude/Gemini 跑通的真实 results | ⏳ |
| 2 | `task7/poc2_matbench_mptrj/results.md` —— MPtrj × CHGNet/fairchem × LLM 跑通的真实 results | ⏳ |
| 3 | `task7/poc3_physicianbench/results.md` —— PhysicianBench 1-3 题 × LLM 跑通,binary vs 加权对比 | ⏳ |
| 4 | `task7/evaluator/` —— 共享的 grader/agent harness 实现(若需扩 src) | ⏳ |
| 5 | `task7/result.md` —— 3 POC 综合判定:cp 设计是否跨域一致?指标管线是否输出有意义?过程量 vs 终答相关性? | ⏳ |

## 5. 与项目的关系

- **上游**:`../task6/result.md` § "MVP 路线图" + `../../src/`(测量机器)+ `../../prd/AI4S_agent_autonomy_metrics.md`(方法学)
- **下游**:论文 method 章 + experiments 章的种子结果;若 POC 跑通,正式 MVP 题集(30-50 题)的招标输入
- **副产出**:对 task6 卡里"怎么改造"描述的**实证收紧/纠错**(预期会出现:某些 cp 定义出来发现无意义,某些方法跨域不一致)

## 6. 需用户决策的事项

详见 [`../todo_user.md`](../todo_user.md):
- LLM API key 准备(Claude / Gemini / OpenAI 至少一个)
- PhysicianBench DUA 是否申请(POC 阶段建议 N,用 Synthea 合成 demo;正式 MVP 再申)
- CHGNet / fairchem 装哪个(MPtrj 弛豫工具,POC 2 用)
- 预算上限(LLM API 调用 × N 题 × 长程 trajectory 不便宜)
