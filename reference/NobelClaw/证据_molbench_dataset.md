# MolBench / MolClaw 数据集事实性核查（调研记录）

> 目的：为 task2「MolBench 能否改造成我们的自主性度量 benchmark」提供**基于真实数据集与代码**的事实底座，
> 而非仅凭微信通稿的二手描述。所有结论均来自实际抓取的论文元数据、GitHub 仓库文件与源码。
> 抓取时间：2026-06-02。原始文件存于 `docs/prd/molbench_repo/`。

## 0. 来源与可信度

| 来源 | 标识 | 状态 |
|---|---|---|
| 论文 | arXiv **2604.21937v2**（2026-04-02，更新 2026-05-17）| ✅ 元数据已核 |
| 标题 | *MolClaw: An Autonomous Agent with Hierarchical Skills for Drug Molecule Evaluation, Screening, and Optimization* | ✅ |
| 代码/数据 | github.com/**InternScience/MolClaw**（MIT，main 分支，26★，244 个文件）| ✅ 全树已遍历 |
| 论文 comment | **"Code and data will be released"** | ⚠️ 即「部分释放」，见 §3 |

作者：Lisheng Zhang, Lilong Wang, Xiangyu Sun, Wei Tang, Haoyang Su … Bowen Zhou, Lei Bai, Zhengwei Xie（上海AI实验室 PJLab + 北大方向）。

## 1. 论文摘要里的关键定性（决定改造方向）

- MolClaw = 自主 agent，三层技能架构（**70 个技能**）：L1 原子工具 / L2 工作流（带 quality check & reflection）/ L3 学科方法论。
- MolBench = benchmark，含 **molecular screening、optimization、end-to-end discovery**，跨度 **8 到 50+ 次顺序工具调用**。
- 消融结论：收益**集中在需要结构化工作流的任务**，在「临时脚本即可解」的任务上消失 → 把**工作流编排能力**认定为 AI 药物发现的主瓶颈。
- ⚠️ 摘要**只字未提污染控制**，被测任务用已知分子（与我们 PRD §3 对 MolBench 的判断一致）。

## 2. 实际释放的数据集结构（**这是判断「能否提取过程量」的核心**）

仓库 `molbench/data/` 实际只有两大类、**共 190 条**样本，全部是**单轮 `prompt → 最终答案` QA**：

| 子集 | 任务 | 实际条数 | 字段 | ground truth 形态 |
|---|---|---|---|---|
| **ms-1** | 理化性质过滤（Lipinski+阈值，从候选 SMILES 选全部满足者）| 50 | `prompt, answer` | 确定性集合（满足约束的 SMILES 行）|
| **ms-2** | 靶点感知的结合亲和力比较 QA（A/B 谁 Ki 更高）| 37 | `question, answer, target` | 二选一 SMILES |
| **ms-3** | 虚拟筛选式排序 / hit 识别（对 60 个候选排序）| 25 | `index, questions(JSON,含candidates[60]), task_type, answer, answer_score, n_active` | 活性子集 + 每分子打分 + 活性数 |
| **mo-edit** | 官能团 add/delete/sub（改写 SMILES）| 10+9+20=39 | `gt, id, meta, query, subtask, task` | `meta.reference`（目标 SMILES）|
| **mo-opt** | 理化性质优化（logp/qed/solubility，迭代提升指标）| 11+20+8=39 | `gt, id, meta, query, subtask, task` | 计算式（输出分子的指标 vs 源分子）|

**关键事实**
1. **MO 来自 ChemCoTBench**：`manifest.json.source_root = .../ChemCoTBench/chemcotbench`；`molbench/eval/ChemCoTBench` 是 git 子模块。即 MO 是对第三方 ChemCoTBench 的重打包+采样（seed=42, 每任务采 40）。
2. **manifest 声明 119 条 MO，但实际只释放 78 条**：`mol_opt_target/{drd,gsk,jnk}.json`（40 条靶点优化）**在树中缺失**。
3. **`gt` 字段在 MO 全部为空字符串**——真值放在 `meta`（编辑类）或靠计算（优化类）。
4. MS/MO 的 `query/prompt` **已经写得相当详细**（指定输入分子、约束、**强制 JSON/行输出格式**）——按我们 PRD §2.3 的分级，更接近 **L2/L3 档**（目标+步骤/格式都给全），不是 L0。

## 3. **E2E（端到端发现）——通稿大书特书的部分，没有释放**

- 全树 244 个路径，正则搜 `e2e|end-to-end|traj|trace|checkpoint|gold|process|intervention` 命中 **0 个数据文件**。
- `config/` 只有 `ms-1/2/3 + mo_edit + mo_opt` 六个，**没有任何 e2e 配置** → E2E 既无数据也不可运行。
- 微信通稿描述的「MolBench-E2E 9 个难度递进任务（构象采样→迭代优化→双靶点从头设计→自主发现科学问题；最多 15 轮、连续 3 轮无进展须换策略）」**只存在于论文/通稿叙述，未进开源数据**。
- 影响：E2E 恰恰是**唯一真正长程、多阶段、可能携带过程量**的部分，却拿不到。

## 4. 评测逻辑：**纯最终答案打分，零过程量**（源码实证）

- **输出契约**（`molclaw_run/templates/answer_format.md`）：最终答案必须包在 `<finish>` 内的 `<answer>...</answer>`，**"with no extra commentary"**。
- **数据加载**（`bench_loaders.py` 文件头自述）：*"get_query returns query as-is, parse extracts from `<answer>` and writes to pred"* → 单轮进、抽 `<answer>` 出。
- **评测器**（`eval_runner.py`，1.4k 行）只做最终量度量：
  - MS：`hit@3`、排序指标、集合 precision/recall（`_list_metrics`）；
  - MO：`improvement delta`（指标增量）+ `scaffold_consistency`（骨架一致性 hard/soft）；
  - 抽取失败计 0。
  - 全文件 grep `traj|checkpoint|intermediate|tool_call|process|intervention|reflect` → **0 命中**。
- 结论：**benchmark 既不存储也不评分 agent 的运行轨迹**。agent 跑动时确有多步工具调用（运行期 `<finish>` 之外的中间动作 transient 存在），但**数据集与评测都把它丢弃**，只留最终 `<answer>`。这正是我们 PRD 批评的「只看最终结果」。

## 5. 仓库里**真正有「过程结构」的东西**——但在 skills/，不在 data/

虽然**数据集**无过程量，**技能库**却把专家过程知识显式写了出来（这对我们改造极有价值）：

- **`skills/L2_workflows/00–13`**（14 个工作流）= **通用「金标准过程模板」**。以 `05-iterative-molecular-optimization.md` 为例，显式含：
  - 阶段分解：`Evaluate → Diagnose → Design → Verify` 四步闭环；
  - **显式 CHECKPOINT**：「CHECKPOINT after Step 1/2/4」「CHECKPOINT before final report」（Quality Gates 小节，带勾选项）；
  - 每阶段**成功判据**（如 docking 改善 ≥0.5 kcal/mol；QED Δ≥+0.1）；
  - **Common Failures & Recovery** 表（失败→原因→恢复策略）；
  - **优化轨迹表**要求（每轮 SMILES/分数/策略/源文件，可溯源）；
  - 决策逻辑（`IF success … ELIF round<max … ELSE`）。
- `00-problem-discovery-and-feasibility-triage.md`、`11-multi-target-selectivity-assessment.md` 等同样是阶段化、带门控的流程。
- **`skills/L1_tools/`**（约 60 个 `SKILL.md`）= **动作空间**（docking/admet/boltz2/esmfold/fpocket/reinvent…），定义了「一次工具调用」长什么样。
- **`skills/L3_methodology/`** = 学科原则（被 L2 以 "L3 Principle N" 反复引用，如 Principle 11 Count-Before-Report、13 Computation-first）。

⚠️ 重要限定：这些 L2/L3 是**任务类型层面的通用模板**（"优化类任务该怎么走"），**不是每个 benchmark 实例的、带具体期望数值的金标准轨迹**。它们是**给我们搭脚手架的 rubric 骨架**，不是现成的 per-instance 过程标注。

## 5.5 数据获取方式：论文有详述,GitHub 有部分标注(回答「数据怎么来的」)

**论文 §"MolBench Dataset" 把每个子集的来源与构造协议写得很清楚**,GitHub 用 config 的 task 标签 + manifest 的 source_root 做了部分标注。汇总:

| 子集 | 一手来源 | 构造方式(论文原文要点) | GitHub 标注 |
|---|---|---|---|
| **MS-1** 性质过滤/结构相似 | **CARA** lead-opt 子集(ref58,源自 ChEMBL)| 按 Assay ChEMBL ID 分组,保留 ≥10 SMILES 的 assay;每 assay 采 10 个候选;算 11 个 RDKit 描述符;Lipinski + 按分位数取 2–3 条额外约束;迭代重采样保证 1–5 个满足。相似度题:1 query+9 候选,Morgan/Tanimoto 定真值 | config task=`rdkit_bench` |
| **MS-2** 亲和力比较 | **ACNet**(ref69,含实验 Ki)| 均匀采样选 **37 个非冗余靶点**,每靶点 1 个分子对;真值=实验 Ki;exact SMILES 匹配 | config task=`acnet_curated` |
| **MS-3** 对接/虚拟筛选排序 | **CARA** 子集(ref58)| 筛 assay:≥60 分子、≥6 活性(pChEMBL≥6)、≥50 非活性 → **25 个 assay**;每个采 60 分子(6–10 活性+非活性)打乱;真值=活性按实验 pChEMBL 排序;指标 Hits@3 | config task=`molbench_vs` |
| **MO** 编辑+理化优化 | **ChemCoTBench**(ref36/55)| 从「分子编辑」「理化优化」两个子任务采样,**剔除歧义/欠定项**;seed=42,每任务采 40 | config dataset=`ChemCoTBench`;manifest `source_root=.../ChemCoTBench/chemcotbench` |
| **E2E** 端到端(3 题)| 自建(基于 EGFR 1M17 / Erlotinib 等公开结构)| 见 §5.6;但**数据/rubric 未开源**,只在论文正文+附录 | 仓库无 |

**关键补充事实**
- **drd/gsk/jnk(靶点优化 40 条)为何 manifest 有、却没释放**:论文明说——预实验发现「LLM 主要靠**背诵已知药化策略**而非真推理」来解这三题,为避免记忆污染混淆跨模型对比,**主动剔除**。↔ 这与我们 PRD §2.1/§5.4 的「污染/记忆」担忧**同源**,但他们的对策是「删题」,我们的对策是「反事实扰动 + 污染探针」,更彻底。
- **数据是 `seed=42` 从公开 benchmark 采样**而来 → 可复现、可重采样、可扩展。
- **Data Availability 声明**(论文):「MolBench dataset(CSV format)与评测代码可从 GitHub 获取」「skills 与配置也在 GitHub」。⚠️ 该声明**不完整/略有出入**:(a) 只说 CSV,但 MO 实为 JSON;(b) **E2E 完全不在 GitHub**;(c) arXiv comment 仍是「Code and data will be released」(滚动/部分释放)。
- **可追溯到一手源**:CARA(→ChEMBL)、ACNet、ChemCoTBench 都是公开数据集。**我们可绕过 MolBench 的二次打包,直接去这些一手源自取 seed**,这正对应我们 PRD §2.2 的种子来源(ChEMBL/BindingDB 等)。

## 5.6 E2E 的评测方式:**是「过程 rubric」打分,不是只看最终答案**(重要)

虽然 E2E 数据未开源,但论文写明其评测协议,这是 MolBench 内**唯一面向过程量**的部分:

- 每题给:靶点规格 + 起始分子/结构 + 明确成功/失败阈值 + 规定输出格式(**结构化 log 文件 + 终报告**)。
- 评分=**任务专属 rubric**,把每题拆成加权准则,跨 **5 个维度**:
  1. 正确的**工具选择与调用顺序**;
  2. **中间推理的科学有效性**;
  3. 跨步骤的**数据完整性与参数一致性**;
  4. **恰当的终止行为**;
  5. 交付物完整性。
- 全部任务规格、分阶段流程、评分 rubric 在 **Supplementary Tables S5–S7**(论文附录,**不在仓库**)。

→ 含义:E2E 在「评分维度」上**已经触及过程量**(工具序列、中间推理、数据一致性、终止行为),比 MS/MO 先进。但它**仍不是我们的 benchmark**:没有「人类介入程度」这根轴,没有 L0–L3 prompt 分级、没有分级介入卡、没有中断分类、没有介入-成功率曲线;它是对**单次自主跑**的静态加权 rubric,且只有 3 题、数据与 rubric 未公开。

## 5.7 实查下载到本地的数据集 vs 我们 goal 的需求(逐文件核对)

> 不只看论文描述,而是把本地 `docs/prd/molbench_repo/data_full/` 的每个文件**逐条打开看了**。结论:**不满足**我们 goal 的需求。下为record级证据。

**逐文件实查**
- **MS-1(50)**:1 个固定候选表(10 个 SMILES)+ 固定约束 → 输出满足子集(答案分子数 min1/中位1/max5)。**单轮、一次 RDKit 即可解**;prompt 连输出格式都写死。论文自承 vanilla agent ≥96%,**区分度低**。
- **MS-2(37,33 个唯一靶点)**:靶点 + 2 个分子 → 选 Ki 更高那个,输出 1 个 SMILES。**二选一 QA,随机基线 50%**,任务极短。
- **MS-3(25)**:60 个候选 → 按对接分排序;`answer`=活性子集,`answer_score`=活性分子的实验 pChEMBL(如 `[8,7.4,...]`),`n_active`=6–10,指标 Hits@3,`task_type` 全为 `zero-shot`。**这是唯一带「中间数值真值」的子集**,但 benchmark item 仍是「一条 prompt → 一个排序列表」单轮。
- **MO-edit(39)**:单分子 + 官能团 → add/delete/sub;`meta.reference`=正确改写后 SMILES,`gt`空。**单步确定性编辑**。
- **MO-opt(39)**:单分子 → 提升 QED/LogP/Solubility;`meta`含 `src_iupac`+`type`(small/middle/large),`gt`空。
  - ⚠️ **最有说服力的一条证据**:MO-opt 的 prompt 原文写着 *"while following a **structured intermediate optimization process**"*——**明确要求中间过程**,但数据只存/只评 `"Final Target Molecule"` 这一个 SMILES(delta + 成功率)。**即 MolBench 连自己 prompt 里点名的过程量都丢弃了**,只留最终分子。这正是我们要补的缺口的活样本。

**需求对照表(我们 goal ⇄ 实查数据)**

| 我们 goal 需要的 | 实查数据是否提供 | 判定 |
|---|---|---|
| 长程多阶段任务(8–50+ 工具调用,才看得出自主性)| MS/MO 全是**单轮、0–1 次工具** | 🔴 不满足 |
| 目标明确性 ⊥ 路径指导性 → L0–L3 prompt 分级 | prompt 固定单档、目标与格式捆绑;任务太短无「任务分解」可分级 | 🔴 退化 |
| 带阶段 checkpoint 的金标准轨迹 | 无;只有 `meta.reference`(终)或可算指标 | 🔴 不满足 |
| 过程量字段(工具序列/中间数值/失败重试)| 仅 MS-3 `answer_score` 是中间数值真值,其余只有最终答案 | 🟡 极少 |
| 运行期介入/中断结构 | 完全没有 | 🔴 不满足 |
| 抗污染 / 反事实可扩展 | 用已知分子、无污染控制;但**可被我们扰动扩展**(换靶/换起点/改阈值)| 🟡 需我们做 |
| 对照裁判可锚定的 ground truth | 最终答案层面:MS-3 pChEMBL、MO 可算、MS-1 确定性 → 有;**过程偏离判定**所需金轨迹:无 | 🟡 半 |
| 能区分 agent 自主性 | MS-1/MO-edit 脚本即可解(区分度低);MS-2 二选一短任务 | 🔴 不满足 |

**可直接拿来当原料的(record 级)**:MS-3 的 `candidates[60]`+`answer_score`+`n_active`(真排序任务+数值锚)→ 可嵌进长任务的「筛选」阶段并锚定该 checkpoint;MS-2 的 33 靶点 + activity-cliff 分子对 → 选择性/反事实对的种子;MO 的源分子+`meta` → 优化类任务种子(需重构为 L0 多轮)。

## 6. 一句话事实结论

> **释放的 MolBench 数据集 = 190 条单轮 QA，最终答案打分，无任何过程量、无轨迹、无 checkpoint、无介入结构；唯一长程的 E2E 未释放。但 MolClaw 仓库的 L1/L2/L3 技能库，提供了可复用的「过程脚手架 + 种子 + 动作空间」。**

→ 详见交付件 `task2/result.md` 的可行性结论与改造方案。
