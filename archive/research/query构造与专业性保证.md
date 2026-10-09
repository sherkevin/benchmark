# benchmark 的 query 怎么构造才合理、专业

> **本文回答**：owner 追问"构造 benchmark 里的 query 时，要怎么构造才能保证整体的合理性和专业性"。**这是出题侧的问题**（题面本身写得对不对、约束组合是否科学、问题是不是真问题），与判分侧（怎么验证 agent 答得对不对）分开——判分侧见 [`各benchmark造题法详解.md`](各benchmark造题法详解.md)。
>
> **⚠️ 框架归属声明**：本文的「四道关」是**我对 4 篇论文各自做法的归纳分类**，不是任何论文的原话；每一关的**内容**均有论文逐字原文支撑（对照见 §0.1）。业界唯一给出完整"出题质量测试标准"的是 InfoMosaic（§0.3），其余论文只描述流程、不报量化验证（§0.2）。
>
> **📎 与既有文档的关系（避免重复阅读）**：本文与 [`query写法调研报告.md`](query写法调研报告.md)（早期版，其"query 不是专家写的""其他论文没提专家参与"两处结论**已被 task14 证伪并订正**）、[`task14/report.md`](../task/task14/report.md)（62 家数据集权威性 + 8 家专家参与逐条取证）同源。**分工**：task14 管"数据源与专家参与的权威性取证（62 家样本）"，本文管"出题四道关与代码级衡量指标（6 家深挖 + 3 家实拉代码）"。
>
> **⚠️ 样本量限定**：§0.2 的"只有一篇有量化验证"等判断**基于 6 篇深读 PDF + 3 家实拉代码**，不是 62 家全样本结论。task14 已有先例：初稿断言"62 家无一家做过 query 措辞自然度评估"被自证为错。**故本文一切"只有／没有一家"的表述，其作用域仅限已核验的样本，不得外推为业界全局规律。**
>
> 状态：2026-08-01 ｜ 证据：`reference/<CONF>-*/*.pdf` 正文原文（行号对 `pdftotext` 提取文本，可复核）｜ 关联：PR-27

---

## ★★ 根本问题的答案：有专家背书的 query 构建模式，不止一家（本文最该先看的一节）

**owner 的根本问题**：
> "是否有被专家背书过的 query 构建模式？你是否从已发表论文里看到了这种做法，还是他们压根没管这个 query 构造的合理性？**本质就一句话：他们构造 query 本身这件事的合理性如何保证，是专家认可的还是怎么做的。**"

**答：有。62 家 benchmark 实测，至少 8 家明写领域专家参与构造或审核。"压根没管"是错的**——我方早先在 `query写法调研报告.md` 写的"query 不是专家写的""其他论文没提这件事"两处结论**已被 task14 证伪并订正**。

### 一、8 家专家背书的原文证据（task14 逐条取证）

| Benchmark | 专家背书的原文证据 |
|---|---|
| **SciCode** | 标题即 *"Curated by **Scientists**"*；领域专家出题时就把题拆成子问题链 |
| **ScienceAgentBench** | *"engage **nine subject matter experts** to validate them"*——9 位资深博士生/教授，来自四个对应学科 |
| **NewtonBench** | **3 位领域专家**（物理奥赛背景共同作者）把关科学合理性 + 交叉验证 |
| **ChemCrow** | **18 个专家设计**的化学工具 + 人类专家评判 |
| **TRAIL** | *"**Four expert annotators** with backgrounds in software engineering and log debugging"* |
| **PaperBench** | rubric 树 8,316 叶节点，专家构造 |
| **RE-Bench** | 分数归一化到 **human expert reference = 1** |
| **Tool-Decathlon** | 108 题人工搜集或编写 |

一手证据路径见 `task/task14/report.md` §四（含各 `reference/*/` 文件名与行号）。

### 二、专家审核分三层，三层都有人做——我方四项只做了一项

**这是回答"合理性如何保证"最关键的一张表：**

