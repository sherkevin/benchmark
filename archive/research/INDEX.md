# research/ 总索引（长程 Agent 评测 · 分类案例集）

> **📁 目录规范变更（已被 ADR-0013 修订）**：单一 task 专属的调研报告已移入对应 `task/taskN/` 文件夹（本次 8 份）；`research/` 现只保留**跨多个 task 共用**或**无 task 归属**的通用知识。查某个任务的结论请直接进 `task/taskN/report.md`。

> 本目录为**调研服务**：只放索引与调研/证据文档，**平铺（单次调研的中间材料可成一个同名文件夹，ADR-0007 §3）**；参考资料（公开数据集采样 + 会议论文原文与阅读报告）归 `reference/`（一数据集/一论文一文件夹），项目级汇总归根 `PROJECT_RESULT.md`，待办归根 `TODO.md`，决策规范归 `governance/adr/`。

## 存放与命名规范

**存放规则**：(a) 属于**某个具体数据集**的调研/解释文档 → 放 `reference/<该数据集>/`；(b) **笼统通用**的文档（对所有调研对象做通用描述、有共同主题，如科学发现/材料发现）→ 放本目录。本目录**平铺、无子目录**。

**命名规则**：本目录只放 (b) 类文档，主题即文件名，**不再需要分类前缀**；唯一保留的角色前缀是 `INDEX`（标识索引）。

