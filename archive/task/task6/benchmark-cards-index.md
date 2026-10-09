---
tags:
  - Contents
  - Agent
  - Benchmark
  - AI4S-benchmark
type: meta
---

> [!important] 用 Obsidian 打开本页(不要用 VSCode)
> 本页靠 `[[wikilink]]` 卡片链接 + `dataview` 自动表工作,**只有在 Obsidian(库根 = `~/papers`)里打开才会渲染**。在 VSCode / 纯文本编辑器里,卡片链接是死文本、dataview 是代码块,会**看起来一片空白**——那是渲染问题,不是卡片真的空。卡片本体在 `~/papers/<名>.md`,也可 `python3 ~/papers/scripts/pq.py --tag AI4S-benchmark --paths` 直接取。

> [[Contents/Agent/@Contents|← 返回 Agent 主控页]] · 上级总览 [[Contents/Agent/Evaluation-Benchmark|通用 Agent 评测]] —— **本页 = 其「科学发现 + 医疗(AI4S)」专项子索引**(通用版的领域深化分支)

# 🧪 AI4S / 医疗 Agent Benchmark 索引

> **这是什么**:`work/benchmark`(task6)项目调研 + 用户 Cubox `medical-benchmark` 收藏沉淀下来的 benchmark 解读卡的**唯一分类入口**。每个 benchmark = 库根目录一张卡(`tags: AI4S-benchmark`,医疗/生物类另加 `medical-benchmark`),本页是主分类表 + 自动索引。
>
> **项目主线**:做「AI4S(药物 + 材料)长程 agent benchmark,带中间 checkpoint + L0–L3 分级人类介入」。这些卡是它的先行工作 / 可复用素材库。项目侧文档见 `work/benchmark/task6/`(本页与之互链,见末尾)。

## 怎么用这页

- **想按主题捞**:`python3 ~/papers/scripts/pq.py --tag AI4S-benchmark --paths`(医疗子集:`--tag medical-benchmark`)。
- **想要通俗 case 版「怎么改 / 怎么用 / 差异化分析」**:看 `work/benchmark/task6/result.md`(本任务对外成品)。
- **想知道 checkpoint 怎么分档 + 怎么造 trace**:看 `work/benchmark/task6/checkpoint_palette.md`。

## 亲核图例

| 标记  | 含义                                                      |
| --- | ------------------------------------------------------- |
| ✅   | 一手亲核(读了源码 / 实际数据 / arXiv 原文)                            |
| 🟡  | 二级来源(deep-research / cubox / 知乎 digest),核心数字或已 arXiv 核对 |
| ⏳   | P0 待亲核(关键、影响论文写法)                                       |
| ⚠️  | 单源待验(仅 Gemini 或单一渠道提及)                                  |

**对我们(用途)**:`R`=可 Retrofit 改造成我们的过程量题 · `B`=借方法/公式/架构 · `seed`=作题目真值数据源 · `baseline`=论文 §2 对照(只评最终答案)· `先例`=设计孪生/理论先例。

---

## 主分类表(唯一事实源)

### 🧬 药物 / 临床试验

| Benchmark                | 验证什么                                   | 对我们                                            | 亲核      | 卡片                |
| ------------------------ | -------------------------------------- | ---------------------------------------------- | ------- | ----------------- |
| **DO Challenge** ⭐       | 离散 cp(集合交集,预算约束探索)                     | **R 改造基石**(MVP 药物虚筛第一题)                        | ✅       | [[DO Challenge]]  |
| **Syntheseus**           | 轨迹连续量(time-to-solution + 单调 diversity) | R / B                                          | ✅       | [[Syntheseus]]    |
| **SEISMO / PMO**         | 轨迹 AUC(优化曲线)                           | B + seed(23 oracle)                            | ✅       | [[SEISMO-PMO]]    |
| **SDDBench**             | 多步 round-trip 标量                       | B 范式(**真无实物**,匿名审稿)                            | ✅ audit | [[SDDBench]]      |
| **PharmaBench**          | 最终答案(ADMET 实验数据)                       | seed(11 类 ADMET)                               | ✅       | [[PharmaBench]]   |
| **MolBench / MolClaw**   | 4 赛道(性质过滤/结合比较/虚筛排序/分子优化),终答 GT        | seed + B(L1 工具可 cp 进 skills)                   | ✅       | [[MolBench]]      |
| **TrialPanorama**        | 最终答案(8 任务)                             | seed(1.6M 试验,**数据全开放 Apache-2.0**)             | ✅ audit | [[TrialPanorama]] |
| **BioProt / BioPlanner** | 步骤序列 GT(协议→伪代码)                        | **B 借范式**(首选)· seed(仅内部,实为 100 条 / license 真空) | ✅ audit | [[BioProt]]       |