| 层次 | 验什么 | 谁做了 | 我方 |
|---|---|---|---|
| **L1 题面措辞** | 术语误用、表达生硬、是否符合本领域科学语言 | **ScienceAgentBench**——请专家**重写**，改了 41/102 条 | ⬜ **未做** |
| **L2 科学合理性** | 科学上说不说得通、数据可不可得、判据能不能执行 | ScienceAgentBench（9 位）、NewtonBench（3 位） | ⬜ **未做** |
| **L3 任务代表性** | 是否代表真实科研工作流 | ScienceAgentBench——**删了 3 题** | ⬜ **未做** |
| **并行路线：从真实来源反向抽取** | 从 44 篇同行评议论文抽题（题目本来就存在，无需自证） | ScienceAgentBench 等 | ✅ **已做**（GT-first 从 TDC 反查） |

**ScienceAgentBench 是唯一三层全做的。它的做法一句话：不假装自己专业，把专业性外包给专家的笔。**

### 三、我方的具体投稿风险（一句话）

审稿人问"你凭什么说这道 CNS 题是药化研究者真会问的问题"，**我方目前没有任何可引用的答复**——既无专家签字，也无"从某篇药化论文抽题"的来源；而 ScienceAgentBench 两条都有。**这是 PR-27 的核心，落地在 T-49。**

### 四、一条重要的方法论区分（避免再次答偏）

**"代码级断言"（如 MLE-bench 的 `assert`，见 §0.5）解决的是"题目数据有没有技术缺陷"（数据丢失/泄漏/重复/格式），完全不等于"题目在科学上合不合理、专家认不认"。** 后者只能靠专家背书或权威来源抽取。两者不可互相替代——这是本文两次答偏的根因，记录于 `task/task15/report.md` §三。

---

## 0. 先交代：哪些是论文原话，哪些是我归纳的（owner 质疑的回应）

**owner 追问："这些关卡是你自己想出来的，还是论文里明确说明的？论文有说自己如何保证过了这些关卡吗？本身有什么测试标准没？"**

诚实分三层回答：

### 0.1「四道关」这个框架是我归纳的，不是任何论文的原话

**没有任何一篇论文说过"出题要过四道关"。** 我做的是把 4 篇论文各自的做法归到 4 个类别里——**分类框架是我的，每一关的内容是论文的**。区分如下：

| 关 | 关卡名（我起的） | 论文里对应的原文概念（有逐字引用） | 出处 |
|---|---|---|---|
| ① | 约束不自相矛盾 | **Coherence Filtering** / "contradictory constraints" | InfoMosaic §3.2 |
| ② | 答案唯一无歧义 | "**unambiguous**, i.e., there is a single correct answer" | GAIA §3.4 |
| ③ | 术语与表述地道 | "**misused terms... do not adhere to your domain's scientific language**" | ScienceAgentBench 附录 G 问卷第 2 题 |
| ④ | 确是真实工作流 | "**a realistic task in their data-driven discovery workflow**" | ScienceAgentBench §3 专家问卷第 1 步 |

即：**这四件事论文确实各自明确做了、且写在正文里，但"合成一套四关清单"是我的加工。** 没有一篇论文同时做了全部四关。

### 0.2 只有一篇论文给出了「我过关了」的量化标准——其余都只描述流程

这是本次核查最重要的发现。逐篇核对"有没有量化验证"：

| 论文 | 有量化的出题质量验证吗 | 具体是什么 |
|---|---|---|
| **InfoMosaic** | ✅ **有，且最完整** | 120 题随机抽样，3 名 NLP 方向研究生**独立**对"过闸前/过闸后"两版打分，三维度（Factual Alignment / Coherence / Difficulty）各按 5-3-1 分级；**一致性 Cohen's κ = 0.92** |
| **GAIA** | 🟡 **半有** | 报了通过率——**68% 的题一次过关**，其余需修正或删除；人类得分 92%。但**没报标注员间一致性系数** |
| **ScienceAgentBench** | ⬜ **无** | 只报"改了 41 条指令、删 3 题、再改 29 条标注"这类**过程计数**；全文**未出现任何 inter-annotator agreement / κ 值**（核查：`grep -i "kappa\|agreement"` 仅命中一处软件许可证文本） |
| **NewtonBench** | ⬜ **无（易误读）** | 它报的 **98.3% agreement 是"LLM-as-a-Judge 与人类评估者"的一致性**（Table 8），属于**判分器可靠性**，不是题目质量验证 |
| **MLE-bench** | ⬜ **无** | 零次提及一致性指标；题目质量靠"竞赛本身存在"来担保 |
| **Toolathlon** | ⬜ **无** | 同上，无出题质量的量化验证 |