| 条目 | 说明 |
|---|---|
| `INDEX.md`（本页） | 总索引：规范 + 归档纪律 + 四类能力 × 代表 benchmark 索引 |
| `INDEX_数据集分类与详解.md` | 全部数据集的分类 + 逐个详解（按 checkpoint 机制） （📁 已移入 `task/task14/`）|
| `science_agent_benchmark.md` | Science agent benchmark 专题调研（全覆盖版） |
| `长程任务Benchmark.md` | 长程任务 benchmark 主调研（v1+v2 合并版） |
| `材料发现长程Checkpoint.md` | 材料发现长程 checkpoint 专项调研 |
| `评测能力维度.md` · `Goal-Drift.md` | 分析框架：长程评测 4 能力 + 目标维持专题 |
| **`测试Agent调研总报告.md`** | **测试方法全部调研合一份：主流壳子与判分 + 商用 CLI 当壳与 context 管理 + 循环与预算（谁发起二次调用）+ 12 仓库源码核验 + 可复现性 + 对本项目结论** |
| **`实验矩阵.md`** | **最终实验待填总表：Harness×Backbone×Case 三因子 + 判分/资源/过程三类指标口径 + 波次0（18点trace待评分）/波次 1（24点待跑）明细表 + 汇总矩阵 + 4 个工程缺口** |
| **`目标会议长程benchmark横评.md`** | **ICLR/ICRA/AAMAS/WWW 四会画像 + 18 篇论文的三问横向对比（数据量 / 数据集形态 / "长"体现在哪）+ 五条规律 + 四个设计缺口** （📁 已移入 `task/task9/`）|
| **`投稿策略_ICLR调性对标.md`** | **四会取舍与 ICLR 2027 时间表、调性逐条对标、Plan A（bench+RL）/ Plan B（纯 bench + 12 项加厚清单）/ 八周排期与 Go-NoGo** （📁 已移入 `task/task9/`）|
| **`query构造与专业性保证.md`** | **出题侧：query 怎么写才合理专业——四道关（约束不矛盾/答案唯一无歧义/术语地道/真实工作流）逐条取证 + 我方过关情况与两个可落地自动闸** |
| **`发现类任务构造方案.md`** | **从"反向造一条 trace"升级为"证据闭包（MSS）+ haystack 埋线索 + 四道门判分"；VIS-DAG 统一形式化** （📁 已移入 `task/task9/`）|
| **`数据集开源状态与样例核验.md`** | **18 篇论文数据集的开源状态 + 采样口径（HF 不可达/GitHub 已拉/gated）+ 一条 case 最小结构总表；每篇详情在 `reference/<CONF>-*/数据格式与样例.md`** （📁 已移入 `task/task9/`）|
| **`各benchmark造题法详解.md`** | **逐条展开 8 篇主力 benchmark 的造题产线（一条完整题 + 怎么构建 + 怎么判分 + 对我方启示）；造细节逐字取自论文正文并回链章节** |
| **`benchmark图鉴_每家测什么怎么判.md`** | **【全景图鉴：每家 benchmark 四问】测什么领域 / 真实 query / 预期答案 / 怎么逐项判分，2、3、4 用同一道真题串起。按五类分段（化学材料 / 软件工程 / 交互工具 / 过程评测 / 我方 NobelClaw）。分详写层（本地三件套齐，约 25 家，逐字引真实题面与答案）+ 名录层（仅有机制说明者标「本地无样例」，不拿论文印象补）。真实数据由四组 subagent 从 reference/ 抽取** |
| **`Playground平台造题与判分机制.md`** | **【结论：不测中间过程量，仅验轨迹真伪】**判分全链路实证：总分=`harbor_reward×100×trace_factor`；所谓检查点实为终态字段拆细（一道生物题 11 个检查点属过程者 0 个）；轨迹分三层（五条机器校验 + 16 条反作弊码 + LLM 判证据链）测的是记录真实性；任务皆为论文复现、交付代码及其跑出的数，与我方材料/药物发现根本不同。根因是信任模型（其 agent 跑在选手本地） |
| **`中间检查点构建方法调研.md`** | **【实现点 1、2 的展开】中间检查点怎么构建：五条构造路径（形式化目标自动拆解 / 金标准轨迹挖掘 / 约束函数库 / 连续质量函数 / 分层 rubric）× 成本与可规模化对比 + 18 个 benchmark 五问表 + 顺序判定专项（BPOP 的 #P-hard、卒中诊疗的规则翻译）+ 五条可落地借鉴 + 我方五项不足。核心发现：造检查点成本差两个数量级且由「目标能否形式化」决定；我方走最贵那条、两条便宜路径未尝试** |
| **`benchmark目标定义_v1.md`** | **定目标总结（量级 100–150 / 特征优势 F1–F5 / 两类可行性 / 6 个技术难点 / dag_poc 实跑 4—15 达标判定）——后续所有 task 的参照系** （📁 已移入 `task/task9/`）|
| **`benchmark数据库调研报告.md`** | **14 个 benchmark 用了哪些库 + 【权威性背书】交叉采用次数（TDC 3＋1 家 / PubChem 3 家 / ChEMBL·ZINC·PDB 各 2 家），venue 经 DBLP 核准；指出材料线背书缺口（SuperCon 零家）** |
| **`权威数据源与数据缺口指南.md`** | **【frozen snapshot 构造指南针】真实 MCP server 调的内部 API 清单（为何必须自建快照）+ 23 工具 × 数据需求 × 缺口等级；缺口分三层（数据错 15/40 SMILES > 数据缺 80.4% > 数据少）；权威库四要素清单 + license + 当场可达性实测 + provenance 口径 + 9 步执行顺序。**已并入旧《权威数据库调研报告》并删除后者** （📁 已移入 `task/task12/`）|
| **`数据集权威性总览.md`** | **【62 个 benchmark × 数据集 × 引用背书】每个数据源锚定到一篇可核高引论文（PDB 40,136｜Materials Project 12,215｜PubChem 4,043｜TDC 159）；**引用数必须分两类看**——数据库类可靠、benchmark 类被接口严重低估（GAIA 实测 1,058 但 OpenAlex 只报 9，差 117 倍）；62 个数据集清单与归属分类。⚠️ **专家参与议题以 `query构造与专业性保证.md` 为准**（本文 §四 初稿曾错写「无一家评估 query 措辞」，已订正） （📁 已移入 `task/task14/`）|
| **`query构造与专业性保证.md`** | **【出题侧专业性的权威答案】query 怎么造才合理专业：四道关（约束不矛盾 / 答案唯一 / 术语地道 / 确是真实工作流），逐字引 ScienceAgentBench 附录 G 专家问卷（请专家**重写**题面，改 41/102 条、删 3 道）、InfoMosaic Coherence Filtering、GAIA 三人独立复答（68% 一次过关）。**本议题的权威底本** |
| **`query写法调研报告.md`** | 别家 query 怎么写 + 我方该怎么改。⚠️ **四处结论已被 task14 订正**（「不是专家写的」的推论、自相矛盾的表格、「数据源硬就不需专家验证」的伪规律、「没一家被质疑」的论证偷换） |
| [`四会检索原始清单/`](四会检索原始清单/) | 一次调研的中间材料（ADR-0007 §3 调研文件夹例外）：ICLR 官方接受列表召回 + ICRA/AAMAS/WWW 的 DBLP 召回清单 |

