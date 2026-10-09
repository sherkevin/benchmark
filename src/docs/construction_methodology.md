# AI4S Agent Benchmark · 构造方法(从调研到可运行 benchmark)

| 字段 | 值 |
|---|---|
| **文档** | 构造方法 —— 回答"30–50 题 MVP 怎么从调研结论变成可运行的 benchmark" |
| **上游** | `goal.md` v1.0(项目 GOAL)· `决议/0001` · task6 调研全量(36 张解读卡 + `checkpoint_palette.md` + `result.md`) |
| **下游** | R/B/E/κ 四阶段的执行手册;论文 methods 章节的骨架 |
| **状态** | 草案 v0.1(2026-06-10) |

---

## 0. 一句话

> **从 3 个可 Retrofit 的现成 benchmark(DO Challenge / Matbench Discovery / PhysicianBench 框架)出发,用「分类→找点→合成」三步法给每道题串 checkpoint,再派生 L0–L3 介入卡,最终用三层评测引擎(确定性 Python → AI 判官 jury → CEF)自动打分。**

---

## 1. 构造流水线总览

```
Phase 1        Phase 2           Phase 3         Phase 4        Phase 5       Phase 6
任务覆盖矩阵  ─→  逐题构造         ─→  工具环境       ─→  评测引擎    ─→  κ 校准     ─→  实验 & 论文
(选题)         (cp + GT + L0–L3)     (agent 运行层)     (三层打分)      (质量门)       (跑真 LLM)
 ~3 天          ~3 周(30 题)         ~1 周              ~1 周          ~1 周(滚动)    ~2 周
```

每个 Phase 的输入/输出、验收标准如下。

---

## 2. Phase 1 · 任务覆盖矩阵(选什么题)

### 2.1 覆盖目标

30–50 题,药物 ≥ 60%,材料 ≥ 30%,跨域方法题 ≤ 10%(仅用于 baseline 对照)。

### 2.2 任务类型 × 数据源矩阵(2026-06-11 POC4 走穿后修正)

> ✅ **真清楚程度修正**(POC4 PharmaBench 走穿实跑后):9 数据源里 **5 真清楚 / 2 半清楚 / 1 弱 / 1 不建议**(不再是占位)。

| 域 | 任务类型 | 来源 | 出题法 | 清楚度 | 数量目标 |
|---|---|---|---|---|---|
| **药物** | 虚拟筛选(top-K in 预算) | DO Challenge (CC-BY-4.0, 1M 分子) | **R**(改造) | 🟢 真清楚(POC1 已设计) | 3–5 |
| 药物 | 分子优化(改善某性质) | SEISMO/PMO 23 oracle (MIT) | **R**(改造) | 🟡 半清楚(cp 链未设计) | 3–5 |
| 药物 | ADMET 多属性评估 | PharmaBench 11 类实验数据 (CC0 1.0) | **B**(种子 + 自建 cp) | **🟢 真清楚(POC4 已样板,7 cp 跑通)** | 3–5 |
| 药物 | 逆合成路线规划 | Syntheseus AND/OR graph | **B**(借范式) | 🟡 半清楚(Step 1+2 需扩) | 2–3 |
| 药物 | 靶点-配体分析 / 文献综合 | MolBench seed + ChEMBL | **B**(从 workflow 抽 task) | 🟠 弱(Step 2 需 workflow→task 反向) | 3–5 |
| **材料** | 稳定性/弛豫(找基态) | MPtrj 158 万帧 (MIT) | **R**(改造,零标注) | 🟢 真清楚(POC2 已设计) | 3–5 |
| 材料 | 性质预测(Tc/带隙/形成能) | LLM4Mat-Bench 1.9M 结构 | **B**(种子 + 自建 cp) | **🟢 真清楚(与 PharmaBench 同构,POC4 方法直接套)** | 3–5 |
| 材料 | 材料筛选(从候选集选最优) | Matbench Discovery DAF | **R**(改造) | 🟡 半清楚(cp 链未设计) | 2–3 |
| 材料 | 催化 / 合金设计 | 文献 + Materials Project | **B**(DiscoveryBench 协议) | 🔴 不建议 MVP(Step 1 卡在数据散) | 0–1 |