**结论（限本节 6 篇样本内）：这 6 篇里只有 InfoMosaic 报了出题质量的一致性系数，它做的是"抽样 + 多人独立打分 + 报 κ"这套社会科学标准动作。** ⚠️ 不可外推为"业界普遍没有"——本仓 62 家清单（task14）未逐家核查此项，且 task14 已有"断言无一家、实为有多家"的翻车先例。**待办：若要在论文里写这条 gap，必须先把 62 家逐家核过。**

### 0.3 InfoMosaic 的评分卡（唯一可直接照抄的「测试标准」，附录 A.10.1 原文）

它给标注员的评分细则比我总结的四道关具体得多，逐字摘要如下：

**规则前提**：只用给定材料（题面 / 条件 / GT）判断；**判的是数据条目的质量，不是世界的真理**。

| 维度 | 判什么 | 5 分 | 3 分 | 1 分 |
|---|---|---|---|---|
| **A. Factual Alignment**（答案—条件一致性） | GT 是否忠实对应所述条件：每个条件都能追溯到答案的一部分，且答案不依赖条件之外的信息 | 每个条件都反映在答案中，无不一致 | 答案覆盖主要意图，但部分条件未描述 GT | 答案不遵守条件（矛盾或条件无关） |
| **B. Coherence**（语义与逻辑自洽） | 题面/条件/答案是否互相一致、无歧义、可执行 | 题面清晰、条件可解释且不冲突、无不自然或自相矛盾措辞 | 小歧义、轻微冗余或措辞稍生硬，但不阻碍执行 | 矛盾条件、指称不清（如未定义实体）、或**条件用给定工具无法执行** |
| **C. Difficulty**（是否真需多工具/多步） | 有能力的 agent 是否**必需**多个工具/多步（看必要性，不看调用次数） | （C5，需真正多源整合） | — | C1 Trivial：单工具或单条件即可解，无需整合 |

**三条可直接搬的判定细则**：
1. **A 维度的三条判据**：答案必须①能由满足全部条件推出 ②不含条件之外的额外信息 ③**不能靠忽略部分条件也解出**。
2. **B 维度把"工具不可执行"算进不自洽**——"条件用给定工具无法执行"直接判 B1。**这正好覆盖我们踩的那个坑**（mock 检索截断导致 3 道题无解，按此标准全是 B1 废题）。
3. **单条件泄题直接判 B1/B0**：原文"If a single condition directly gives away the answer, mark B1/B0"——且注明这会连带影响 Difficulty 评分。

---

## 0.5 代码级核验：拉了三家仓库，看造题代码到底有没有（2026-08-01 实拉）

**owner 追问"直接拉原文和代码，彻底搞清他们做好 query 的衡量指标"。结论先说：**

> **造题 pipeline 的代码，两家没开源、一家把它写成了 83 份可执行断言。** 论文里描述得最花哨的（InfoMosaic-Flow 双 agent 合成）恰恰**没有代码**；论文里写得最朴素的（MLE-bench 重切数据集）**每道题都有一份可执行的出题校验脚本**。

| 仓库 | 实拉结果 | 有造题代码吗 | 出题质量的衡量指标在代码里长什么样 |
|---|---|---|---|
| **InfoMosaic** `DorothyDUUU/Info-Mosaic` | 470 文件，**425 个是工具后端**（biomcp 等），业务代码只有 `eval/`(3) + `data/`(2) + `inference/`(10) | ⬜ **无**。InfoMosaic-Flow、三道闸、人工评分卡**全部未开源** | 只有判分侧：`eval/judge_config.yaml` 里是 LLM 抽取答案的 prompt |
| **ScienceAgentBench** `OSU-NLP-Group/ScienceAgentBench` | 全仓仅 **31 文件**，全是 harness / engine / grading | ⬜ **无**。5 步标注、专家问卷、rubric 生成**均未开源**（题目数据在 HF，本环境不可达） | 无出题侧代码；判分侧为 `evaluation/harness/grading.py` |
| **MLE-bench** `openai/mle-bench` | 1015 文件，**83 个 `prepare.py` + 83 个 `grade.py` + 82 个 `checksums.yaml`** | ✅ **有，且是唯一一家** | **每道题一份 `prepare.py`，出题正确性写成可执行 `assert`** |

