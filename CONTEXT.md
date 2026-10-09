# CONTEXT

当前任务由 ADR-0021 收窄为医药单次直接工具选择：分子/性质/菜单进入，选择一个具体模型，终止后释放 0/1 对错反馈，更新后续选择。动态 JEV 是在线判别模型方向，backbone 和最终算法尚未冻结。主 Agent 的复核/拒答与长轨迹属于扩展。当前唯一研究入口是 [研究主线文档](docs/研究主线/README.md)；决议规则见 [docs/决议](docs/决议/README.md)。旧的目标、提案和综合进度文件保留作历史与证据索引，不再与六份主线文档并列定口径。

## 当前概念：区分对象再比较

| 概念 | 当前定义 |
|---|---|
| 具体工具 / provider / arm | 能对应到一个模型、版本、配置与输出端点的选择动作；不是家族多数票 |
| 来源家族 family | 记录共享来源、训练谱系或相关模型的分组，用于依赖分析与泛化控制；不意味着成员独立 |
| 功能 key | 同一科学任务的能力标识，如 BBB_prediction；只能在输出定义与方向可比后分组 |
| Contextual Bandit | 每轮依据分子特征和合法历史反馈选择一个具体工具，并用可获得的反馈更新；不是 PPO/GRPO 的别名 |
| 主 Agent | 请求功能、处理已见结果、决定复核/作答/弃答；其参数训练与选择器训练分开定义 |
| 缓存回放 | 读取真实历史预测替代重复运行科学工具；不自动授予策略查看全表或真值的权利 |
| 参考真值 | 独立来源的实验标签；BBB 当前按已登记矛盾名单排除，来源质量与预测器训练重叠仍需审计 |
| 事后最佳上界 | 看过全表与真值后逐样本选对模型；只能量化机会，不能当成可学习或已实现准确率 |
| 少反馈与全反馈 | 每个样本仅取得所选模型反馈，与使用全部训练模型输出是不同信息条件；比较时必须说明 |
| 当前完成边界 | 已有数据、旧粒度分析和具体模型回放；尚无当前具体模型选择的完整主实验 |

## 当前目录：按证据职责阅读

- `data/tool_results/` 与 `data/tool_tables/` 保存原始预测、交付表和失败记录；不修改真实数值以配合故事。
- `src/probe_headless/pharma_bbb_replay.py` 和 `pharma_bbb_judge.py` 是当前具体模型回放与题后判分；选择器由外部提供，回放没有自动训练能力。
- `task/task36-pharma-provider-replay/` 保存回放验收，`task/task37-project-reconciliation/` 保存本轮项目综合对账。
- `docs/研究主线/生效/` 是故事线、方法论、Benchmark+Baseline 及三份评价标准的唯一生效入口；`docs/任务清单.md` 维护执行状态；`TODO_LOG.md` 记录每次变化。
- `docs/研究主线/版本/` 保存每类主线文档的历史版本。旧的 `docs/AAMAS2027_论文主线与进度.md`、`docs/动态JEV数学建模_v*.md`、`docs/论文质量规则_动态Jev可信工具调用.md` 等文件仍可追溯，但不再是当前主线。
- Task26–35 的通用新工具接入结果及旧六格工程只按证据相关性复用，不自动纳入当前医药主结果。

持续推进及 Nebula A800 实验已由 Owner 授权，见 ADR-0022；一次一条任务，实验前写清主张、对照、预算和否定条件。开发数据可用于诊断，最终确认仍需资格与冻结。源码里“已实现”、测试里的“通过”和论文里的“有效”是三个不同结论，不能互换。

<details>
<summary>历史词汇与交接快照：截至 2026-09-17</summary>

下文原样保留。当时的阶段、目录数量和训练口径不代表当前状态；定义与上方冲突时，按当前对象区分，不把旧家族投票等同于模型选择。

# CONTEXT

本仓在造一套评测 agent 的基准：当多个制药科学预测工具都正常返回、却对同一个分子给出矛盾结论时，agent 能不能发现异常、定位是哪个属性可疑、挑出更可靠的那个结果，或者在证据不足时弃答；再用这套基准把 agent 对熟悉工具的偏心纠回来。

[README.md](README.md) 是仓根入口，项目是什么、怎么跑起来、当前交付到哪、下一步做什么都在那里。目标与路线写在 [docs/目标.md](docs/目标.md)，技术论证写在 [docs/Proposal.md](docs/Proposal.md)，当前执行计划写在 [docs/计划_全工具远程跑批与结果库.md](docs/计划_全工具远程跑批与结果库.md)，全部在飞的活收成一张清单在 [docs/任务清单.md](docs/任务清单.md)，全部文档的地图与权威级别见 [docs/README.md](docs/README.md)。本文件不重复它们的内容，只做两件事：把项目专用词定下来，把目录职责划清。

