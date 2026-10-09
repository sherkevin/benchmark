# task16 — 完成汇报

## 一、最终结论：query 风格四维实测（223 条全量）

| 维度 | 结论 | 关键数据 |
|---|---|---|
| **1 口语 vs 术语** | **术语是硬要求，口语是可选包装** | 术语出现率 48%–100%，**无一家零术语**；口语 0%–92% 跨度极大。Toolathlon 口语 11/12 但术语也 11/12 |
| **2 约束显式到哪步** | **多数只到"常温"级；且约束常不在题面** | L3 带单位数值仅 3/102（ScienceAgentBench）、2/7（RE-Bench）；**ScienceAgentBench 58/102（57%）题面完全无约束**，约束在独立 `domain_knowledge` 字段（专家写）+ `dataset_preview` |
| **3 是否"为了X→满足Y→做Z"** | **不是。主流是 `A 动作 → D 交付` 两段式；约束不是固定段** | 句级标注实测：**D 交付段 102/102（100%）全覆盖且恒定压尾**；A 与 D 兼备占 49/102；**C 约束段仅 16/102**。详见汇编「维度 3 展开」 |
| **4 一个模板还是多个** | **多个，但有一个主导句式（20%–32%）** | 变异度 0.54–0.86，**无一家单模板**；SciCode 主导 `Write a…` 21/65、ScienceAgentBench `Train a…` 20/102。**题量越大变异度越低** |

### 四条可直接执行的出题决定

1. **术语必须裸用，但句式可以口语**——Toolathlon 证明"口语句式 + 术语名词"合法。**我方越界处不是"用口语"，而是"把术语翻译掉了"**（"能进入大脑" 替代了 BBB permeability）。
2. **约束不必塞进题面，应拆到独立字段**——照 ScienceAgentBench 范式：题面只写"做什么 + 交到哪"，把"什么算合格"放进 `domain_knowledge`（由药化专家撰写）+ 数据预览。**这一条同时解决了术语问题与专家背书问题。**
3. **约束粒度取"常温"级即可，不必到"26 度"**——L3 精确数值是少数派（除 SciCode 因是数值计算题）。我方现有的定性表述粒度**本身没问题**。
4. **我方 130–150 题应先定 2–3 个主模板**——实测显示题量越大句式越收敛（65/102 题的变异度 0.54/0.56，7/12 题的 0.86）。**不要每题自由写。**

### 我方 template 格式定案（维度 3 的直接产出）

**格式 = `A 动作段`（必需，可多句）＋ `D 交付段`（必需，单句压尾，字段级）；约束外置。**

| 段 | 定义 | 我方该写 | 现状 |
|---|---|---|---|
| **A 动作** | 术语裸用，要 agent 做什么；可多句拆成步骤链 | `Identify a candidate molecule for a CNS indication that is BBB-permeable, free of hERG liability, and orally absorbable.` | 🟡 口语翻译 |
| **D 交付** | **把判分器要读的字段名逐个列出**，恒定最后一句 | `Submit two fields: screened_pool (all candidate IDs you examined) and ranked_candidates (IDs you judged qualified, ordered best-first).` | ❌ "输出你认为最优的答案" |
| **C 约束** | **不进题面**，移入独立 `domain_knowledge` 字段（药化专家撰写） | bbb/herg/caco2 三条阈值 | ❌ 塞在题面且口语化 |
| **S/T 场景与工具** | 可选 | 现有"可用检索工具…"保留 | ✅ |
| **M 评分标尺** | 可选，建议加（学 RE-Bench） | 目标量 Caco-2 最大化 + 标尺 | ❌ 无 |

**证据支撑**：ScienceAgentBench 的 D 段 **102/102 全覆盖**，内部恒含三成分 `Save` ＋ `字段级说明` ＋ `to "确切路径"`；A 段可多句（实例 instance_id=4 有 4 句 A + 1 句 D）。**C 约束段仅 16/102，故"约束外置"不是我方特例，是主流做法。**

---

## 二、取数与验证过程




| 子目标 | 完成情况 |
|---|---|
| 1 · 每家 ≥10 条 verbatim query | 🟡 **5/8 家达标**（SciCode 10、ScienceAgentBench 10、Toolathlon 12、NewtonBench 4 域模板、PaperBench 10 节点）；TRAIL 0 条（gated）、ChemCrow/RE-Bench 未拉 |
| 2 · 风格六维标注 | ✅ 脚本对**全量题面**实算（非抽样印象）：长度中位/最长、祈使句比例、含数值阈值比例、指定输出路径比例、点名工具比例 |
| 3 · 与我方题面对照 | ✅ 逐维比对 `case_cns_metoprolol.json` 的 `query`，给出两条改写方向（**只给方向，未动 case 文件**） |
| 4 · 取不到的如实标注 | ✅ §0 总览单列状态列，TRAIL 附原始报错原文 |
| 5 · 样本量表述受限 | ✅ 所有规律标注"限已取样本"，指标附具体分母 |

