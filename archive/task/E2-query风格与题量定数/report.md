# E2 收口报告 · query 风格与题量定数

| 字段 | 值 |
|---|---|
| **任务** | goal v3.0.1 §6 步 2：query 风格定调 + 模板类别/题量定数 + 专家审题发起 + 防污染机制设计 |
| **认领人** | E2-Lee |
| **日期** | 2026-08-06 |

## 1. 做了什么

1. **核对上游原文**（防转述失真）：逐条比对 goal.md §1.5/§2b/§3/§5/§6、task16 汇编 §9/§10、task17 分化设计全文、ADR-0012 §2/§3，并实读 `dag_poc/templates/` 下 constraint_satisfaction 与 constrained_optimization 两份 template.json，确认 D 段字段名现状（`screened_pool` / `ranked_candidates`）。
2. **产出主规范** `task/E2-query风格与题量定数/query格式与风格规范.md`：风格硬约束清单（13 条 MUST / 10 条 MUST NOT，E5 可逐条打勾）、四类模板类别表（含 open_discovery 撞名警告与旧模板迁移口径）、题量定数表（五条依据链）、题面段结构规范（段定义、必需性矩阵、D 段字段命名、主句式）、防污染机制设计（等价匿名变换 5 规则 + 裸答基线 5 步流程 + open 预算题面写法，标注 E2/E5/E6 分工）、专家审题发起包（邀请函草稿 + 背书清单模板 + 审题记录表模板）。
3. **产出机器可读冻结契约** `dag_poc/query_spec.json`：schema_version/frozen_by/frozen_date/总题量/配比/per_type 明细/open 冗余配额/风格要点指针/旧模板迁移说明；已用 python3 断言验证可解析且题量自洽。
4. **更新 task.md 表头**（状态 🚧 进行中、认领人 E2-Lee、日期 2026-08-06），正文未动。
5. **登记 progress.md**（含专家审题阻塞项）。
6. **第二轮收尾**：待审题面样例包初版、130–150 依据链订正、新增 §4.7 open 题量可行性与降级阶梯。
7. **第三轮大修（任务 ID 8，冻结前）**：落实补派清单全部条目——必修 5 项（M7/§5.3 字段改按 ADR-0014 与 answer_schema.json；投造改 43 且阶梯改区间完备四档；新增 §4.8 题量×存量可行性、150 标为条件值；删 15%+15% 伪分解改待标定+首批 10 题、投造公式化、A 规模风险移交 E1/E5；类型4 domain_knowledge 改必需、目标窗口三要素入字段）；应修 8 项（M9 强制变体率≥0.3；150 依据改两条硬约束交集；M8 按类型分档 30–120/60–200 词；审题口径改 ADR 实际口径+背书出处+分层抽审≥30%+吞吐核算；最低可发表改功效导向；fixed 内配比 40:80 改 60:60；§4.2 逐格缺口表；§5.2 矩阵补 T/C 行、段序 S→A→M→T→D）；送审包重做（4 子领域新稿 + 术语征询表）；轻量 3 项（N9 goal 口径、13 条 MUST 判定方式列、§6.1 R1/R4）；全局同步（query_spec.json 增 degradation_ladder/min_publishable/required_manufactured、type3 分域 15/15、"223 条全量"改抽样口径）。

## 2. 结论（题量定数与风格定调，第三轮修订后）

- **总题量 150 为条件值**：依据为两条硬约束的交集——下限 = 与形态最接近两家同量级（ScienceAgentBench 102、Toolathlon 108，即 ≥102）；上限 = 存量与专家审题产能可支撑的最大值；150 落在区间内，**规模主张仅需 ≥108**；150 依赖 E1 扩库达标（药物 ≥10⁴、材料 5,771）方成立，不达标按规范 §4.8 回退表降级；
- **fixed : open = 4 : 1 → fixed 120、open 30**；
- **fixed 内**：类型1 约束满足 60、类型2 约束最优 60（原 40:80 已作废：按"可造出的独立约束簇/独立目标量数量"重分配，材料 tc 单目标决定类型2 材料题上限；cp5 是唯一考察多候选比较能力的检查点）；类型3 证据溯源属第二阶段，建议追加 30 题（药 15/材 15）、不占第一版；
- **open 30**：投造配额按公式 `required_manufactured = ceil(30 ÷ (1−30%)) = 43`（药物/材料 22/21），30% 废弃率为待标定先验（无实测支撑，标定样本 = 首批 10 题，第 4 周末实测回填重算）；
- **领域配比**：药物 : 材料 = 1 : 1，每类模板内均分（成立前提 = E1 材料扩库达标）；
- **风格定调**：英文骨架、术语裸用、A+T+D 为主骨架（类型2 加 M 段、类型3 加 S 段）、约束外置 `domain_knowledge`（类型4 必需且含目标窗口三要素）、D 段字段级恒压尾（字段名以 answer_schema.json 为唯一来源，ADR-0014）、题面按类型分档 30–120/60–200 词、每类一个主导句式 + 强制变体率 ≥0.3；
- **最低可发表口径（功效导向）**：Δ=0.2/α=0.05/功效 80% → 每组约 97 题；open 30 题只支撑描述性独立子集 + 单题 0/1 命中率，差异显著性主张挂 fixed 120 侧；10 题为描述性独立呈现下限；
- **open 进第一版**：维持 goal v3.0.1 裁定（open 是与全部参考 benchmark 的核心差异化卖点；因造题成本高、0 存量，配比压至 1/5 且配额按公式上浮）。

## 3. 证据路径

| 结论 | 证据 |
|---|---|
| 风格硬约束各条 | `task/task16/专家背书query原文汇编.md` §9（四维实测，223 条样本：含 Toolathlon 12/108 抽样、ScienceAgentBench 41/102 经专家改写）、§10 |
| 四类模板与段结构、迁移口径 | `task/task17/query类型分化设计.md` §0b/§1/§2/§3/§3b/§4/§5 |
| 题量定数依据 | 两条硬约束交集：下限 ≥102（形态最接近两家 ScienceAgentBench 102/Toolathlon 108），上限 = 存量与审题产能（规范 §4.8/§7）；150 落在区间内，规模主张仅需 ≥108；150 为条件值、依赖 E1 扩库达标（`task/task16/专家背书query原文汇编.md` §9；`task/E1-后端数据库构造/后端库规格契约.md`；task16 "130–150"一句仅为旁证） |
| open 定义、open_score、裸答基线阈值、max_attempts ≤10、答卷 schema | `goal.md` v3.0.1 §1.5/§2b/§4 支柱5 |
| 撞名警告、open 进第一版裁定、ICLR 硬排期 | `goal.md` §1.5 命名陷阱、§6 范围裁定、§7.1 |
| 专家背书三条、等价匿名变换三原则 | `governance/adr/0012-两类benchmark设计范式与query专家背书要求.md` §2/§3 |
| D 段字段名 | `dag_poc/answer_schema.json`（v0.1.0-stub，唯一字段来源，ADR-0014）；落盘句式现状另见 `dag_poc/templates/constraint_satisfaction/template.json`、`constrained_optimization/template.json` |

## 4. 遗留问题

1. **30% 淘汰率无实测支撑（本轮最高风险）**：规范 §4.7.2 已如实声明——30% 是待标定先验，标定样本 = 首批 10 题实测，第 4 周末回填重算投造量；另有两项连带风险：①若实测裸答门淘汰率显著 >0 即匿名变换失效（踩 goal §5 红线 5，须停机排查）；②**质量门通过率是 A 规模的函数**（A 从 mock 40 → E1 契约 ≥10⁴ 后通过率可能低于 20%），已写为风险条目移交 E1/E5，open 的 gt 选取策略为"只在目标量位于 A 分布之外/边缘选 gt"。
2. **专家审题待人工发出**：发起包三件套已在规范 §7 备妥（待审题面样例包已重做为 4 子领域新稿 + 术语征询表），外部专家联络需人工执行；lead time 最长，应尽早发出（已登记 progress.md ⛔️ 阻塞）。另："每题背书出处 + 分层抽审 ≥30%"执行方案是否升格为新 ADR，待监工裁定（已写入 progress.md，E2 不自行立 ADR）。
3. **150 是条件值，依赖 E1 扩库达标**：药物 ≥10⁴、材料 5,771 未达标时按规范 §4.8 回退表降级（当前快照仍是 mock_db：药物 40 / 材料 17）；E1 构造阶段尚未启动，需跨线跟进。
4. **TRAIL 题库 gated 未纳入样本**：task16 的 223 条样本不含 TRAIL（访问受限），其风格结论未参与本规范定调；若后续取得访问权限，仅需复核是否影响 M9（句式收敛与变体率）条目，不动题量定数。
5. **类型3 属第二阶段**：evidence_grounded 未落盘，其 D 段字段命名在落盘时按"判分器读什么就写什么"总规则另定（规范 §5.3 已留接口）；建议的第二阶段 +30 题（药 15/材 15）不占第一版配额，最终是否采纳由负责人裁定。
6. **一处契约缺口已随字段切换重述并移交 E5**：`constrained_optimization/template.json` 的 response 节缺候选池字段（其 query_template 已要求提交），按 ADR-0014 该 response 节须切换为 answer_schema.json 新字段集（candidates/ranking/answer），缺口随之消解（E2 不改代码，仅登记）。

---

## 5. 监工验收意见（2026-08-06）

**结论：通过。** 逐项复核如下（全部为实测，非采信自述）。

| 验收项 | 核验方式 | 结果 |
|---|---|---|
| 题量自洽 | 脚本断言 | ⚠️ 部分成立：`fixed 120 + open 30 = 150 = total` ✅；`per_type` 合计 150 ✅；**但"open 投造 40 → 冻结 30（30% 冗余）"算术不成立** —— 40×0.7=**28**，要保证 30 须投造 **43**；40 对应的实际冗余率是 **25%** 而非 30%。**监工首轮误判为"成立"（当时用 40×0.75 核算，等于把 30% 悄悄换成 25% 去迁就结论），此处订正** |
| 各家题量对照真实性 | 回查 task16 | ✅ SciCode 80、ScienceAgentBench 102、Toolathlon 108 **原文可查** |
| 风格清单可当检查表 | 抽查条目 | ✅ 13 MUST + 10 MUST NOT，**每条带 task16 维度出处**（如 M2 引"术语出现率 48%–100%"、N2 引"三段式齐备率 0–20%"） |
| 机器可读契约 | `json.load` + 字段核对 | ✅ `dag_poc/query_spec.json` 可解析，含 frozen_by/frozen_date/per_type/discard_gates |
| 写作区合规 | `git status` | ✅ **只写自己目录 + `query_spec.json`**；未动 `template.json`、未动 TODO/problems/总览/goal |
| 阻塞如实上报 | 读 progress.md | ✅ 专家审题"需人工对外发出"如实标 ⛔️（agent 确实无法发邮件，非推责） |
| 专家审题发起包 | 查 §7 | ✅ 三件套齐（邀请函草稿 §7.1 / 背书清单模板 §7.2 / 审题记录表模板 §7.3） |

