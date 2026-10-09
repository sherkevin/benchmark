# 文档地图

## 唯一生效研究入口（2026-09-28）

研究主线已经收拢为六类文档；每类只能有一个生效版本。请先读 [docs/研究主线/README.md](研究主线/README.md)：

| 类别 | 唯一生效文件 |
|---|---|
| 故事线与创新 | [研究主线/生效/01-故事线与创新.md](研究主线/生效/01-故事线与创新.md) |
| 方法论 | [研究主线/生效/02-方法论.md](研究主线/生效/02-方法论.md) |
| Benchmark + Baseline | [研究主线/生效/03-Benchmark与Baseline.md](研究主线/生效/03-Benchmark与Baseline.md) |
| 故事线评价标准 | [研究主线/生效/评价标准/01-故事线评价标准.md](研究主线/生效/评价标准/01-故事线评价标准.md) |
| 方法论评价标准 | [研究主线/生效/评价标准/02-方法论评价标准.md](研究主线/生效/评价标准/02-方法论评价标准.md) |
| Benchmark + Baseline 评价标准 | [研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md](研究主线/生效/评价标准/03-Benchmark与Baseline评价标准.md) |

`task/`、`research/`、`experiments/logs/` 是证据和执行层；旧的目标、综合进度、数学候选和质量规则文件保留作历史参考。决议只记双方确认的小取舍和争议，管理规则见 [决议/README.md](决议/README.md)。

## 执行与证据入口（不定义研究主线，2026-09-28）

下面这些文件仍然是执行台账或证据入口，但不再定义故事线、方法论或 benchmark 的唯一口径。研究口径以本页上方六份主文档为准；旧文件里的“当前/权威”只描述它们写作时的状态。

| 当前文件 | 职责 |
|---|---|
| [目标.md](目标.md) | 历史目标与范围记录；需要追溯旧约束时查阅 |
| [Task49 计划](../task/task49-pharma-benchmark-baseline-freeze/next_plan.md) | 已获批准的持续执行顺序 |
| [数据合同](../task/task49-pharma-benchmark-baseline-freeze/data_contract_v0.md) | 逐端点资格、分母、来源与未解决问题 |
| [baseline 合同](../task/task49-pharma-benchmark-baseline-freeze/baseline_adapters_v0.md) | 原论文适配、初始化和在线信息公平性 |
| [方法卡](../task/task49-pharma-benchmark-baseline-freeze/method_card_v0.md) | 当前候选推导、反例和表示诊断；尚无冻结主算法 |
| [Task50](../task/task50-aamas-paper-and-research-loop/report.md) / [论文草稿](../paper/aamas2027/README.md) | AAMAS 官方要求、LaTeX 题目摘要和逐任务复盘 |
| [任务清单.md](任务清单.md) / [TODO_LOG.md](../TODO_LOG.md) | 验收与执行记录；不替代原始实验日志 |
| [长期任务与推进原则.md](长期任务与推进原则.md) | 长期目标、迭代顺序、信息边界、失败处置和当前阶段；不替代三份生效主线文档或评价标准 |
| [决议文件](决议/README.md) | 小范围双方确认的取舍与争议裁定；不承载完整研究文档 |

下表是截至 09-24 的资料索引，保留其来源和历史职责；不与上方六份生效文档竞争口径。

## 历史资料索引（2026-09-24）