### MLE-bench 的出题校验：**已实拉的 3 家中**唯一"可执行的 query 质量标准"

`mlebench/competitions/aerial-cactus-identification/prepare.py` 结尾的**九条断言逐字**（原文）：

```python
# checks
assert (public / "train.zip").exists(), "public/train.zip should exist"
assert (public / "test.zip").exists(), "public/test.zip should exist"
assert not (raw / "train").exists(), "raw/train/ should not exist"
assert len(new_train) + len(new_test) == len(old_train), "The lengths of the splits should add up to the original"
assert len(new_train) > len(new_test), "The train set should be larger than the test set"
assert len(new_test) == len(sample_submission), "The test set should match the sample submission"
assert new_train.columns.tolist() == new_test.columns.tolist() == old_train.columns.tolist() == sample_submission.columns.tolist(), "All dataframes should have the same columns"
assert set(new_train["id"]).isdisjoint(new_test["id"]), "Train and test ids should not overlap"
assert new_train["id"].nunique() == len(new_train), "There should be no duplicate ids in train"
```

**把这九条翻译成"出题质量指标"，正是四类可机器判定的题目缺陷：**

| 断言 | 拦的是哪种废题 |
|---|---|
| `len(new_train) + len(new_test) == len(old_train)` | **数据丢失**——重切时漏样本，题目基础被悄悄改变 |
| `set(new_train).isdisjoint(set(new_test))` | **答案泄漏**——训练集里出现测试样本，题目变 trivial |
| `nunique() == len()` | **重复条目**——同一实体出现两次，判分口径不唯一 |
| `columns 全等` + `test 与 sample_submission 长度/ID 相等` | **格式不自洽**——题面承诺的输出格式与判分器期待的不一致 |

### 还有一层：出题产物的 md5 冻结

每道题配一份 `checksums.yaml`（逐字）：

```yaml
private:
  test.csv: ebdb02d62dbaf60270a9b3a8617840af
public:
  sample_submission.csv: 14c675bde0de98994efafac8e008277e
  train.csv: 4f5f752b84f8836c852f4f70e988a545
zip: 72018554bfbb009f2145f56121380078
```

`mlebench/data.py`（407 行）在准备数据时**逐文件比对 md5，不一致就报错**：

> `if actual_checksums != expected_checksums: logger.error(f"Checksums do not match for {competition.id}!")`

**即"这道题是不是还是当初那道题"由 md5 保证**——任何人重跑 `prepare.py` 得到的题目与作者的完全一致，否则立刻发现。这是"出题可复现性"的硬实现，也是我方目前完全没有的东西。

### 三条最该学的（按可立即落地排序）

1. **出题脚本自带 `assert`，不是出完题再人工检查**。⚠️ 更正：我方**已有** `src/benchmark/verify_snapshot_identity.py`（T-45 交付：非法 SMILES / 分子式比对 / 重复检测 / 填充率 + `--strict` 非零退出），即"入库门"已存在。**真正缺的是 case 级断言**：解集非空、候选池含 GT、trivial baseline 不能一击命中（登记为 T-50）。
2. **出题产物 md5 冻结**。我方 `mock_db.json` + 各 case 应生成一份 checksums，防止"题被悄悄改了而实验结果还在用旧结论"（这正是 PR-26 那 15 点数据作废的教训）。
3. **"答案泄漏"要写成可执行检查**。MLE-bench 用 `isdisjoint` 一行拦住；我方对应的是"候选池里不能只有 GT 一个合格者且被 trivial 排序命中"，需要一个同等级别的一行断言。

---

## ★ 一句话结论

**一道 query 要"合理且专业"，这 18 篇一共只用了四道关，缺一道都会出废题：**

| 关 | 查什么 | 不过关会出什么废题 | 谁做得最细 |
|---|---|---|---|
| **① 约束不自相矛盾** | 多个条件能否同时被满足 | 无解题（agent 再强也答不出） | InfoMosaic 的 Coherence Filtering |
| **② 答案唯一无歧义** | 题面是否漏了限定，导致多个答案都算对 | 争议题（判分判不了） | GAIA 的三人独立复答 |
| **③ 术语与表述地道** | 是否用了本领域的科学语言 | 外行题（专家一看就假） | ScienceAgentBench 的专家改写 |
| **④ 确是真实工作流** | 这个问题真有人问吗 | 伪问题（为造题而造题） | ScienceAgentBench 的专家判定 |

