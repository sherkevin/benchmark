# 目标会议长程 Benchmark 横评（ICLR / ICRA / AAMAS / WWW）

> **一句话结论**：四个会议里**只有 ICLR 是我们的主战场**（长程 agent benchmark 与 agentic RL 在 ICLR 2026 已成显学：29 篇标题含 long-horizon、44 篇标题同时含 agent 与 bench）；WWW 是可行的次选（"agentic web" 主题下已收 ColorBench 这类长程里程碑 benchmark 与 AgentPRM 这类过程奖励模型）；**ICRA 与 AAMAS 结构性不匹配**，前者要物理本体与 TAMP，后者要多智能体——后者直接违反 GOAL.md 硬边界 1。
>
> 落盘证据：18 篇论文原文 + 逐篇阅读报告在 [`reference/`](../../reference/)（`<CONF>-<slug>/`）；四会检索原始召回清单在 [`四会检索原始清单/`](四会检索原始清单/)。
>
> 状态：2026-07-31 首版 ｜ 上游 task：[`task/task9/task.md`](task.md) ｜ 规范：ADR-0010

---

## 1. 检索口径（可复现）

| 会议 | 数据源 | 口径 |
|---|---|---|
| **ICLR** | `https://iclr.cc/virtual/<year>/papers.html` 官方接受论文列表 | 本地下载 → 正则抽全部标题（2024 **2296** 篇 / 2025 **3827** 篇 / 2026 **5468** 篇）→ 关键词召回。**这是全量、无漏检的口径**。 |
| **ICRA** | DBLP `streamid:conf/icra:` | `src/data_download/dblp_slow_scan.py`，10 组关键词，2022 起，48 条 |
| **AAMAS** | DBLP `streamid:conf/ifaamas:` | 同上，25 条。⚠️ 注意 `conf/atal`（ATAL 工作坊）与 `conf/aamas`（ALAMAS 研讨会）**都不是** AAMAS 主会，主会是 `conf/ifaamas` |
| **WWW** | DBLP `streamid:conf/www:` | 同上，78 条 |

⚠️ 局限：DBLP 只检索**标题**，短语视为词的 AND 前缀匹配，故 ICRA/AAMAS/WWW 三会为**关键词召回**而非全量；ICLR 为全量。ICLR 2026 尚未进入 DBLP（`dblp.org/db/conf/iclr/iclr2026` 返回 404），故必须用官方 virtual 站列表。OpenReview 的 `/notes` 与 `/pdf` 端点对脚本请求返回 403 challenge，PDF 全部改从 arXiv 获取（AAMAS 从 IFAAMAS 官网）。

## 2. 四会画像（收什么、什么形态）

### 2.1 ICLR —— 主战场，且赛道正在爆发

| 关键词 | ICLR 2024 | ICLR 2025 | ICLR 2026 |
|---|---|---|---|
| 标题含 long-horizon / long horizon | 3 | 4 | **29** |
| 标题同时含 agent + bench | 3 | 20 | **44** |
| 标题含 bench | 40 | 149 | **293** |
| 标题含 agent | 39 | 135 | **317** |
| 标题含 reinforcement learning / RL | 145 | 155 | **414** |
| 科学/分子/材料 × (bench|agent|discover) | — | 24 | **64** |

**三条读数**：
1. **两年内长程 agent 从边缘变主流**（3 → 4 → 29）。这是目前最热的窗口，也意味着**竞争激烈、必须有清晰的差异化**。
2. **ICLR 完全接受"纯 benchmark"论文**：ICLR 2026 有 293 篇标题含 bench，其中 44 篇是 agent benchmark。**不带 RL 也能投**（详见投稿策略文档）。
3. **ICLR 也完全接受"benchmark + 在其上验证 RL"的打包形态**：AgentGym-RL（环境套件 + ScalingInter-RL）、RLVMR（过程级 verifiable reward）、WebRL（ICLR 2025，自演化课程 RL）都是现成范式。

### 2.2 WWW —— 可行次选（"Agentic Web" 主题）

WWW 2026 明显在把 agent 作为主题词吃下（DBLP 召回 78 条中 2026 年占 49 条），且**有一个专门的 "Foundations and Architectures for the Agentic Web" workshop**。相关正会论文：
- **ColorBench**（长程手机 agent benchmark，图结构 + 里程碑 + 原子能力诊断）→ 与我们设计最接近，**必须区分**；
- **AgentPRM**（步级 promise/progress 过程奖励模型）→ 对我们方法论的最强质疑；
- AgenticShop（agentic 商品编排 benchmark）、ES-MemEval（长期情感支持对话 benchmark）、CiteLLM（科学文献发现 agentic 平台）。

