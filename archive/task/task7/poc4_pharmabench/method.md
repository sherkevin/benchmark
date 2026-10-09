# 「论文 → cp 链」6 步方法学 · POC4 PharmaBench 走穿

> **职责**:把"一篇带数据集的论文"系统转成"一道带 checkpoint 链 + L0-L3 介入卡 + 可机判分的 agent benchmark 题"。可复用到剩下 4 个 B 类(Syntheseus / LLM4Mat-Bench / MolBench seed / 文献+MP)。
>
> **状态**:草案 v0.1(2026-06-11)· **走穿前**(Day 1 起稿;Day 3 跑通后回写「踩了什么坑」节)。

---

## 0. 一句话

> 从一篇带数据集的论文出发,**6 步走出一道可机判分的过程量 agent benchmark 题**:解析数据 → 定 e2e 任务 → 拆 5 阶段 → 挖 cp(Tier 1≥70%)→ 锁 GT 来源 → 派生 L0-L3。每步必须有一个可验收的产物。

---

## 流水线总览

```
Step 1     Step 2          Step 3       Step 4         Step 5      Step 6
解析数据   定 e2e 任务      拆 5 阶段    逐阶段挖 cp     锁 GT 来源   派生 L0-L3
形态       (不是单点 QA)    (策略→报告)  (Tier 1≥70%)   + license   介入卡
~0.5d      ~0.5d            ~0.2d        ~1d            ~0.3d       ~0.5d
```

每步「输入 → 动作 → 输出 → 验收」全闭环,**任一步过不了不允许进下一步**。

---

## Step 1 · 解析数据形态

| 字段 | 内容 |
|---|---|
| **输入** | 论文 + 配套数据 repo(github / zenodo / supplementary) |
| **动作** | 用 schema-mining(列名 / 字段类型 / split 标签 / metric 代码)拆出真实数据形态 |
| **输出** | 一行 schema 摘要 + 数据规模 + 已结构化程度(三档:csv/json 已结构化 / 半结构化 jsonl / 散在论文文本) |
| **验收** | 能在 1 屏内说清"这数据每条长什么样、有多少、可分什么 split" |

### PharmaBench 实例

- **schema**: 5 列 csv (`index, Smiles_unify, value, property, scaffold_train_test_label, random_train_test_label`)
- **规模**: 52,482 条 / 11 类 ADMET / 8.0 MB
- **结构化档**: ✅ **完全结构化** csv(最容易档)
- **数据出处可溯**: scaffold + random 两种 split,seed=2 可复现
- **关键缺口**: 0 行 metric/baseline 代码(`audit_pharmabench.md:125`)→ **必须自建评测**

**反例(Step 1 不通过的样子)**:Syntheseus 的逆合成数据 = AND/OR graph,不是表;LLM4Mat-Bench 1.9M 结构 = parquet,大但需自动 NL 描述层。两者 Step 1 都要先做"schema 抽象"才能进 Step 2。

---

## Step 2 · 定一个 end-to-end 任务

| 字段 | 内容 |
|---|---|
| **输入** | Step 1 的 schema |
| **动作** | 选一个"agent 在真实科研里会做的多步活动",而**不是把数据当 QA 题逐条问** |
| **输出** | 一句话任务描述 + 输入(给 agent 什么)+ 输出(要 agent 交什么) + 资源约束(预算/工具/时长) |
| **验收** | 任务自然分得出 ≥3 个阶段(策略/探索/决策/提交/报告里至少占 3 个) |

### PharmaBench 实例

**反例**(单点 QA,❌ 不要):"这分子的 logD 是多少?" — 一问一答没过程

**正例**(e2e 任务,✅):
> 给你 50 个候选小分子(SMILES,从 PharmaBench logd test 集分层抽样),其中部分 logD 在成药窗口 [1, 3]、部分外。可调用 `predict_logd(smiles)` 工具,**预算 30 次预测(60% 覆盖)**。**输出 logD ∈ [1, 3] 的所有分子 SMILES,并给出每个的预测值与依据。**

- **输入**: 50 SMILES 列表 + 工具签名
- **输出**: SMILES 子集 + 预测值表 + 决策依据文字
- **资源约束**: 30 次预测预算(模拟"实验员有限")
- **自然阶段**: 策略(怎么用预算) / 探索(批量预测) / 决策(阈值过滤) / 提交(集合) / 报告(rubric)— **5 阶段全占** ✓