**最该记住的一句**：ScienceAgentBench 在给专家的问卷里直接写明"**题面是非专家写的，可能有术语误用、表达生硬、或不符合你们领域的科学语言**"，然后请专家**重写**。它不假装自己专业——**把专业性外包给专家的笔，而不是外包给作者的自信**。

---

## 一、关卡①：约束不能自相矛盾（否则题无解）

**这是我们刚踩过的坑**：`case_cns_metoprolol` 因 mock 检索截断，3 道题的正确答案根本不在候选池里——**题目结构性无解**（ADR-0011）。业界的做法是把这一关自动化。

### InfoMosaic 的 Coherence Filtering（正文 L412–414，逐字）

> "**Coherence Filtering.** We further remove tasks that exhibit **incoherent or ill-formed conditions**, such as **contradictory constraints** or **unnatural phrasing**, to guarantee that each problem maintains **semantic coherence across its question, intermediate conditions, and final answer**."

三个可直接落地的检查点：
1. **矛盾约束**（contradictory constraints）——多个条件不可能同时满足；
2. **措辞不自然**（unnatural phrasing）——读起来不像人会问的问题；
3. **三者语义一致**——题干、中间条件、最终答案必须互相对得上。

### 一个反直觉但关键的设计：让出题者看不见工具细节

InfoMosaic 让 organizer（定条件的那个）**对工具内部无感知**（tool-agnostic），只选目标领域，具体调哪个工具由 worker 决定。理由写在正文 L370–371：

> "by **hiding tool internals**, the synthesizer focuses on maintaining **coherence and naturalness in the problem statement**, rather than **overfitting to tool quirks**."

**翻译成大白话**：如果出题的人一边盯着"我有哪些工具可用"一边编题，编出来的题会变成"为了用上这个工具而设计的问题"，而不是"科学上真正要问的问题"。它还专门提到这样能"**减少把约束往现有工具上硬凑**"（reduces retrofitting of constraints to available tools，L343）。

> **对我方的直接警示**：我们的 `case_cns_metoprolol` 三条约束（`bbb`/`herg`/`caco2`）恰好就是 mock 库里**正好有的那三个字段**——这正是"把约束往工具上硬凑"的典型症状。真实的 CNS 候选筛选会看的远不止这三项（还有 P-gp 外排、血浆蛋白结合率、CYP 代谢稳定性等）。**约束该由科学问题决定，而不是由我们 mock 了哪些字段决定。**

---

## 二、关卡②：答案必须唯一无歧义（否则判不了分）

### GAIA：把"消歧"当成出题的主要工作量

正文 L400–402 说得很直白：

> "**Most of the work associated with crafting questions consists in ensuring that they are unambiguous**, i.e., there is a single correct answer. ... **Ambiguities can be subtle and rarely obvious to the creator of a question.**"

**歧义的典型来源**（原文举的例）：题目问某网页上的信息，但**没指定网页版本**——而不同版本的答案不一样。

**它的解法与真实代价**（L404–409）：
- 请**两位新标注员各自独立作答**，与原出题人三者答案一致才收题；
- **只有 68% 的题一次过关**，其余必须修正或删除；
- 不一致的题一般只需小修（补个限定词），修不好就丢。

> **可直接抄的一条**：**出题人自己看不出自己题里的歧义**——这是 GAIA 明写的观察。所以消歧不能靠出题人自查，必须靠独立第二方复答。我方对应做法：用两个不同模型（或穷举脚本）独立解题，**若得出不同的"合格集"，说明约束有歧义**。

### 与我方的具体关联：|G| 必须显式验证

我方 `case_cns_metoprolol` 的合格集 `|G| = 1`（穷举 20 个分子确认）。**但这是我事后穷举才知道的，不是出题时验证的**。正确做法是把"穷举确认解的个数"变成出题流程的强制一步：
- `|G| = 0` → 无解题，废；
- `|G| = 1` → 唯一解，最好判分；
- `|G| > 1` → 需明确"是否所有合格者都算对"，或补约束收敛到唯一。