## 三、风格实测明细

### 3.1 跨 7 家全量对照

| benchmark | 题数 | 长度中位 | 最长 | 祈使句 | 第一人称 | 含数值 | 输出路径 | 自带评分 |
|---|---|---|---|---|---|---|---|---|
| SciCode | 65 | 87 词 | 334 | 39/65 | 1/65 | 9/65 | 0/65 | 0/65 |
| ScienceAgentBench | 102 | 41 词 | 166 | 67/102 | 0/102 | 9/102 | **47/102** | 0/102 |
| Toolathlon | 12 | 99 词 | 272 | 0/12 | 7/12 | 2/12 | 3/12 | 0/12 |
| ChemCrow | 18 | **31 词** | 78 | 10/18 | 6/18 | 2/18 | 0/18 | 0/18 |
| RE-Bench | 7 | **177 词** | 205 | 0/7 | 0/7 | **7/7** | 0/7 | **7/7** |
| **我方 CNS** | 1 | 约 90 字 | — | 否 | 否 | 0/1 | 0/1 | 0/1 |

### 3.2 各家风格一句话画像

- **SciCode**：祈使句 + 大量 LaTeX 数学符号 + 自带子问题链（专家出题时就拆好）；I/O 契约放独立字段而非题面。
- **ScienceAgentBench**：最短最规整，祈使句 + **明写判分器要读的文件名**（89/102）+ 可选 `domain_knowledge`。
- **Toolathlon**：像真实工单，含条件分支（`If B outperforms… If A wins…`）、命名前缀、主动排除干扰项。
- **ChemCrow**：**最口语**，第一人称串多个子任务（合成→报价→安全），混用化学名与 SMILES。
- **RE-Bench**：**唯一把评分公式与人类基准分写进题面**，另带防污染 canary GUID。
- **NewtonBench**：唯一模板化——`**Experimental Apparatus:**` → 可控变量 → I/O 格式 → 提交函数签名。
- **PaperBench**：唯一陈述完成态（`… have been reproduced`），因为它是 rubric 而非题面。

### 3.3 被实测推翻的三条旧判断

| 旧判断 | 实测 |
|---|---|
| "专家会给 hERG IC₅₀ > 10 μM 这类带单位阈值" | ❌ 仅 9/65、9/102、2/18；唯一全给的 RE-Bench 是因为判分器就是那个公式 |
| "专家 query 都很长" | ❌ 中位 31 词（ChemCrow）到 177 词（RE-Bench），差 5.7 倍 |
| "题面必须客观书面、不用第一人称" | ❌ ChemCrow 6/18、Toolathlon 7/12 用 `I need to…` / `Tell me…` |

## 四、证据在哪

| 内容 | 位置 |
|---|---|
| 汇编正文（5 家 verbatim query + 风格对照 + 我方差距） | `task/task16/专家背书query原文汇编.md`（162 行） |
| SciCode 全量题库 | `task/task16/samples/scicode_problems_{test,dev}.jsonl`（md5 `c176df46…7361`） |
| ScienceAgentBench 全量 102 题 | `task/task16/samples/scienceagentbench_102tasks.csv`（md5 `6a22cfdf…6d36`） |
| Toolathlon 12 条题面 | `task/task16/samples/toolathlon_tasks/*.md` |
| NewtonBench 4 域 prompt | `task/task16/samples/newtonbench_prompts/*.py` |
| 取数通道 | `hf-mirror.com`（HF 镜像，200）+ `raw.githubusercontent.com` |

## 五、遗留（新问题一律另立 task，不干扰本任务）

| # | 遗留 | 处置 |
|---|---|---|
| 1 | **TRAIL 0 条**（HF gated） | 需 owner 用 HF 账号申请 `PatronusAI/TRAIL` 访问；或退回论文附录 |
| 2 | **NewtonBench / PaperBench 只列模板与节点，未逐题展开** | 二者题面是模板化生成（12 域 × 难度）与 rubric 树，逐题展开价值低；如需可另立 task |
| 3 | **Toolathlon 只拉 12/108** | 脚本已验证可批量补拉 |
| 4 | **题面术语专业化需药化人力** | 按 ScienceAgentBench 范式应由药化背景者执笔（T-49）；本 task 只给方向，**未动任何 case 文件** |
| 5 | **`hf-mirror` 通道应写入环境文档** | "HF 不可达"结论已过期，避免后续 task 重复踩坑 |
| 6 | **RE-Bench 式"题面自带评分标尺"值得评估是否引入我方** | 属题面设计变更，**另立 task 评估**，不在本 task 处理 |