## 当前阶段

第一层（离线结果库）进行中。本地已完成计划文档的 Step 1、3、8、9：工具总账清理、交付表契约定稿、性质配对表重建、分歧分析缺陷修复。工具的安装与预测运行全部归远程工程师，本地不调用任何预测工具、不产生新预测值。

**本项目自 2026-09-13 起整体移交远程工程师，他是长期唯一执行者。** 移交不是「远程跑一次批」，仓里的文档因此必须自足：一个新读者只靠 clone 下来的内容就能知道项目要做什么、当前到哪、下一步做什么。为这件事做的两处结构调整见下文「目录职责」的 `docs/inherited/` 与 `README.md` 两条。

## 词汇表

写文档、起工单、命名变量时用这里的词，不要漂移到同义词。

| 术语 | 定义 | 权威出处 |
| --- | --- | --- |
| 分子池 / 730 分子 | 全部工具共用的同一批输入，730 条 SMILES，来自 BBB 候选池 `bbb_authoritative_v2`，已冻结。`entity_id`（形如 `z000282`）是全仓对账的主键 | `data/tool_results/molecule_pool.csv`（以 CSV 为准，同名 `.json` 是同一份内容） |
| SMILES | 用一行文本表示分子结构的写法 | — |
| 工具总账 | 一个工具一行，写明归属线、远程状态、本地历史结果、依据。49 行，与矩阵的 49 个工具列一一对应 | `data/tool_property_matrix/LEDGER.tsv` |
| 归属线 | 工具当前归谁推进：跑批线 13（已装成，走冒烟到双轮全量）、安装线 23（待装）、接口线 5（走 HTTP 服务，瓶颈是服务方频率上限）、待裁定 5（装失败，等证据包与 owner 裁定）、缺陷记录 3 | `LEDGER.tsv` 第 2 列 |
| 缺陷记录 | 程序跑得起来，但预测值不是分子的函数：同一分子每次跑结果不同，或结果取决于它在批次里排第几。三个：QupKake、SolPredictor、DeePred-BBB。缺陷本身作为结果收进数据集，不打补丁修官方程序 | 计划文档口径 10 |
| 性质族 | 测同一种性质的一组来源，是配对与比较的单位。两个口径不要混：调研期矩阵列了 45 族（含只有单一来源、出不了题的），进入配对表并核到官方出处的只有 16 族，其中 14 族产出比较、2 族不比较 | `matrix.tsv`（45 族）与 `property_pairings.json`（16 族） |
| 来源家族 family | 同一工具的多版本、多变体在投票时合成一票的归并单位。例：ADMET-AI 的两个版本、B3clf 的 24 个变体、DeepPurpose 的三个编码器各算一票。只有跨家族的不同意见才算分歧——一个工具无法与自己矛盾 | 配对表的 `family` 字段 |
| 性质配对表 | 每一行来源带三样：官方定义原文、DOI 或官方页 URL、仓内证据路径。核不到出处的进 `excluded_sources` 并写明剔除理由。比较环节只读这张表，不再读代码里写死的对照 | `property_pairings.json`，生成器 `tools/step8_pairing_evidence/build_property_pairings.py` |
| 分歧 `n_conflicts` | 同一性质上两个不同来源家族给出不同标签的分子数。分母是 `n_comparable`（真正凑得出两票有效意见的分子数），不是 730 | `data/tool_results/divergence/summary.json` |
| 平票 `family_ties` | 家族内多列合成一票时票数相等（例 12:12），记为该家族对这个分子无意见，不参与比较也不计入分歧。旧代码用 `round()` 取整，而 Python 的 `round(0.5) == 0`（银行家舍入），会把平票静默判成阴性，已修 | `src/probe_headless/divergence_analysis.py` 的 `family_vote()` |
| 容差待定 `tolerance_pending` | 连续值性质没拿到官方容差时，分歧数写 `null` 而不是 `0`，并附 `conflict_status="not_scored"` 与原因。这个 null 不表示工具意见一致。当前 5 族：亲脂性 logP、极性表面积 TPSA、类药性 QED、Caco-2 渗透、血浆蛋白结合 | 计划文档口径 13 |
| 待核实区 `status=待核实` | 核不到官方定义出处，或多个来源不在同一刻度、同一方向上，整族不进比较，但脚本必须打印原因，不静默丢弃。当前 2 族：人肠吸收 HIA（三源正类方向不一致）、急性毒性 LD50（连续剂量、0–3 等级、EPA 四档三种刻度）。另有 P-gp 底物因活跃家族只剩 1 家而跳过 | `data/tool_results/divergence/not_compared.json` |
| 假零 | 会被读者当成「工具意见一致」的零。三个来源：没设容差、平票被静默取整、整列为空的表格列。三处都已堵掉，脚本末尾另有一道零值自检，违反即退出码 1 | `divergence_analysis.py` |
| 三件套 | 值列 + 状态列 + 原因列。禁止用 `-999` 之类哨兵值填数值列：它与真实概率同列，判分脚本一旦漏过滤就静默污染结论 | ADR-0003 第三节 |
| 主表 / 失败清单 | 主表一行一分子、一列一性质端点、值是工具原样输出（不阈值化）；失败清单 `<工具>.failures.csv`，四列 `entity_id, endpoint, error_type, error_message`。硬约束：主表每一个空格，失败清单里必须有且仅有一条对应记录（本地 15 份表实测 290 对 290） | 计划文档口径 6、Step 3 |
| 冒烟 smoke | 用三个探针分子试跑，确认工具能出数。判据已改为端点级：每端点至少一个分子出数且值合法，单分子失败记录但不阻塞；某端点在全部探针分子上无值仍拦下 | 计划文档口径 4、派单「第二轮批次总派发」§一 |
| 双轮全量 `full-r1` / `full-r2` | 对 730 个分子跑两个独立进程、独立目录，两轮结果分别保留、逐端点比较、不取平均。两轮逐位相同则在交付表里合成一列，不同则保留两列（本地只有 B3clf 需要保留两轮） | 计划文档口径 7 |
| L3 / W / V 三档 | 工具按可锁版本能力分档，档位由部署形态决定不由重要性决定。L3 本地可部署且版本可锁，进数据集判分；W 仅 web 服务、版本不可知，存完整证据链但不与 L3 混入同一判分列；V 商业需 license，只登记不测 | ADR-0003 第一节 |
| 工具同一性 | 名称 + 模型权重 + 训练数据 + 端点定义。涉及重训或换训练数据的版本升级算两个工具，各占一行 | ADR-0001 |
| 三层终点 | 第一层离线结果库（本轮）；第二层工具冲突数据集（只报工具互相不一致，不判谁对谁错）；第三层可评测 agent 的 benchmark（题目、可用工具、标准答案、评分另设计） | 计划文档 §一 |
| 工具过拟合 | 训练后 agent 的工具偏好分布不等于任务最优分布：对训练时见过的工具过度自信，对功能等价的新工具系统性回避 | Proposal §1.2 的 A 层 |
| 双置信度 | 事前置信度（调工具前对任务难度的评估）→ 工具调用 → 事后置信度（看完证据后对答案的把握）→ 作答或弃答 | Proposal §3.4、§3.7 |
| 弃答 yield / 损失 L | 答错罚 1，弃答付代价 c，`L = I(答错) + c·I(弃答)`。c 低于下界则最优策略退化为全部弃答，高于上界则永不弃答，正式实验在有效区间内取低中高三档 | Proposal §3.4 |

