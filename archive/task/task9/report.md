# task9 · 完成汇报（report）

> Status: **done** ｜ 日期：2026-07-31 ｜ 任务契约：[`task.md`](task.md) ｜ 决议：[ADR-0010](../../governance/adr/0010-datasets改名reference并容纳论文原文.md)

## 1. 结论（先总）

**四会里只有 ICLR 是主战场，owner 设想的"双任务 benchmark + agentic RL"完全合它的调性；真正的风险不是调性而是"拼盘感"，解法是先立一个统一形式化（VIS-DAG）再落两个实例；只出 benchmark 的备用方案成立但不省时间；发现类任务应从"反向造一条 trace"改为"造证据闭包 + 长上下文埋线索 + 四道门判分"。**

四条子结论：

1. **会议取舍**：ICLR 主投（ICLR 2027 摘要 2026-09-18 / 正文 2026-09-25 AOE，官方 CFP 明列 datasets & benchmarks 与 applications to physical sciences）；**ICRA、AAMAS 结构性不投**——ICRA 的 long-horizon 是机器人 TAMP、benchmark 类几乎全是感知数据集；AAMAS 的核心身份是多智能体，直接撞 GOAL.md 硬边界 1（单 Agent）。WWW 2027 留作 Plan C。
2. **调性判定**：合，且正当风口。ICLR 标题含 long-horizon 的论文从 2024 年 3 篇 → 2025 年 4 篇 → **2026 年 29 篇**；标题同时含 agent 与 bench 的从 3 → 20 → **44**。"benchmark + 在其上验证 agentic RL"有 RLVMR、AgentGym-RL（ICLR 2026）、WebRL（ICLR 2025）三个同型先例。
3. **我们的空白是真的**：落盘的 18 篇里**没有一篇**做到"中间产物级、数值真值比对、可执行"的过程判分。六篇分别承认过程重要却都没做硬——ScienceAgentBench 上 rubric + GPT-4o judge、NewtonBench 用 LLM 事后标注意图、ColorBench 的里程碑是屏幕状态、RLVMR 的标签由 agent 自己声明、AgentPRM 是学习型评分、GAIA 采了人类步骤元数据却只用来分难度。
4. **发现类怎么造**：不造路径，造**证据闭包**——把"为什么答案是它"拆成原子事实，算出**多个最小充分集（MSS）**，打散埋进可控长度的合成长上下文并配四类干扰项，用**结论 / 证据覆盖 / 落地 / 抗诱导**四道门判分，且**结论对但证据覆盖不过半直接给 0**（"猜对不算发现"的机器化表达）。

## 2. 做了什么

| # | 动作 | 交付物 | Todo |
|---|---|---|---|
| 1 | `datasets/` → `reference/`（git mv 保留改名血缘），语义扩为**参考资料区**（公开数据集采样 + 会议论文原文与阅读报告）；仓内自指路径改写 **121 处**（另 18 处目录内相对链接归一化），并回退 3 处误命中的上游路径（HF / pip 包 / 上游仓库自指）；规范文档 4 份 + 新 ADR 同步 | `reference/`、`ACTION_RULES.md` §2/§4、`CLAUDE.md`、`README.md`、`task/_templates/README.md`、[ADR-0010](../../governance/adr/0010-datasets改名reference并容纳论文原文.md) | T-24 |
| 2 | 四会检索：**ICLR 走官方接受列表全量**（2024 2296 / 2025 3827 / 2026 5468 篇标题）；ICRA/AAMAS/WWW 走 DBLP `streamid` 关键词召回（48 / 25 / 78 条） | [`src/data_download/dblp_venue_scan.py`](../../src/data_download/dblp_venue_scan.py)、[`dblp_slow_scan.py`](../../src/data_download/dblp_slow_scan.py)、[`research/四会检索原始清单/`](../../research/四会检索原始清单/) | T-25 |
| 3 | 精筛并下载 **18 篇** PDF（ICLR×13 / ICRA×2 / WWW×2 / AAMAS×1），每篇附 `SOURCE.txt` 记录来源与版本 | `reference/{ICLR,ICRA,WWW,AAMAS}-*/*.pdf` + [`fetch_paper_pdfs.py`](../../src/data_download/fetch_paper_pdfs.py) + [`paper_specs.tsv`](../../src/data_download/paper_specs.tsv) | T-26 |
| 4 | 逐篇阅读报告，**必答三问**（数据量 / 数据集形态 / "长"体现在哪）+ 判分粒度 + 对我方可借与可打点 + **证据锚点行号** | `reference/{ICLR,ICRA,WWW,AAMAS}-*/阅读报告.md`（18/18 齐） | T-27 |
| 5 | 四会画像 + 18 篇三问横评表 + **五条规律** + **四个设计缺口** | [`目标会议长程benchmark横评.md`](目标会议长程benchmark横评.md) | T-28 |
| 6 | 投稿策略：四会取舍、ICLR 2027 时间表、调性逐条对标、Plan A/B、**12 项加厚清单**、八周排期 + Go/No-Go | [`投稿策略_ICLR调性对标.md`](投稿策略_ICLR调性对标.md) | T-29 |
| 7 | 发现类构造方案：证据闭包 + haystack 四旋钮 + 四类干扰 + 四道门 reward 伪代码 + **VIS-DAG 统一形式化** + 六步造题管线 + 五道闸门 | [`发现类任务构造方案.md`](发现类任务构造方案.md) | T-30 |
| 8 | 账本与议题同步：TODO 追加 T-24~T-35（无删行、无重号，T-01~T-35 连续）；`problems.md` 关闭 PR-14、新增 PR-22/23/24 | `TODO.md`、`problems.md` | — |