**出题法解释**:
- **R(Retrofit)**: 在现成 benchmark 上加 checkpoint。数据和终态 GT 已有,只写 cp 判定函数 + L0–L3 提示卡。**成本最低,优先做。**
- **B(Borrow & Build)**: 从已发表论文或数据集提取 discovery workflow,自己造 cp + 写 GT。**成本中等。** **2026-06-11 起 B 类有可复用方法学**:见 [`task/task7/poc4_pharmabench/method.md`](./task/task7/poc4_pharmabench/method.md) 6 步流程。

**清楚度图例**(2026-06-11 起用):
- 🟢 真清楚 = cp 链 + L0-L3 + GT 来源全设计完,有 POC 跑通或同构样板可套
- 🟡 半清楚 = 数据/方向已定,cp 链或 Step 1+2 待设计;~2-3 天/题
- 🟠 弱 = 方法学已知但需要 Step 2 前置(workflow→task / schema 抽象);~3-5 天/题
- 🔴 不建议 MVP = Step 1 难度过高,30 题里 0-1 道占位即可

### 2.3 每题必备字段(任务 spec schema)

```yaml
task_id: drug_001
domain: drug_discovery | materials_discovery
type: virtual_screening | optimization | admet | retrosynthesis | ...
source: DO_Challenge | PharmaBench | paper:10.xxxx/yyyy | ...
license: CC-BY-4.0 | MIT | ...

instruction:
  L0: "..."   # 只给目标,不给任何过程提示
  L1: "..."   # 给工具提示(该用什么工具)
  L2: "..."   # 给 workflow 骨架(先做 X 再做 Y)
  L3: "..."   # 给完整参考路径(含预期中间值)

tools: [tool_1, tool_2, ...]      # 该题可用的工具集
budget: {type: oracle_calls, limit: 100000}  # 如有预算约束

checkpoints: [...]                 # 见 §3
ground_truth: {source: "...", values: {...}}
```

---

## 3. Phase 2 · 逐题构造(三步法,每题产出一条完整 trace)

沿用 `checkpoint_palette.md` 的三步法,这里规范化为可执行的步骤:

### 步骤 A · 分类(给 cp 定模具)

对每个任务的每个科学阶段,问三句话:

1. **能不能用脚本判?**(数值比对 / 集合交集 / 排序相关 / RDKit 解析 / 守恒律 / 工具调用)→ **Tier 1 硬校验**
2. **不能,能不能让 LLM 按细则判?**(有 rubric + 近二值判断)→ **Tier 2 AI 判官**
3. **还不能** → **Tier 3 人工**(仅做 κ 标尺,不做大规模评测)

**配额铁律:每题 Tier 1 ≥ 70%。** 不到 70% 就重新设计 cp——往往是还没挖够「守恒律 / 单调性 / 幂等 / 集合交集」这些免费硬 cp。

### 步骤 B · 找点(从数据源挖 cp)

不凭空编。按 cp 类型去对应数据集挖:

| cp 类型 | 数据源 | 挖什么 |
|---|---|---|
| 数值校验 | MPtrj 帧(力/能量)· PharmaBench(ADMET 值)· ChEMBL(pIC50) | agent 算出的中间值 vs 实验真值,定容差 |
| 集合校验 | DO Challenge labels.pkl · Matbench Discovery top-k | agent 提交集 ∩ 真值集,算 precision/recall |
| 排序校验 | SEISMO/PMO 优化曲线 · Syntheseus time-to-solution | agent 排序 vs 真排序的 Spearman/NDCG/AUC |
| 结构校验 | RDKit · pymatgen · UniProt schema | SMILES 合法 / CIF 可解析 / JSON 字段齐全 |
| 守恒校验 | 化学定律(质量守恒/电荷守恒/化学计量)· 物理约束(能量下界) | 不需要真值,违反定律 = 错 |
| 工具落地 | AgentBoard 进度公式 · PhysicianBench 工具调用 check | agent 真调了工具 + 环境状态真变了 |
| 语义判断 | HealthBench 正负分细则 · MedHELM 三判官 | agent 推理 / 报告中不可硬判的部分 |

每挖出一个 cp,记三件事:

```yaml
- id: cp3
  stage: exploration        # 科学阶段(strategy / exploration / decision / submission / reporting)
  tier: T1                  # 硬校验 / AI 判官 / 人工
  type: conservation        # 数值 / 集合 / 排序 / 结构 / 守恒 / 工具 / 语义
  description: "反应产物原子总数 = 反应物原子总数"
  weight: 1.0               # 权重(默认 1.0;关键安全 cp 可加权)
  verifier: |               # Tier 1 直接写代码;Tier 2 写 rubric
    reactant_atoms = sum(mol.GetNumAtoms() for mol in reactants)
    product_atoms = sum(mol.GetNumAtoms() for mol in products)
    assert reactant_atoms == product_atoms
  gt_source: "化学定律(零标注)"
```

### 步骤 C · 合成(串成 trace)

选一个真实科学任务,把 cp 按 agent **自然会走的科学阶段**排列:

```
策略(strategy) → 探索(exploration) → 决策(decision) → 提交(submission) → 报告(reporting)
```

每个阶段挂 1–N 个 cp。三条铁律:

1. **硬校验占 70%+**
2. **同一阶段叠不同类型 cp**(防钻空子——形式对但内容空)
3. **部分得分用加权求和,不连乘**(一步错不归零)

### 完整样题:改造 DO Challenge

| # | 阶段 | checkpoint | 档 | 类型 | 判定 |
|---|---|---|---|---|---|
| 1 | 策略 | agent 输出的靶点描述含有效 UniProt ID + 结合位点 | T1 | 结构 | regex + UniProt API |
| 2 | 探索 | 前 50K 预算后,已查分子中 precision@1000 ≥ 20% | T1 | 集合 | `\|queried ∩ top1000\| / min(len(queried),1000)` |
| 3 | 探索 | 所有被 agent 引用的分子 SMILES 可被 RDKit 解析 | T1 | 结构 | `Chem.MolFromSmiles()` |
| 4 | 探索 | 随预算消耗,best_score 单调不退化 | T1 | 排序 | 检查 submit 历史 |
| 5 | 决策 | agent 换策略的理由是否基于观测到的 SAR 模式 | T2 | 语义 | HealthBench 式 rubric |
| 6 | 提交 | 最终 3000 ∩ true_top1000 / 1000 | T1 | 集合 | `grade()` |
| 7 | 报告 | 报告每条结论是否被前面步骤数据支撑 | T2 | 语义 | rubric + 交叉验证 |

硬校验 5/7 ≈ **71%** ✓

L0–L3 介入卡(DO Challenge 样例):

| 级别 | 给 agent 什么 |
|---|---|
| **L0** | "从 100 万候选分子中找出综合得分前 1000 名。预算 10 万次查询,最终提交 3000 个。" |
| **L1** | L0 + "可用工具:lab_experiment(查分子得分,每次扣 1 预算)、remained_budget、submit。" |
| **L2** | L1 + "建议流程:先随机采样探边界 → 聚类识别高分簇 → 对高分簇做密集采样 → 筛选提交。" |
| **L3** | L2 + "参考路径:随机采 5K → Morgan 指纹聚类 → 取 top-3 簇质心做 similarity search 各 15K → 对命中最高簇 active learning 15K → ADMET 过滤 → 按得分排序提交前 3000。预期 precision ≈ 35%。" |

---

## 4. Phase 3 · 工具环境(agent 运行层)

### 4.1 架构

借 PhysicianBench 的 `tool_registry + trajectory.py` 模式:

```
agent_harness/
├── tools/                    # 工具注册表
│   ├── drug/                 # 药物域工具
│   │   ├── molecule_db.py    # 分子数据库查询(ChEMBL/ZINC)
│   │   ├── docking.py        # 分子对接(oracle 或真实)
│   │   ├── admet.py          # ADMET 预测
│   │   ├── retrosynthesis.py # 逆合成规划
│   │   └── similarity.py     # 相似性搜索
│   ├── materials/            # 材料域工具
│   │   ├── crystal_db.py     # 晶体数据库(Materials Project)
│   │   ├── property_predict.py # 性质预测(formation energy/Tc/bandgap)
│   │   ├── structure_relax.py  # 结构弛豫(oracle over MPtrj)
│   │   └── phase_diagram.py   # 相图查询
│   └── common/               # 通用工具
│       ├── literature.py      # 文献检索
│       ├── calculator.py      # 数值计算
│       └── report.py          # 报告生成
├── oracle/                    # 预算约束 oracle(借 DO Challenge client.py 模式)
│   ├── budget_tracker.py      # 统一预算管理
│   └── call_logger.py         # 每次工具调用写 JSONL
├── trajectory.py              # 借 PhysicianBench:完整 JSONL 轨迹记录
└── runner.py                  # agent 执行器(接各种 LLM backend)
```