| 文件 | 当前职责 | 状态与级别 |
|---|---|---|
| [目标.md](目标.md) | 记录一段时期内 Owner 明确的研究对象、范围和成功边界 | 历史目标记录；当前主线见六份生效文档 |
| [AAMAS2027_论文主线与进度.md](AAMAS2027_论文主线与进度.md) | 保存旧阶段的进度、贡献候选、证据和优先级 | 历史综合说明；不定义当前主线 |
| [任务清单.md](任务清单.md) | 当前P01–P11任务、依赖、完成判据，以及历史欠账 | 执行索引；新跑批和判据变更另行授权 |
| [论文质量规则_动态Jev可信工具调用.md](论文质量规则_动态Jev可信工具调用.md) | 旧版本的审稿视角、拒稿信号和投稿前自检 | 历史质量规则；当前规则已拆入三份评价标准 |
| [开源复用与集成路线_动态Jev.md](开源复用与集成路线_动态Jev.md) | 旧版本的功能合同、benchmark 和开源组件拼装路线 | 历史候选；当前实验矩阵见生效 Benchmark 文档 |
| [动态JEV数学建模_v2.md](动态JEV数学建模_v2.md) | 动态 JEV 的参考问题、延迟反馈和可实现近似 | 历史数学候选；不定义当前方法 |
| [动态JEV数学建模_v3_最小符号案例.md](动态JEV数学建模_v3_最小符号案例.md) | 最小符号、Bellman 参考问题和案例 | 历史建模候选；不定义当前方法 |
| [动态JEV数学建模_v4_可实现版.md](动态JEV数学建模_v4_可实现版.md) | 回放环境、最小模型和 M0–M2 实验审查 | 历史可实现性审查；证据仍可引用 |
| [动态JEV数学建模_v5_线性状态递推与创新边界.md](动态JEV数学建模_v5_线性状态递推与创新边界.md) | selected-only 递推、验证门控和创新边界 | 历史方法候选；不定义当前方法 |
| [候选方法_动态JEV_在线验证增益.md](候选方法_动态JEV_在线验证增益.md) | 从任务痛点倒推的候选主创新 | 历史候选；是否进入主线以生效故事线文档为准 |
| [研究迭代规范.md](研究迭代规范.md) | 故事—创新—方法—审查—小批验证的流程、主张对应与失败处置 | Owner本轮明确的执行方式；不替代实验权限或当前目标 |
| [../research/Agent_Benchmark_Baseline_Publication_Survey_20260924.md](../research/Agent_Benchmark_Baseline_Publication_Survey_20260924.md) | 近期 Agent benchmark/baseline 发表范式、PTRB-v0 现状审计与 Task47 关闭顺序 | 研究审计；不把候选 benchmark 或 baseline 结果写成已完成 |
| [../research/Method_Paper_Public_Benchmark_Survey_20260924.md](../research/Method_Paper_Public_Benchmark_Survey_20260924.md) | 方法论论文实际复用的公开 benchmark、公开性分层与 RouterBench/ToolSandbox/BBB 选型 | 当前 benchmark 组织原则；以 ADR-0015 为准 |
| [../research/m1_dynamic_jev_runner_20260925.md](../research/m1_dynamic_jev_runner_20260925.md) | M1 随机 probe、delayed verification gain、IPW 状态合同与 smoke | 已实现合同；不是效果结论 |
| [../research/m2_dynamic_jev_gain_gate_20260925.md](../research/m2_dynamic_jev_gain_gate_20260925.md) | M2 gain/STOP gate、精确 propensity 与 delayed state smoke | 已实现合同；risk gate 与最终算法未实现 |
| [../research/m2_baseline_comparison_20260925.md](../research/m2_baseline_comparison_20260925.md) | M2 与 never/always/random verification 的匹配 frozen replay 对照 | 管线检查；30-case 数字不是最终效果结论 |
| [../research/m1_m2_theory_gate_fix_20260925.md](../research/m1_m2_theory_gate_fix_20260925.md) | M1/M2 的任务损失、冷启动、provider ESS 和 LCB gate 修正 | 当前本地理论合同；未提交 Nebula |
| [../research/m1_m2_ht_gate_20260925.md](../research/m1_m2_ht_gate_20260925.md) | 否定旧 Kish 半径并验证 HT + martingale Hoeffding + alpha spending | 当前进入 BBB 本地确认前的理论闸门 |
| [../research/m2_bbb_ht_confirmation_20260925.md](../research/m2_bbb_ht_confirmation_20260925.md) | 722 个可评分 BBB 分子的 HT 点估计与 gate 确认 | HT 点估计可对齐；当前严格 gate 尚未打开，不能做效果结论 |
| [../research/m1_m2_freedman_epoch_gate_20260925.md](../research/m1_m2_freedman_epoch_gate_20260925.md) | Freedman 半径与 64-episode 批次 gate 的合成和 BBB 本地确认 | 当前可尝试方案；远程只做单条规模化复现 |
| [../task/task48-method-public-benchmark-selection/report.md](../task/task48-method-public-benchmark-selection/report.md) | Task48 方法论文公开 benchmark 选型摘要 | 研究报告；split 和运行预算仍待冻结 |
| [../CONTEXT.md](../CONTEXT.md) | 当前模型、家族、Bandit、Agent与缓存概念 | 当前术语与目录入口；旧词汇折叠保留 |
| [../TODO_LOG.md](../TODO_LOG.md) | 每次执行的状态、结果链接与下一步 | 台账，不作为唯一结果存储 |
| [../task/task36-pharma-provider-replay/report.json](../task/task36-pharma-provider-replay/report.json) | 当前逐模型回放与题后判分验收 | 工程事实；不是训练有效性证明 |
| [计划_全工具远程跑批与结果库.md](计划_全工具远程跑批与结果库.md) | 既有科学工具采集、正式跑批纪律和欠账 | 仅在数据采集对象内适用，不再充当整篇论文路线 |
| [Proposal.md](Proposal.md) 与 inherited/ | 原可靠性提案和决议血缘 | 历史方案与仍需承接的约束，不能掩盖新对象 |
| DynamicToolRouting/Onboarding 各版矩阵及 research/AAMAS2027_Ours_Problem_Formulation.md | 通用接入方向的探索过程 | 历史参考；不是当前医药选择主实验协议 |
| [adr/README.md](adr/README.md) | 原始决议与状态 | 不静默改写；新训练对象的承接单独登记 |