---

## Step 3 · 拆 5 阶段(策略→探索→决策→提交→报告)

| 字段 | 内容 |
|---|---|
| **输入** | Step 2 任务描述 |
| **动作** | 把 agent 自然会走的活动拆成 5 个标准阶段;不强求每阶段都有,但缺的要说明为啥 |
| **输出** | 5 阶段表,每阶段一句话说"这阶段 agent 干什么、产出什么" |
| **验收** | 5 阶段贯通能讲一个完整故事;无跳跃、无重复 |

### PharmaBench 实例

| 阶段 | agent 干什么 | 产出 |
|---|---|---|
| **策略** | 解析任务,识别需 logD 预测 + 规划怎么用 30 次预算 | 策略文字(自然语言) |
| **探索** | 调 `predict_logd` 工具,挑选 25-30 个分子查 | 工具调用 trace + 预测值表 |
| **决策** | 用阈值 [1, 3] 过滤预测值落在窗口内的分子 | 候选子集 |
| **提交** | 输出最终 SMILES 列表 | submission 文件 / JSON |
| **报告** | 对每个提交分子说明决策依据 | 自然语言报告 |

---

## Step 4 · 逐阶段挖 cp(Tier 1 ≥ 70% 配额铁律)

| 字段 | 内容 |
|---|---|
| **输入** | Step 3 五阶段表 |
| **动作** | 每阶段挖 1-3 个 cp;对每个 cp 走「三句话筛」 |
| **输出** | cp 链表(每行:阶段 / 描述 / 档 / 类型 / 判定方法) |
| **验收** | 总 cp 数中 Tier 1 占比 ≥ 70%;不达标重新设计(往往是没挖够「守恒/单调/集合/结构」) |

**三句话筛**(每个 cp 必走):
1. 能脚本判吗?(数值比对/集合交集/排序相关/RDKit 解析/守恒律/工具调用)→ **Tier 1 硬校验**
2. 不能 → 能让 LLM 按 rubric 判吗?(近二值判断 + 细则)→ **Tier 2 AI 判官**
3. 还不能 → **Tier 3 人工**(仅做 κ 标尺,不大规模)

### PharmaBench 实例 cp 链(7 个 cp,Tier 1 占 5/7 = 71% ✓)

| # | 阶段 | cp 描述 | 档 | 类型 | 判定方法 |
|---|---|---|---|---|---|
| 1 | 策略 | 策略含「批量预测 → 筛选阈值 → 提交」三动作 | T2 | rubric | LLM judge(strategy_rubric.md) |
| 2 | 探索 | 调用 `predict_logd` 次数 ≤ 30(预算守恒) | T1 | 守恒 | trace 计数比较 |
| 3 | 探索 | 调用次数 ≥ 25(覆盖率,避免少查瞎选) | T1 | 守恒 | trace 计数比较 |
| 4 | 探索 | 引用的所有 SMILES RDKit 可解析 | T1 | 结构 | `Chem.MolFromSmiles()` |
| 5 | 决策 | 提交集中无真值 ∉ [0.8, 3.2] 的离谱项(容差 0.2) | T1 | 数值 | csv 真值比对 |
| 6 | 提交 | precision = \|提交 ∩ 真正例\| / \|提交\| ≥ 0.6 | T1 | 集合 | F1-style |
| 7 | 报告 | 报告对每个提交分子给出预测值依据 | T2 | rubric | LLM judge(report_rubric.md) |

**Tier 1 占比 71%** ✓ — 满足铁律。

**反例(Tier 1 不够 70% 时的样子)**:如果一开始把"agent 推理合理性"全归 T2 LLM judge,Tier 1 可能只到 40%;**必须回头挖**——"预算守恒""SMILES 合法""提交集 precision"这些都是免费 Tier 1,该挖没挖就是漏。

---

## Step 5 · 锁 GT 来源 + license

| 字段 | 内容 |
|---|---|
| **输入** | Step 4 cp 链表 |
| **动作** | 对每个 cp 明确:① 真值从原始数据哪里来 ② license 允许我们重发布吗 ③ 真值是否可能污染(LLM 训练时见过) |
| **输出** | cp 真值溯源表 |
| **验收** | 每个 cp 真值能溯源到原始 csv/json 行号 + license 兼容(CC-BY / MIT / CC0 OK;arXiv-only no derivative ❌)|

### PharmaBench 实例

