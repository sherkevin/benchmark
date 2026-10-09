# task7 · 数据集改造试单(一个个试 + 实验结果落档)

> **目标**:把 task6 调研里"可改造"的数据集**一个个真试改造**,实测看到底哪些能用。每试一个,实验结果落到对应 `task7/poc_<name>/results.md`。

## ⚠️ trace 定义(用户校正,优先)

**trace = 一条完整的 ckpt 链**(演化流程 / 逻辑链):说明 agent 解题时**先做什么 → 后做什么 → ... → 终点 cp**。

> 例:**查毒性** = ① 查 PubChem 拿结构 → ② 调毒性预测器 → ③ 拿到毒性值(终点 cp)。这三步 = 一条 trace。

- 真实数据集若**没有 cp 链,只是单点 ckpt(或终态)**,就如实说"无 trace"。
- **改造目标本质 = 让那些只有 cp 点的数据集长出 cp 链**(从单点 ckpt 反推/补完链)。

---

## 候选清单(按 trace 现状分三档,改造成本从低到高)

### 第一档 · 真有原生 cp 链,改造最简(优先试)

`<数据集>` 已经把"先做什么→后做什么→终点"的逻辑链写在数据里了,我们只需 retrofit 接 LLM + 改聚合公式 / 借公式即可。

| # | 数据集 | 原生 cp 链(一句话) | 改造目标 | 状态 |
|---|---|---|---|---|
| ① | PhysicianBench | 每任务 5–8 cp 临床决策链(Data Retrieval → Reasoning → Action → Documentation) | 升加权部分得分(已 POC3 验证)+ 接真 LLM agent + 跨域 | ✅ **POC3 done**(机制层) → 见 [poc3_physicianbench/results.md](poc3_physicianbench/results.md) |
| ② | χ-Bench | 每任务 4–6 stage rubric 链(per-stage rubric 锚定政策条文)| fork verifier(Apache-2.0 可整套 lift)接我们环境 | ⏳ open |
| ③ | TheAgentCompany | 每任务多 cp 业务流程链(数据采集 → 文档 → 协作 → 提交)| lift partial-credit 公式(源码 bit-identical)+ 接 LLM | ⏳ open |
| ④ | BioAgent Bench | 7 步生物信息学流水线 cp 链(trimming → alignment → ...) | 接 LLM + 把 LLM-judge 替换为确定性 grader | ⏳ open |
| ⑤ | BioProt | 100 条协议的伪代码函数调用序列(每协议一条 trace) | lift `evaluate_metrics.py` 5 类 deterministic 指标 + 接 LLM | ⏳ open |
| ⑥ | FALAT (Who&When) | 184 条 agent 完整 history + 人工标 `mistake_step`(=链中哪个 cp 开始错) | 借失败归因方法 → 我们 trace 加 root-cause cp 标记 | ⏳ open |
| ⑦ | AgentBoard | 每实例的有序子目标序列 `[g₁,...,g_K]`(每子目标=一 cp)| 借 progress-rate 公式 `max(1/K·Σf)` | ⏳ open |
| ⑧ | AutoMat | 每 claim 的 `reproduction.txt` 文本步骤序列(SME 撰写) | 只能借 idea(repo 空壳,实物不可得);κ 校准协议可 lift | ⏳ 受阻 · 无实物 |
| ⑨ | DiscoveryBench | gold_workflow 短 NL 步骤(实测平均 ~2 步,链非常短) | 借造题协议(从论文抽 workflow → 配真值)| ⏳ open |

### 第二档 · 无原生 cp 链,但有结构化数据可造(中等成本)

`<数据集>` 提供的是单点 ckpt(终态评分)+ 一些过程数据(物理轨迹、算法 trace、API 调用),**我们要从中设计 cp 链**。