完整 reusable 结论应在 task 报告、docs 或数据产物中保存，TODO_LOG 只链接。数字以带来源/版本的实际产物为准；原始台账若有更正，保留当时记录并明确新的适用范围。

<details>
<summary>历史文档地图：截至 2026-09-17（保留职责与来源索引）</summary>

以下“在役”“最新”“唯一执行计划”等措辞描述当时状态，当前阅读顺序以上方为准。未被显式取代的具体数据质量和授权限制继续保留，不将历史任务自动全部重开。

# 文档地图

本仓文档分四类：目标与提案、执行计划、派单与验收、决策与约定。下面逐份写清职责、权威级别和状态。

**权威级别**的用法：同一件事如果两处说法不同，以级别高者为准，级别相同者以更新者为准。

- **权威**：这件事的唯一出处，改它要 owner 裁定。
- **执行**：当前照它干活的文件，内容随执行更新。
- **台账**：记录当时的判断和证据，发现错了追加更正段，不改写原文。
- **参考**：背景材料，不约束执行。

## 先读这几份

| 顺序 | 文件 | 一句话职责 | 级别 |
| --- | --- | --- | --- |
| 1 | [../CONTEXT.md](../CONTEXT.md) | 项目是什么、专用词怎么定义、哪个目录装什么 | 权威 |
| 2 | [目标.md](目标.md) | 我们到底要做什么：三层终点、为什么选制药域、六步路线、一处待裁的目标冲突 | 权威 |
| 3 | [计划_全工具远程跑批与结果库.md](计划_全工具远程跑批与结果库.md) | 当前怎么干：十五条已定口径、十步分工、停机条件、执行进度 | 执行 |
| 4 | [任务清单.md](任务清单.md) | 现在该做什么：全部在飞的活收成一张可勾选清单，每条标执行人、判据、出处 | 执行 |

仓根另有一份 [../README.md](../README.md)，是 clone 下来第一个看到的文件：项目是什么、怎么跑起来、当前交付到哪、下一步做什么、哪些事在等 owner。它不重复上面三份的内容，只做入口。

## 目标与研究