## 目录职责

| 路径 | 装什么 | 状态 |
| --- | --- | --- |
| `docs/` | 目标、提案、执行计划、远程派单、工具调研、ADR、agent 约定 | 在役，逐份说明见 `docs/README.md` |
| `docs/inherited/` | 继承决议链，9 个文件：ADR-0028 原文、它要求的提案 v2（七步稿）、含 §3.4 训练闭环的提案入库版、三条前置检验讨论、方向7 调研、四份评审原文。2026-09-13 从未跟踪的 `archive/governance/` 逐字复制入库，md5 与原件核过一致 | 在役的台账。目录说明与三份提案的血缘考据见 `docs/inherited/README.md` |
| `data/tool_results/` | 工具原样输出、`manifest.json`、分子池、分歧分析产物、正式跑批交付物 `formal_campaigns/`、重叠端点对照 `reference_comparison.{csv,json}` | 在役 |
| `data/tool_tables/` | 交付表：15 份主表 + 2 份失败清单 | 在役 |
| `data/tool_property_matrix/` | 工具总账 `LEDGER.tsv`、工具×性质矩阵 `matrix.tsv`、工具族归属 `tools.json`（49 条）、矩阵自检报告 `AUDIT.json`、性质配对表、处置记录 `DISPOSITION.json`、证据 `evidence/` | 在役，`LEDGER.tsv` 是工具清单的唯一权威 |
| `data/bbb_census_v3/` | 早期 BBB 工具冲突普查产物，`PROVENANCE.md` 记录从 archive 搬出的逐项哈希校验 | 在役的历史资产 |
| `src/probe_headless/` | 活代码：分歧分析 `divergence_analysis.py`、交付表生成 `build_tool_tables.py`、跑批控制层 `drug_prediction_campaign.py`、正式跑批控制层 `numeric_qualification_campaign.py`、运行时核验 `verify_runtime.py`、BBB 方差分解 `bbb_variance_decomposition.py`、BBB 恒答基线 `bbb_constant_answer_baseline.py`、BBB 裸答难度交叉 `bbb_naked_difficulty_crossref.py`，`tests/` 下 13 个测试文件共收集 229 条。同目录下另有 413 个上一版项目的运行产物（`chain9_*`、`sweep*`、`sw_*`、`ws_*`、`err*`、`*.out`、`*.log`），是上一版六格过程量基准的 agent 跑批日志，已跟踪但与本项目无关 | 在役。运行产物属清理候选。229 条中 1 条在 macOS 上失败（Linux 上全过），见下方「已知测试缺陷」 |
| `src/` 其余目录 | 上一版项目（六格过程量基准）的代码与文档 | 过期。`src/README.md` 仍引用已删除的 `../goal.md` |
| `tools/` | 生成器与核验脚本：`step1_survey_evidence/`（调研期入口存活证据）、`step8_pairing_evidence/`（配对表生成器与核验日志）、`build_recovered_manifest.py`（ADR-0008 的纳入清单生成器）、`nebula/`（星云平台远程执行的统一入口：CLI `neb`、agent skill、18 条回归测试，见 ADR-0010） | 在役 |
| `governance/` | 远程工程师交回的验收报告与状态汇总：验收报告 `进展_<日期>_<主题>.md`、接手清单 `交接_<日期>_<主题>.md`、状态汇总 `现状_<日期>_<主题>.md` | 在役四份：进展 20260909、进展 20260911、现状 20260912、交接 20260912。同目录下其余文件是上一版项目的，owner 已在工作区把它们搬进 `archive/governance/`，删除动作未提交 |
| `reference/` | 116 个外部基准的采样与笔记，每个目录一份 `阅读报告.md`，其中 72 个带 `SOURCE.txt` 记出处 | 参考 |
| `research/` | 专题调查报告 | 参考 |
| `archive/` | 上一版项目的封存材料，含旧 `goal.md`（v3.0.1 材料+药物 long-horizon benchmark，已作废） | 封存，git 未跟踪 |
| `archive/governance/adr/` | 上一版项目封存期的 28 份 ADR，编号自成一套，与本仓 `docs/adr/` 不通 | 封存，git 未跟踪。其中 0028 是例外：它裁的是本项目的训练管线，已复制进 `docs/inherited/`，状态 Accepted、未作废，与本项目现行目标口径冲突且未裁，见 `docs/目标.md` 的「一处待裁的目标冲突」 |
| `.scratch/` | 工单，约定见 `docs/agents/issue-tracker.md` | `step1-verification/` 的 12 张工单已被计划文档的十步取代 |
| `reports/`、`goverence/` | 空。`goverence` 是拼写错误的目录名 | 清理候选 |