### 🔬 材料

| Benchmark                        | 验证什么                                     | 对我们                                                         | 亲核      | 卡片                     |
| -------------------------------- | ---------------------------------------- | ----------------------------------------------------------- | ------- | ---------------------- |
| **AutoMat** ⭐                    | 5 维 holistic rubric + κ=0.69(**非离散 cp**) | **B 借 idea**(κ 校准协议)· 材料唯一过程量 agent benchmark,**但代码/数据未公开** | ✅ audit | [[AutoMat]]            |
| **Matbench Discovery + MPtrj** ⭐ | 最终答案 + **158 万 DFT 弛豫帧**                 | R(MPtrj 过程量金矿)/ B(DAF)                                      | ✅       | [[Matbench Discovery]] |
| **LLM4Mat-Bench**                | 最终答案(1.9M 结构)                            | seed                                                        | ✅       | [[LLM4Mat-Bench]]      |
| **ALDbench**                     | 70 道开放问答(原子层沉积,专家 rubric)                | baseline(非 seed,无步骤 GT)                                     | ✅       | [[ALDbench]]           |
|                                  |                                          |                                                             |         |                        |

### 🏥 医疗 / 生物(另带 `medical-benchmark` tag)

| Benchmark | 验证什么 | 对我们 | 亲核 | 卡片 |
|---|---|---|---|---|
| **PhysicianBench** ⭐ | 离散 cp(670 个,4 类 taxonomy) | **R 改造基石**(最低成本 retrofit) | ✅ audit | [[PhysicianBench]] |
| **χ-Bench / CHI-Bench** ⭐ | 复合验证器(确定性 ∧ judge,**纯 binary**) | **设计孪生** + B(verifier 源码可借)· 三条 delta 全干净 | ✅ audit | [[CHI-Bench]] |
| **BioAgent Bench** | 进度率(steps_completed/steps_to_completion,LLM 估分母) | R(借模式不借评分;grader 代码未公开) | ✅ audit | [[BioAgent Bench]] |
| **BioXArena** | 最终答案(承诺释放轨迹) | R(待轨迹) | ✅ | [[BioXArena]] |
| **HealthBench Professional** | rubric(支持负分) | B(rubric 模板) | ✅ | [[HealthBench]] |
| **MedHELM** | LLM-jury(3 判官×3 轴) | B(jury 架构) | ✅ | [[MedHELM]] |
| **MEDIC / CEF** | 参考免评测 + 安全 | B(无 gold 也能评) | ✅ | [[MEDIC]] |
| **AgentClinic** | 最终答案 / 顺序决策 | baseline(弹药:顺序化最弱掉<1/10 + 静态分无预测力 Fig3)| ✅ audit | [[AgentClinic]] |
| **MedAgentBench** | 终答 + FHIR 结构重查/payload assert(强语法硬校验) | baseline(数据=STARR 真实非 Synthea;refsol 404 社区 fork 可恢复) | ✅ audit | [[MedAgentBench]] |
| **Biomni-Eval1** | 终答字符串/字母精确匹配(零过程量) | baseline / 弱 seed(污染高;433 条/10 类已坐实) | ✅ audit | [[Biomni-Eval1]] |
| **LAB-Bench** | MCQ 终答(含工具族子任务 SeqQA/DbQA) | baseline | ✅ | [[LAB-Bench]] |
| **MedAgentsBench** | 最终答案(862 Hard 多步推理,无 cp) | baseline + B(难度蒸馏法) | ✅ | [[MedAgentsBench]] |
| **AI Hospital / MVME** | MVME 5 维事后打分(过程雏形,非 in-flight cp) | baseline | ✅ | [[AI Hospital]] |
| **CLIMB** | 多模态临床数据源(4.51M/19TB,非 agent) | seed(弱相关,无 LICENSE) | ✅ | [[CLIMB]] |
| **MedEvalKit** | 统一评测 runner(24 benchmark,纯静态 QA) | B(runner 架构可借) | ✅ | [[MedEvalKit]] |
| **DAS Red-Teaming** | 4 安全轴红队(鲁棒/隐私/偏见/幻觉) | B(安全 cp,κ=0.952 judge 校验法) | ✅ | [[DAS Red-Teaming]] |
| **MedXpertQA** | 最终答案(专家级 MCQ,4460 题) | baseline(难度池) | ✅ | [[MedXpertQA]] |
| **医疗 LLM benchmark 综述** | 综述地图(~300 数据集,无过程量评测) | 索引用 + 空白佐证 | ✅ | [[Medical LLM Benchmark Survey]] |