**WWW 的调性**：偏 **web / 推荐 / 电商 / 安全 / 社会计算**。我们的材料药物题材与 WWW 主题的连接点很弱，只能靠"agentic 工具生态与协议（MCP）"这个侧门进。**作为 Plan C。**

### 2.3 ICRA —— 结构性不匹配

DBLP 召回的 48 条里，long-horizon 全部是**机器人操作/导航/TAMP**（Long-Horizon Manipulation、Loco-Manipulation、Deformable Object Manipulation、Multi-Agent Collective Construction、Quadrupedal Pushing…），或**LLM 作为 PDDL 翻译器/高层规划器**（FLTRNN、ISR-LLM、CaStL、LaMMA-P、Guiding Long-Horizon TAMP with VLMs）；dataset/benchmark 类几乎全是**感知数据集**（LiDAR 位置识别、越野可行区、V2V 感知、水果形状补全、导盲机器人指令）。

**结论：不投。** 我们的题目没有本体、没有运动规划、没有传感器流；投过去第一个审稿意见会是 "where is the robot?"。

### 2.4 AAMAS —— 违反硬边界，不投

DBLP `conf/ifaamas` 召回 25 条，分两类：**MARL benchmark**（MOSMAC、EnEnv、TaxAI、Neural MMO 鲁棒性）与 **LLM 多智能体社会/博弈仿真**（合作演化、社会困境、人格驱动决策、洗钱数据生成）。**两类都以"多个 agent"为前提**，而 GOAL.md §3.1 硬边界 1 明确是**单 Agent**。

**结论：不投。** 改成多 agent 协作发现即撞红线；除非未来另起项目。

---

## 3. 横向对比表（owner 三问：数据量 / 数据集形态 / "长"体现在哪）