---

## 三、关卡③：术语与表述必须地道（否则一眼是外行题）

**这一关是 owner 问题的正中靶心，也是 ScienceAgentBench 做得最露骨的一环。**

### 它给专家的原始问卷第 2 题（附录 G，正文 L3572–3578，逐字）

> "**2. Task Instruction**
> The task instructions were **created by non-experts** and thus might contain some **misused terms, awkward expressions, or inaccurate descriptions of the task that do not adhere to your domain's scientific language**. Do you see such issues for this task instruction? If so, please **revise or rewrite the task instruction** for any issues you can find.
> Finally, if needed, please help make the task instruction **more fluent and natural sounding**."

拆出三条可核查的标准：
1. **术语没有误用**（misused terms）；
2. **表达不生硬**（awkward expressions）；
3. **符合本领域的科学语言**（adhere to your domain's scientific language）。

**结果是真的改了**：专家反馈后**改了 41 条题面指令**（占 102 题的 40%），并**删掉 3 道**"不足以代表本学科科研工作流"的题。

### 它还顺手解决了"专业知识该不该给"的问题

问卷第 3 题让专家写**最多三条解题所需领域知识**，类比是"给学生出考题时可以给点提示"，并明确设定 agent 的知识基线：

> "You may assume the AI agent has a general sense of your domain, **like a new graduate student**..."

于是"专业知识"变成了**可控输入**，还能量化其价值（有专家知识 SR 34.3% vs 无 32.4%）。

> **对我方的直接警示（已按 task16 全量实测更正）**：我们的题面写的是"**能进入大脑、对心脏安全、口服吸收良好**"——**大白话而非领域术语，这一条差距成立**（专家背书样本一律裸用 Clintox / SMILES / Berendsen thermostat 等术语，假定 agent 有"新入学研究生"水平）。
>
> ⚠️ **但本处原先写的"药化研究者会写 hERG IC₅₀ > 10 μM 这类带单位阈值"已被证伪**：task16 对 SciCode 65 题、ScienceAgentBench 102 题、Toolathlon 12 题全量实算，**含数值阈值的仅 9/65、11/102、2/12（均 <17%）**——专家题面同样极少给数值阈值。**故"我方不给数字"并非缺陷，是主流做法。** 真实差距只有两条：① 术语大白话 ② 未在题面给输出格式/路径（ScienceAgentBench 47/102 明确给）。详见 [`task/task16/专家背书query原文汇编.md`](../task/task16/专家背书query原文汇编.md) §8–§9。

---

## 四、关卡④：必须是真实工作流里的真问题（否则是伪问题）

### ScienceAgentBench 的两道防线

**防线一：题源即真实性**。只从**同行评审论文**里抽任务，且该论文必须开源了代码与数据（44 篇 → 102 题 / 4 学科）。题目不是编的，是**真实研究里已经做过的事**。

**防线二：专家判定代表性**。问卷第 1 步要专家确认这道题"**是否代表他们真实的数据驱动发现工作流**"（正文 L262–263）。**判定权在专家，作者不能自封**——结果删了 3 道不够代表性的题。

### NewtonBench：当题目必须是合成的，就用形式化约束兜底

题面无法从真实论文抽（因为它要造"平行宇宙定律"），于是把合理性变成**机器可检的硬约束**（正文 L414–416、L1833–1837）：
- 种子定律必须是领域基础定律、跨多个物理域、**且允许量纲一致的突变**；
- 改写四准则：不引入新变量 / 必须增加复杂度 / 不得是上次突变的逆操作 / 连续突变结果不可由单次突变达到；
- 量纲被突变破坏 → 用内嵌物理常量补偿，**因此每条定律必须至少含一个补偿常量**。

**最值得学的是它划边界的写法**（正文 L1838–1840，逐字）：

> "All mutated laws are **dimensionally coherent by construction**, but **some may be physically implausible in our universe**; we intend these counterfactual variants as **scientifically grounded stress tests of reasoning**..., **not as claims about real-world physics**."

即：**我保证量纲自洽，不声称物理真实**。把适用范围写清楚，比含糊宣称"很专业"更能过审。

### 三条路线的题面来源对比（决定了合理性从哪来）