**仓根有一份 `README.md`，2026-09-13 重写。** 原先仓根没有 README（上一版项目的 `README.md` 在 owner 的未提交删除列表里），而本项目整体移交远程后，clone 下来的第一个文件就是它。它不重复 `CONTEXT.md`、`docs/目标.md` 与计划文档的内容，只做入口：项目是什么、这个仓现在归谁、终点分三层、当前状态的总数、怎么把环境跑起来、下一步、一处未裁的目标冲突、四条停机条件。

## 文档约定

1. 决策一事一文件，进 `docs/adr/`，编号递增，索引在 `docs/adr/README.md`。被取代的标 `Superseded by NNNN`，不就地改写正文。
2. 工单进 `.scratch/<feature-slug>/`，一单一文件，状态写在文件头的 `Status:` 行。
3. 给远程的派单写 `docs/远程跑批任务_*.md`，一份自足——远程同事读完不需要看别的资料。远程交回的验收报告进 `governance/`。
4. 执行进度写进计划文档的「执行进度」段，不另开进度文件。台账级文档发现错了就追加更正段，不改写原文。
5. 生成的产物不写时间戳。同一份代码连跑两次，产物 md5 必须一致。
6. 每个数字都能指到具体文件。计划文档 Step 7 的完成判据原文：结账文档中不出现「预计」「应该」这类措辞。
7. 判据要改先停下交 owner，四条停机条件见计划文档 §七。
8. 文档名用中文，与现有命名一致；ADR 文件名用英文 slug。