### ⭐ 值得记功的一处发现

E2 在核对 D 段字段时查出一个**真实的既有 bug**（非它引入）：`dag_poc/templates/constrained_optimization/template.json` 的 `response` 节**只声明 `ranked_candidates`，缺 `screened_pool`**，而其 `query_template` 已要求 agent 提交该字段。监工已复核确认成立——这会导致 cp2/cp3 取不到候选池而判分偏差。该 bug 由 task17 造 template 时引入，**已转 E5 在迁移时补齐**（E2 按边界"不改代码，仅登记"的处置正确）。

### ⚠️ 需订正的一处：依据链归因过度

**报告 §3 把"130–150"称为"task16 §9 证据区间"，归因过强。** 实测：task16 全文"130"**仅出现 1 次**，位于 §9 维度4 结论 3 的一句顺带表述（"这对我方要做 130–150 题有直接参考价值"），**该数字本身没有推导过程**——它是当时的预期值，不是从证据推出的区间。以它为"证据区间"再定 150，构成**循环引用**。

**但结论 150 本身可辩护**，只需换成真正的依据链：**各家实测题量最大为 Toolathlon 108（其次 ScienceAgentBench 102、SciCode 80），取 150 即高于全部参考 benchmark，可声称规模优势**。

**订正要求**：把 §3 证据表中"task16 §9 维度4 的 130–150 区间"改为"各家题量对照（80/102/108），150 > 最大值 108 故具规模优势"。**数字不变，只改依据表述。**

---

## 6. 监工二轮严格复审（2026-08-06，独立审稿驱动）

**首轮验收结论下调：从"通过"改为"有条件通过，冻结前须修 5 项"。** 首轮只核了内部自洽，未做跨线契约核对与算术复算，标准偏松。

### 监工自身两处失误（先认）

| # | 失误 | 性质 |
|---|---|---|
| 1 | 把"投造 40 → 冻结 30（30% 冗余）"判为算术成立 | **拿参数迁就结论**（实际用 25% 核算 30% 的命题）。正确：40×0.7=28，须投造 43 |
| 2 | 未做跨线契约核对，漏掉 M7 与 E3 同日冻结的 `answer_schema.json` 撞车 | 我自己设了"契约纪律"却没执行检查——这正是并行开发最易崩的接口层 |

### 🔴 冻结前必须修的 5 项

| # | 问题 | 实测证据 | 后果 |
|---|---|---|---|
| 1 | **M7 字段名与 E3 契约撞车** | E3 `answer_schema.json` 主字段为 `candidates`/`ranking`/`answer`，**不含** `screened_pool`/`ranked_candidates`（后者被标为"单向机械派生的旧字段，收敛后引擎不再维护双解析"）。而 M7 是 MUST，规定题面必须点名旧字段 | E5 按 M7 写完 150 道题面后**全部返工** |
| 2 | **投造配额算术错 + 顶格档是死分支** | 40×0.7=28 < 30；而 §4.7.3"维持 30"档的触发线是"第 5 周末存量 ≥30"——在投造 40 的前提下期望产出 28，**该档永不触发** | 降级阶梯顶格档失效；且档位区间不完备（第4周末≥20 且第5周末∈[20,30) 时无档匹配） |
| 3 | **题量与数据存量矛盾，全文 0 次提及 mock_db** | 材料侧仅 **17 晶体（常压 14）**，却分到 60 道 fixed 题 → **每候选 4.3 题**；类型2 材料 40 题的唯一可排序目标量只有 `tc`（常压候选 tc = 134/125/110/92/39/…）→ **40 道题的 cp5 排序答案是同一序列，cp5 失去区分度**。药物侧 29/40 = **72% 的分子已被旧 case 用作 gt** | 题目雷同、agent 记住一题即迁移全类；"评过程"的过程分退化为常量 |
| 4 | **15%+15% 淘汰率分解与自己的匿名变换机制互相否证** | §6.1 要求 gt 与候选池做等价匿名变换 → agent 无工具时在匿名符号空间猜中 `G_B` 的概率≈0，**裸答门淘汰率应≈0**；若真有 15%，说明匿名变换失效（踩 goal §5 红线 5）。另质量门通过率是 A 规模的函数，A 从 40 涨到 E1 契约的 ≥10⁴ 时通过率会急剧下降，规范未提 | open 配额 40 的唯一支点浮空 |
| 5 | **open 的量化目标窗口无处安放** | N4 禁数值进题面（例外仅给类型2 M 段）+ 类型4 M 段"不可用" + 类型4 `domain_knowledge`"可选" → open 题面可以完全没有量化目标。但 goal §2b 要求"推断分子 ∈ G_B **且经 B 侧性质复核满足题面目标窗口**" | open_score 第二个因子判不了分 |

### 🟠 其他重要问题（择要）

- **M9「每类只 1 个主句式」与所引 task16 结论相反**：task16 原文是"**没有一家用单一模板**，主导句式占 20%–32%"。我方 4 句式/150 题 → 变异度 0.027，比最统一的 SciCode（0.54）还统一 20 倍，主导句式占比 100%（实测上限 32% 的 3 倍）。
- **"150 因规模优势"这条依据被自己拆掉**：§4.7.3 两处写"fixed 120 独立支撑…规模优势主张仍在（120 > 108）"——**等于承认 120 就够**，那 150 的依据仍未被钉住。而 goal §6 步2 要求"从参考规模**反推**"，反推应落 ~100 量级，"高于全部参考"是营销口径。
- **M8 的 30–180 词把"中位数区间"当"个体长度区间"**，且写成"七家"实为 **5 家**有长度统计；上限 180 低于 3 家的实际最长值（334/272/205）。
- **"每题必过专家审题（ADR-0012 §2）"不是 ADR 原文**——ADR 三条无此句，是自造要求，且被单向用于卡 open 配额而不约束 fixed 120（按 §7.3 每批 ≤20 题，150 题需 8 批往返，吞吐从未核算）。
- **迁移存量算错**：按 §3.3 逐格核对，可用存量 ≤27、新造 ≥123（非 113）；且现有 37 case 的 `provenance` 全是"数据来源"而非题目背书，**37/37 不满足 M10**；另 12/37 的 gt 受 T-55 影响（该项已于 2026-08-06 修复）。
- **送审 4 份题面实际只覆盖 2 个科学场景**：样例1 与样例3 的 gt **同为 `aa`**、约束几乎相同（一份英文一份中文）；样例2 与样例4 同为常压超导 Tc 最大化、同一陷阱。且样例3/4 是我方已判作废的中文口语稿，其"已知问题"由我方自行列出——请专家审已诊断完毕的废稿，收不到新信息。
- **送审题面泄漏出题手法**：样例1 的 domain_knowledge 写 `Caco-2 window [-4.38,-4.30] (a narrow absorption band that pins a unique candidate)`——括号里直接告诉读者"窄窗是为钉唯一解而设"；且该 case 自带 `domain_knowledge_status: 英文骨架+数值占位，待 T-49 专家核`，以占位稿充"已改造版"送审。
- **report 未上报最大风险**：§4 遗留 4 条中没有"30% 淘汰率无实测支撑"这一条，而规范 §4.7.2 自己承认了。
- **"223 条全量实测"夸大**：Toolathlon 是 **12/108 抽样**（11%），却被用于支撑 N2 等条目；ScienceAgentBench 是 41/102 经专家改写（60% 未经）。

---

## 7. 监工三轮复验（2026-08-06，逐条实跑复算）

**结论：通过并冻结。** 13 项（5 必修 + 8 应修）**全部修到位，残留 0 项**。所有数字均由监工独立复算，未采信文档自述。

### 5 项必修的复验证据

| 必修项 | 复验方式 | 结果 |
|---|---|---|
| 1 M7 字段撞车 | **跨线核对** E3 的 `answer_schema.json` | ✅ M7 改为"以 answer_schema 为唯一来源"；open 必点六字段 `candidates/ranking/answer/anchor_molecules/cited_values/edit_op` 与 `answer_schema.required` **逐字一致**；旧字段降为派生并给出派生式 |
| 2 投造算术 + 死档位 | 复算 | ✅ `required_manufactured=43=ceil(30/0.7)`，公式落进 json；`degradation_ladder` 改为区间完备划分（≥30 / [20,30) / [10,20) / <10），死档位消除 |
| 3 题量×存量矛盾 | 文本 + 契约核对 | ✅ 规范提及 `mock_db` **10 次**（原 0 次）；新增 §4.8 题量×存量可行性；150 标为"依赖 E1 扩库达标"的**条件值** + 回退表；成本估算改引 E1 契约的 A 规模 |
| 4 淘汰率伪分解 | 语境核查 | ✅ 15%+15% 已删，仅作"不再给出该分解"的废弃说明；改为"待标定 + 首批 10 题为标定样本 + `投造量=ceil(冻结÷实测通过率)`" |
| 5 open 目标窗口 | 文本核查 | ✅ 类型4 `domain_knowledge` 改必需；目标窗口三要素须与 `G_B` 落盘条件逐字一致；N4 增例外③ |

### 8 项应修：全部有实质改动

其中三处**改得比要求更好**：

1. **功效分析真算了**（应修10）。原本只有"≥10 即可独立成类"的无依据门槛，现给出两比例检验：`n = [z_{α/2}·√(2p̄(1−p̄)) + z_β·√(p₁(1−p₁)+p₂(1−p₂))]² / Δ²`，取 p₁=0.4/p₂=0.6/Δ=0.2/α=0.05/功效80% 得**每组约 97 题**。
   **监工复算：96.9 ≈ 97 ✓；n=30 的 95%CI 半宽 ±0.179（它写 ±0.18）✓；n=10 为 ±0.310（它写 ±0.31）✓——三处全部算对。**
   并由此得出**比要求更严的结论**：open 30 题只支撑描述性报告与单题命中率，**不支撑两模型差异显著性主张**；显著性主张挂 fixed 120 侧（120 > 97 功效达标），论文中明示 open 侧功效不足。