| 文件 | 一句话职责 | 级别 | 状态 |
| --- | --- | --- | --- |
| [目标.md](目标.md) | 项目目标文档。三层终点、为什么选制药域、六步路线、当前到哪一步、与 Proposal 和计划文档的分工 | 权威 | 在役 |
| [Proposal.md](Proposal.md) | 技术论证全文。问题的三层拆解、研究现状对照、六步的具体设计与判分公式、期望结论、token 预算、参考文献 | 权威 | 在役。§3.7 的训练口径原与 ADR-0028 冲突，**已于 2026-09-13 由 [ADR-0006](adr/0006-training-goal-decision-usability.md) 裁定**：训练目标以 ADR-0028 D1「判断这组预测能不能用」为准，§3.7 仅这一处被覆盖（已就地加横幅），其余 §1–§3.3 评测层、§2 文献表、§3.4 判分损失式仍有效。开头有两个 H1，行 1 是旧题，行 3 是现题，属清理候选 |
| [inherited/README.md](inherited/README.md) | 继承决议链的目录说明：9 个文件装什么、三份提案是什么关系、七步稿为什么是 ADR-0028 要求的 v2、它自己还缺哪两处 | 台账 | 在役。目录本身是 2026-09-13 从未跟踪的 `archive/governance/` 逐字复制入库的，md5 与原件核过一致 |
| [inherited/0028-calibration管线v1四方评审裁决.md](inherited/0028-calibration管线v1四方评审裁决.md) | ADR-0028 原文。四方评审裁决：D1 主任务换轨、D2 五条训练前置检验、D3 作废清单、D4 保留清单、D5 叙事重定位 | 台账 | Accepted，未作废。与 [Proposal.md](Proposal.md) §3.7 的冲突**已于 2026-09-13 由 [ADR-0006](adr/0006-training-goal-decision-usability.md) 裁定：维持 D1，§3.7 判为未吸收的回退** |
| [inherited/提案_20260902_七步v2_未入库稿.md](inherited/提案_20260902_七步v2_未入库稿.md) | 七步设计稿，283 行。Step 2 核查实验值、Step 4 排除假信号、Step 6 改权重、Step 7 换工具换领域 | 台账 | 从未入过 git。它就是 ADR-0028 触发动作 1 要求补写的 v2，九条依据见 [inherited/README.md](inherited/README.md)。**ADR-0006 裁定以它为权威骨架**，待并入缺的 D2-⑤ pass@k 与 §3.4.1 方差分解推导 |
| [inherited/提案_20260901_含3.4训练闭环_入库版.md](inherited/提案_20260901_含3.4训练闭环_入库版.md) | 提案入库版，358 行。§3.4 训练闭环含 §3.4.1 方差分解二难与 §3.4.6 三条前置检验，是 D1 那条推导的唯一完整出处 | 台账 | git HEAD 在，owner 已在工作区删除且删除未提交。复制进 inherited 是为了删除提交后仍读得到 |
| [inherited/讨论_20260901_工具冲突诊断题型的三条前置检验.md](inherited/讨论_20260901_工具冲突诊断题型的三条前置检验.md) | 三条前置检验的原案与判分设计讨论。文首与文末都写明「不是决议，是否立项须 owner 裁定」 | 台账 | 同上，git HEAD 在、工作区已删 |
| [inherited/调研_20260902_方向7_工具遮蔽与风险覆盖曲线.md](inherited/调研_20260902_方向7_工具遮蔽与风险覆盖曲线.md) | 弃答代价 c 的机械锚推导，把有效区间锁成 (0.157, 1.59]；AURC 定义与逐档报告口径 | 台账 | 从未入过 git。`docs/目标.md` 判分口径一节里 c 的有效区间出自这里 |
| [inherited/review_20260902_calibration_pipeline/](inherited/review_20260902_calibration_pipeline/) | ADR-0028 的四份评审原文：方法论、文献、RL 稳定性、devil's advocate，共 621 行 | 台账 | 从未入过 git |

## 执行与计划

