# 开源 Agent 测评 Benchmark 全景调研报告(task4)

| | |
|---|---|
| **子任务** | task4 ——「调研所有开源 agent 测评 benchmark,尤其医疗方向;有没有过程量度量的例子;能否复用 data 或借鉴构造方式」 |
| **方法** | deep-research workflow:5 个角度并发搜索 → 19 个一手源 → 94 条声明 → **25 条经 3 投票对抗式核验,23 通过、2 被驳** |
| **下游需求** | `prd/AI4S_agent_autonomy_metrics.md`(原 PRD;项目级 GOAL 现已统一到 `../../goal.md` v1.0,主轴由"人类介入度"还原为更一般的"过程量 / 中间 checkpoint",本报告"人类介入轴"列读作"过程量评测轴的核心切面"即可,findings 不变)|
| **原始拉取** | `prd/task4_papers/`(10 篇关键论文 abstract / html) |
| **调研笔记** | `search/task4_deep_research_raw.json`(完整 workflow 输出) |
| **更新日期** | 2026-06-03 |

---

## TL;DR(一页结论)

1. **过程量度量很少,真正能拿来用的更少。** 在所调研的全景中,**绝大多数 agent benchmark(包括所有医疗/生物医学方向)仍是「只看最终答案」**——结构上与 MolBench 释放的形式同构。少数几个做了过程量,但定义、形态、是否带「人类介入轴」差异很大。
2. **最值得借鉴的过程评分模板 = `TheAgentCompany`(CMU,arXiv 2412.14161)。** 它把每个任务拆成**专家撰写的 checkpoint(带分值)**、用**确定性 Python 评测器** + 必要时 LLM-judge 评分,公开了**部分得分公式**和 **3000 人时**的策展协议;尤其有一个 `Collaboration` checkpoint 类别,显式给 agent 主动向(模拟)人类发起澄清打分——这正好映射到我们「人类介入度量」的子方向。⚠️ 任务本身是数字员工(SWE/HR/PM/Finance),**不是 AI4S**——评分方法可借,任务数据不是科学 seed。
3. **医疗/生物医学方向全军覆没。** `MedAgentBench`、`AgentClinic`、`Biomni-Eval1`、`LAB-Bench` **全部只评最终答案**;没有任何一家发布带 checkpoint 的金标准轨迹或人类介入轴。
4. **AI4S 方向只有 1 个「2 点 ≠ 曲线」的人类介入实验**:`ScienceAgentBench`(ICLR'25)报告"无专家知识 32.4% vs 有专家知识 34.3%"——这是我们能找到的**唯一**已发表的 with/without expert-knowledge 对照,但是**静态、2 点、不是 L0–L3 分级**。
5. **可复用的具体动作(按价值排序)**:
   - **(a) 借** `TheAgentCompany` 的 checkpoint+rubric+`Collaboration` 类别+部分得分公式作为**评分模板**;
   - **(b) 复用** `BioProt`(EMNLP'23)的"协议 ↔ 伪代码"对作为**步骤序列 GT** 的现成范例,并复用 `DiscoveryBench` 264 个**从已发表论文人工提取的 discovery workflow** 作为 seed 脚手架;
   - **(c) 复用** `MedAgentBench` 的 300 个医生撰写 FHIR 任务、`AgentClinic` 的 sentinel-tagged 病例、`Biomni-Eval1` 的 433 实例,**作为最终答案 seeds,我们自己加 checkpoint + L0–L3**——因为没有现成的 L0–L3 介入分级 GT。
6. **诚实的结论**:**没有任何现成 benchmark 已经做了我们要的「人类介入分级 GT」**。最佳路径是"借评分模板 + 复用 seed + 自建过程量产物",和 task2 给 MolBench 的判断结构一致。这不是坏消息——它说明我们的差异化站位(自主性/介入度量)在这条赛道上**没有正面竞争者**。

---

## 第 1 部分 · 全景一览(按"评分对象"分类)

| Benchmark | 领域 | # 任务 | 评分对象 | 有轨迹 GT? | 有人类介入轴? | License | 数据可直接复用? | 构造方式可借鉴? |
|---|---|---|---|---|---|---|---|---|
| **TheAgentCompany** [^TAC] | 数字员工(SWE/HR/PM/Fin) | 175 | **Checkpoint(带分值)+ 部分得分** | ✅ checkpoint-级 | 🟡 `Collaboration` 类别 | 公开 | ❌(非科学) | ⭐⭐⭐ 评分模板 |
| **AppWorld** [^AppWorld] | 数字生活(9 apps/457 APIs) | 750 | 状态-based 单元测试(含 collateral damage) | 🟡 状态而非轨迹 | ❌ | Apache-2.0 | ❌(非科学) | ⭐⭐ 多步 harness |
| **MLAgentBench** [^MLAB] | ML 实验自动化 | 13 | 最终性能(≥10% 改进=成功)| 🟡 轨迹**只 log 不评**(`trace.json`) | ❌ | MIT | 🟡 ML 任务 | ⭐⭐ 轨迹格式 |
| **DiscoveryBench** [^DBench] | 数据驱动科学发现 | 264 真+903 合成 | Facet-decomposed 最终假设 | ✅ `gold_workflow` 作 eval 参数 | ❌ | Apache-2.0 | 🟡 当 seed | ⭐⭐⭐ 从论文提取 workflow |
| **BioPlanner / BioProt** [^BioPlanner] | 生物实验协议 | ~1000 对 | 步骤序列(伪代码重建) | ✅ 整个 GT 就是步骤序列 | ⚠️ paper CC-BY,repo 无 LICENSE | 🟡 待澄清 | ⭐⭐⭐ 协议→伪代码 |
| **AgentClinic** [^AgentClinic] | 医疗诊断 | 多 case set(MedQA-/NEJM-derived) | **只评最终诊断字符串** | 🟡 sentinel 可恢复(stdout) | ❌ | MIT | 🟡 当 seed | ⭐ sentinel 协议 |
| **MedAgentBench** [^MedAB] | 医疗 EHR(FHIR) | 300 | **最终成功率** | ❌ | ❌ | MIT(refsol 在 Box) | 🟡 当 seed(医生写) | ⭐⭐ 医生撰写协议 |
| **Biomni-Eval1** [^Biomni] | 生物计算 | 433 实例 × 10 任务 | **字符串/letter/OMIM/gene-set 匹配** | ❌ | 公开 | 🟡 当 seed | ⭐ 任务多样性 |
| **LAB-Bench** [^LABBench] | 生物文献/实验推理 | 多选题 | **`accuracy/precision/coverage` 聚合** | ❌ | 公开 | ❌(MCQ) | ❌ |
| **ScienceAgentBench** [^SAB] | AI4S 数据分析 | 102 | 最终程序+运行结果 + **2 点 with/without 专家知识对照** | ❌ | 🟡 1 点静态人类介入 | 公开 | 🟡 当 seed | ⭐ 介入对照协议 |
| **ChemCrow** [^ChemCrow] | 化学 agent 系统 | 14 任务 case study | EvaluatorGPT + 4 专家 Likert | ❌ | 论文 demo | 公开 | ❌ | ⭐ 工具清单 |
| **GeneAgent** [^GeneAgent] | 基因集功能注释 | 1106 gene sets | ROUGE/MedCPT/Likert(最终叙述) | ❌ | 公开 | 🟡 GO 集污染风险 | ❌ | ⭐ self-verify 模块 |

[^TAC]: TheAgentCompany, Xu et al., CMU, arXiv:2412.14161 (v3 Sep 2025) · https://arxiv.org/abs/2412.14161 · https://the-agent-company.com
[^AppWorld]: AppWorld, Trivedi et al., ACL 2024 Best Paper, arXiv:2407.18901 · https://arxiv.org/abs/2407.18901 · https://appworld.dev
[^MLAB]: MLAgentBench, Stanford, arXiv:2310.03302 · https://github.com/snap-stanford/MLAgentBench
[^DBench]: DiscoveryBench, AI2, arXiv:2407.01725 · https://github.com/allenai/discoverybench · https://huggingface.co/datasets/allenai/discoverybench
[^BioPlanner]: BioPlanner / BioProt, Rodriques et al., EMNLP 2023, arXiv:2310.10632 · https://github.com/bioplanner/bioplanner
[^AgentClinic]: AgentClinic, Schmidgall, https://github.com/SamuelSchmidgall/AgentClinic
[^MedAB]: MedAgentBench, NEJM AI 2025, arXiv:2501.14654 · https://github.com/stanfordmlgroup/MedAgentBench
[^Biomni]: Biomni / Biomni-Eval1, Stanford SNAP, https://github.com/snap-stanford/Biomni
[^LABBench]: LAB-Bench, Future-House, arXiv:2407.10362 · https://github.com/Future-House/LAB-Bench
[^SAB]: ScienceAgentBench, ICLR 2025, arXiv:2410.05080
[^ChemCrow]: ChemCrow, arXiv:2304.05376 / Nature MI 2024
[^GeneAgent]: GeneAgent, arXiv:2405.16205

---

## 第 2 部分 · 「做过过程量度量」的 Benchmark 深读卡

只列**真正测量过程量**的(状态-based 单元测试、轨迹 log、checkpoint、步骤序列 GT)。每张卡:**任务形态 / 过程量怎么定义 / GT 形态 / 是否含人类介入轴 / 我们能借什么**。

### 2.1 ⭐⭐⭐ TheAgentCompany(最值得借鉴)

- **任务形态**:在自托管的"模拟公司"(GitLab + RocketChat + ownCloud + Plane)里做 175 个数字员工任务(SWE/HR/PM/Finance/Admin)。
- **过程量定义**:每个任务被拆成**多个 checkpoint(带显式分值)**;每个 checkpoint 由**确定性 Python 评测器**检查环境状态或轨迹片段(浏览历史、动作序列、git 提交、聊天记录…),非结构化交付物用 LLM-judge(Claude-3.5-Sonnet)按预制 rubric 打分。
- **GT 形态**:**专家撰写的 checkpoint 列表**(每条:期望状态/动作 + 分值 + 评测器代码),**3000 人时**策展 + 独立双人复核。
- **人类介入轴(关键!)**:有一个显式的 `Collaboration` checkpoint **类别**,把"agent 是否在恰当时机向(模拟)同事/上级发起澄清/求助"作为**可打分的过程量**。论文给的具体例子:Finance 任务的最后一个 checkpoint 给 3 分,**当且仅当 agent 联系了正确的财务总监 David Wong 来解决两个有歧义的问题时**得分。评测器直接检查 RocketChat 消息——这是真正的轨迹度量,不是最终答案打分。
- **部分得分公式**:`S_partial = 0.5·(Result/Total) + 0.5·S_full`(Result=已得分,Total=满分,S_full=完成全部 checkpoint 的奖励)。
- **我们能借什么**:
  - ✅ **整套 checkpoint+rubric+评测器三件套**直接当我们 `task3/A` 模板的工程化范本;
  - ✅ **`Collaboration` 类别**几乎就是我们「能力性求助 vs 许可性中断」分类的现成正例,**可作为「人类介入轴」的最低级演化版**(虽然它不是 L0–L3 分级,但思路一致);
  - ✅ **3000 人时双人复核** = 我们 PRD §5.1 "专家审核制"的现成案例(他们也走的就是这条路);
  - ❌ 任务本身不是科学,无法当 seed。
- **诚实校准**:对抗式核验过程中,**有一条把 TAC 的部分得分公式说成"现成介入-成功率曲线"的声明被 1-2 投票驳回**。它是**静态部分得分聚合器,不是介入-vs-成功率曲线**——可以借,不要过度套用。

### 2.2 ⭐⭐⭐ BioPlanner / BioProt(生物领域唯一的步骤序列 GT)

- **任务形态**:1000 多对"生物实验自然语言协议 ↔ 有序伪代码(可允许函数调用序列)"。
- **过程量定义**:**整个 GT 就是步骤序列**——从高层描述 + 可允许函数列表中,模型要重建对应的伪代码序列;评分=步骤匹配度。
- **GT 形态**:每条 = `id, title, original-description, ai-generated-description, steps(NL), generated_pseudocode(类型化函数调用序列), number-of-steps, optional edited_pseudocode`。
- **人类介入轴**:❌
- **作者自我定位(关键引用)**:论文摘要原文 ——
  > *"evaluation of the accuracy of scientific protocols is challenging, because experiments can be described correctly in many different ways, require expert knowledge to evaluate, and cannot usually be executed automatically."*
  > 解决方案:用 LLM 把 NL 协议转伪代码,再用伪代码重建任务做评测。
  > **作者明确说该框架可推广到其他 AI4S 领域。**
- **我们能借什么**:
  - ✅ **"协议 → 伪代码 → 步骤序列 GT"这套范式**,正是我们 PRD §2.6 "纯过程评分"在不能直接执行的领域的现成解;
  - ✅ 数据可作为 seed(若 license 厘清);
  - ⚠️ **license 风险**:论文 CC-BY 4.0,但 GitHub 仓库 `license:null`(GitHub API 确认),**重发布前需联系作者(Samuel Rodriques / FutureHouse)澄清**;
  - ⚠️ **静态 vs 交互**:这是**一次性伪代码重建**,不是 WebArena 式的运行时轨迹——作为 checkpoint-shape GT 的范例足够,作为"运行时轨迹评测已存在"的证据**不够**。

### 2.3 ⭐⭐⭐ DiscoveryBench(从论文人工提取 workflow 的范例)

- **任务形态**:264 个真实任务(+ 903 合成)跨 6 领域;每题让 agent 提出一条数据驱动假设。
- **过程量定义**:**Facet-based 分解**——把假设 `h := ψ(c, v, r)` 拆成 context / variables / relationship 三个面,任意一面错都扣分;**`gold_workflow`** 作为 eval 参数公开 (`--gold_workflow` / `--add_workflow_tags`)。
- **GT 形态**:**真实任务由人工从已发表论文中提取 discovery workflow**——这正是我们 PRD §2.2 "从 seed 到反事实扩展" 第二步("从 seed 抽取真实路径整理为带阶段 checkpoint 的金标准轨迹")的现成做法。
- **人类介入轴**:❌
- **我们能借什么**:
  - ✅ **"从已发表论文人工提取 workflow"协议**——可直接照搬到我们的种子构建;
  - ✅ Facet-decomposition 思路:把开放式答案拆成可分别打分的面,**避开纯字符串匹配的脆弱**;
  - 🟡 `gold_workflow` 是分析流水线参考、不是逐轮 agent 轨迹——作为我们 checkpoint 的脚手架可,作为现成 GT 不可。

### 2.4 ⭐⭐ AppWorld(状态-based 单元测试,处理 collateral damage)

- **任务形态**:60K 行代码的"模拟数字生活"沙盒(9 apps/457 APIs/~100 用户),750 个多步任务。
- **过程量定义**:**状态-based 单元测试**(post-execution world state):**既检查正确性,又检查 collateral damage**——这是少有的把"agent 没多干什么不该干的"列为评测维度的工作。
- **GT 形态**:per-task Python 单元测试。
- **人类介入轴**:❌
- **我们能借什么**:
  - ✅ "collateral damage" 检查思路:可作为我们「许可校准度」(PRD §2.5)的现成实现——agent 在不该自主的高风险操作上是否乱来;
  - ✅ 多 API 沙盒架构可参考(尤其当我们要把 agent 接入真实工具时)。

### 2.5 ⭐⭐ MLAgentBench(轨迹 log 但不评)

- **任务形态**:13 个 ML 实验任务(CIFAR-10 改进 → BabyLM 等)。
- **过程量定义**:eval pipeline 写 `trace.json` + workspace 快照 + agent state——**轨迹完整记录**;但 headline 指标是**最终快照**(性能 ≥10% 改进 = 成功;Claude-3 Opus 37.5% 平均成功率)。
- **GT 形态**:基线性能;每步动作的预期形态没有 GT。
- **人类介入轴**:❌
- **我们能借什么**:
  - ✅ **轨迹 log 格式**(`trace.json` schema)直接当我们运行期插桩的输出格式;
  - ✅ Per-timestep 性能曲线(论文 Figure 3)和失败模式分类(Hallucination / Bad Plan / Format Errors)是**诊断**,可启发我们的过程评分维度。

### 2.6 🟡 AgentClinic(sentinel 协议,但不评轨迹)

- **任务形态**:Doctor-Patient-Measurement 多 agent 互动诊断。
- **过程量定义**:**sentinel-tagged 动作**——agent 必须按格式发 `REQUEST TEST: [test]` / `REQUEST IMAGES` / `DIAGNOSIS READY: [diagnosis]`;循环检测这些字符串。
- **GT 形态**:**只评最终的 `DIAGNOSIS READY` 字符串**(`compare_results()` 让 moderator LLM 答 Yes/No)。
- **关键观察**:每轮被 append 到 `agent_hist`,**stdout/agent_hist 字符串可解析出工具调用序列**——也就是**底层场景可以离线重新打分,不必重跑 agent**。
- **我们能借什么**:
  - ✅ **sentinel-tagged 动作协议**=我们 L1 工具调用动作空间的轻量替代(让 agent 必须显式声明动作);
  - ✅ **可被离线重新评测的场景**=低成本 seed(我们重打分,不必重跑);
  - ❌ 没有结构化 tool-call 日志文件,要做 stdout 解析。

### 2.7 🟡 ScienceAgentBench(AI4S 唯一的人类介入对照)

- **任务形态**:102 个 AI4S 数据分析任务。
- **过程量**:**没有逐步过程量**;**有 1 点静态人类介入对照**——
  > 论文摘要:*"the best-performing agent can only solve **32.4%** of the tasks independently and **34.3%** with expert-provided knowledge."*
- **专家知识 = 什么**:任务输入的 4 大组件之一(`Expert-Provided Knowledge`):科学术语解释、分析公式、编程工具示例,**由资深 PhD/教授静态提供**——是任务设置时的固定 hint,**不是运行时介入**。
- **我们能借什么**:
  - ✅ **with/without 专家知识对照协议**=我们 L0–L3 介入-成功率曲线的最简 2 点版本(L3 静态版),可以作为最低门槛的可发表对照;
  - ❌ 但**不是分级 L0–L3**;**不是运行时介入**;论文也**不**自称是"人类介入"度量(这是我们的类比重述,需明确标注);
  - 重要校准:对抗式核验中,**有一条把 SAB eval 描述为"final program + run output"的声明被 0-3 投票驳回**——实际评测形态比这更精细,报告里不该靠这个简化说法支撑结论。

---

## 第 3 部分 · 医疗 / 生物医学方向单列

(用户特别关注的方向。**结论:全军覆没——没有任何一家做过程量度量。**)

| Benchmark | 任务规模 | 评分函数实证(从仓库直接看) | seed 价值 |
|---|---|---|---|
| **MedAgentBench** [^MedAB] | 300 患者特定任务 × 10 类,基于 100 个 FHIR 患者档案 + 70 万数据元素;由医生撰写 | `eval.py` 调用 `grader_func(case_data, results, fhir_api_base)` 返回 binary True/False;`calculate_overall` 仅算成功率;**`refsol.py` 在 Stanford Box,需用户自取**(redistribution 障碍) | 🟡 高,医生撰写 + Docker 化 FHIR + 大规模真实数据 |
| **AgentClinic** [^AgentClinic] | MedQA / NEJM-derived 病例集 | `compare_results()` 让 moderator LLM 答 Yes/No,指标 = `total_correct/total_presents`;只评最终诊断 | 🟡 中,场景可离线重打分 |
| **Biomni-Eval1** [^Biomni] | 433 实例 × 10 任务 | `evaluate(task_name, instance_id, user_answer:str)→float`;`_compute_reward()` 只做 string / letter / OMIM_ID / gene-set 匹配 | 🟡 中,生物计算多样性 |
| **LAB-Bench** [^LABBench] | 多选题 | `agent_fn(input)→str` 返回单字母;评测器只返聚合 `{accuracy, precision, coverage}`;**per-instance dict 没有 `tool_calls / reasoning_trace / sub_goal / human_intervention` 任何字段** | ❌ MCQ 形态 |

> **共同特征**:**评分函数都是 binary / 字符串匹配,数据 schema 里没有 `trajectory / checkpoint / sub_goal / intervention` 字段。** 这与 task2 给 MolBench 的判断**完全一致**——医疗方向当下的 SOTA benchmark 与 MolBench 是**同一种结构性局限**。
>
> **直接含义**:我们要做的「自主性 / 人类介入」度量在医疗 agent 方向**确实没有正面竞争**;但同样,**没有现成 GT 可抄**——必须自己加 checkpoint 和 L0–L3。

### 医疗方向数据可复用清单(我们当 seed 用)

| 来源 | 复用形态 | 我们补什么 |
|---|---|---|
| MedAgentBench 300 任务 | seed:医生撰写的 FHIR 操作目标 + 患者档案 | checkpoint 序列(临床推理阶段)、L0–L3 prompt、介入卡 |
| AgentClinic 病例 + sentinel 动作协议 | seed:多 agent 诊断对话场景 | 用 sentinel 序列做轨迹打分 + 加 `Collaboration` 类型介入(向模拟同事求助) |
| Biomni-Eval1 433 实例 | seed:生物计算多样性 | 同上,但要先做污染评估(GO 等公共本体易被记忆) |

---

## 第 4 部分 · 数据可复用性 + 构造方式可借鉴性(总表)

### 4.1 数据可复用(作为 seed)

| 数据源 | 规模 | License | 可直接复用? | 备注 |
|---|---|---|---|---|
| MedAgentBench 测试集 | 300 task × FHIR | MIT(refsol 在 Box) | ✅ seed | 需用户自取 refsol |
| AgentClinic 病例 | 多套 case set(JSONL) | MIT | ✅ seed | sentinel 动作离线可解析 |
| Biomni-Eval1 | 433 实例 × 10 类 | 仓库公开 | ✅ seed | 注意 GO 集污染 |
| LAB-Bench | 多选题 | 公开 | ❌ MCQ 形态不合 | |
| BioProt 伪代码对 | ~1000 | ⚠️ paper CC-BY,repo `license:null` | 🟡 待澄清 | **重发布前必须联系作者** |
| DiscoveryBench 真任务 | 264 | Apache-2.0 | ✅ seed + workflow | gold_workflow 当脚手架 |
| ScienceAgentBench | 102 AI4S 任务 | 公开 | 🟡 seed 候选 | 并提供 with/without 专家知识对照 |

### 4.2 构造方式可借鉴

| 协议 | 来源 | 可借鉴的具体动作 |
|---|---|---|
| **Checkpoint + 部分得分** | TheAgentCompany | 直接当 `task3/A` 工程化模板;Python 评测器优先,LLM-judge 兜底 |
| **`Collaboration` checkpoint 类别** | TheAgentCompany | 把"agent 主动求助"作为可打分的过程量;映射到我们能力性求助 |
| **3000 人时双人复核** | TheAgentCompany | 我们 PRD §5.1 "专家审核制"的现成案例 |
| **协议 → 伪代码** | BioPlanner | 不能执行的科学领域,用 LLM 把 NL 转成可评测的步骤序列 |
| **从已发表论文人工提取 workflow** | DiscoveryBench | 我们 seed 构建第二步的现成做法 |
| **Facet-based 评分** | DiscoveryBench | 开放式答案分面打分,避开字符串匹配脆弱 |
| **state-based 单元测试 + collateral damage** | AppWorld | 我们「许可校准度」的现成实现 |
| **with/without 专家知识对照** | ScienceAgentBench | 我们 L0–L3 介入-成功率曲线的 2 点最简版 |
| **sentinel-tagged 动作** | AgentClinic | 让 agent 显式声明动作,降低过程量解析成本 |

---

## 第 5 部分 · 与 MolBench、与我们 PRD 的对照

| 维度 | MolBench(task2) | 医疗/生物医学 SOTA | 我们 PRD 要的 |
|---|---|---|---|
| 评分对象 | 最终答案 | 最终答案 | **过程量为主**(checkpoint 通过率+介入次数+autonomy@0)|
| 长程性 | MS/MO 单轮;E2E 未释放 | 多步 harness 有(MedAB/AgentClinic),但不评过程 | **多阶段长程是主条件** |
| 阶段 checkpoint | 无(L2 模板是任务类型级)| 无 | **per-instance 必备** |
| 人类介入轴 | 无 | 无 | **核心轴**(L0–L3 + 介入卡 + 介入-成功率曲线)|
| 反污染 | 用已知分子,无控制 | 同样;Biomni 有 GO 污染风险 | **反事实扰动 + 污染探针** |
| 现成可借的最强者 | MolClaw 仓库的 L1/L2/L3 脚手架 | 无领域内现成 | **跨领域借** TheAgentCompany |

**结论**:我们的差异化站位(**自主性/介入度量**)在调研到的全景中**没有正面竞争者**——医疗、AI4S、通用 agent 三大方向均**未发布带 checkpoint + 人类介入分级 GT 的 benchmark**。这是机会,也意味着**没有现成 GT 可抄,必须自建**。

---

## 第 6 部分 · 操作建议(对项目)

按"最低成本 → 最高价值"排序:

1. **评分模板直接借 TheAgentCompany**:把 `Checkpoint + 分值 + Python 评测器 + 部分得分公式 + Collaboration 类别` 整套搬到我们的 evaluator 设计;`task3/B` 已经是这个方向的雏形。
2. **种子优先用 DiscoveryBench 的协议**:对每个 seed,**手工从原始论文中提取 discovery workflow** —— 这就是我们 PRD §2.2 第二步,DiscoveryBench 已证明这条路可行,且公开了 264 个范例。
3. **医疗方向 seed 三件套**:MedAgentBench 300 任务 + AgentClinic 病例 + Biomni-Eval1 433 实例,**作为最终答案 seeds 我们自己加 checkpoint**(用 TheAgentCompany 的 schema)。注意 license 自取问题与 GO 污染。
4. **2 点介入对照 = MVP 基线**:在出 L0–L3 全套之前,先做 ScienceAgentBench 风格的 with/without 专家知识 2 点对照——成本极低,可作为论文里的"最简版本"对比强项。
5. **不能执行的领域用 BioPlanner 做法**:把 NL 协议转成伪代码作为 GT;前提是 license 澄清。
6. **collateral damage 借 AppWorld**:作为「许可校准度」的现成实现。
7. **再做一轮针对性补充调研**(下一步,本任务范围外):2025–2026 是否有未surfaced 的新医疗/AI4S benchmark 含 checkpoint?(开放问题列表见 §7。)

---

## 第 7 部分 · 调研留下的开放问题(明确、可下一轮回答)

1. **TheAgentCompany 全 175 任务集是否有任何科研类(literature review / data wrangling / plotting)与 AI4S 重叠?** 若有,有少量任务数据也可借,而不只是评分方法。
2. **BioProt 数据 license 到底能不能重发布?** 联系 Samuel Rodriques / FutureHouse。
3. **2025–2026 是否有我们没找到的医疗/生物 agent benchmark 已加 checkpoint GT?** 包括中文实验室发布(类似我们正在做的 work)。
4. **DiscoveryBench 的 `gold_workflow` 字段粒度够不够?**(伪代码级 vs 段落级)——需直接拉数据看一行。

---

## 附录 A · 调研方法的诚实校准

- **5 个搜索角度**:`general-agent-benchmarks-process-metrics` / `medical-biomedical-agent-benchmarks` / `ai4s-science-agent-process-evaluation` / `trajectory-rubric-construction-protocols` / `human-intervention-autonomy-metrics`
- **流程**:5 路并发 WebSearch → 19 个一手源 fetch → 抽取 94 条声明 → 取 top 25 做 3 投票对抗式核验(2/3 反对则 kill)→ 23 通过、2 被驳。
- **被驳 2 条(已剔除,本报告未引用)**:
  1. "TheAgentCompany 部分得分公式 = 现成介入-成功率曲线"(1-2 驳;它是静态聚合器,不是曲线);
  2. "ScienceAgentBench 评测 = final program + run output 简化二值"(0-3 驳;实际更精细)。
- **核验未覆盖的范围**:本报告基于 2026-06-03 的仓库 main 分支与已发表论文;后续版本可能加过程量字段,**重大引用前应再 cross-check**。
- **类比性的明确标注**:`TheAgentCompany.Collaboration` 与 `ScienceAgentBench.expert-knowledge` 都不是原作者用"人类介入轴"命名的——结构相似是真实的(求助=介入;静态 hint=粗 1 点介入),但**原论文都没声称做"分级自主性度量"**——这是我们的类比重述,在文中已标注。
- **不在范围**:本任务**只产出报告**,不搭原型(用户明确边界)。

## 附录 B · 信源(已 fetch 的一手资料)

| 论文 ID | 名称 | 本地 |
|---|---|---|
| arXiv:2412.14161 | TheAgentCompany | `prd/task4_papers/2412.14161.html` |
| arXiv:2407.18901 | AppWorld | `prd/task4_papers/2407.18901.xml` |
| arXiv:2310.10632 | BioPlanner | `prd/task4_papers/2310.10632.html` |
| arXiv:2407.01725 | DiscoveryBench | `prd/task4_papers/2407.01725.html` |
| arXiv:2310.03302 | MLAgentBench | `prd/task4_papers/2310.03302.xml` |
| arXiv:2410.05080 | ScienceAgentBench | `prd/task4_papers/2410.05080.html` |
| arXiv:2501.14654 | MedAgentBench | `prd/task4_papers/2501.14654.xml` |
| arXiv:2304.05376 | ChemCrow | `prd/task4_papers/2304.05376.xml` |
| arXiv:2405.16205 | GeneAgent | `prd/task4_papers/2405.16205.xml` |
| arXiv:2407.10362 | LAB-Bench | `prd/task4_papers/2407.10362.xml` |

完整 deep-research 输出(94 声明、25 核验、信源全表):`search/task4_deep_research_raw.json`。