## 已知测试缺陷

`src/probe_headless` 共收集 229 条测试，2026-09-15 在 Linux 实测 228 通过、1 跳过。在 macOS 上会多一条失败：`tests/test_numeric_qualification_campaign.py::test_completed_pending_phase_is_published_without_rerunning_worker`，原因是 macOS 把临时目录写成 `/var/folders/...`，而 `pathlib.Path.resolve()` 会把它解析成软链真实路径 `/private/var/folders/...`，测试用字面路径比对 `run_dir` 与 `target`，两者字符串不相等就判失败。这是测试自身的平台缺陷，不是被测代码的问题，在 Linux 上不出现。该测试属远程跑批控制层，运行环境验证类改动归远程，本地不擅自修改，只登记在此。

### 更正（2026-09-17）：上段两处断言被实测推翻，macOS 上是 10 条失败、三个根因

上段写「在 macOS 上会多一条失败」，并把根因单归为 `/var` → `/private/var` 软链。2026-09-17 在 macOS 实测全仓 `src/probe_headless` 229 条：**10 failed / 218 passed / 1 skipped**，不是 1 条失败。10 条分属三个互不相干的根因：

| 类 | 条数 | 根因 | 涉及测试 |
|----|------|------|---------|
| A | 8 | **被测代码缺 Darwin 分支**。`drug_prediction_campaign.py:60` 的 `process_start_ticks()` 只读 `/proc/<pid>/stat`，macOS 无 `/proc`，`except (FileNotFoundError, IndexError, ValueError)` 把缺失吞成 `None`；下游 `jplogp_cdk_campaign.py:535` 判 `Run process identity is invalid`（7 条）或测试直接 `assertGreater(None, 0)` 抛 `TypeError`（1 条） | `tests/test_jplogp_cdk_campaign.py` 全部 8 条 |
| B | 1 | `/var` → `/private/var` 软链路径字面比对，即上段描述的那条（**该条描述本身正确**） | `tests/test_numeric_qualification_campaign.py::test_completed_pending_phase_is_published_without_rerunning_worker` |
| C | 1 | **真实缺陷，非平台问题**：task23 逐字节重放的原始出处 `runs_naked/` 在 2026-09-16 环境清空事故中永久丢失，见 problems.md PR-69 | `tests/test_bbb_naked_difficulty_crossref.py::DeterminismTests::test_rerun_byte_identical` |

上段把 A、B 合算成「一条路径缺陷」是错的，两者修法不同：

- **B 类**是测试自身缺陷，修法即上段所述（比对前两边都调 `.resolve()`，或用 `Path.samefile()`）。
- **A 类**是被测代码问题，且仓内已有现成可复用实现：`tool_conflict_census.py:753` 的 `_darwin_process_start_ticks()`（sysctl `KERN_PROC_PID` 读 `kinfo_proc`，其注释写明已与 `ps -p <pid> -o lstart=` 交叉核对到秒一致）。同一 pid 实测对照：`tool_conflict_census.process_start_ticks()` 返回正整数 `1789634082051964`，`drug_prediction_campaign.process_start_ticks()` 返回 `None`。`tool_conflict_census.py` 不 import `drug_prediction_campaign`（依赖方向相反，`jplogp_cdk_campaign.py:23` 以 `import drug_prediction_campaign as base` 依赖后者），故复用无循环依赖风险。

A 类**不构成证据链漏洞**：`process_start_ticks` 为 `None` 时 `jplogp_cdk_campaign.py:535` 会显式抛 `Run process identity is invalid`，属失败关闭（fail closed），不会产出假证据。实际后果是 macOS 上的 campaign 跑批产物一律过不了 verify、等于不可用。

**数字自洽核验**：229 = 218 + 10 + 1。A、B 两类共 9 条在 Linux 上不触发（`/proc` 存在、临时目录不经 `/var` 软链），故当前 Linux 应为 218 + 9 = **227 passed / 1 failed / 1 skipped**，与 problems.md PR-69 登记的数字逐位吻合（C 类在两个平台都红）。上段「2026-09-15 Linux 228 通过」当时成立：那时 `runs_naked/` 尚在，C 类是绿的，228 + 1 skipped = 229；09-16 事故后才掉到 227。

处置口径同 B 类：A 类属运行环境验证类改动，归远程，本地不擅自修改，只登记在此。议题登记见 problems.md PR-70，可执行条目见 [docs/任务清单.md](docs/任务清单.md)「先做：环境与现场」。

</details>