## 3. 关键数据（证据在哪）

### 3.1 ICLR 赛道热度（口径：官方 `iclr.cc/virtual/<year>/papers.html` 全量标题）

| 关键词 | 2024 | 2025 | 2026 |
|---|---|---|---|
| 全部接受论文 | 2296 | 3827 | 5468 |
| long-horizon / long horizon | 3 | 4 | **29** |
| agent ∧ bench | 3 | 20 | **44** |
| bench | 40 | 149 | **293** |
| reinforcement learning / RL | 145 | 155 | **414** |
| 科学/分子/材料 × (bench\|agent\|discover) | — | 24 | **64** |

### 3.2 三问横评摘要（完整 18 行见横评文档 §3）

| 论文 | 数据量 | "长"体现在哪 | 过程判分 |
|---|---|---|---|
| Toolathlon（ICLR 26） | 108 题 / 604 工具 | ~20 turns 工具调用 | ✗ 终态二元 |
| NewtonBench（ICLR 26） | 324 题（12→108 反事实改写×3 系统） | 搜索深度；**交互仅 ≤10 轮** | ✗（过程靠 LLM 事后标注） |
| AstaBench（ICLR 26） | 2400+ 题；端到端子集仅 40+40 | 流程链长 + 成本轴 | ✗ |
| Illusion（ICLR 26） | 可无限合成探针 | **步数轴本身**；给出 H_s 等 5 个指标 | ✅ 但任务是合成字典检索 |
| InfoMosaic-Bench（ICLR 26） | 621 题 / 77 工具 / 含医学生物 | 跨源依赖链深度 | ✗（有证据链但不判） |
| ScienceAgentBench（ICLR 25） | 102 题 / 44 论文 / 9 专家 | 分析流水线必需步骤链 | 半（rubric + GPT-4o judge） |
| ColorBench（WWW 26） | 175 题 / 15 类原子能力 | 平均 13+ 步 | 半（里程碑=屏幕状态） |
| SWE-bench（ICLR 24） | 2,294 实例（可扩展 19,000） | 代码库规模 + 无回归约束 | ✗ |
| GAIA（ICLR 24） | 466 题（166 公开/300 保留） | 按标注者步数分三层 | ✗ |

**规律**（横评 §4）：数据量由真值获取成本决定（自动可得 10³–10⁵，需专家把关 10²）；"长"有五种互不相同的来源（轮数 / 依赖深 / 搜索深 / 挂钟时间 / 上下文长度）；全行业公认只判终态不够但没人做硬过程判分；benchmark 论文必须自带严谨性套件；agentic RL 的三大障碍我们结构性解掉两个。

## 4. 遗留问题（全部已入账本与议题表）