| 文件 | 一句话职责 | 级别 | 状态 |
| --- | --- | --- | --- |
| [计划_全工具远程跑批与结果库.md](计划_全工具远程跑批与结果库.md) | 当前唯一的执行计划。开头「执行进度」段记 Step 完成情况和实测数字，§二 十五条口径，§四 十步分工，§五 错误台账，§七 停机条件 | 执行 | 在役，2026-09-11 owner 确认 |
| [任务清单.md](任务清单.md) | 全部在飞的活收成一张可勾选清单，按第一层、第二层、第三批工具、可重复性缺口、第三层与训练闭环分组。每条标执行人、判据与出处，末尾「清理候选」一节列会让新读者迷路的对象 | 执行 | 在役，2026-09-13 新建。与计划文档冲突时以计划文档为准，本文件只是索引与状态 |
| [../task/task46-open-source-composition/report.md](../task/task46-open-source-composition/report.md) | 动态Jev开源复用、PTRB-v0 benchmark、B0–B8 baseline 与组件组合证据 | 执行 | 2026-09-24 新增；规格与审查完成，真实学习未开始 |
| [远程跑批任务_全部工具.md](远程跑批任务_全部工具.md) | 给远程工程师的派单，自足，读完不需要别的资料。前半是各工具装法与背景，末尾「第二轮批次总派发」一节是 Step 2 的判据规格与现行执行顺序，**以该节为准** | 执行 | 在役 |
| [远程跑批任务_本地装不上的工具.md](远程跑批任务_本地装不上的工具.md) | 早期派单，只含 10 个难装工具 | 台账 | 已被上一份取代，保留作历史。清理候选 |
| [Step1_Issue05_运行时建设进展报告.md](Step1_Issue05_运行时建设进展报告.md) | 2026-09-10 运行时建设收口报告，22 个逻辑工具的资格结论 | 台账 | 内容已被计划文档的 Step 1 工具总账与 `LEDGER.tsv` 取代，仓内零引用。清理候选 |
| [星云A800远程执行手册.md](星云A800远程执行手册.md) | 在星云 A800 队列上跑代码的操作手册，自足。十节：卡型澄清（A800 非 A100）、两种执行形态怎么选、代码进容器的三种模式与默认打包陷阱、cluster.json 三键必填、worker_count 语义、镜像白名单、OSS 挂载、完整提交流程、七个实测坑与三条红线、五项待办。每条断言标了出处（实测／源码／文档） | 执行 | 在役，2026-09-21 新建。配套 CLI `tools/nebula/neb`、skill `tools/nebula/skill/SKILL.md`、回归测试 `tools/nebula/tests/`。定为「执行」而非「权威」：平台侧事实（队列配额、algo_name 白名单、CLI 默认行为）会随平台版本变，这份要能随实测更新，不必每次走 owner 裁定。**尚未有一次成功的真实提交**，第十节列明五项待办 |

## 调研与清单

| 文件 | 一句话职责 | 级别 | 状态 |
| --- | --- | --- | --- |
| [Pharma_Tools_Survey.md](Pharma_Tools_Survey.md) | 制药预测工具调研 v4，十二类共 185 条（✓ 入口存活 139、△ 受限或待补测 46、出局 12 移附录五），九列固定表头，含权威性分档 A/B/C/V。覆盖面比本项目用得到的宽，末两类是 agent 框架与权威数据库 | 台账 | 在役参考。核验边界写在开头：核过的是「文献真实存在」与「获取入口存活」，经过功能实跑的为 0，✓ 只表示能拿到 |
| [step1_tool_inventory_v2.csv](step1_tool_inventory_v2.csv) | 按 ADR-0002 的 schema 重做的工具表，42 个工具一行一个（含版本），输出性质类型逐点列 | 台账 | 在役参考。工具清单的权威已转移到 `data/tool_property_matrix/LEDGER.tsv` |
| [step1_tool_inventory.csv](step1_tool_inventory.csv) | v1 工具表，181 条 | 台账 | 已被 v2 取代。清理候选 |

## 决策与约定