| cp # | GT 来源 | license | 污染风险 |
|---|---|---|---|
| 1 (策略 rubric) | 人编 rubric | — | 无 |
| 2 (预算守恒) | trace 自身 | — | 无 |
| 3 (覆盖率) | trace 自身 | — | 无 |
| 4 (SMILES 合法) | RDKit 库 | BSD | 无 |
| 5 (容差) | `logd_test_sample.csv:value` 列 | **CC0 1.0** ✓ | 中(LLM 可能记 logD)→ 用匿名 ID + scaffold split 抗污染 |
| 6 (集合 F1) | `logd_test_sample.csv:value` 列 | **CC0 1.0** ✓ | 中,同上 |
| 7 (报告 rubric) | 人编 rubric | — | 无 |

**关键发现**:PharmaBench CC0 1.0 = **可重发布**(`audit_pharmabench.md:148`),所以 logd_test_sample.csv 50 分子子集可作为 POC4 数据资产入库,无 license 风险。

---

## Step 6 · 派生 L0-L3 介入卡

| 字段 | 内容 |
|---|---|
| **输入** | Step 2 任务描述 + Step 4 cp 链 |
| **动作** | 任务级写 L0-L3 prompt;每阶段写 D1-D3 介入卡(深度递增) |
| **输出** | L0-L3 prompt 4 段 + 各阶段 D1-D3 介入卡(共 5×3 = 15 张) |
| **验收** | L3 含「参考路径 + 预期中间值」;D3 含「明确动作 + 数值锚」(借 `drug_admet.json` 模板风格)|

### PharmaBench 实例

**任务级 L0-L3**:

| 级别 | 给 agent 什么 |
|---|---|
| **L0** | "从这 50 个分子中选 logD 在 1-3 的全部分子。预算 30 次预测。" |
| **L1** | L0 + 工具签名:`predict_logd(smiles: str) -> float`、`submit(smiles_list: list)` |
| **L2** | L1 + 流程提示:批量预测 30 个代表性分子 → 阈值 [1, 3] 筛选 → 提交 |
| **L3** | L2 + 参考路径:① scaffold 多样性采样选 30 个有代表性的(避免类簇集中)② 调用 predict_logd 30 次记表 ③ 按 \|pred - 2\| 排序 ④ 阈值 [1, 3] 过滤 ⑤ 提交全部命中者。预期 precision ≥ 0.7。 |

**stage 级 D1-D3 介入卡**(借 [`drug_admet.json:intervention_cards`](../../../src/tasks/drug_admet.json) 风格):

```json
{
  "explore": {
    "D1": "你查的分子数偏少/偏多,复审预算用法。",
    "D2": "建议查 25-30 个不同 SMILES,覆盖代表性结构。",
    "D3": "查 30 次,优先选 Murcko scaffold 多样性高的(避免同类簇重复采)。"
  },
  "filter": {
    "D1": "你的过滤阈值或方向不对,复审。",
    "D2": "用 [1, 3] 的硬阈值过滤所有 logD 预测值。",
    "D3": "把预测值落在 [1, 3] 闭区间的所有 SMILES 全提交;边界值(0.99/3.01)也算 out。"
  }
}
```

**省略原则**:策略 / 提交 / 报告阶段 D1-D3 在 task_logd_screen.json 里给全;此处不重复。

---

## 复用性判断(走穿后回写)

> ⏳ POC4 Day 3 跑完后必须回答:**这 6 步对剩下 4 个 B 类适用度多少?**

| B 类候选 | Step 1 数据形态 | Step 2 e2e 任务 | 适用度预判 |
|---|---|---|---|
| **Syntheseus**(逆合成) | AND/OR graph,非结构化 | 给目标分子 → 找前驱路线 | 🟡 Step 1 难度高,Step 4 cp 挖法不一样(路径搜索类),需扩展三句话筛 |
| **LLM4Mat-Bench**(性质预测) | parquet 1.9M 结构 | 给晶体 → 预测形成能 | 🟢 与 PharmaBench 同构(都是回归 ADMET 类),6 步应可直接套 |
| **MolBench seed**(靶点-配体) | 190 条单轮 QA + 14 个 workflow | 用 workflow 重建多步任务 | 🟡 Step 2 不能直接用,要先从 workflow 抽出任务定义 |
| **文献+MP**(催化合金) | 散在论文文本 | 文献综合 + Materials Project 查 | 🔴 Step 1 直接卡(数据散),要先做"文献抽 schema"前置步 |