| 题面来源 | 代表 | 合理性靠什么 | 我方可用性 |
|---|---|---|---|
| **从真实论文/竞赛抽** | ScienceAgentBench、MLE-bench | 题目本来就存在，无需自证 | ✅ 药物/材料线可抄 |
| **工具证据反向拼** | InfoMosaic | 每个条件都被真实工具证实过 | ✅ 我方主路线 |
| **形式化合成** | NewtonBench | 量纲/复杂度等硬约束 + 专家交叉验证 | 🟡 需找到药化的同类硬约束 |
| **人工基于可信来源编** | GAIA | 来源权威 + 三人复答一致 | ⚠️ 门槛在领域知识，我方难做 |

## 五、对我们的直接结论：出题侧四道关，我方过了几道

**结论：我方四道关只过了半道（关①部分），另三道全空——而且刚被 ADR-0011 证明关①的缺失会直接造出无解题。**

| 关 | 业界做法 | 我方现状 | 缺口 |
|---|---|---|---|
| **① 约束不矛盾/题有解** | InfoMosaic 自动 Coherence Filtering（矛盾约束+措辞+三者一致） | 🟡 **事后穷举才发现** `\|G\|=1`；3 道题曾结构性无解且当时未察觉 | 需把"出题时穷举验证解的个数"变成强制闸 |
| **② 答案唯一无歧义** | GAIA 三人独立复答，仅 68% 一次过关 | ⬜ 无任何独立复答机制 | 需双模型/穷举脚本独立解题比对 |
| **③ 术语与表述地道** | ScienceAgentBench 专家逐题改写，改了 41/102 条 | ⬜ 题面是**大白话**（"能进入大脑、对心脏安全"） | 投稿版必须由药化背景者改写为科学语言 |
| **④ 确是真实工作流** | 从同行评审论文抽题 + 专家判定代表性（删 3 题） | ⬜ 约束组合由**我们自己定**，无来源、无专家确认 | 需从真实论文/指南反查约束组合 |

### 三个最该先修的具体问题（按易被质疑程度排序）

**问题 1：约束组合是"照着 mock 字段凑"的，不是照着科学问题定的。**
`case_cns_metoprolol` 的三条约束 `bbb`/`herg`/`caco2` 恰好就是 mock 库里有的那三个字段——这正是 InfoMosaic 明确要避免的 "retrofitting constraints to available tools"（把约束往现有工具上硬凑，正文 L343）。真实 CNS 候选筛选还会看 P-gp 外排、血浆蛋白结合、CYP 代谢稳定性等。**正确顺序是：先从药化文献确定"CNS 候选该满足哪些条件"，再去补 mock 库字段**，而不是反过来。

**问题 2：阈值取值无来源。**
`caco2 > -5.0`、`bbb == 1` 这些界从何而来没有落盘。这是四条缺口里**最容易被具体追问**的一条（审稿人只需问一句"为什么是 -5.0"）。修法：为每条阈值补文献引用，写进 `reference/NobelClaw/证据_*.md`。

**问题 3：题面是大白话，不是科学语言。**
现在写"能进入大脑、对心脏安全、口服吸收良好"。这在内部沟通时是优点（通俗），但**投稿版会被一眼看出是外行造题**。药化的写法是 "BBB permeability、hERG liability、Caco-2 permeability" 并给出带单位的阈值。注意 ScienceAgentBench 的做法：**题面由非专家写、再由专家改写**——所以我们不必一开始就写对，但必须有专家改写这一步。

### 可立即落地的两个自动闸（不依赖外部人力）

1. **可解性闸**：出题时穷举候选宇宙，报告 `|G|`；`|G|=0` 直接废题，`|G|>1` 强制声明判分口径。**这一步能拦住 ADR-0011 里那 3 道无解题。**
2. **歧义闸**：用两个不同模型（或一个穷举脚本 + 一个模型）独立解同一题，若得出的合格集不一致，标记该题送人工复核。这是 GAIA 三人复答的低成本版。

### 照 InfoMosaic 评分卡给我方 CNS 题自评（唯一可对标的量化标准）

用 §0.3 那张卡评 `case_cns_metoprolol`（修复前 / 修复后）：

