# Task Result · TASK-4 —— 开源 Agent Benchmark 全景调研(尤其医疗方向)

> ⚠️ **2026-06-08 框架注**:本文写作时项目级核心轴表述为"人类介入 / 自主性度量",已由 `../../goal.md` v1.0 **还原为更一般的"中间 checkpoint / 过程量度量"**(介入度降为操作化之一)。调研结论本身**完全成立**——"AI4S 方向几乎全部只评最终答案、没有现成 GT 可抄"这个全景判断在新框架下不变,只是把"人类介入轴"读作"过程量评测轴"的一个核心切面。

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | TASK-4 / `task4/` |
| **Goal 文件** | `task4/goal.md` |
| **负责人** | jingwu |
| **起止** | 2026-06-03(deep-research workflow 后台 28 分钟运行) |
| **状态** | ✅ 完成(报告已带引用、对抗式核验、4 个开放问题供下轮) |
| **上游来源** | task2 结论需要扩展寻找:有没有别人已经做过过程量度量? |
| **下游消费方** | 项目级汇总(`PROJECT_RESULT.md`)——确认差异化站位、明确可借的具体动作 |

---

## 一句话结论

> **✅ 没有任何现成 benchmark 做了我们要的"L0–L3 人类介入分级 GT"——医疗/AI4S/通用 三大方向均未发布;最值得借的过程评分模板是 TheAgentCompany(CMU,arXiv:2412.14161),它的 Collaboration checkpoint 类别可直接映射到我们的人类介入轴。**

---

## 1. Goal 对照清单(★)

- [x] **通用 agent benchmark 盘点**(SWE-bench / AgentBench / GAIA / WebArena / TAU-bench / AppWorld / TheAgentCompany / MLAgentBench)→ 报告 §1 全景表
- [x] **医疗/生物医学专向盘点**(MedAgentBench / AgentClinic / Biomni-Eval1 / LAB-Bench / BioPlanner / GeneAgent)→ 报告 §3
- [x] **AI4S/科学计算方向盘点**(ScienceAgentBench / DiscoveryBench / ChemCrow / MLAgentBench)→ 报告 §1 + §2.7
- [x] **把"做过过程量"的拎出来**(TheAgentCompany / AppWorld / MLAgentBench / DiscoveryBench / BioPlanner / AgentClinic / ScienceAgentBench)→ 报告 §2 七张深读卡
- [x] **数据可复用性 + 构造方式可借鉴性评估**→ 报告 §4 总表

---

## 2. 关键产出 / 发现

| # | 发现 | 证据 | 置信度 |
|---|---|---|---|
| 1 | **TheAgentCompany 是最直接借鉴的过程评分模板**:checkpoint+分值+确定性 Python 评测器+部分得分公式+3000 人时双人复核;`Collaboration` 类别已操作化"agent 主动求助"作为可打分过程量 | arXiv:2412.14161 §4 + Appendix C | 3-0 高 |
| 2 | **医疗方向全军覆没**:MedAgentBench / AgentClinic / Biomni-Eval1 / LAB-Bench 均只评最终答案,与 MolBench 同构 | 报告 §3 评分函数实证 | 3-0 高 |
| 3 | **AI4S 方向只有 1 个 2-点对照**:ScienceAgentBench 32.4% (无专家知识) vs 34.3% (有) | arXiv:2410.05080 §2.1 + §F.2 | 3-0 高 |
| 4 | **BioPlanner / BioProt 是生物领域唯一的步骤序列 GT**(协议 ↔ 伪代码,~1000 对);**license 风险**:repo 无 LICENSE,用前需联系作者 | arXiv:2310.10632 + GitHub API | 3-0 高 |
| 5 | **DiscoveryBench 公开"从论文人工提取 workflow"**作为 264 个真实任务的 GT;`gold_workflow` 是 eval 参数 | arXiv:2407.01725 + allenai/discoverybench | 3-0 高 |
| 6 | **AppWorld 的 collateral damage 检查**可作为我们「许可校准度」的现成实现 | arXiv:2407.18901 | 3-0 高 |
| 7 | **没有任何现成 benchmark 已做 L0–L3 graded 人类介入 GT**——差异化站位无正面竞争 | 综合 12 条 high-confidence 发现 | 综合判断 中 |

---

## 3. 下一步建议

| # | 动作 | 谁来做 | 何时 | 优先级 |
|---|---|---|---|---|
| 1 | 把 TheAgentCompany 评分三件套(checkpoint + Collaboration + 部分得分)纳入 MVP 评测器设计 | 项目维护者 | 阶段 2 | P0 |
| 2 | 用 DiscoveryBench"从论文人工提取 workflow"协议作为我们 seed 构建的标准做法 | 维护者 + 专家 | 阶段 1 | P0 |
| 3 | 把 MedAgentBench 300 / AgentClinic / Biomni-Eval1 433 当作最终答案 seed,我们自己加 checkpoint + L0–L3 | 维护者 + 专家 | 阶段 1 | P1 |
| 4 | ScienceAgentBench with/without 专家知识对照协议作为 MVP 最简版 2 点对照(在出 L0–L3 全套之前) | 维护者 | 阶段 2 | P2 |
| 5 | 补充调研(1 轮)回答报告 §7 的 4 个开放问题(尤其 BioProt license + 2025–2026 是否有未 surface 的 medical benchmark) | 维护者 | 阶段 1 前 | P1 |

---

## 4. 阻碍 / 受阻情况

| 阻碍 | 类型 | 何时发现 | 当前状态 | 待决方 |
|---|---|---|---|---|
| BioProt 数据 license 不明(repo 无 LICENSE,paper CC-BY 4.0)| 法务 | 调研中 | 待联系 Samuel Rodriques / FutureHouse | 项目负责人 |
| MedAgentBench `refsol.py` 在 Stanford Box(非仓库内)| 数据获取 | 调研中 | 已规避(用户自取);需在使用文档里说明 | 维护者 |

---

## 5. 交付物指引

| 类别 | 路径 | 说明 |
|---|---|---|
| 主报告 | `task4/benchmark_landscape_report.md` | 268 行带引用全景报告 |
| 一手论文拉取 | `prd/task4_papers/` | 10 篇关键论文 abstract / html |
| Deep-research 原始输出 | `search/task4_deep_research_raw.json` | 完整 workflow 输出(101 agent 调用、94 声明、25 核验) |
| 上游 / 下游 | `task2/result.md`、`PROJECT_RESULT.md` | 报告结论喂入项目级汇总 |

---

## 6. 对项目的轻量观察

- **TheAgentCompany 的 `Collaboration` checkpoint 几乎就是我们「人类介入轴」的最低级 1 点版本**——把它移植做我们 MVP 的"主动求助"评分,可省去自建检测协议。
- **医疗方向是 inflection point**:现有 benchmark 朴素得令人惊讶(都是 MCQ 或字符串匹配),**对"加过程量"的供给侧空白巨大**——这是我们 PRD 的现成发表理由。
- **deep-research workflow + 对抗式核验**很有效:25 条声明里 2 条被驳回(直接挽救了报告里的 2 处过度声明),证明这套方法能压住"靠论文摘要做研究"的常见偏差。