| # | 论文（会议·年） | 数据量 | 数据集形态（一条样本） | "长"体现在哪 | 判分粒度 | 有 RL？ |
|---|---|---|---|---|---|---|
| 1 | **Toolathlon**（ICLR 26） | **108 题** / 32 应用 / 604 工具 | 指令 + 真实软件初始状态快照 + 专属评测脚本 | **平均 ~20 turns 工具调用**；跨应用状态耦合 | 终态执行式**二元**；pass@1/@3/^3 | ✗ |
| 2 | **NewtonBench**（ICLR 26） | **324 题** = 12 正典定律 → 108 反事实改写 × 3 系统复杂度 | 可执行黑箱模型系统 `M=(f₁..f_k)` + 隐藏目标方程（表达式树） | **搜索深度**（4–6 次突变）+ 观测间接性；**交互仅 ≤10 轮** | 终态二元 Symbolic Accuracy（忽略常量）+ RMSLE；过程只用 **LLM 事后标注**做分析 | ✗ |
| 3 | **AstaBench**（ICLR 26） | **2400+ 题** / 11 子集；端到端发现子集只 **40+40** | 各子集原形态 + 统一 Asta 环境（8 个 MCP 检索工具 + 有状态 Jupyter + **语料日期截止**） | **流程链长**（检索→理解→代码→分析→端到端）+ 成本轴 | 各子集自有（执行式为主）；重点是**控混淆变量** | ✗ |
| 4 | **Illusion of Diminishing Returns**（ICLR 26） | 可无限合成的受控探针 | 给定 plan + key→value 字典 + 期望终态；每步 retrieve+compose | **步数轴本身**；给出 5 个指标：Step/Turn Acc、Turn Complexity K、Task Acc、**Horizon Length H_s** | **过程级、确定性**（逐步状态比对） | ✗ |
| 5 | **InfoMosaic-Bench**（ICLR 26） | **621 题** / 77 工具 / 6 域（含医学生物） | 复合问题 + 可验证答案 + **工具证据锚定的跨源依赖链** | **跨源依赖链深度**（A 源结果喂 B 源） | 终态 accuracy + pass rate；有失败归因 | ✗ |
| 6 | **AgentGym-RL**（ICLR 26） | 5 类场景 / 27 任务 | 环境三层解耦 + 多轮 loop（`max_rounds`） | **交互轮数是一等旋钮**；长 horizon 会策略崩塌 | 环境原生稀疏奖励 | ✓ ScalingInter-RL |
| 7 | **RLVMR**（ICLR 26） | 用现成 ALFWorld + ScienceWorld；自切 L2 未见类别 | 4 个元推理 XML 标签 + 规则化过程奖励 | 多步 embodied；**病症=inefficient exploration**（重复动作、错误恢复差） | **过程级 + 终态双层**，过程规则可验证 | ✓ GRPO-MR |
| 8 | **ScienceAgentBench**（ICLR 25） | **102 题** / 44 篇论文 / 4 学科 / 9 位专家 | 指令+数据+参考程序+可执行成功判据+**逐题 step-by-step rubric** | 复现一条完整分析流水线的**必需步骤链** | VER/SR 硬二元 + CBS + Cost；**rubric 部分分**；图用 GPT-4o judge | ✗ |
| 9 | **MLE-bench**（ICLR 25） | **75 竞赛**（+7 dev）；Low/Med/High = 23/38/15 | Kaggle 竞赛 + 重切 split + **人类排行榜分布**当标尺 | **挂钟时间 24h** + 人力小时（<2h / 2–10h / >10h） | 终态，但标尺连续（铜/银/金牌百分位） | ✗ |
| 10 | **WebRL**（ICLR 25） | WebArena-Lite（5 类站点）；训练任务**自演化生成** | 自演化课程 + ORM（结果监督奖励模型）+ KL 约束 + 置信度过滤回放 | 多步网页操作；**长程 credit assignment 是核心痛点** | 终态二元；训练期用 ORM 近似 | ✓（4.8%→42.4%） |
| 11 | **SWE-bench**（ICLR 24） | **2,294 实例** / 12 仓库（可扩展池 19,000 / 37 仓库） | 代码库快照 + issue + 隐藏 gold patch + test_patch + F2P/P2P 测试表 | 代码库规模 + 跨文件编辑 + **PASS_TO_PASS 无回归约束** | execution-based 二元 | ✗ |
| 12 | **GAIA**（ICLR 24） | **466 题**（166 dev 公开 / 300 答案保留） | 一句问题 + **唯一事实性短答案** + 标注者元数据（工具/步骤/耗时） | **按标注者步数与工具数分三层**：≤5 步 / 5–10 步 / 任意长 | 终态精确匹配；Level 3 全员 0% | ✗ |
| 13 | **LoTa-Bench**（ICLR 24） | ALFRED+AI2-THOR、自扩展 WAH-NL+VirtualHome | 指令 + 初始状态 + **goal condition** | 原子技能序列长度 + 强前置依赖 | 模拟器 goal condition 二元 | ✗ |
| 14 | **ColorBench**（WWW 26） | **175 题**（74 单 app + 101 跨 app）；15 类原子能力 | 指令 + 状态转移图（屏幕为节点）+ 图上里程碑 + **≥2 条正确路径** | **平均 13+ 步**；顺序/并行/递归子任务组合 | **SR + CR（里程碑部分分）+ AC（条件化分母）** | ✗ |
| 15 | **AgentPRM**（WWW 26） | 无新数据集 | 步级 **promise（离目标多近）+ progress（推进多少）**，TD+GAE 造标签 | 多轮决策的相互依赖 | **步级但学习型连续分** | ✓（可用于 agent RL） |
| 16 | **MOSMAC**（AAMAS 25） | StarCraft II 场景；2–3 目标；9 个 MARL 算法 | 场景配置 + 顺序子任务 + 每目标一个奖励函数 | **顺序子任务 + 可变时间跨度**；引 Rademacher 复杂度随 horizon 增长 | 环境多目标奖励（每步） | ✓（MARL） |
| 17 | **RoboVQA**（ICRA 24） | **829,502 (video,text) 对** / 29,520 唯一指令 | 视频 + 文本 QA 对；自顶向下众包拆解 | 物理动作序列 + 视觉时序 | VQA 匹配 + 真机成功率 | ✗ |
| 18 | **CaStL**（ICRA 25） | 3 个 PDDL 域（非 benchmark） | NL 约束 → PDDL + Python，SMT 求解 | TAMP 层级深度 | 规划成功率 + 约束满足率 | ✗ |

---

## 4. 从横评里提炼出的五条规律（直接指导我们的设计）

### 规律 1 · 数据量由"真值获取成本"决定，不由会议门槛决定

| 真值来源 | 典型量级 | 例 |
|---|---|---|
| 自动可得（测试/排行榜/工具输出） | **10³–10⁵** | SWE-bench 2,294、RoboVQA 829K、InfoMosaic 621 |
| 半自动（模板 + 程序化改写） | **10²–10³** | NewtonBench 324、GAIA 466 |
| 需人工/专家逐题把关 | **10²** | ScienceAgentBench 102、Toolathlon 108、ColorBench 175、AstaBench 端到端 40 |