2. **匿名变换的一致性检查升级为停机条件**（必修4 延伸）。不只是删掉伪分解，而是写成可执行判据："若实测裸答门淘汰率显著 >0，即说明匿名变换失效，踩 goal §5 红线 5，**必须停机排查而非继续造题**"，并列入首批 10 题标定项。

3. **类型配额从循环论证改为 60:60**（应修11）。原"类型2 唯一带完整 6cp 故配 2 倍题量"是循环论证（类型1 就是类型2 去掉 cp5）。现改为每格 30 的平均分配，`query_spec.per_type` 同步为 60/60/0/30（合计 150 自洽）。**副作用是正面的**：类型1 药物配额从 20 提到 30 后，29 道旧题不再有 10 道作废，可迁存量从 ≤27 升至 ≤36、新造从 ≥123 降至 ≥114。

其余五项：M9 加"变异度 ≥0.3"可机器判定指标；150 的依据改为"下限 ≥102（形态最接近两家）∩ 上限（存量与审题产能）"的交集，删掉"故可声称规模优势"；M8 按类型分档（类型1/4：30–120，类型2：60–200）并改述为"中位数区间为目标带、最长值不作硬上限"；"每题必过专家审"改为"每题背书出处 + 分层抽审 ≥30%"并**明确对 fixed 120 与 open 30 同样适用、非单向卡 open**，附吞吐核算（8 批往返、单批 lead time ≤4 天）；段结构补 T 段（`A + T + D` / `A + M + T + D`，段序改 `S→A→M→T→D`）。

### 残留问题扫描：0 项

| 检查 | 结果 |
|---|---|
| report 是否把"30% 淘汰率无实测"列为遗留风险**第 1 条** | ✅ 已置首位，并附两项连带风险 |
| `query_spec.json` 是否含 `min_publishable` / 功效口径 | ✅ 有 |
| type3 分域数字是否补 | ✅ `phase2_proposed_drug: 15` / `phase2_proposed_material: 15` |
| "223 条全量"是否改为标注抽样 | ✅ 已标 Toolathlon 12/108 |
| 送审包是否仍泄漏出题手法（`pins a unique candidate`） | ✅ 已删 |
| 送审包是否仍含已作废中文口语稿 | ✅ 已删 |

### 冻结声明

`dag_poc/query_spec.json`（总 150 / fixed 120 = 类型1 60 + 类型2 60 / open 30，投造 43 / 药材 1:1）**即刻冻结**。**变更须报监工立 ADR + 通知全部下游线**，E5 按此造题。

**唯一仍待外部动作**：专家审题需人工发出 `待审题面样例包.md`（agent 无法执行）。这是全链路 lead time 最长的一环。

---

## 8. 第四轮：校验脚本 query_lint.py 落地与 37 case 实跑（任务 ID 11，2026-08-06）

**交付**：`dag_poc/query_lint.py`（新建，唯一代码产出），把规范 §2 的 11 条机器可判规则落成确定性判据；规范 14 处"脚本断言"已闭环指向本脚本函数名/规则号。E2 只报告哪些 case 不过，未改任何 case（E5 造题、E2 验题）。

### 8.1 规则实现清单（规则号 ↔ 函数 ↔ 层级）

| 规则 | 函数 | 层级 | 判定要点 |
|---|---|---|---|
| M1 | `check_m1_english` | 题级 | CJK 字符计数 == 0 |
| M4 | `check_m4_last_sentence` | 题级 | 末句含该类型全部必点字段名 |
| M5 | `check_m5_domain_knowledge` | 题级 | domain_knowledge 非空且 query 含引用句 |
| M6 | `check_m6_scoring_rubric` | 题级（仅 type2） | 必含 "Scoring:" 标尺句 |
| M7 | `check_m7_field_contract` | 题级 | 必点字段全集词边界包含（fixed 三字段/open 六字段）且不含 screened_pool/ranked_candidates |
| M8 | `check_m8_length_band` | 题级 | type1/4 30–120 词、type2 60–200 词；type2 超 200 触发复核项（pending） |
| M11 | `check_m11_budget_sentence` | 题级（仅 type4） | 含 §6.3 预算句标准写法原文 |
| M12 | `check_m12_no_placeholder` | 题级 | 排除 TBD/TODO/占位/待补等标记词 |
| M9 | `check_m9_variation` + `check_m9_gt_unique`（lint_corpus） | 库级 | 开头两词模式种类÷该类题数 ≥ 0.3；gt 全库 ≤1 |
| M13 | `check_m13_domain_symmetry`（lint_corpus） | 库级 | 读 query_spec.json：per_type 各格 drug==material，并与存量 case 分布核对 |
| R4 | `check_r4_anonymization_mapping`（lint_corpus） | 库级 | 映射不得与周期序/字母序单调相关；映射表未落盘 → 报"待 E5/E6 落盘映射表后启用"（pending），不静默跳过 |

人工/专家判定条目（M2/M3/M10/N1/N3/N6/N7/N8）脚本不假装能判，逐 case 输出"需人工复核"清单（37 case × 7–8 条）。

**类型判定口径**：按 case 所在模板目录推断（`constraint_satisfaction`→类型1、`constrained_optimization`→类型2、`discovery_haystack`→类型4、`evidence_grounded`→类型3）；旧目录 `fixed_discovery`/`open_discovery` 属 fixed 且为待迁移状态（规范 §3.3）：分别按类型1/类型2 判（open_discovery 撞名警告 §3.2——目录名带 open 但实为 fixed 约束最优型），违规记录带 legacy 标记。

**接口与退出码**：`python dag_poc/query_lint.py --cases '<glob>' [--strict] [--json] [--mapping PATH] [--spec PATH]`；可 import（`load_cases`/`lint_case`/`lint_corpus`/`run`）。退出码：0=全过；1=有违规；2=无违规但 `--strict` 下有复核/待启用项（pending）。每条违规输出 case_id/规则号/实测值/期望值。

### 8.2 --strict 自测证据（避开 verify_snapshot_identity.py 退出码失效先例）

自测 fixture：`task/E2-query风格与题量定数/lint_selftest/fixtures/constraint_satisfaction/`（2 个全绿 case，药/材各一，gt 互异、开头模式不同）。

| 测试 | 命令 | 实测退出码 | 结论 |
|---|---|---|---|
| 全绿路径 | `python3 dag_poc/query_lint.py --cases '…/lint_selftest/fixtures/*/case_*.json'` | **0** | ✅ 全过=0 |
| strict 改变退出码 | 同上 + `--strict` | **2**（R4 映射表未落盘 pending 计入） | ✅ --strict 真实改变退出码 |
| 违规样本 | `--cases 'dag_poc/templates/*/case_*.json'`（非 strict 与 strict） | **1 / 1** | ✅ 有违规=非 0 |

另做双向判定抽查（防"恒黑/恒白"）：M6 对含 Scoring: 句的存量 case 不误报；M11 缺预算句报/含标准句不报；M12 含 TBD 报/不含不报；R4 周期序单调映射报/乱序映射不报——全部通过。

### 8.3 37 case 实跑（如实报告，未放宽任何断言）

实跑命令：`python3 dag_poc/query_lint.py --cases 'dag_poc/templates/*/case_*.json' [--json]`（在仓库根目录执行）。

**总结果：37/37 个 case 全部不过，共 184 条违规 + 2 条 pending；退出码 1。** 现有 37 case 是旧规范产物，大面积不过正是预期——这正是脚本的价值：把"需要迁移多少"变成确定数字。

| 规则 | 不过数 | 实测摘要 |
|---|---|---|
| M1 英文骨架 | 33 | fixed_discovery 29 + open_discovery 4 为中文题面（CJK 计数 81–105）；cs/co 4 个英文 case 过 |
| M4 末句字段 | 37 | 全部 case 末句缺新字段名（旧题面末句为"最终输出你认为最优的答案"或旧字段 screened_pool/ranked_candidates） |
| M5 约束外置 | 33 | fd 29 + od 4 无 domain_knowledge 字段且无引用句；cs/co 4 个已过 |
| M6 Scoring 句 | 4 | od 4 个按类型2 判但无 "Scoring:" 句；co 2 个已过 |
| M7 字段契约 | 41 | 37 条缺必点字段（candidates/ranking/answer）+ 4 条含旧字段字样（cs/co 的 screened_pool/ranked_candidates） |
| M8 长度分档 | 33 | 中文题面无空格分词，词数=1–7，全部低于 30 词下限 |
| M9 gt 唯一 | 1 | gt='aa' 同类型（类型1）复用：case_cs_cns_drug（constraint_satisfaction）与 case_cns_metoprolol（fixed_discovery，同判类型1）；变体率两项均 ≥0.3 过（注：中文题面使"开头两词"退化为整句 token，该子项对旧中文题约束力有限，将在 E5 英文造题后真正生效） |
| M11 预算句 | 0 适用 | 存量无 type4 case（discovery_haystack 0 template），无违规也无通过样本 |
| M12 标记词 | 0 | 题面与 domain_knowledge 均无 TBD/占位类标记 |
| M13 领域对称 | 2 | spec per_type 各格 drug==material ✅；但存量分布不对称：类型1 drug=30/material=1，类型2 drug=1/material=5 |
| R4 匿名映射 | pending | 映射表未落盘 → 报"待 E5/E6 落盘映射表后启用"（未静默跳过） |

pending 另一条：6 个 case 无 gt.id 字段（co 2 + od 4），gt 唯一性检查未覆盖，需 E5 迁移时补齐。

### 8.4 迁移工作量结论（脚本把"需要迁移多少"变成确定数字）