| 编号 | 遗留 | 归属 |
|---|---|---|
| T-31 | VIS-DAG 统一形式化定为 ADR（否则"拼盘"风险不解） | PR-23 |
| T-32 | 落地四个设计缺口：条件通过率、捷径过滤闸、`no_regression` cp、开放类分位标尺 | PR-22 |
| T-33 | `discovery_haystack` 第三模板家族 + 首个 case + haystack 生成器 + MSS 枚举器 + 五闸 | PR-24 |
| T-34 | 采纳 `H_s` 与 step accuracy 口径为一级指标；补 Cost、pass@k 与方差 | PR-22 |
| T-35 | 人工下载《Towards Long-Horizon Agents: A Survey》PDF（OpenReview 与 preprints.org 均对脚本返回 403） | — |

**方法学层面的已知限制（诚实记录）**：

1. **ICRA/AAMAS/WWW 三会是关键词召回而非全量**——DBLP 只检索标题且短语按词 AND 前缀匹配，故这三会可能漏检标题不含关键词的相关论文。ICLR 为全量，结论可靠性最高。
2. **ICLR 2026 尚未进入 DBLP**（`dblp.org/db/conf/iclr/iclr2026` 返回 404），元数据取自官方 virtual 站；OpenReview 的 `/notes` 与 `/pdf` 端点对脚本请求返回 403 challenge，故 PDF 全部改从 arXiv 取（AAMAS 从 IFAAMAS 官网）。**部分 PDF 是 arXiv 版而非会议正式版**，各篇 `SOURCE.txt` 与阅读报告表头已注明。
3. **`reference/` 内既有的采样说明文档存在若干上游 README 断链**（如 `reference/*/REPO_README.md` 指向未下载的上游子目录），属改名前既有状态，本次未处理；由本次改名引入的 18 处仓内自指断链已全部修复（复检 0 条残留）。

---

## 5. 追加轮（2026-07-31 owner 追问：开源状态 + 采样 case + 定目标）

### 5.1 做了什么

| # | 动作 | 交付物 | Todo |
|---|---|---|---|
| 9 | 确定 18 篇数据集开源状态（16 开源 / 2 方法论文无数据集）；**实探发现 HF 本环境不可达**（HTTP 000），GitHub/ModelScope 可达，据此调整采样通道 | `数据集开源状态与样例核验.md` | T-36 |
| 10 | 6 篇拉到真实样本（InfoMosaic GT+线索 / NewtonBench 代码定律 / Toolathlon task+eval / WebRL / ColorBench / AgentGym 配置），落到各 `reference/<CONF>-*/sample_*` | `src/data_download/gen_case_docs.py` + 各 sample 文件 | T-36 |
| 11 | 18 篇目录各补 `数据格式与样例.md`（开源状态 + 一条完整 case + 数据格式） | `reference/<CONF>-*/数据格式与样例.md`（18/18） | T-36 |
| 12 | 核查 dag_poc：发现**引擎已实现且三模型实跑过**（`_all_results.json` 15 条，命中 4/15，过程分 0.03–0.87），区分度成立；同时暴露 2 个 bug | `benchmark目标定义_v1.md` §5 | T-36/T-37 |
| 13 | 写定目标总结（回答 owner 5 问） | `benchmark目标定义_v1.md` | T-36 |

### 5.2 追加结论（定目标）

- **量级**：100–150 题（fixed ~100 + open ~30–50）+ 可扩展性论证；对标 ICLR 科学 benchmark 的 10² 合格线。
- **特征优势**：唯一"中间产物级 + 数值真值 + 可执行 + 免 LLM judge"的科学发现过程 benchmark（F1–F5）。
- **两类**：都能做、都适合、互补；fixed 已实跑验证，open 有 InfoMosaic testcase 同构先例；统一于 VIS-DAG。
- **难点**：核心 D3 防蒙对 + D4 防捷径（决定测的是发现力还是记忆/检索力）。
- **dag_poc**：内核已验证但未达标——差量级(20×)、缺 open 类、horizon 偏短、缺 4 个判分口径、且实跑暴露 2 个 bug（cns 疑似过易/泄漏、cp5 空答案给满分）。

---

## 6. 追加轮（2026-08-01 owner 追问：文档是否清楚 → 触发 T-37 修复）

### 6.1 触发经过