| # | 数据集 | 现状 | 我们设计的 cp 链 | 状态 |
|---|---|---|---|---|
| ⑩ | DO Challenge | 只终态评分 + budget API,**无 cp 链** | 50K 预算时 precision@1000 ≥ 20% → SMILES 全可解析 → 打分曲线单调上升 → 终态命中率 | ⏳ POC1 起步,**14MB labels.pkl 下载中**(Zenodo 卡断) |
| ⑪ | Matbench·MPtrj | 弛豫帧物理轨迹(50–200 帧/材料),**无原生决策 cp 链** | 力收敛 (mean\|F\|<0.01) → 对称性保持 → 能量单调下降 → 基态 | ⏳ open(需装 CHGNet) |
| ⑫ | Syntheseus | 算法搜索图(AND/OR 节点),**无原生决策 cp 链** | 找到首解 → 路线深度 ≤ N → 多样性 packing number | ⏳ open |
| ⑬ | SEISMO·PMO | per-step optimization trace + AUC,**无原生决策 cp 链** | best-so-far 达 0.5 时机 → 达 0.7 时机 → 终态分 | ⏳ open |

### 第三档 · 只终答,要造完整 cp 链(改造重,前面跑通再回头)

- PharmaBench(ADMET CSV)/ LLM4Mat(结构-性质对)/ MolBench·MolClaw 部分赛道 / TrialPanorama 部分任务 / ChemCrow / ScienceAgentBench
- 改造思路:把"(SMILES → 数值)"拆成"查 PubChem → 调工具 → 预测 → 比较"等 cp 链

### 不进试单(只作论文 §2 baseline 对照)

AgentClinic / MedAgentBench / LAB-Bench / Biomni-Eval1 / ALDbench / MedXpertQA / CLIMB / DAS Red-Teaming / MedAgentsBench / 医疗 LLM 综述 / Mozi · 评分组件可借:HealthBench / MedHELM / MEDIC(它们不是 retrofit 候选,是评分公式工具)

---

## 实验顺序(主线)

| 步骤 | 数据集 | 为什么这个时机 | 工作量 |
|---|---|---|---|
| 1 | ① PhysicianBench | 最快验证机制,POC3 半天 ✅ done | done |
| 2 | ⑩ DO Challenge | 数据 CC-BY-4.0 全公开 + 改造步骤已写好,跨域(医疗→药物) | 3-4 天 |
| 3 | ② χ-Bench | 真原生 cp 链 + verifier 源码可整套 lift,验证"设计孪生"接入 | 2-3 天 |
| 4 | ③ TheAgentCompany | partial-credit 公式可一字 lift(POC3 我们的加权聚合 = TAC 公式离散版) | 1-2 天 |
| 5 | ⑤ BioProt | 100 条 ingest 简单 + 5 类 deterministic 指标可 lift,验证生物步骤序列形态 | 1-2 天 |
| 6 | ⑪ Matbench·MPtrj | 装 CHGNet/fairchem(重头),材料方向 + 物理真值零标注 | 1 周 |
| 7 | ⑥ FALAT | 184 条标注样本可作失败归因器训练/校准 | 1-2 天 |
| 8 | ⑦ AgentBoard | 公式借用纯算 | 半天 |

完成 1–5 后即可写 task7/result.md 初稿,验证"跨域一致性"主张。

---

## 实验结果落档规范

- 每数据集 → `task7/poc_<name>/results.md`(自包含)
- 每个 results.md 包含:
  - 跑了什么(设置/数据/LLM/工作量实测)
  - cp 真实通过分布
  - 两种聚合对比(binary vs 加权)
  - **顺手发现**(实测过程中纠正卡里的描述)
  - 不验证什么(诚实边界)
  - 下一步候选(加深 or 跳下一个)

## 状态总览(本文是主跟踪表,跑完一个更新一行)

| 试 # | 数据集 | 实验报告 | 进度 |
|---|---|---|---|
| 1 | PhysicianBench | [poc3/results.md](poc3_physicianbench/results.md) | ✅ done(2026-06-10) |
| 2 | DO Challenge | poc1/results.md | ⏳ data 下载中 |
| 3+ | 其他 | — | open |