1. **题面重写：37/37 全部需要**。其中 33 个（fd 29 + od 4）是中文口语题面，需全文重写为英文骨架 + domain_knowledge 外置 + 字段级 D 段（M1/M5/M8 同时不过）；cs/co 4 个已是英文骨架，只需字段名切换（screened_pool/ranked_candidates → candidates/ranking/answer）与末句改写，属轻量迁移；
2. **gt 去重 1 处**：gt='aa' 被两题复用，迁移时其中一题须换 gt 并重验可解性；另有 6 个 case 缺 gt.id 需补齐；
3. **领域对称补缺**与 §4.2 逐格缺口表完全呼应：类型1 材料 +29、类型2 药物 +29（M13 实测分布 drug/material = 30/1 与 1/5），叠加总缺口 = 可迁 ≤36、新造 ≥114；
4. **背书补欠 37 条**：M10 属专家审，脚本逐题列入"需人工复核"清单（与 §3.3"37/37 provenance 均为数据来源"核实一致）；
5. **判分内核不动**：以上全部为题面层工作量，cp reward/门控/task_score 不受影响（§3.3 口径），与"迁移不减少题面撰写与审题工作量"的结论互证。

### 8.5 边界遵守

本轮只写：`dag_poc/query_lint.py`（新建）、`task/E2-query风格与题量定数/` 下（规范 14 处闭环、本小节、progress.md、lint_selftest fixture）。未动 `dag_poc/templates/**`、`query_spec.json`、gateway/evaluate/schema、共享账本。写脚本过程中未发现新的需专家确认题面点（存量问题均属已知迁移项），故未追加「待确认清单」。

---

## 9. E2-02：容量判据修订 + ADR-0016 落地 + lint 补 provenance 断言（任务 ID 14，2026-08-06）

依据：`task/E2-query风格与题量定数/task-02-容量判据修订.md`、`task/_prompts/裁定-题量容量判据修订.md`、`governance/adr/0016-专家背书执行方案分层抽审.md`（Accepted）。

### 9.1 本轮改动清单

| # | 改动 | 落点 |
|---|---|---|
| 1 | **§4.8 判据替换为「可造题格数」**：类型1 可用格 = 切分后格内目标量最优者唯一的格数；类型2 可用格 = 格内成员 ≥3 的格数。`\|G\|=1` 语义澄清（"最优者唯一"≠"候选池仅 1 成员"，误读会把材料侧 69 格误算成 10 格）写在 §4.8.1 的 ⚠️ 警示框内 | 规范 §4.8.1 |
| 2 | **附实测容量表**（药物 154/91、材料 69/59，四格全达标）与切分参数（药物 `atom_count//5 × round(logS)` →156 非空格；材料 `family × tc五档 × 元素种类数` →73 非空格）；旧实体数判据与 mock 快照"材料类型2 ≤10"推导均声明作废 | 规范 §4.8.2 |
| 3 | **条件值状态理顺**：150 从"依赖 E1 扩库达标的条件值"转为"实测支撑成立"，冻结值不动、不走变更 ADR；回退表改为未来库变动时的触发口径（以新判据重测为准）；§0/§4 小结/§4.3/§4.5/§4.6/§4.7.1 中旧条件值与 mock 表述同步理顺 | 规范 §4.8.3 及全文 7 处 |
| 4 | **M5 药物侧约束族示例更新**：改为「1 个密集性质 + 结构约束」= logS（2,311 有值）+ atom_count（92 种取值）；注明原 bbb+herg+caco2 交集仅 14、再交现有 case 约束仅 7，不足以支撑配额 | 规范 §2.1 M5 行、§4.8.2 |
| 5 | **原三性质题型保留为「第二版候选」**：待 ADMET 覆盖补齐后启用，原描述与样例不删除；第一版造题不用该约束族 | 规范 §4.8.2、§2.1 M5 |
| 6 | **材料侧两条硬前置写进规范**：①必须排除 tc==0（1,764/5,771=30.6%，3DSC 含非超导样本）；②题面不得限定"常压/ambient"（condition 实测：未知 5,755/ambient 15/high_pressure 1，仅 16 条有标注），替代写法用 "in the candidate database"；启用常压题型需先补 condition 标注（另立任务，非本轮） | 规范 §4.8.4 |
| 7 | **§7 改引 ADR-0016**：执行口径以 ADR 原文为准成表（背书出处 100%/抽审 ≥30% 类型×领域分层/抽中走三问/未抽中兼核/系统性问题升全审/降级 ≥15%）；删自造的"报监工裁定"句（已获裁定升格）；补"题目背书 ≠ 数据溯源"区分段 | 规范 §7、§4.5/§4.7.3 指针 |
| 8 | **query_spec.json 增注记（数字一个未改）**：`capacity_basis`（判据定义 + 切分维度 + 四格可用格数 + 状态）与 `material_constraints`（exclude_tc_zero: true、condition_annotated_only: 16 及不限常压原因）；git diff 确认纯新增 22 行、0 删除 | `dag_poc/query_spec.json` |
| 9 | **query_lint.py 增 provenance 形态断言**（新规则 M10 机器形态，ADR-0016 影响项）：`check_m10_provenance` —— provenance 须含 DOI（`10\.\d{4,}/`）或 URL（`https?://`）且长度 >60、不匹配 `mock_db\|snapshot\|证据_` 纯数据来源模式；只判形态，背书真实性仍列人工复核清单由专家按 ADR-0016 抽审；两个自测 fixture 的 provenance 同步升级为背书形态 | `dag_poc/query_lint.py`、lint_selftest fixtures |

### 9.2 query_spec.json 自洽断言结果

改后重跑自洽断言**全部通过**：`json.load` 可解析 ✅；`total_questions==150` ✅；`fixed_total==120`、`open_total==30` ✅；per_type 各格 `drug+material==count`（四格全核）✅；`required_manufactured==43` ✅。**git diff 性质确认：`1 file changed, 22 insertions(+)`，删除行 0**——仅新增 `capacity_basis`/`material_constraints` 两个注记字段，无任何原字段/原数字变动。

### 9.3 lint 重跑：新规则违规数与原 184 条的关系

**fixture 自测（保全绿路径）**：升级后的 2 个 fixture 0 违规；非 strict 退出码 0、`--strict` 退出码 2（仅 R4 映射表待落盘 pending）——三态退出码语义不变。

**37 case 实跑（只报告、不修任何 case）：37/37 不过，总违规 184 → 221，退出码 1。**

| 项 | 旧（任务 ID 11/13） | 本轮 |
|---|---|---|
| 总违规 | 184 | **221**（+37，全部来自新规则 M10） |
| **M10 provenance 形态（新）** | —（原属人工清单） | **37/37 不过**（现有 provenance 全为 `mock_db.admet（真值来自…证据文档）`类纯数据来源：无 DOI/URL 且命中数据来源模式，与 ADR-0016 备案的"37/37 不合格"核实一致） |
| 其余规则 | M1 33/M4 37/M5 33/M6 4/M7 41/M8 33/M9 1/M13 2 = 184 | **逐条完全不变，仍为 184**——新规则纯增量，未扰动原结果 |
| pending | 2（R4 待落盘、6 case 缺 gt.id） | 2（不变） |
| 退出码 | 1 | 1（新规则违规按 exit 1 计；`--strict` 三态语义保持） |

**迁移影响**：背书补欠从"37 条待专家补"升级为"37 条被脚本硬拦"——E5 迁移时每题必须落含真实论文标题 + DOI/URL 的 provenance 方可过 M10；注意后端库数据溯源（如 TDC_ADMET:BBB_Martins）不能替代题目背书。

### 9.4 边界遵守