→ **6 步打法当前预判**:**1 个 🟢 直接复用 / 2 个 🟡 需局部扩展 / 1 个 🔴 需前置步**。

**实跑后再修**(Day 3 回写):略。

---

## 踩了什么坑(2026-06-11 实跑后回写)

详细 6 坑见 [`results.md` §「踩了什么坑」](results.md);本节摘要,回灌到方法学每一步:

| 坑 | 影响哪步 | 教训 → 方法学补丁 |
|---|---|---|
| **1. env 命名分裂**(OPENAI_API_BASE vs OPENAI_BASE_URL) | Step 5 | Step 5「锁 GT」加 *「校 env 命名兼容,做映射 fallback」* |
| **2. src judge 输入 schema 不对齐你的真值** | Step 4 | Step 4「挖 cp」加 *「现成 judge ≠ 你的真值 schema 时,做编码适配(把 csv `value` 列编码成 targets/tol/values)」* |
| **3. LLM judge 输出不可控**(围栏 / json 前缀) | Step 4 (Tier 2) | Step 4 Tier 2 cp 必配「LLM 输出鲁棒解析器」,不要相信 LLM 守 prompt 格式 |
| **4. `_judge_selection` 不是经典 F1 是 Jaccard-flavor** | Step 5 | Step 5「锁 GT」要明确判定函数的**具体语义**;别假设"F1"是经典 F1 |
| **5. 容差太宽 → lazy agent 蒙混 0.667** | Step 4 (cp 设计) | Step 4 Tier 1 cp 设计加规则:**先用 lazy/random baseline 反向测**,如果 lazy 能拿 ≥0.5 就说明 cp 被钻空子,要收紧 |
| **6. src 不原生支持 LLM judge kind** | Step 4 (架构) | Tier 2 cp 独立写,**不强求合入主框架的 kind dispatch** — 内联更干净 |

**最重要的教训**:**Tier 1 cp 设计后必须跑 lazy/random baseline 验收** — 这是 plan 阶段没料到的(原 plan 只说 Tier 1≥70% 配额,没说"配额满了但每个 cp 都能被蒙混"的风险)。

## 复用性判断(走穿后修正)

> ✅ POC4 已跑完(2026-06-11),修正 Day 1 起稿时的预判:

| B 类候选 | 起稿预判 | 实跑后判断 | 改动 |
|---|---|---|---|
| Syntheseus(逆合成) | 🟡 | 🟡 不变 | Step 1 难度高(AND/OR graph 非表),Step 4 cp 类型要扩(路径搜索类 hits-on-path) |
| **LLM4Mat-Bench**(性质预测) | 🟢 | **🟢 强化** | 与 PharmaBench 几乎同构(都是 csv + value 列回归),6 步可直接复用,~1 天可出题 |
| MolBench seed(靶点-配体) | 🟡 | **🟡 → 🟠 弱化** | Step 2 要先从 14 workflow 抽 task 定义;workflow 本身不是任务,需"用 workflow 反向出题"步骤 |
| 文献+MP(催化合金) | 🔴 | **🔴 不变,建议不做** | Step 1 直接卡(数据散),要先做"文献抽 schema"前置步,**MVP 阶段不建议** |

→ **修正 `construction_methodology.md` §2.2**:9 数据源真清楚程度更新为 **🟢 真清楚 5 / 🟡 半清楚 2 / 🟠 弱 1 / 🔴 不建议 1**。

---

## 关联文档

- **POC4 入口**: [`run_poc4.py`](run_poc4.py)
- **真题 spec**: [`task_logd_screen.json`](task_logd_screen.json)
- **数据**: [`data/logd_test_sample.csv`](data/logd_test_sample.csv)
- **工具 oracle**: [`tools/predict_logd_oracle.py`](tools/predict_logd_oracle.py)
- **Tier 2 rubric**: [`cps/strategy_rubric.md`](cps/strategy_rubric.md) · [`cps/report_rubric.md`](cps/report_rubric.md)
- **结果**: [`results.md`](results.md)(Day 3 写)
- **上游方法学**: [`../../../construction_methodology.md`](../../../construction_methodology.md) §3
- **PharmaBench 卡**: [`~/papers/PharmaBench.md`](../../../../papers/PharmaBench.md) · [`audit_pharmabench.md`](../../../../papers/audit_pharmabench.md)
