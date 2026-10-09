# task16 — 专家背书 benchmark 的原始 query 汇编（每家 ≥10 条，看风格）

| 字段 | 值 |
|---|---|
| **状态** | 📋 已记录，**未开工**（owner 指示"先记录到新的 task 文档里"） |
| **建立日期** | 2026-08-01 |
| **上游** | `task15`（已确认 8 家专家背书）· `task14`（62 家专家参与核查） |

## 上游诉求（owner 原话）

> "接下来要做的 task 是：去看看这些**被专家承认核验过**的论文，看看他们的 benchmark 里的 query 是怎么构造的，**每一篇论文都写最少 10 个原论文数据里构造的 query**，我要去看看这些 query 的风格是什么"

## 对齐 GOAL（ACTION_RULES §1.1）

- **最终目标**（`goal.md` §1）：为材料+药物发现造 long-horizon agent benchmark，评过程而非只评终答。
- **本 task 服务于**：GT-first **第 2 步「构造专属 Query」**——我方现有题面是**大白话**（"能进入大脑、对心脏安全"），而 L1 层（题面措辞是否符合领域科学语言）三层专家审核**全空**（见 `research/query构造与专业性保证.md` §★★.二）。**先看专家写的 query 长什么样，才能知道我方题面该怎么改。**
- **为什么必须做**：这是 T-49 的**前置**。在没见过 80+ 条专家背书的真实 query 之前，改写我方题面只能靠猜。
- **红线关联**（`goal.md` §5）：「跳过调研直接造题」是红线。

## 范围：8 家专家背书 benchmark（task15 已锁定）

| # | Benchmark | 专家背书证据 | 本仓已有素材 | 取 query 的路径 | 风险 |
|---|---|---|---|---|---|
| 1 | **SciCode** | 标题即 *Curated by Scientists* | `first_problem.jsonl`（**1 条真实 query，字段 `problem_description_main` 已验证**） | GitHub 仓库补至 ≥10 | 低 |
| 2 | **ScienceAgentBench** | 9 位资深博士生/教授 | PDF + harness 代码 | **题目数据在 HF（本环境不可达）** → 从 PDF Table 1/附录 + GitHub 提取 | **中高** |
| 3 | **NewtonBench** | 3 位物理奥赛背景专家 | `sample_m0_gravity_laws.py` | GitHub 拉多个 domain 的定律文件 + PDF Table 5 | 低 |
| 4 | **ChemCrow** | 18 个专家设计工具 + 专家评判 | `agent_prompts.py`、`example_tasks_test_agent.py` | 论文正文的任务列表 + 仓库 example | 中 |
| 5 | **TRAIL** | 4 位专家标注员 | `DATASET_README.md` | HF 数据集（可能不可达）→ 退回论文附录 | **中高** |
| 6 | **RE-Bench** | human expert reference = 1 | `README_采样说明.md` | GitHub 任务目录 | 中 |
| 7 | **Tool-Decathlon** | 108 题人工编写 | `sample_task_ab-testing.md`（1 条完整） | GitHub `tasks/` 目录批量取 | 低 |
| 8 | **PaperBench** | rubric 树 8,316 叶节点专家构造 | `rice_rubric_full.json` | rubric 顶层节点即任务描述 | 低 |

> **环境已知限制**：HuggingFace 在本环境不可达（HTTP 000），GitHub / ModelScope 可达。对 2、5 两家须优先走 PDF 附录与 GitHub，取不到就**如实标注"未取到"，不得用相似内容顶替**。

## 子目标与验收（三要素）

| # | 子目标 | 打勾通过标准 | 验证方式 | 红线 |
|---|---|---|---|---|
| 1 | **每家 ≥10 条原始 query** | 8 家 × ≥10 = **≥80 条**，每条为 **verbatim 原文**（英文原样，不翻译不改写） | 逐条附来源（文件路径+行号 或 论文 Table/附录编号），可复核 | **编造或改写 query**；用"类似的题"顶替真实取不到的 |
| 2 | **风格维度提取** | 每家标注：句式（陈述/疑问/指令）· 长度 · 术语密度 · 是否给数值阈值 · 是否给工具提示 · 是否给输出格式 | 汇总成一张跨 8 家对照表 | 只堆原文不做风格归纳 |
| 3 | **与我方题面对照** | 明确指出我方 CNS/超导题面在每个风格维度上的差距，给出改写建议（**建议，不直接改题**） | 与 `case_cns_metoprolol.json` 的 `query` 字段逐维度比对 | 越界直接改动 case 文件（那属 T-49，需专家参与） |
| 4 | **取不到的如实标注** | 若某家 <10 条，写明卡在哪（HF 不可达/仓库无数据/论文未列），不凑数 | 文档内单列"未达标家数与原因" | 用编造内容补足 10 条 |
| 5 | **样本量表述受限** | 一切"专家写的 query 都是 X 风格"须限定在已取样本内 | 全文检索"都/全部/普遍"等词逐一确认作用域 | 小样本外推为普遍规律（task14/task15 已两次翻车） |

## 交付物（计划）

- `research/专家背书query原文汇编.md`——8 家 × ≥10 条 verbatim query + 每家风格标注 + 跨家对照表 + 我方差距
- `reference/<各家>/sample_queries_*.{jsonl,md}`——批量拉取的原始 query 落盘（含 md5）
- 账本：新增 PR/T 条目登记"我方题面改写"的具体依据（编号须先查 `TODO.md` 最大号，现为 **T-50**）

## 执行前必做（吸取 task15 偏离的教训）

1. **动手前先 `git log --oneline -3` 与 `grep -o "^| T-[0-9]*" TODO.md | sort -n | tail -1`**——确认账本编号与他会话进度，避免重号（task15 曾造成 T-44/T-45 重号）。
2. **先查 `research/` 是否已有同议题文档**——task15 曾与 task14 重复整轮调研。已知相关：`query构造与专业性保证.md`、`query写法调研报告.md`、`各benchmark造题法详解.md`。
3. **分清出题侧与判分侧**——本 task 只看 **query 题面本身的写法风格**，不涉及判分逻辑。