本轮只写：`task/E2-query风格与题量定数/` 下（规范、report、progress、task-02 表头状态行、fixtures）+ `dag_poc/query_spec.json`（仅增注记）+ `dag_poc/query_lint.py`。未动 templates/**、gateway/evaluate/schema、data/backend/**、共享账本与 governance/adr/**；需立 ADR 的事项（背书执行方案升格）已由监工裁定为 ADR-0016，E2 未自行立 ADR。

---

## 10. 审题包重做定稿（任务 ID 17，2026-08-06）：容量裁定对齐版

**背景**：上午做的审题包四份样例全部与下午的容量判据裁定冲突（样例 1 用被弃用的三性质题型交集仅 14；样例 2 用稀疏性质 hERG 仅 104 有值；样例 3/4 题面含被禁的压力条件表述，condition 仅 16 条标注、限定后池剩 15 个）。本项卡着 owner 发出专家审题，按 task-02 §2b 七条要求重做。

### 10.1 改动清单（四份样例新约束口径）

| 样例 | 旧口径（与裁定冲突） | 新口径（容量裁定对齐） |
|---|---|---|
| 1 药物·CNS（类型1） | BBB + hERG + absorption 三性质 | **logS 溶解度窗 + atom_count 结构规模窗**（约束写入 domain_knowledge，切分口径 `(atom_count//5, round(logS))`，|G|=1 语义随占位说明附） |
| 2 药物·抗菌（类型1） | hERG（稀疏，仅 104 有值） | **同约束族不同场景**（口服给药兼容溶解度窗 + 结构规模窗），子领域仍与样例 1 不同 |
| 3 材料·超导（类型2） | Tc + 压力条件限定 | **cuprate 族 family 分层包装 + 排除 tc==0（占 30.6%）声明 + `in the candidate database` 库内范围限定**，目标量仍为 Tc 最大化 |
| 4 材料·电池电极（类型2 草案） | 含压力条件表述 | 同骨架改库内限定；**"数据待定稿"标注保留**；注明 E1 电极性质族就位后套用 family 分层、本族无 tc 字段故排除口径届时替代再送核 |

**其余改动**：①专家说明工作量口径按 **ADR-0016 修订 R1 新口径**写（100% 覆盖 + 分级审：一级全量扫读每题 2–3 分钟、二级 ≥30% 深审按类型×领域分层抽取、多专家并行每人 ≤40 题）——**未用 §2b.2 第 7 条的旧口径**（"抽审 ≥30%"已被 R1 作废，该条写于修订前）；②新增"如实说明三"容量裁定对齐声明（含旧题型降为第二版候选、专家可回执复议）；③术语征询表更新为 10 项（增溶解度/分子规模/库内限定/族名/Tc=0 剔除等，删旧压力条件限定项）；④D 段 candidates/ranking/answer（ADR-0014）未动；⑤保留结构：4 个不同子领域、类型/判分字段/考点、背书三问+作答栏、open 类如实标注无样例。

### 10.2 与裁定的逐条对照

| 裁定/要求 | 本包落实 |
|---|---|
| 弃用三稀疏性质交集题型（交集仅 14） | 样例 1/2 已换 logS + atom_count；旧题型仅在如实说明三中作历史背景提及、不送审 |
| 不用稀疏性质（hERG 仅 104） | 样例 2 已换约束族 |
| 题面不得含压力条件限定（标注仅 16 条、限定后池剩 15） | 样例 3/4 题面与 domain_knowledge 均改用库内限定；自查 grep 确认全文件 0 处相关字面残留 |
| 排序目标量仍为 tc、须声明排除 tc==0（30.6%） | 样例 3 domain_knowledge 含 Superconducting validity 条目 + 口径说明；样例 4 注明本族不适用、届时另定 |
| family 分层包装 | 样例 3 用 cuprate 族（E1 9 族：Cuprate 874 / Ferrite 517 / Heavy_fermion 409 / Oxide 320 …） |
| 工作量口径（R1 新口径） | 专家说明已按 100% 覆盖 + 分级审写实际负担 |

**边界**：本轮只改 E2 目录内三个文件（待审题面样例包.md、progress.md、report.md），未动 templates/** 与任何其他线产物。**progress.md 已完成段第 15 条已写明信号行"审题包已重做定稿，可发"，请监工据此通知 owner 发出。**

---

## 11. E2-03 决议落地与规则化补强（任务 ID 18，2026-08-06）

**背景**：落地 owner 三项决议（ADR-0016 修订 R1 专家审口径升级、ADR-0016 修订 R1 五条规则化断言补强、ADR-0014 修订 R1 版本同步授权）+ 解决 §5.4×M9 内部矛盾；ADR-0017 知悉不实施。

### 11.1 改动清单

1. **规范 §7 重写为 ADR-0016 修订 R1 口径**：专家覆盖率 100%（全部 150 题）；多专家并行（药化 2 + 材料 2，每人 ≤40 题），8 批串行压缩为 2 轮并行；分级审（一级全量扫读每题 2–3 分钟、二级 ≥30% 背书三问深审按类型×领域分层）；一级发现系统性问题⇒该层全升二级；κ 不启用；LLM 仅作召回器（无放行权），报数只报"LLM 提示被专家采纳率"并明示非质量门。留痕"为什么不用 LLM 预审替代"：κ 可信需专家标注 ≈300 题 > 全部 150 题（自相矛盾）+ 违反 N9（第一版不启用该例外）。同步替换全文 5 处旧"分层抽审 ≥30%"口径（§4.4/§4.5/§4.7.3/§7.3 审题记录表/§8 证据回链）。
2. **5 条断言从人工清单移入 `dag_poc/query_lint.py`**（N7/N1/N3/M4 句末标点/N8 前置，判定表达式见 §11.3）；`MANUAL_RULES_ALL` 收敛为 ["M2","M3","M10"]。规范 §2.1 M4/M9/M10 行与 §2.2 N1/N3/N7/N8 行同步标注脚本断言；§6.2 补 N8 前置断言注。
3. **§5.4×M9 矛盾解法落地**（详见 §11.4）：规范 §5.4 重写为主句式骨架清单（类型1 20 种/类型2 10 种/类型4 10 种开头两词模式）；M9 变体率校验从库级挪到**每批冻结时**执行（lint 新增 `--freeze-batch NAME`，日常运行报 pending 留痕不静默跳过）；§2.1 M9 行、§2.3.1/§2.3.2 骨架行同步。
4. **版本同步（task-03 §4 已授权，本轮唯一动 query_spec.json 处）**：`answer_field_contract.single_source` 0.1.0-stub → 0.2.0；规范内 4 处版本引用同步（表头、M7、§5.3、§8 证据回链）。

### 11.2 重跑结果（37 case，一个未修，只报告）

| 口径 | 结果 |
|---|---|
| 总违规 | **221 → 318**（净增 97 = N7 29 + N1 29 + N3 33 + N8 6；M4 句末标点断言新增 0 违规） |
| per_rule | M1:33 / M10:37 / M13:2 / M4:37 / M5:33 / M6:4 / M7:41 / M8:33 / M9:1 / **N1:29 / N3:33 / N7:29 / N8:6** |
| pending | 3 条：M9 变体率未执行（非冻结批次，留痕）；6 case 无 gt.id；R4 映射表待 E5/E6 落盘 |
| 退出码 | 1（--strict 三态语义保持：新规则违规按 exit 1） |
| fixture | 非 strict exit 0 / strict exit 2（0 违规 2 pending），全绿路径未被新规则误伤 |
| freeze-batch 实测 | `--freeze-batch poc-b0` 下 M9 变体率执行：type1 31 题 31 种开头模式 ratio=1.0 通过（存量少使 ratio 虚高，与 §5.4 小结判断一致） |

**N7 命中 29 个 case_id（全部为 case_id 携带 gt 真名，即任务书预指的漏检实例类别，含 case_antibiotic_sulfamethoxazole、case_anticoagulant_warfarin、case_acute_cns_risperidone 等）**——只报告，重命名归 E5，本轮不修。完整命中清单：case_acute_cns_risperidone、case_antibiotic_sulfamethoxazole、case_anticoagulant_warfarin、case_anticonvulsant_phenytoin、case_anxiolytic_diazepam、case_bronchodilator_theophylline、case_cardiac_sotalol、case_cns_metoprolol、case_cns_stimulant_caffeine、case_highsolubility_antidiabetic_metformin、case_intermediate_antiarrhythmic_quinidine、case_lipophilic_antipsychotic_clozapine、case_longacting_antidepressant2_mirtazapine、case_longacting_antidepressant_fluoxetine、case_longacting_antipsychotic_haloperidol、case_longacting_peripheral_antihypertensive_amlodipine、case_moderate_ace_inhibitor_ramipril、case_moderate_antidepressant2_nortriptyline、case_moderate_antidepressant_sertraline、case_moderate_antihistamine_cetirizine、case_moderate_peripheral_antihypertensive_lisinopril、case_nonsedating_antihistamine_loratadine、case_peripheral_cardiosafe_antihypertensive_atenolol、case_peripheral_diuretic_furosemide、case_shortacting_antidepressant_bupropion、case_shortacting_antihypertensive_propranolol、case_shortacting_diuretic_hydrochlorothiazide、case_ultrashort_antacid_omeprazole、case_ultrashort_antibiotic_amoxicillin。

**N8 命中 6 case**（gt.id 缺失）：case_co_cns_drug、case_co_supercon_material、case_open_balanced_superconductor、case_open_high_tc_superconductor、case_open_stable_superconductor、case_open_superconductor。

### 11.3 5 条断言判定表达式

| 规则 | 判定表达式（`dag_poc/query_lint.py`） |
|---|---|
| N7 | `visible = query + case_id + provenance + target_domain + domain_knowledge`；逐条断言不含：CAS `\b\d{2,7}-\d{2}-\d\b`；InChIKey `\b[A-Z]{14}-[A-Z]{10}-[A-Z]\b`；造题侧已知真名清单（29 个）`(?<![A-Za-z])name(?![A-Za-z])`（IGNORECASE；注意不能用 `\b`，case_id 下划线两侧均为 word char）；`gt.name` 不出现于 query；`gt.id` 以 `(?<![A-Za-z0-9_])id(?![A-Za-z0-9_])` 不出现于 query |
| N1 | query + domain_knowledge 命中口语黑名单即违规（7 组）：进入大脑→BBB permeability；对心脏安全\|不伤心脏→hERG safety；阻断钾通道→hERG channel blockade；水溶性→logS；脂溶性→logD；作用时间→half-life/duration；口服吸收→oral absorption |
| N3 | query 命中泛指短语黑名单即违规（6 个）：`输出你认为最优的`、`你认为最[好佳优]的?(答案\|候选\|分子)?`、`你觉得最[好佳优]`、`the best answer you think`、`your final choice`、`your best guess`（英文 IGNORECASE） |
| M4 | 原有"末句含该类型全部必点字段名"保持；新增**单句收尾 = 句末标点计数 == 1**：`query.rstrip()` 尾部匹配 `[.!?。！？]+$`，组内字符数 ≠ 1 即违规（无标点收尾或多连标点均违规，纯文法） |
| N8 | `gt.id` 为空（None/""）即违规——B3/B4 命中判定的标的前置；B3/B4 实跑（k=10）归 E6，匿名覆盖待 ADR-0017 映射表落盘后由 R4 转实判 |

### 11.4 §5.4×M9 矛盾解法与依据

矛盾：§5.4"沿用已落盘句式只填 slots" vs M9 变体率 ≥0.3——60 道类型1 若全以 Identify a/an 开头，变体率 = 2/60 = 0.033 ≪ 0.3，必然违 M9（此前仅 2 个 case 使 ratio=1.0 掩盖矛盾）。**选定解法（方向一：骨架清单）**：为每类定义 N 个允许的主句式骨架（开头两词模式），**N ≥ 0.3 × 该类题数**——类型1 20 种/60 题、类型2 10 种/30 题、类型4 10 种/30 题（均 ≈0.333 > 0.3），尾部骨架仍沿用已落盘 query_template 只填 slots。依据与防污染论证：M9 初衷是防"记住一题迁移全类"；均匀轮换时单骨架承载 ≤ Q/(0.3Q) ≈ 3.3 题，记住一题句式至多迁移到同骨架约 2–3 题（迁移面 ≤ 约 5%），且骨架间 slots/约束族/gt 均不同（gt 全库 ≤1 另保答案不重复）。骨架清单是造题侧封闭枚举，不违 R1"滚动新增规则只许黑名单"边界；专家反馈只许往 N1/N3 黑名单加词、不许改骨架清单为白名单句式断言。执行时机按 R1 挪到每批冻结时（`--freeze-batch`），日常运行报 pending 留痕。

### 11.5 版本同步自洽结果

query_spec.json 仅改 `answer_field_contract.single_source` 一行（git diff 核验：+1/-1，其余数字未动）。自洽断言全部通过：json 可解析、total_questions==150、fixed_total==120、open_total==30、per_type 各格 drug+material==count、open_surplus_quota.required_manufactured==43；single_source 含 "version 0.2.0" 且不含 "0.1.0-stub"；answer_schema.json `meta.version`=="0.2.0" 对齐。

### 11.6 ADR-0017 知悉不做（记录备查）

两项影响待 E1 交付匿名映射表后再做，本轮不实施：① R4 从 pending 转实判（映射表落盘后 `check_r4_anonymization_mapping` 启用单调相关性断言）；② A 侧返回值不含真实化学身份断言（N7 的匿名覆盖部分届时并入）。当前 lint 对 R4 如实报 pending"映射表文件不存在、待 E5/E6 落盘后启用"，不静默跳过。

**边界**：本轮只写 task/E2-query风格与题量定数/ + dag_poc/query_spec.json（仅版本号一行）+ dag_poc/query_lint.py；未动 templates/**（N7 命中的 case_id 只报告不修）、gateway/evaluate/answer_schema/tools_schema、dag_poc/data/**、共享账本与 adr/**；未自行立 ADR。

---

## 12. E2-04 容量数字订正 + 匿名断言转实判（任务 ID 20，2026-08-07）

**背景**：E2-03 验收通过；本轮落地两件事——药物侧容量数字订正（156/154/91 → 155/153/92，依 E1 logS 方案修复后实算值）与两条 pending 匿名断言转实判（E1 已落盘匿名映射表，ADR-0017 依赖就位）。

### 12.1 改动清单

1. **capacity_basis 订正（query_spec.json）**：药物侧三数 156 非空格/154 类型1可用/91 类型2可用 → **155/153/92**，新增 `correction_E2-04` 注记（依 E1 在 logS 方案修复后的实算值订正，原值为监工早期核算、未计入实体集变化；差异不影响结论，总量 150 不动、不走变更 ADR）。git diff 核验：仅改 3 数 + 新增 1 行注记，其余数字（总 150/fixed 120/open 30/投造 43/材料侧）未动。规范 §4.8.2 实测容量表同步订正并写明订正原因，全文 7 处旧数字（§4 小结/§4.3 上限推理/§4.6/§4.8 小结/§4.8.2 表与切分参数/§8 证据回链）同步替换。
2. **R4 匿名映射断言从 pending 转实判**（`query_lint.py:check_r4_anonymization_mapping` 重写）：适配 E1 落盘结构（element_to_label + carrier，兼容旧式 mapping 键）；排除 '*' 保留标签（非元素，映射表 element_source 明示不参与 R4 断言）。
3. **新增 A1 "A 侧无真实化学身份"断言**（`query_lint.py:check_a_side_identity`），接入 lint_corpus 恒库级执行。
4. **规范 §6.1 R4 行**更新为实判三项口径与实判结果。

### 12.2 两条新断言判定表达式与实判结果

**R4（三项，实判全过，0 违规）**：
- ① 映射表存在且可解析：`dag_poc/data/anonymization_mapping.json` 可 json.loads，映射条目（element_to_label）≥2 条；
- ② 非单调：标签序列按元素符号字典序排不得单调非递减（字母序），按原子序排不得单调非递减（周期序，仅 ATOMIC_NUMBER 收录元素参与；'*' 排除）——防"试几次就能反演"；
- ③ 载体：标签均为正整数同位素编号 N≥1 且 carrier.type=="RDKit dummy atom"、carrier.notation 以 `[N*]` 开头；标签为真实元素符号（元素替换载体，ADR-0017 禁止）即违规。
- 映射表不存在时仍如实报 pending、不静默跳过（防回退保护）。

**A1（实扫三项 0 命中，与监工基线对齐）**：
- 扫描对象与依据：A 侧返回值落盘 = `dag_poc/data/backend/*/[tool]_A.jsonl`（ADR-0017 A/B 分区中 agent 可达分区；B 侧与 judge 账本 agent 不可达，不属扫描对象）。实扫 4 个文件 189,351 行（get_entity_A 80,712 / search_candidates_A 80,712 / get_property_A 27,618 / literature_search_A 309）。
- 判定式：① 真实 InChIKey 正则 `\b[A-Z]{14}-[A-Z]{10}-[A-Z]\b` 无命中（全字段）；② 真实分子式无命中：Hill 整式 `^(?:C\d*)?(?:H\d*)?(?:[A-Z][a-z]?\d*)+$`（无方括号）或子串 `C\d+H\d+`（全字段）；③ 裸元素符号无命中：先剔除 dummy atom 记号 `[N*]`/`[N*H]`/`[N*H3+]`/`[N*-]`/`[*]` 与记号前缀（FORMULA:/SPACEGROUP: 等），仅扫化学记号字段（formula/smiles/masked_structure/structure_hint）的无空格分节（晶体学元数据 SPACEGROUP "I 4/m m m"、SYSTEM "tetragonal" 等英文词不在扫描范围，前后字母边界另防单词内子串误伤）。
- 字段策略：get_entity_A/search_candidates_A 全字段扫 InChIKey/分子式；get_property_A 只扫 entity_id/value（prop 是规范允许的性质术语如 bbb/herg/logS）；literature_search_A 豁免（文献元数据不含分子身份，标题引用真实文献为 M10 要求，如实说明）。
- 结果：**三项 0 命中**，与监工验收 E1-03 基线（80,712 条两项 0 命中）对齐；正例自检（注入 C16H19N3O5S/真实 InChIKey/dummy 样本）确认断言能抓到泄漏且不伤 dummy 记号。
- 全绿时以 pending 形态留痕扫描范围与行数（非违规、非待启用）；若命中则报违规并明示"报监工裁定、不自行下结论"。

### 12.3 重跑结果（37 case，一个未修）

| 口径 | E2-03 时 | 本轮 |
|---|---|---|
| 总违规 | 318 | **318（不变，逐规则全不变）** |
| pending | 3（M9 变体率未执行 / 6 case 无 gt.id / **R4 映射表不存在**） | **3（M9 变体率未执行 / 6 case 无 gt.id / A1 扫描留痕）——R4 已从 pending 转为实判通过，A1 新增留痕** |
| 退出码 | 1 | 1（--strict 三态语义保持） |
| fixture | 非 strict 0 / strict 2（0 违规 2 pending：M9+R4） | 非 strict 0 / strict 2（0 违规 2 pending：M9+A1），全绿路径未被新断言误伤 |

**无需报监工裁定的异常**：容量复算未执行（任务书明令直接用 E1 实算值，未自行重算）；A1 命中数 = 0 与基线一致；R4 三项全过。

**边界**：本轮只写 task/E2-query风格与题量定数/ + dag_poc/query_spec.json（仅 capacity_basis 三数与注记）+ dag_poc/query_lint.py；未动 dag_poc/data/**（映射表只读）、templates/**、gateway.py/evaluate.py、共享账本与 adr/**；未自行立 ADR。

---

## 13. E2-05 补 open 类专用 lint 规则 + M13 分阶段口径（任务 ID 22，2026-08-07）

**背景**：ADR-0018 定的 open 类约束此前一条都没有机器断言；本轮把四条能机器判的补进 `query_lint.py`（规则数 18 → 22），并给 M13 加分阶段口径（分批构造中降告警、冻结升违规，规则不删）。E5 已交付 15 道 open（`dag_poc/templates/discovery_haystack/case_dh_drug_01..15.json`）作实跑对象。

### 13.1 改动清单

1. **query_lint.py 新增四条 open 专用规则**（只对 type4/discovery_haystack 生效，`lint_case` 内类型前置过滤；fixed 类无 gradient/view_ref/g_b 字段不误报）：`check_open_gradient`（O1）、`check_open_k_floor`（O2）、`check_open_view_ref`（O3）、`check_open_gb_registered`（O4）；新增常量 `GRADIENT_RANGES`/`OPEN_K_FLOOR`/`PER_CASE_VIEWS_DIR`/`DEFAULT_B_PARTITION_PATH`（g_b 注册表实际位置：`dag_poc/mcp_mock_server/b_partition.json`，按路径缓存加载 `load_b_partition`）。
2. **M13 分阶段口径**（`check_m13_domain_symmetry`）：新增 `warning()` 记录类型（kind=warning，非违规、不计退出码）；query_spec.json 声明 `batch_construction.state=="under_construction"` 且非冻结批次时，存量领域不对称降告警；`--freeze-batch` 冻结时升回违规。spec per_type 各格不对称属配置错误、恒违规。`run()` 拆分 warnings 单独展示，JSON 输出增 `warnings`/`warnings_total`。
3. **query_spec.json 新增 `batch_construction` 字段**（state=under_construction，含阻塞原因注记）；自洽断言全过（total==150/fixed==120/open==30/各格 drug+material==count/required_manufactured==43）。
4. **CLI**：新增 `--b-partition PATH`（缺省自动找注册表）；`--freeze-batch` 帮助文本补 M13 升回语义。退出码三态语义保持（0/1/2，告警不计入）。

### 13.2 四条规则判定表达式（沿用 E2-03 做法，指向函数名）

| 规则 | 判定表达式 | 函数 |
|---|---|---|
| O1 梯度三参数范围 | `isinstance(gradient, dict)` 且 ∀k∈{n,d,K}: `gradient[k] is not None and lo≤gradient[k]≤hi`，范围表 n∈[3,5]、d∈[1,2]、K∈[20,30]；字段缺失/任一参数缺失/超范围均违规（ADR-0018 §3.2） | `check_open_gradient` |
| O2 干扰项硬下限 | `gradient.K is not None → gradient.K ≥ 10`（K 缺失由 O1 报；数值 K<10 时 O1/O2 独立双报；K=0 使质量门第一条退化为重言式，ADR-0018 §3.2 强制项） | `check_open_k_floor` |
| O3 view_ref 有效 | `view_ref ∉ (None, "")` 且 `resolve(view_ref)` 落在 `dag_poc/data/backend/per_case_views/` 之下且 `.is_file()`（相对路径按仓库根解析；只查非 null 不够，路径写错一样报） | `check_open_view_ref` |
| O4 g_b 注册一致性（双向） | ① `b_partition["cases"][case_id]` 存在且含 `g_b` 键（存在性）；② `entry["g_b"] == case["g_b"]`（值相等）；任一不满足即违规。顶层 g_b 仅 fixture、不参与比对（ADR-0018 修订 R1-①）。注册表不可读时逐案跳过、库级 pending 留痕一次不静默 | `check_open_gb_registered` + `load_b_partition` |

前置过滤：`lint_case` 内 `if type_id == "type4_discovery_haystack"` 才跑四条（现有 open 类识别逻辑 DIR_TO_TYPE）。

### 13.3 对现有 15 道 open 实跑结果（逐规则，一个未修、不放宽判据）

| 规则 | 命中 | 说明 |
|---|---|---|
| O1 梯度三参数范围 | **0** | 15 道 gradient 全部在范围内（n∈{3,4,5}、d∈{1,2}、K 全=25） |
| O2 干扰项硬下限 | **0** | K=25 ≥ 10 全部满足 |
| O3 view_ref 有效 | **15（全部）** | view_ref 非 null 但指向的 per_case_views/case_dh_drug_NN.json 均不存在（E5 视图回填在它的 task-04 里，预期报红，未降级为告警） |
| O4 g_b 注册一致性 | **15（全部）** | b_partition.cases 里 15 道均未注册（现只有隔离 fixture 条目；E5 的 R1-① 注册还没做，预期报红，未降级为告警） |

实跑口径：`--cases 'dag_poc/templates/discovery_haystack/case_dh_drug_*.json'`（spare_* 备件不在 case_ glob 内）：30 条违规（O3×15 + O4×15）、15/15 不过、退出码 1。任务书 §6 风险预判兑现：g_b/view_ref 两条现在全红是预期，规则先落地、E5 做完自然转绿。

**fixed 类误报检查：0**。全量 `--cases 'dag_poc/templates/**/case_*.json'`（当前存量 82 case，含 E5 新交付 fixed）中 O1–O4 共 30 条违规全部落在 15 道 open 上，非 open case 命中数 = 0。

### 13.4 M13 分阶段口径实现方式与验证

- **实现**：query_spec.json 新增 `batch_construction: {state: "under_construction", ...}` 声明分批构造中；`check_m13_domain_symmetry` 读到该状态且未传 `--freeze-batch` 时，存量分布不对称以 `warning()`（kind=warning）输出：非违规、不计退出码、单独分区展示；`--freeze-batch NAME` 冻结批次时同一断言升回 violation。规则未删：领域均衡（goal §6 步 2 配比）仍判，只是判定时机分阶段；冻结本批后应将 state 改 frozen。
- **实测**：非冻结全量跑 → M13 违规 0、告警 3 条（type1 drug=38/material=9、type2 drug=8/material=12、type4 drug=15/material=0）；`--freeze-batch batch1` → 告警 0、M13 升回 3 条违规。open 侧 drug=15/material=0 的成因（材料侧受 E3 接口 B 扩展硬前置阻塞，31 达标族仅 3DSC 5 个可用、本批最多 3~4 道）已写入告警 note 与 query_spec 注记，属必然而非缺陷。

### 13.5 自检与三态保持

- **正例自检**（临时注入样本，验完即删）：超范围梯度样本命中 O1×3+O2×1+O3+O4；注册但值不一致样本命中 O4 双向查②；全合规样本（注册一致 + 视图文件真实存在）O 规则 0 命中——能抓违规、不误伤合规。
- **fixture 三态**：非 strict 0 / strict 2（M9+A1 pending，未被新规则误伤）/ 真实违规 case 退出码 1——三态语义保持。新规则只对 type4 生效，现有 fixture 均为 type1，无需 fixture 配合；未新增 open fixture，因为合规形态需 per_case_views 视图文件（E1 边界，本轮不写 dag_poc/data/**）与 b_partition 注册（E5 边界，只读），绿路径由正例自检覆盖。
- **全量终态**（82 case，一个未修）：总违规 346。存量规则逐条计数与本轮前全量跑一致（M1:33/M10:37/M4:37/M5:33/M6:4/M7:41/M8:33/M9:1/N1:29/N3:33/N7:29/N8:6，合计 316；上轮总数 318 中的另 2 条为 M13 存量分布违规，本轮按分阶段口径转为 3 条告警——存量已增至三类不对称），另新增 O3:15/O4:15；pending 3（M9 变体率未执行/6 case 无 gt.id/A1 扫描留痕）；告警 3（M13 分阶段）；退出码 1。注：A1 本轮实扫 350,418 行（E1 后端数据已扩容，上轮为 189,351 行），仍三项 0 命中。

**异常**：无。O3/O4 全红为任务书 §6 预判的预期状态（E5 注册/回填未完成），未因"现在全红"降级判据；无需报监工裁定事项。

**边界**：本轮只写 task/E2-query风格与题量定数/ + dag_poc/query_spec.json（仅增 batch_construction 字段）+ dag_poc/query_lint.py；未造 case、未动 dag_poc/data/**、b_partition.json（只读）、gateway.py/evaluate.py/answer_schema.json；未调 LLM、无需 LLM 落盘；未自行立 ADR。

---

## 14. E2-06 格位唯一性断言 + grid_key 字段契约（任务 ID 24，2026-08-08）

**背景**：题集要从 30 道 fixed 扩到 120 道，但 lint 无任何跨题查重能力（grid/cell/bin_key/duplicate/collision 全无；check_m9_gt_unique 只管单题内部 gt 唯一）。两道题落进同一切格时合格集与最优解可能完全相同——等于同题出现两次且无人能发现。本轮补上格位唯一性断言（规则数 22 → 23）并定 grid_key 字段契约（E2 为 query_spec.json 所有者，契约归 E2 定、E5 按它落盘）。

### 14.1 改动清单

1. **query_lint.py 新增 G1 格位唯一性断言**（`check_grid_uniqueness`，库级、接入 `lint_corpus`）：只对 fixed 类（类型1/2，含旧目录 fixed_discovery/open_discovery）生效，函数内前置过滤（open 类走 per-case 配对、无切格概念，不参与）；跨批次生效（lint 每次跑的都是全集，新造 90 道与已有题天然一起查）。新增常量 `GRID_KEY_CONTRACT_SUMMARY`/`GRID_KEY_SIDES`（契约本体在 query_spec.json，lint 侧只定比较语义）。
2. **query_spec.json 新增 `grid_key_contract` 字段**（契约 v1.0）：逐位类型写死、比较语义、与 capacity_basis 切分口径的逐位对应、落盘示例、自检命令；自洽断言全过（total==150/fixed==120/open==30/各格 drug+material==count/required_manufactured==43/batch_construction 仍在）。
3. **fixture 补合规 grid_key**（保全绿路径）：case_selftest_a_drug 加 `[3, -2]`（药物侧 2 位 int）、case_selftest_b_material 加 `["cuprate", 2, 4]`（材料侧 str+2 int）。

### 14.2 断言判定表达式（指向函数名）

**G1（`check_grid_uniqueness`，库级）**：
- 前置过滤：`infer_type(path)` 得 type_id ∈ {type1_constraint_satisfaction, type2_constrained_optimization} 才参与（含旧目录映射；type3/4 与未知目录不参与，open 类由此挡住）；
- 字段缺失：`grid_key is None` → 违规“字段缺失”（存量 fixed 回填归 E5，不降级不豁免，同 task-05 O3/O4 处理方式）；
- 形态校验：`isinstance(grid_key, list) and len(grid_key) > 0`，否则违规；
- 逐位类型校验（按 `infer_domain` 判侧）：药物侧 2 位均 int（atom_count_bin, logS_bin）；材料侧 3 位 [family:str, tc_band:int, element_count:int]；`isinstance(val, bool)` 拒收（bool 是 int 子类，不拒会把 true 当 1），逐位类型不符即违规；
- 撞格判定：比较键 = (type_id, domain, tuple((type(x).__name__, x) for x in grid_key))，逐位带类型，int 与 str 永不互等（[3, "3"] 与 [3, 3] 不是同一格也不互撞）；同键出现 ≥2 题即违规，报文含全部撞题 case_id、type_id/domain/grid_key 值（E5 可直接定位）。

### 14.3 grid_key 字段契约全文（已写进 query_spec.json grid_key_contract，v1.0）

- **适用**：fixed 类 case（类型1/2，含旧目录迁移件）；open 类不适用。
- **字段**：case JSON 顶层 `grid_key`，JSON 数组（非空）；回填与新造均归 E5（E5-05）。
- **药物侧（2 位均 int）**：`[atom_count_bin, logS_bin]` = `[atom_count // 5, round(logS)]`，与 capacity_basis.partitions.drug 切分 `(atom_count//5, round(logS)) -> 155 non-empty cells` 逐位对应。
- **材料侧（3 位：str, int, int）**：`[family, tc_band, element_count]` = 族名、tc 五档档位编号 0..4、元素种类数，与 capacity_basis.partitions.material 切分 `(family, tc five-band, element-species count) -> 73 non-empty cells` 逐位对应。
- **类型规则（写死，防 [3,"3"] 漏判）**：int 与 str 永不互等；不得用字符串数字、浮点、null、布尔；位数与所属侧必须一致（drug=2 / material=3）。
- **比较语义**：同一 (template_type, target_domain, grid_key) 三元组全 case 集只允许出现一次；跨批次生效；逐位类型+值相等；target_domain 取 case JSON 原值逐字比较，template_type 取所在目录判定的类型。
- **示例**：药物 `{"grid_key": [3, -2]}`（atom_count∈[15,19] 且 round(logS)=-2）；材料 `{"grid_key": ["cuprate", 2, 4]}`（cuprate 族、tc 第 2 档、4 种元素）。
- **自检命令**：`python dag_poc/query_lint.py --cases 'dag_poc/templates/**/case_*.json'`（G1 行无违规即合格）。

### 14.4 实跑结果（全集 86 道，一个未修、不放宽）

当前全集与任务书预估（约 49）不同，以实盘为准：**86 道 = fixed 类 67（constraint_satisfaction 18 + constrained_optimization 16 + 旧目录 fixed_discovery 29 + open_discovery 4，后者虽名 open 实为 fixed 类型2，规范 §3.2 撞名条款）+ open 类 19（discovery_haystack，含 E5 新增 4 道材料侧）**。任务书“30 道 fixed”为旧口径，存量已随 E5 交付增长，断言按实际全集跑、跨批次语义不变。

| 项 | 结果 |
|---|---|
| G1 字段缺失 | **67（全部 fixed 类，预期，E5 回填后自然转绿）**；撞格 0；形态/类型违契约 0 |
| open 类误报 | **0**（19 道 discovery_haystack 无一命中 G1，前置过滤生效） |
| 存量规则 | 逐条不变（M1:33/M10:37/M4:37/M5:33/M6:4/M7:41/M8:33/M9:1/N1:29/N3:33/N7:29/N8:6，合计 316），M13 仍为分阶段告警 3 条 |
| 总违规/退出码 | **383**（存量 316 + G1 字段缺失 67）/ exit 1；pending 3 不变；--strict 三态语义保持 |
| fixture 三态 | 非 strict 0（G1 对合规 grid_key 0 命中）/ strict 2（M9+A1 pending）/ 真实违规 case exit 1——全绿路径未被新规则误伤 |

### 14.5 正例自检（临时注入，验完即删）

构造 5 样本：① 撞格对 case_g_a/case_g_b（同 domain 同 [3,-2]）→ 命中 1 条撞格，报文含两道题 ID 与格值（`撞格 2 题: ['case_g_a', 'case_g_b'] @ type1.../drug/grid_key=[3, -2]`）；② 类型陷阱 [3,"-2"] → 违契约类型违规、且不与 [3,-2] 互撞（int≠str 比较语义生效）；③ 布尔陷阱 [3,true] → 拒收（未被当 1）；④ 合规独格 [4,-1] → 0 命中。能抓撞格、能抓类型陷阱、不误伤合规。

### 14.6 知会 E5 的契约文本（请监工转发，E2 不能直接联系 E5）

> **【E2 → E5 契约知会】grid_key 字段契约 v1.0 已定稿，造 90 道与回填存量均按此落盘**
>
> 契约本体：`dag_poc/query_spec.json` 的 `grid_key_contract` 字段（E2-06 定稿，2026-08-08）。要点：
> 1. case JSON 顶层新增字段 `grid_key`，JSON 数组（非空）。仅 fixed 类（类型1/2）需要；open 类不加。
> 2. **药物侧（2 位均 int）**：`[atom_count // 5, round(logS)]`，例：`"grid_key": [3, -2]`。
> 3. **材料侧（3 位：str, int, int）**：`[family, tc_band(0..4), element_count]`，例：`"grid_key": ["cuprate", 2, 4]`。
> 4. **类型写死**：不得用字符串数字/浮点/null/布尔（[3, "3"] 与 [3, 3] 会被判不同格但会报类型违规；true 会被拒收不当 1）。位数与侧必须一致（drug=2 / material=3）。
> 5. **唯一性**：同一 (template_type, target_domain, grid_key) 全集只许一道题，跨批次生效——新造 90 道必须与已有题一起查，不得只查本批内部。
> 6. **自检命令**：`python dag_poc/query_lint.py --cases 'dag_poc/templates/**/case_*.json'`，G1 规则无违规即合格；撞格时报文直接给出撞题的两道（或多道）case_id 与格值，可据此改格。
> 7. 存量 67 道 fixed（含旧目录）当前报“grid_key 字段缺失”属预期，请用造题脚本重算回填；回填后自然转绿。E2 只报告不修 case。

**异常**：无。G1 全红（字段缺失）为任务书 §6 预判的预期状态（E5 回填未完成），未降级未豁免；无需报监工裁定事项。注：任务书预估“约 49 道（30 fixed）”与实盘 86 道（67 fixed）的差异已如实报告，不影响断言语义。

**边界**：本轮只写 task/E2-query风格与题量定数/（report/progress/task-06 表头/fixtures）+ dag_poc/query_spec.json（仅增 grid_key_contract）+ dag_poc/query_lint.py；未造 case、未回填任何 case 的 grid_key（E5 的）、未动 dag_poc/data/**、gateway.py/evaluate.py、共享账本与 adr/**；未调 LLM、无需 LLM 落盘；未自行立 ADR。

---

## 监工验收（E2-03，2026-08-06，六项逐条实跑）

**结论：通过。** 六项交付标准全部达标，其中第 3 项（解决内部矛盾）的完成度超出要求。**我按"假设有问题"的基调挑了三处可疑点，全部经查站得住。**

### 六项实跑核验

| # | 标准 | 实测 |
|---|---|---|
| 1 | §7 改 100% 覆盖 + 分级审 + 4 位并行 | ✅ 规范 4 处落地；`κ 校准 = 不启用`（§467） |
| 2 | 5 条断言接进 lint 并重跑 | ✅ **规则数 9 → 13**：新增 N1 **29** 条、N3 **33** 条、N7 **29** 条、N8 **6** 条；M4 已含单句判定 |
| 3 | §5.4 与 M9 矛盾的解法 | ✅ **超出要求**，见下 |
| 4 | 版本同步 0.2.0 | ✅ `query_spec` 引用 = `answer_schema` 实际 = **0.2.0**；自洽断言 total=150、和=150 通过 |
| 5 | 记录"为何不用 LLM 预审替代全量审" | ✅ §471 留痕：κ 要可信需标注 ≈300 题 > 全部 150 题（自相矛盾）+ κ 悖论 + 违本规范 N9 |
| 6 | report 收口含断言表达式 | ✅ 各规则的判定式已写进规范的"脚本断言"列，指向具体函数名 |

### 第 3 项：解法比我要求的更完整

我只要求"给出可执行解法并写清依据"，你多做了两件事：

**① 给了防污染效果的量化论证**（原文 §383）：

> 设骨架数 N、题数 Q，均匀轮换时单骨架平均承载 Q/N 题：N ≥ 0.3Q ⇒ 单骨架 ≤ Q/(0.3Q) ≈ 3.3 题，即记住一道题的句式至多直接迁移到同骨架的约 2–3 题（**迁移面 ≤ 约 5%**）

——这回答了"新方案是否仍能防记忆迁移"这个我没明确问但审稿人一定会问的问题。

**② 主动核对了不违反另一份 ADR 的边界**：

> 骨架清单本身是造题侧的封闭枚举（非从专家反馈滚动新增），不违 ADR-0016 修订 R1「滚动新增规则只许黑名单」的边界；**专家反馈只允许往 N1/N3 黑名单加词，不允许改骨架清单为白名单句式断言**

——跨 ADR 的一致性自查，这是我此前漏做过两次（ADR-0014 与 E3 字段撞车、ADR-0015 与 E1 布局冲突）的动作。

骨架清单规模：类型1 **20 种/60 题**、类型2 **10 种/30 题**、类型4 **10 种/30 题**，均 ≈0.333 > 0.3。

### 我挑的三处可疑点，全部站得住

| 疑点 | 查证结果 |
|---|---|
| **`--freeze-batch` 是否只是文档写了？** | ✅ 实跑可用。输出了库级 M9 检查（`gt='aa'` 出现 2 次于 `case_cs_cns_drug`/`case_cns_metoprolol`），且**主动报出检查盲区**："6 个 case 无 gt.id → gt 唯一性检查未覆盖这些 case"——不是静默跳过 |
| **N8 是"裸答基线 k=10"，需实跑模型才能判，怎么会有 6 条违规？** | ✅ 实现诚实。docstring 明确划分："B3/B4 的判定表达式本身可执行、**实跑判定归 E6**；lint 断言其**前置**——每题有非空 `gt.id`，否则 B3/B4 无法执行、防污染门形同虚设"。**没有假装能判基线** |
| **N7 是否只查题面、没查 `case_id`？**（这是我明确要求的） | ✅ 检查范围写明 `query + case_id + provenance + target_domain + domain_knowledge`；实测 `case_antibiotic_sulfamethoxazole` 被命中并报出真名 `sulfamethoxazole`，且注明"重命名归 E5，E2 只报告"——**守住了出题人/验题人职责分离** |

### 当前 lint 全貌（供下游参考）

13 条规则、**318 条违规**（M1 33 / M4 37 / M5 33 / M6 4 / M7 41 / M8 33 / M9 1 / M10 37 / M13 2 / N1 29 / N3 33 / N7 29 / N8 6），37 个旧 case **无一通过**。

**这个数字是资产而不是负债**——它把"迁移工作量"从模糊判断变成了确定清单，E5 造题时可逐条对照。

### 遗留（不阻塞，已在别处登记）

1. **R4 匿名映射断言仍 pending** —— 等 E1-03 交付 `anonymization_mapping.json` 后转实判（ADR-0017 影响项，你在 task-03 §5 已知悉）；
2. **6 个 case 缺 `gt.id`** —— 使 M9 的 gt 唯一性与 N8 的前置检查覆盖不全，归 E5 迁移时补。

---

## 监工验收（E2-05，2026-08-07 实跑）

**结论：通过，四条规则全部落地且判据正确。**

### 实测

| 交付项 | 实测 |
|---|---|
| 四条 open 专用规则 | ✅ 规则函数 **18 → 22**：**O1** 梯度参数范围（`check_open_gradient`）、**O2** 干扰项下限（`check_open_k_floor`）、**O3** `view_ref` 有效（`check_open_view_ref`）、**O4** `g_b` 注册（`check_open_gb_registered`） |
| 只对 open 生效 | ✅ 30 道 fixed 跑 lint 退出码 0、无误报 |
| M13 分阶段口径 | ✅ 已改，退出码说明里明写"分阶段告警不计入"；**规则未删** |
| 现有 15 道 open 实跑 | ✅ 完整跑 **退出码 0、零违规** |

### 我重点查了 O4，判据写得对

`check_open_gb_registered` 的实现是**双向查**：① 未注册或条目无 `g_b` → 违规；② 注册值 ≠ case JSON 值 → 违规。并且 docstring 明写「顶层 `g_b` 仅 fixture、判分不得回落，**lint 同样不拿它做比对**」——这正是 ADR-0018 修订 R1-① 的口径，没有取巧走顶层。

**我原本准备挑的一处，查完是我多疑了。** 我第一次跑 lint 时 O3 报 15 条违规、O4 零违规，怀疑 O4 是不是被降级或默认跳过（`lint_case` 有 `if bp is not None` 的条件）。逐层查下来：`load_b_partition(None)` 走 `DEFAULT_B_PARTITION_PATH` 能正常加载、O4 确实在跑；**零违规是因为 E5 在这期间把注册做完了**。

### 顺带核实了 E5 的两项收尾也已完成

| 项 | 实测 |
|---|---|
| `g_b` per-case 注册 | ✅ `b_partition.cases` 从 2 条 → **20 条**（dh open 18 条）；逐条核对 **15/15 注册且与 case JSON 完全一致**，不一致 0、未注册 0 |
| `view_ref` 回填 | ✅ **15/15 非空且指向的视图文件真实存在**；视图目录 19 份 |

你的 O3/O4 两条规则正好把这两项验绿——**规则先落地、E5 做完自然转绿**，这个顺序是对的。

### 一句提醒

风险预判里我写过"不要因为现在全红就把这两条降级为告警"。你没有降级，而是保持违规级别、等 E5 补齐。**这个处理是对的**——如果当时降成告警，现在就无法确认 E5 真的做完了。