| 维度 | 修复前 | 修复后 | 依据 |
|---|---|---|---|
| **A. Factual Alignment** | **A3** | **A5** | GT `aa` 由三条约束唯一确定、无额外信息；但修复前另 3 道题的 GT 不可达 → 那 3 道是 **A1** |
| **B. Coherence** | **B1** | **B5** | 修复前"条件用给定工具无法执行"（检索只返回 `aa`–`ae`），按原文定义直接判 B1；修复后全库可检索 |
| **C. Difficulty** | **C1–C3** | **C3** | 20 个候选 × 3 条性质需多次查证，但**只需一类工具**（`predict_admet`），未达"多源整合"的 C5 |

**三个自评结论**：
1. **修复前那 3 道题按 InfoMosaic 标准是 B1 废题**——不是"偏难"，是不合格。这给 ADR-0011 的作废决定提供了业界口径依据。
2. **C 维度是我方真实短板**：我们的题只需一类工具反复查，不需要跨异构工具整合。InfoMosaic 的 C5 要求"必需多工具"，我方目前达不到。**这是 horizon 偏短的另一种表述方式。**
3. **我方尚未做任何多人独立打分**。要对标 InfoMosaic，需：抽样 ≥100 题 → ≥3 人独立按 A/B/C 三维打分 → 报 Cohen's κ。这应写进论文的 benchmark quality 小节，否则我方连"业界唯一样板"都没跟上。

## 附：证据索引（可复核）

| 结论 | 出处 |
|---|---|
| ScienceAgentBench 9 位专家资质与四步问卷 | `reference/ICLR-ScienceAgentBench/*.pdf` 正文 L259–271 |
| ScienceAgentBench 单题标注 2.5–3 h | 同上 L189 |
| ScienceAgentBench 删点例外（破坏完整性则跳过） | 同上 L252–253 |
| NewtonBench 种子定律三标准 | `reference/ICLR-NewtonBench/*.pdf` 正文 L414–416 |
| NewtonBench 改写四准则 + 物理合理性警示 | 同上 L1833–1840 |
| NewtonBench 专家为物理奥赛背景共同作者 + 交叉验证 | 同上 L1243–1244 |
| InfoMosaic "无单一作者具跨域专长" | `reference/ICLR-InfoMosaic-Bench/*.pdf` 正文 L123 |
| InfoMosaic 三道自动闸 + 人工筛查 | 同上 L401–416 |
| GAIA 非专家近乎满分 / sources of truth / 三人一致 / 2h 每题 | `reference/ICLR-GAIA/*.pdf` 正文 L95、L390–396、L404–406、L413 |
| MLE-bench 586 竞赛人工相关性筛查 | `reference/ICLR-MLE-bench/*.pdf` 正文 L148–149 |
| AstaBench 语料日期截止防污染 | `reference/ICLR-AstaBench/*.pdf` 正文 L450、L463 |

| **代码级证据（2026-08-01 实拉 GitHub，已落盘含 md5）** | |
| MLE-bench 出题九条 assert | `reference/ICLR-MLE-bench/sample_prepare_with_asserts.py`（md5 `87b5ed55…44f6`）源自 `openai/mle-bench` → `mlebench/competitions/aerial-cactus-identification/prepare.py` |
| MLE-bench 出题产物 md5 冻结 | `reference/ICLR-MLE-bench/sample_checksums.yaml`（md5 `b2c3b615…ae20`） |
| MLE-bench 校验框架（407 行） | `reference/ICLR-MLE-bench/sample_data_checksum_framework.py` 源自 `mlebench/data.py` |
| InfoMosaic 判分 prompt（含子条件逐条抽取） | `reference/ICLR-InfoMosaic-Bench/sample_judge_config.yaml`（md5 `cdea023b…6ecf`）源自 `DorothyDUUU/Info-Mosaic` → `eval/judge_config.yaml` |
| InfoMosaic / ScienceAgentBench 无造题代码 | 实拉仓库树核验：前者 470 文件中 425 为工具后端、业务仅 eval/data/inference；后者全仓 31 文件皆 harness/grading |

> 提取方式：PDF 用 `pdftotext -q reference/<CONF>-<name>/<CONF>-<name>.pdf out.txt`（行号对应该纯文本）；代码用 `curl raw.githubusercontent.com/<repo>/main/<path>`，md5 已留痕可复现比对。