### 4.2 关键设计

- **工具调用 = 预算消耗**:借 DO Challenge 的 `lab_experiment` 模式,每次调真实计算工具扣预算,重复查同一输入不重扣。
- **状态隔离**:借 PhysicianBench 的 `task_timestamp` before/after 分隔模式——区分"环境自带"和"agent 创建"的状态。
- **轨迹完整不截断**:借 PhysicianBench 的 `MAX_LOG_OUTPUT_LEN=0`(日志不截)。agent 发送给 LLM 的可以截断(`MAX_TOOL_OUTPUT_LEN`),但评测用的原始日志不截。
- **工具可以是 oracle(mock)**:MVP 阶段不要求接真实 DFT/docking;用 `labels.pkl` / MPtrj 预计算值做 oracle 即可。真实工具是 nice-to-have。

---

## 5. Phase 4 · 评测引擎(三层打分)

### 5.1 三层架构

```
Layer 1 · 确定性 Python (Tier 1 cp)
  → pass(1.0) / fail(0.0) / 数值分(0–1)
  → 无需外部调用,毫秒级,完全可复现

Layer 2 · AI 判官 jury (Tier 2 cp)
  → 3 个 LLM judge × per-cp rubric
  → PASS(1.0) / PARTIAL(0.5) / FAIL(0.0)
  → 取 3 judge 均值(借 MedHELM 架构)
  → 温度=0,模型版本 pin

Layer 3 · CEF 交叉验证 (reference-free,可选)
  → 借 MEDIC 框架:从 agent 产出生成布尔问题 → 用原始输入交叉验证
  → 用于没有 GT 的开放式输出(如"提出新假设")
  → 仅少量 cp 使用
```

### 5.2 评分公式

**per-checkpoint**:
```python
cp_score_i = weight_i * verifier(agent_trace, checkpoint_i)
# verifier 返回 0.0–1.0
```

**per-task at intervention level k**:
```python
# 借 TheAgentCompany 部分得分,升级为加权版
R = sum(cp_score_i for all i)          # 已得分
T = sum(weight_i for all i)            # 满分
S_full = 1.0 if all critical_cp passed else 0.0

task_score(k) = 0.5 * (R / T) + 0.5 * S_full
# critical_cp = 安全/守恒类 cp,错了就不该给高分
```

**global metrics(论文报告)**:
```python
# 1. 过程量主指标
avg_process_score = mean(task_score(L0) for all tasks)

# 2. 介入-成功率曲线(每题 4 个点)
curve_i = [task_score_i(L0), task_score_i(L1), task_score_i(L2), task_score_i(L3)]

# 3. autonomy@0 = 零介入成功率(L0 下通过所有 critical cp 的任务比例)
autonomy_at_0 = mean(S_full(L0) for all tasks)

# 4. AUC_intervention = 介入曲线下面积(越小越好 = 越少介入就够用)
AUC_intervention = trapz(curve_i, x=[0, 1, 2, 3]) / 3.0

# 5. AgentBoard 进度率(用于分析"走了多远")
PR_i = max over t of (completed_subgoals_at_t / total_subgoals)
```

### 5.3 与现有公式的溯源

| 公式组件 | 来自 | 原文位置 |
|---|---|---|
| `R/T` 部分得分 | TheAgentCompany | arXiv:2412.14161 §4 |
| `S_full` 全通过奖励 | TheAgentCompany | 同上 |
| `PR = max(Σf/K)` 进度率 | AgentBoard | arXiv:2401.13178 §3 |
| PASS/PARTIAL/FAIL 三档 | PhysicianBench | `eval_helpers.py:L670` |
| 负分细则 | HealthBench | `criterion_text + points ∈ [-10,+10]` |
| 3-judge 均值 | MedHELM | `jury_score = mean(9 scores)` |
| 复合验证器 AND | χ-Bench | `DeterministicPass ∧ JudgePass` |