**对我们的结论**：**100–200 道精选题 + "模板可批量实例化"的可扩展性论证，就足够 ICLR。** 不要为了凑数牺牲真值质量。写法照抄 SWE-bench："正式集 N 题 + 管线可生成 M 万实例"。

### 规律 2 · "长"有五种互不相同的来源，必须明确宣称自己用哪一种

| 类型 | 代表 | 度量方式 |
|---|---|---|
| **交互轮数长** | Toolathlon（20 turns）、AgentGym-RL、ColorBench（13 步） | turns / steps |
| **依赖链深** | InfoMosaic（跨源依赖）、SWE-bench（跨文件） | 依赖图深度 |
| **搜索空间深** | NewtonBench（4–6 次突变） | 假设空间大小 |
| **挂钟/人力时间长** | MLE-bench（24h / 人力小时） | 小时数 |
| **上下文/记忆长** | 长上下文类（ICLR 2026 有 36 篇相关） | token 数、needle 位置 |

**对我们的结论**：我们的 checkpoint 型任务应当宣称 **交互轮数 + 依赖链深度**双来源；发现类任务应当宣称 **上下文长度 + 搜索空间深度**双来源。**两个任务用不同的"长"来源，这本身就是一个可讲的贡献点**（"long-horizon 不是一个维度"）。

### 规律 3 · 全行业公认"只判终态不够"，但**没人给出硬的过程判分**

| 论文 | 承认过程重要的方式 | 为什么不硬 |
|---|---|---|
| ScienceAgentBench | 引入逐题 rubric 部分分 | rubric 人写 + 图用 GPT-4o judge |
| NewtonBench | 统计 exploration rate | 靠 **LLM 事后标注**意图 |
| ColorBench | 里程碑 CR + 原子能力 AC | 里程碑是**屏幕状态**，非内容正确性 |
| RLVMR | 元推理标签的规则奖励 | 标签由 **agent 自己声明**，`planning` 分靠"最终成功"延迟归因 |
| AgentPRM | promise/progress 步级评分 | **学习型**价值估计，可被 hack |
| GAIA | 采集了人类步骤元数据 | **只用来分难度，不判分** |

**这就是我们最干净的缺口**：**中间产物级、数值真值比对的、可执行的过程判分**。全部 18 篇里没有一篇做到。

### 规律 4 · benchmark 论文必须自带"严谨性套件"，否则被打成"只是个数据集"

AstaBench 给出的五条（直接当我们的 checklist）：
1. 工具随 benchmark 出厂（否则不可控比较）→ 我们的取数网关 ✅（须显式声明）
2. 控制 model cost / tool access 混淆变量 → **必须报 Cost** ⬜
3. 标准化接口便于快速接入 → 答卷契约 ✅
4. 贴真实使用场景的整体度量 ⬜
5. 充分的 baseline agent → 四档轨迹已有雏形，需扩充 ⬜

外加：**GAIA 的答案保留机制**（公开 dev、隐藏 test 答案）、**MLE-bench 的 pass@k 方差报告**、**ScienceAgentBench 的专家验证描述**、**RoboVQA 的采集成本经济学**。

### 规律 5 · agentic RL 论文的三大障碍，我们的 benchmark 结构性地解掉两个

| WebRL 列出的障碍 | 通行解法 | 我们的结构性优势 |
|---|---|---|
| 训练任务稀缺 | 自演化课程生成 | **模板 + slots 批量实例化**，天然可规模化 |
| 反馈信号稀疏 | 训 ORM / 学 PRM（AgentPRM）/ 限 horizon（AgentGym-RL） | **checkpoint 直接给稠密可验证奖励，零训练成本、不可 hack** |
| 分布漂移 | KL 约束 + 置信度过滤回放 | 无优势，照抄 |

**这一条是"benchmark + RL 一体"叙事的全部立足点**：别人做 agentic RL 时必须自己造一个近似的过程奖励，而**我们的 benchmark 出厂即带真实的过程奖励**。

---

## 5. 立刻要补的四个设计缺口（横评直接暴露出来的）

| # | 缺口 | 来源 | 动作 |
|---|---|---|---|
| G1 | **checkpoint 指标未区分"未执行到"与"执行了但错"** | ColorBench 的 AC 条件化分母 + Illusion 的 step accuracy 定义（"不管前一步对不对"） | 每个 cp 报**条件通过率** `P(cp_i ✓ \| pre(cp_i) 全 ✓)`，并单列"未到达率" |
| G2 | **缺捷径过滤闸门** | InfoMosaic 的 shortcut filtering | 用 "单次全表查询 + 排序" 的 trivial baseline 跑全题集，被解掉的题淘汰或升难度 |
| G3 | **缺无回归约束 checkpoint** | SWE-bench 的 PASS_TO_PASS | 新增 cp 类型 `no_regression`：前序 cp 已建立的产物在后续不得被破坏 |
| G4 | **开放发现类缺客观标尺** | MLE-bench 的人类排行榜分位 | 把 agent 候选放进**已知材料/分子的性质分布**里报分位（"击败数据库中 x% 的已知材料"） |