owner 要求"自己再看一下造题详解是否写清楚了，若清楚就复述一遍"。自查 + 启动独立审稿 subagent 逐条挑错后，判定**不够清楚**，核心缺口是"通篇讲别人怎么造题，唯独没有我们自己的题长什么样"。补 A0（我们自己的 CNS 题从题面走到得分）时核对真实源文件，**发现文档里三处判分口径写错**，进而回溯出 dag_poc 五个实现问题。owner 批准修复。

### 6.2 做了什么

| # | 动作 | 交付物 | 账本 |
|---|---|---|---|
| 14 | 独立审稿驱动修订造题详解：A0 提为独立第 1 章；修正 cp4/cp5/cp6 三处口径错述；补 5 家缺失关键数字（621/2294/466/175/~20turns/≤10轮）；统一"无人做过程分"限定语消四处自我矛盾 | `research/各benchmark造题法详解.md` | T-40 |
| 15 | **诊断出 5 个判分问题**（立项时只知 2 个）并全部修复 | 见 6.3 | T-37 ✅ |
| 16 | 按新口径重跑 15 个实验点，输出 `process/gate/task_score/submitted/gold_pass_set` 全字段 | `dag_poc/traces/_all_results.json` | T-37 |
| 17 | 立 ADR 记录契约变更与数据作废口径 | [ADR-0011](../../governance/adr/0011-答卷契约拆分候选池与合格集.md)；顺补 ADR 索引漏登的 0010 | — |
| 18 | 账本收口：PR-25 CLOSED、新开 PR-26（旧数据作废）、两条从 G5 移入 G4；新增 T-43（重跑） | `problems.md`、`TODO.md` | — |

### 6.3 五个问题与修法（详见 ADR-0011）

| # | 问题 | 修法 | 原判断是否成立 |
|---|---|---|---|
| ① | **cp4 指标冲突**：`ranked_candidates` 被 cp2/cp3 当"候选池"（交多得分高）、被 cp4 当"合格集"（交多被 F1 罚），激励自相矛盾 | 答卷契约拆 `screened_pool` / `ranked_candidates`，旧答卷自动回落 | ✗ 原写"F1 是天花板、永远拿不到 1.0"**可证伪**——交 1 个就满分 |
| ② | **cp5 空集给满分**：`set()==set()` 也判 1.0，glm 4 道题未交答案却 cp5=1.0 | 空答卷置 0（4 处 1.0→0.0，process 由 0.17–0.20 降至 0.067–0.10） | ✓ 成立 |
| ③ | **伪命中** | 定位真因 | ✗ 原归因"匿名化被穿透"**被推翻**，真因见 ④ |
| ④ | **mock 检索截断（最根本）**：fallback 返回 `[:5]`，自然语言查询恒走 fallback → 20 个分子只有 `aa`–`ae` 可见，**3 道题（gt=`ba`/`bb`/`bc`）任何模型都不可能做对** | fallback 改返回全库 | 此前完全未被发现 |
| ⑤ | **评分未读真实答卷**：main 从 trace 入参"推断"假 response，推出 `C0CC`/`Donepezil` 这类非候选 ID | 改为只读 `_response.json`，未交卷即判 0 不做补救推断 | 此前完全未被发现 |

### 6.4 修复后的真实数据（15 点）

**命中 4/15 ｜ 门控通过 0/15 ｜ 未交卷 5/15 ｜ task_score 均值 0.151**

CNS 题三模型：process 0.846–0.867（能区分——glm 40 次工具调用且漏查一格，kimi 9 次全查到），但 cp4 未过阈 → **task_score 全为 0.300**。

新暴露事实：**5/15 是 agent 压根没交卷**（glm 4 次、kimi 1 次），此前被"推断假 response"掩盖。

### 6.5 遗留（已入账本）

- **PR-26 / T-43**：旧 15 点数据在"检索截断"环境下跑出，其中 3 道题不可解，**命中率 4/15 不反映真实能力、不得作为论文数据引用**，必须用修复后的 mock server 重跑。
- 重跑后确认各 case 合格集 `|G|` 是否仍为 1；若 >1 则 cp6 接近度衰减分支首次可测，可关 T-21。
- cp6 规格（按距离衰减）与实现（常数 0.5）仍不一致，留待 T-43 一并对齐。