| 文件 | 一句话职责 | 级别 | 状态 |
| --- | --- | --- | --- |
| [adr/README.md](adr/README.md) | ADR 索引，一条一行 | 权威 | 在役，新增 ADR 必须同步 |
| [adr/0001-tool-identity-version-lineages.md](adr/0001-tool-identity-version-lineages.md) | 工具同一性以权重 + 训练数据 + 端点定义为准，重训版本算两个工具 | 权威 | Accepted |
| [adr/0002-tool-table-schema.md](adr/0002-tool-table-schema.md) | Step 1 工具表 schema：一行一工具、输出性质类型必填、可用性作主表门槛 | 权威 | Accepted。文中写的 `docs/Pharma_Tools_Survey_v2.md` 这个文件名从未存在过，现名是 `Pharma_Tools_Survey.md`，属清理候选 |
| [adr/0003-step1-verification-protocol.md](adr/0003-step1-verification-protocol.md) | 核验分 L3/W/V 三档、值列+状态列+原因列三件套、失败重试边界；文末 2026-09-04 修订节把 L3 准入从「逐位一致」松绑为「标签跨轮稳定」 | 权威 | Accepted。Step 10 已补 ADR-0004/0005，文末修订节已改指向 ADR-0004 |
| [adr/0004-verification-label-level.md](adr/0004-verification-label-level.md) | 核验准入判标签级，概率漂移只记账不拦截，重复轮次从 2 提到 8 | 权威 | Accepted，2026-09-13 补立，取代 ADR-0003 §一 L3 准入第 3 条 |
| [adr/0005-endpoint-level-smoke.md](adr/0005-endpoint-level-smoke.md) | 冒烟判据改端点级，正式阶段一次性不重试，判据改版后整批重跑 | 权威 | Accepted，2026-09-13 补立。**代码部分已于 2026-09-13 落地**：两个控制层四处旧分子级判据改为端点级，共享函数 `smoke_endpoints_without_values()`，落地细节见其「当前代码状态」一节 |
| [adr/0006-training-goal-decision-usability.md](adr/0006-training-goal-decision-usability.md) | 训练目标裁定：可训练主任务为「判断这组预测能不能用」（α_i 驱动）+ 选择性作答，选工具偏心降为评测目标；维持 ADR-0028 D1，判 `Proposal.md` §3.7 为未吸收的回退 | 权威 | Accepted，2026-09-13 立（owner 授权代裁）。GPU 闸门保持关闭，须先过 ADR-0028 D2 五条零机时检验 |
| [agents/domain.md](agents/domain.md) | agent 进仓先读哪些领域文档、怎么用词汇表、怎么标 ADR 冲突 | 权威 | 在役 |
| [agents/issue-tracker.md](agents/issue-tracker.md) | 工单怎么落盘：`.scratch/<feature-slug>/`，一单一文件，`Status:` 行记状态 | 权威 | 在役 |

## 治理文档（governance/）

命名规则：`进展_` 是远程交回的验收报告，`交接_` 是接手清单，`现状_` 是某一时点的全局状态汇总。三类都是台账级，记当时的判断与证据，发现错了追加更正段，不改写原文。

| 文件 | 一句话职责 | 级别 | 状态 |
| --- | --- | --- | --- |
| [../governance/进展_20260909_十个药物预测工具安装与批量预测准备.md](../governance/进展_20260909_十个药物预测工具安装与批量预测准备.md) | 远程第一组 10 个工具的安装验收：5 个装成冻结 42 个端点，5 个诚实失败带 receipt，控制层 13 项单元测试通过 | 台账 | 在役 |
| [../governance/进展_20260911_正式跑批控制层与三工具冒烟验收.md](../governance/进展_20260911_正式跑批控制层与三工具冒烟验收.md) | 730 分子输入冻结、控制层加固、三工具冒烟验收：CYPstrate 54/54、TRIDENT 27/27、CypReact 因上游拒绝蔗糖被闸门正确拦下 | 台账 | 在役 |
| [../governance/交接_20260912_首批正式跑批完成状态与后续TODO.md](../governance/交接_20260912_首批正式跑批完成状态与后续TODO.md) | 首批十工具闭账后的接手清单：8 工具 38 端点双轮交付的终态、2 工具失败闭账的现场保留规则、P0 到 P6 七档 TODO、已知风险 | 台账 | 在役，最新 |
| [../governance/现状_20260912_任务全景_目标与当前位置.md](../governance/现状_20260912_任务全景_目标与当前位置.md) | 远程写的全局状态汇总：目标权威链、五阶段 pipeline 进度、工具面扩建、旧线硬结论、待裁清单、下一步 | 台账 | 在役，但**与 [目标.md](目标.md) 职能重叠**，且引用的文件大面积断链。见下方两点说明 |

**职能重叠。** 这份「现状」文档的 §1 讲目标、§6 讲下一步，与 `目标.md` 和计划文档的职责重复。两份对训练目标的口径也不同：它按 ADR-0028 写「判断这组预测能不能拿去做决策」，`目标.md` 按 `docs/Proposal.md` 写「纠正选工具偏心」。差异登记在 [目标.md](目标.md) 的「一处待裁的目标冲突」，裁定前两份并存，读者以 `目标.md` 为准，因为它带冲突标注。

**引用断链。** 它引用的 `governance/DECISIONS.md`、`CLAUDE.md`、`goal.md`、`BENCH_SPEC.md`、`USER_TODO.md` 等，在 owner 的未提交删除里已搬进 `archive/`，从仓根按相对路径都读不到。要读原文得去 `archive/` 下找，或用 `git show HEAD:<路径>` 取已跟踪版本。本文档不修正它的引用，只登记事实。