**另加两条指标层面的采纳**：
- **Horizon Length H_s**（Illusion）作为我们的一级指标：模型能以概率 s 无错走完多少个 checkpoint；
- **promise / progress 的解析版**（AgentPRM 的无学习对偶）：`progress` = 本步新增 checkpoint reward；`promise` = DAG 上剩余可达 reward 上界。

---

## 6. 附：各论文阅读报告索引

| 会议 | 论文 | 阅读报告 |
|---|---|---|
| ICLR 2026 | Tool Decathlon | [`reference/ICLR-Tool-Decathlon/阅读报告.md`](../../reference/ICLR-Tool-Decathlon/阅读报告.md) |
| ICLR 2026 | NewtonBench | [`reference/ICLR-NewtonBench/阅读报告.md`](../../reference/ICLR-NewtonBench/阅读报告.md) |
| ICLR 2026 | AstaBench | [`reference/ICLR-AstaBench/阅读报告.md`](../../reference/ICLR-AstaBench/阅读报告.md) |
| ICLR 2026 | Illusion of Diminishing Returns | [`reference/ICLR-Illusion-of-Diminishing-Returns/阅读报告.md`](../../reference/ICLR-Illusion-of-Diminishing-Returns/阅读报告.md) |
| ICLR 2026 | InfoMosaic-Bench | [`reference/ICLR-InfoMosaic-Bench/阅读报告.md`](../../reference/ICLR-InfoMosaic-Bench/阅读报告.md) |
| ICLR 2026 | AgentGym-RL | [`reference/ICLR-AgentGym-RL/阅读报告.md`](../../reference/ICLR-AgentGym-RL/阅读报告.md) |
| ICLR 2026 | RLVMR | [`reference/ICLR-RLVMR/阅读报告.md`](../../reference/ICLR-RLVMR/阅读报告.md) |
| ICLR 2025 | ScienceAgentBench | [`reference/ICLR-ScienceAgentBench/阅读报告.md`](../../reference/ICLR-ScienceAgentBench/阅读报告.md) |
| ICLR 2025 | MLE-bench | [`reference/ICLR-MLE-bench/阅读报告.md`](../../reference/ICLR-MLE-bench/阅读报告.md) |
| ICLR 2025 | WebRL | [`reference/ICLR-WebRL/阅读报告.md`](../../reference/ICLR-WebRL/阅读报告.md) |
| ICLR 2024 | SWE-bench | [`reference/ICLR-SWE-bench/阅读报告.md`](../../reference/ICLR-SWE-bench/阅读报告.md) |
| ICLR 2024 | GAIA | [`reference/ICLR-GAIA/阅读报告.md`](../../reference/ICLR-GAIA/阅读报告.md) |
| ICLR 2024 | LoTa-Bench | [`reference/ICLR-LoTa-Bench/阅读报告.md`](../../reference/ICLR-LoTa-Bench/阅读报告.md) |
| WWW 2026 | ColorBench | [`reference/WWW-ColorBench/阅读报告.md`](../../reference/WWW-ColorBench/阅读报告.md) |
| WWW 2026 | AgentPRM | [`reference/WWW-AgentPRM/阅读报告.md`](../../reference/WWW-AgentPRM/阅读报告.md) |
| AAMAS 2025 | MOSMAC | [`reference/AAMAS-MOSMAC/阅读报告.md`](../../reference/AAMAS-MOSMAC/阅读报告.md) |
| ICRA 2024 | RoboVQA | [`reference/ICRA-RoboVQA/阅读报告.md`](../../reference/ICRA-RoboVQA/阅读报告.md) |
| ICRA 2025 | CaStL | [`reference/ICRA-CaStL/阅读报告.md`](../../reference/ICRA-CaStL/阅读报告.md) |

**未落盘的相关文献**：《Towards Long-Horizon Agents: A Survey》（2026-07，人民大学等；OpenReview `HyhfhlbWGh`、preprints.org DOI 10.20944/preprints202607.1328.v1）——OpenReview 与 preprints.org 均对脚本下载返回 403，未取得 PDF；其配套论文清单已本地核对（RUC-NLPIR/Awesome-Long-Horizon-Agents）。写 Related Work 前需人工下载补入。