**已迁出**：证据/GT 事实底座（admet/semiconductor/superconductor/molbench×2）→ `reference/NobelClaw/`；方法论支撑（未闭环5问调研证据、达摩院方法梳理）→ 项目根目录；调研原始输出（素材 xml/json）→ 已清除（论文以 arXiv 号引用）。

## 归档纪律

- **历史快照不重写**：`决议/` 各 ADR 与 `task/` 存档保留时点称谓，不回溯改写（见 `决议/0001`）；
- **新文档对号**：benchmark 解释文档 → `reference/<Benchmark>/<Benchmark>.md`；通用调研/框架 → 本目录主题命名；失效即清，不留孤儿文件。

## 四类能力 × 代表 benchmark × 一句话怎么测（证据索引，框架见 [`评测能力维度.md`](./评测能力维度.md)）

| 能力 | Benchmark | 一句话怎么测 | 解释文档 |
|---|---|---|---|
| **1 用对工具**（做得对） | τ-bench | 让 agent 调数据库 API 办客服，结束后比对数据库终态是否等于标准答案 | [`tau-bench.md`](../reference/tau-bench/tau-bench.md) |
| | SUPER | 让 agent 跑通研究仓库，用正则在执行日志里查"必经关键输出"命中几条 | [`SUPER.md`](../reference/SUPER/SUPER.md) |
| | ToolBench | 给一个任务+一堆候选 API，让 agent 连续选调多个真实工具解决；用 Pass Rate + Win Rate（LLM 评审）打分 | [`ToolBench.md`](../reference/ToolBench/ToolBench.md) |
| **2 事前规划**（想清楚） | TravelPlanner | 让 agent 一次产出逐日行程，用一组 Python 约束函数逐条判满足几条 | [`TravelPlanner.md`](../reference/TravelPlanner/TravelPlanner.md) |
| | SciCode | 把科研题拆成十几个有依赖的子函数，每个配可执行断言，逐个判过没过 | [`SciCode.md`](../reference/SciCode/SciCode.md) |
| | LongProc | 让 agent 按规程一次性生成长过程，用确定性规则逐步校验每一步 | [`LongProc.md`](../reference/LongProc/LongProc.md) |
| **3 反馈应变**（会应变） | SWE-bench | 让 agent "改代码→跑测试→再改"修真实 issue，只看两组测试过不过 | [`SWE-bench_Verified.md`](../reference/SWE-bench_Verified/SWE-bench_Verified.md) |
| | AgentBoard | 让 agent 环境里多轮行动，正则匹配每轮观测，看达成几个子目标（进展率） | [`AgentBoard.md`](../reference/AgentBoard/AgentBoard.md) |
| | Commit0 | 让 agent 从零写整个库，测试通过率随实现单调上升＝过程分 | [`Commit0.md`](../reference/Commit0/Commit0.md) |
| **4 目标维持**（不跑偏） | RE-Bench / METR | 给开放式研究目标+连续打分函数，长时间自主优化，取最佳分 | [`RE-Bench.md`](../reference/RE-Bench/RE-Bench.md) |
| | Goal Drift | 先给目标，再长时间塞反向诱导，看它会不会慢慢偏离最初目标 | [`框架_Goal-Drift.md`](./Goal-Drift.md) |
| | OdysseyBench | 把信息打散在跨多天对话里，看 agent 能否回忆整合完成任务（长期记忆） | [`OdysseyBench.md`](../reference/OdysseyBench/OdysseyBench.md) |

## 说明

- 每个 benchmark 解释文档 = 真实 case + 判分机制，均为**真实数据**（逐字摘录，或摘自官方判分脚本/数据卡），存放于 `reference/<Benchmark>/`；
- 新增案例：解释文档进 `reference/<Benchmark>/`（附 `README_采样说明.md`），回本页登记一行。