### 🛠️ 方法 / 通用(借公式与架构)

| Benchmark | 验证什么 | 对我们 | 亲核 | 卡片 |
|---|---|---|---|---|
| **AgentBoard** ⭐ | 进度率公式(单调,max-over-history) | B(过程量主指标) | ✅ | [[AgentBoard]] |
| **TheAgentCompany** | 部分得分公式 + Collaboration cp | B(评分模板) | ✅ audit | [[TheAgentCompany]] |
| **TRACE** | λ_min 分级介入(理论先例,Eq.1–9 全核) | B(L0–L3 理论身;**超参§A.3全给,但数据/代码从未公开**) | ✅ audit | [[TRACE]] |
| **FALAT / Who&When** | 失败归因(dependency search) | B(失败归因) | ✅ | [[FALAT]] |
| **DiscoveryBench** | gold_workflow | R | ✅ | [[DiscoveryBench]] |
| **ChemCrow** | 论文级 demo | baseline | ✅ | [[ChemCrow]] |
| **AstaBench** | 科学研究套件 | baseline / 参考 | ✅ | [[AstaBench Rigorous Benchmarking of AI Agents with a Scientific Research Suite\|AstaBench]] |
| **ScienceAgentBench** | 最终答案(102 任务) | baseline ⭐ | ✅ | [[ScienceAgentBench Toward Rigorous Assessment of Language Agents for Data-Driven Scientific Discovery]] |
| **MLAgentBench** | trace.json + 失败分类 | B | ✅ | [[MLAgentBench Evaluating Language Agents on Machine Learning Experimentation]] |

> ⭐ = 与我们主线最近 / MVP 直接候选。完整「怎么改、改多久、隐患 + 差异化分析」见 `work/benchmark/task6/result.md`。

---

## 自动索引(库内 `#AI4S-benchmark` 全卡)

```dataview
table venue, time, source
from #AI4S-benchmark
where !contains(file.name, "Contents") and !contains(file.name, "@") and type != "report"
sort time desc
```
> (上表自动排除 `type: report` 的 14 份 audit 亲核报告;它们经各自卡片的 `[[audit_*]]` 链接进入。)

医疗子集:

```dataview
table venue, time
from #medical-benchmark
sort time desc
```

---

## 与项目(task6)的关系

| task6 文档 | 作用 | 与本页关系 |
|---|---|---|
| `result.md` | **本任务对外成品**:case 驱动「怎么改/怎么用」+ 3 档先例分析 + 3 首创面 | 把本页的卡翻译成通俗汇报 + 差异化论证 |
| `checkpoint_palette.md` | checkpoint 三档分类 + 三步造 trace | 卡里的"验证什么"在这里被串成题 |
| `task6/archive/survey_landscape.md` | 原始 deep-research 存档 | 卡的素材来源 |

**关联 synthesis**(库内,本项目专属):[[Contents/Syntheses/autonomy-benchmark/related_work|autonomy-benchmark related_work]](Paper A 先行工作核查)· [[Contents/Syntheses/medical-agent-autonomy-audit/related_work|medical-agent-autonomy-audit]]。

> 来源说明:🟡 cubox/知乎卡的核心数字已用 arxiv-MCP 一手核对;知乎正文抓取链路见 `work/.claude/skills/zhihu-fetch`。χ-Bench 复合验证器内部机制 ✅ 已亲核(2026-06-09,arXiv 全文 + repo `actava-ai/chi-bench` 源码,见 [[audit_chibench]])。