`governance/` 同目录下其余文件是上一版项目的，已被 owner 删进 `archive/governance/`，删除动作尚未提交。

## 数据产物在哪读

文档里出现的每个数字都应能指到下面某个文件。

| 文件 | 装什么 |
| --- | --- |
| `data/tool_property_matrix/LEDGER.tsv` | 工具清单的唯一权威，49 行，一个工具一行 |
| `data/tool_property_matrix/matrix.tsv` | 工具×性质矩阵，49 个工具列 + 1 列性质族名，45 个性质族行 + 9 行元信息（调用方式、机构、方法、许可证、安装方式、权重、实测状态、风险、覆盖性质族数） |
| `data/tool_property_matrix/property_pairings.json` | 性质配对表，16 族，56 行来源每行带官方定义原文 + DOI 或官方页 + 仓内证据路径，另有 14 行核到出处但判定不可比并写明理由 |
| `data/tool_property_matrix/DISPOSITION.json` | 工具的删除与缺陷处置记录，含理由 |
| `data/tool_property_matrix/evidence/` | 配对表的 28 个证据文件 |
| `data/tool_results/manifest.json` | 结果库清单，逐文件 sha256、字节数、列数 |
| `data/tool_results/molecule_pool.csv` | 冻结的 730 分子输入 |
| `data/tool_results/divergence/summary.json` | 14 族的分歧汇总，含 `family_ties`、`tolerance_pending`、`conflict_status` |
| `data/tool_results/divergence/not_compared.json` | 不进比较的族及原因与解决路径 |
| `data/tool_results/formal_campaigns/` | 首批正式跑批交付物，8 个工具目录各三件（`predictions.csv`、`manifest.json`、`verification.json`），共 24 个跟踪文件。两个 campaign：`drug_tools_20260908` 装 cypstrate、trident、cyplebrity、bayesherg；`numeric_qualification_formal_20260911` 装 kowwin、openadmet、silicos_it_filterit、rdkit_sa_score。8 工具 38 端点，730 分子双轮。端点数按列数核：每端点 8 列，即两轮各 value、label、status、error 四列，(列数 − 2) / 8 得 18、9、5、1、1、1、1、2，合计 38 |
| `data/tool_results/reference_comparison.{csv,json}` | 重叠参考端点对照：64 组映射、128 条比较序列、每序列 730 分子、明细 93,440 行、93,340 对有限数值、100 行非 ok。json 的 `excluded_candidates` 列 7 条核过但判定不可比的候选，每条带理由，如标签与概率不同刻度、LogD 与 logP 不是一回事 |
| `data/tool_property_matrix/tools.json` | 工具族归属，49 条，与 `LEDGER.tsv` 49 行一一对应 |
| `data/tool_property_matrix/AUDIT.json` | 矩阵自检报告，2026-09-12 核过：49 工具 × 45 性质族，TSV 55 行 × 50 列，227 个归属 |
| `data/tool_tables/` | 交付表 15 份主表 + 2 份失败清单 |
| `data/bbb_census_v3/PROVENANCE.md` | BBB 普查产物从 archive 搬出的溯源与逐项哈希校验 |
| `tools/step8_pairing_evidence/logs/` | 配对表核验日志 408 条、Step 9 核验日志 18 条 |

## 写文档的规矩

1. 同一件事只在一处写权威版本，别处引用不复制。目标写在 `目标.md`，怎么干写在计划文档，为什么这么判写在 ADR。
2. 决策一事一 ADR，编号递增，同步进 `adr/README.md` 索引。被取代的标 `Superseded by NNNN`。
3. 台账类文档发现错了，追加更正段并写明改了什么、为什么，不改写原文。计划文档 §五 和 Step 8 段落就是这个做法的样例。
4. 执行进度写进计划文档开头的「执行进度」段，不另开进度文件。
5. 生成的产物不写时间戳，同一份代码连跑两次产物 md5 必须一致。
6. 数字要能指到文件。不用「预计」「应该」这类措辞，计划文档 Step 7 的完成判据就是这么写的。
7. 判据要改先停下交 owner，四条停机条件见计划文档 §七。
8. 文档名用中文，ADR 文件名用英文 slug，与现有命名保持一致。

</details>