**我们的升级**:加权(不等权)+ PARTIAL 透出(不压成 binary)+ L0–L3 介入维度 + 介入守恒框架。

---

## 6. Phase 5 · κ 校准(质量门)

### 6.1 Tier 2 cp 一致性校准协议

借 AutoMat 的分层盲评法:

1. 从每种 Tier 2 cp 类型抽 n=50 个 agent 输出
2. 2 位领域专家独立打 PASS/PARTIAL/FAIL
3. 算专家间 Cohen's κ → 若 κ_expert < 0.7 则 cp 的 rubric 本身有问题,先修
4. 算 AI 判官 vs 专家多数票的 κ_judge
5. **门槛:κ_judge ≥ 0.7 → 接受;κ_judge ∈ [0.5, 0.7) → 重写 rubric 再测;κ_judge < 0.5 → 降级为 Tier 3(人工)**
6. 每换一次 judge 模型版本,重测 κ

### 6.2 反污染

- **反事实扰动**:对 seed 数据做微扰(换 SMILES 中一个取代基 / 改材料组成中一个元素),保持 cp 判定逻辑不变,让 agent 不能靠记忆答对
- **污染探针**:每 10 题插 1 道"这道题的答案是 X 吗?"——如果 agent 直接说"是",说明它见过原题

### 6.3 版本纪律

- Judge 模型版本 pin(如 `gpt-4o-2024-08-06`)
- 温度 = 0
- 每次评测记录所有 API call hash
- Judge 换代 = 重测 κ = 新版本号

---

## 7. Phase 6 · 实验 & 论文

### 7.1 实验设计

- **横轴**:3–5 个前沿 LLM(如 Claude Opus / GPT-4o / Gemini / DeepSeek / Qwen)
- **纵轴**:4 个介入级别(L0–L3)
- **每格**:跑 30–50 题,每题记录 `task_score(Lk)` + 全部 cp 结果
- **关键对照**:过程量得分 vs 终答得分的 agent 排名是否不同(如果排名一样,说明过程量没测出新东西)

### 7.2 论文卖点(3 首创 + 2 已验证范式)

**首创**(调研证实无人做过):
1. **首个公开可复现的 L0–L3 分级人类介入 benchmark**(TRACE 不可复刻让出)
2. **首次落到 AI4S discovery 长程任务**(χ-Bench=医疗运营,PhysicianBench=EHR,AutoMat=论文复现,均非 discovery)
3. **加权部分得分 + 介入守恒框架**(χ-Bench/PhysicianBench 仍 binary)

**采用已验证范式**(不 claim 首创,cite χ-Bench):
- 复合验证器(确定性 ∧ 多票 LLM judge)
- 三步造 case 法(分类→找点→合成)

---

## 8. 工作量估算(MVP 30 题)

| Phase | 人力 | 瓶颈 |
|---|---|---|
| 1 覆盖矩阵 | 3 天 | 选题优先级讨论 |
| 2 逐题构造(R 类 10 题) | 1 周 | 写 cp 判定函数 + L0–L3 |
| 2 逐题构造(B 类 20 题) | 2 周 | 从论文抽 workflow + 造 GT |
| 3 工具环境 | 1 周 | oracle API 实现 |
| 4 评测引擎 | 1 周 | 三层架构 + 评分聚合 |
| 5 κ 校准 | 滚动 | 专家资源 |
| 6 实验 | 2 周 | LLM API 费用 + 调试 |
| **合计** | **~7–8 周** | 专家审核 + LLM API |

R 类题(DO Challenge / MPtrj / Matbench Discovery)可在第 1 周就出第一道可运行的题——这是 MVP 的 day-1 产出。

---

## 附:与现有文档的关系

| 文档 | 关系 |
|---|---|
| `goal.md` | 本文是 goal 中 DRBEκP 工作流 R+B+E+κ 阶段的执行计划 |
| `checkpoint_palette.md` | 本文 §3 直接引用其三步法 + 三档分类;palette 定义模具,本文定义流水线 |
| `archive/task6/result.md` | 本文 §2 的数据源矩阵 = result.md 的 MVP 路线图落地版 |
| 解读卡(36 张) | 本文 §3 "找点"的具体数据源,per-benchmark 细节在卡里 |
