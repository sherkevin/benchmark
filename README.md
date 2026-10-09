# benchmark — 医药工具选择与在线学习

本项目研究一个具体问题：给定分子、性质和候选预测工具，判别式选择器调用一个工具；得到这次调用的对错反馈后，及时更新后续选择。论文方法名为 **MIRA-Relay**，历史实现标识为 `mira_context_reliability`；JEV 只作为历史功能参照，不作为论文方法名。主线以 [ADR-0021](docs/adr/0021-pharma-first-direct-selection-benchmark.md) 和当前生效研究文档为准；复核、拒答和长轨迹是后续扩展。

## 当前：8 页主版已编译，声明范围内 H7 回放闭环已通过

截至 2026-10-08，当前投稿切片使用公开 TDC BBB truth、四个 checkpoint-bound fresh providers、157 个 scaffold-disjoint confirmation molecules、一次调用、selected-only delayed feedback 和延迟 $D\in\{0,1,4\}$。自然 fresh 上 MIRA-Relay 与 Frozen 均为 89.81%；controlled correctness-drift stress 上 MIRA-Relay 为 87.56%，Frozen 为 73.38%，提升 14.18 pp。Stage-B gate v3 在这一声明范围内通过；hERG/AMES、post-call verifier/refusal、自然 lifecycle drift 和完整 33-provider model-fresh 仍是扩展。

新增的 [128-order完整重放](task/task49-pharma-benchmark-baseline-freeze/primary_arrival_robustness_v1/report.md)已独立核验：受控MIRA−Online SGD平均+4.736pp，条件Monte Carlo均值区间[+4.317,+5.156]，125/128顺序胜出；自然MIRA−Frozen平均−.287pp，区间[−.372,−.202]。上段89.81%和+14.18pp特指原始顺序；新结果条件于同一已检查队列、固定prior/策略seed，不能作新scaffold泛化。新旧点估计已合入唯一主版的同一主表，完整科学gate仍HOLD。

最新稿件正文恰好 8 页、参考文献从第 9 页开始，稳定主版是 [paper/aamas2027/main.pdf](paper/aamas2027/main.pdf)，对应源码和提升记录见 [论文目录](paper/aamas2027/README.md) 与 `main_provenance.json`。声明范围的构建预检已通过；Benchmark/Baseline Gate A/B/C 可进入联合复核，H7 已达到 `PASS_FOR_DECLARED_POLICY_REPLAY`，但**三份完整科学标准对 broader scope 仍 HOLD**。[六格实验](task/task49-pharma-benchmark-baseline-freeze/calibration_centering_v1/report.md)发现分数修正有效，但新中心化未到升级线；[四工具诊断](task/task49-pharma-benchmark-baseline-freeze/headroom_feedback_v1/report.md)确认104/722题全工具共同出错、Corrected Frozen还有39题可救机会，错误多数已有正确候选反馈。进一步的[context残差干预](task/task49-pharma-benchmark-baseline-freeze/contextual_residual_v1/report.md)自然/受控均未超过原Q，不升级。[四工具学习型prior对照](task/task49-pharma-benchmark-baseline-freeze/fresh_learned_prior_v1/report.md)已完成20次A800训练和独立核验：BCE为78.53/78.95%，BT为78.81/78.25%，均低于Corrected Frozen的80.19%，不升级。[两组表示对照](task/task49-pharma-benchmark-baseline-freeze/fresh_representation_v1/report.md)已完成并独立核验，1024两seed为77.84/79.09%，仍未过双对照线，停止此固定增维构造；旧 confirmation 保持只读。

本轮复核撤回的 Shared-Credit 机制解读仍只作为历史记录保留。新的 matched cyclic control 已固定 cold-start action，只改变 released residual 的 destination；在声明的 evaluator-side correctness drift 下，Local 相对 permutation 的 paired contrast 为 +15.67 pp，natural stream 的区间跨 0。fresh receipt closure v1 又补齐了 terminal/final-state、checkpoint suffix、logical-clock reorder、provenance 和 fail-closed mutation 证据；M14/H7 只在 declared policy-replay 范围内升级，不能外推 natural drift、family sharing、verification/refusal 或 agent gain。

不能把 33 个相关模型当独立来源，也不能把受控 stress 结果写成自然部署漂移或 generic online gain。完整数字、证据边界、论文贡献定位和历史工作的取舍见下方主线文档。

## 从这里读

1. [研究主线文档](docs/研究主线/README.md)：三份唯一生效的研究文档和三份对应评价标准。
2. [决议文件](docs/决议/README.md)：只保存双方确认的小取舍、约束和争议裁定。
3. [CONTEXT](CONTEXT.md)：术语、目录职责和当前信息边界。
4. [任务清单](docs/任务清单.md)：未完成工作、依赖和验收标准。
5. [文档地图](docs/README.md)与 [TODO_LOG](TODO_LOG.md)：资料职责与执行记录。
6. [Task49 执行计划](task/task49-pharma-benchmark-baseline-freeze/next_plan.md)：实验卡和运行顺序。
7. [论文草稿](paper/aamas2027/README.md)：官方 LaTeX 模板、题目、摘要和可查看 PDF（当前正文严格 8 页）。

当前按 [研究迭代规范](docs/研究迭代规范.md) 推进：故事线 → 候选创新 → 连贯方法 → 审查 → 小批真实数据验证。Owner 已授权持续研究和 Nebula A800 实验，见 [ADR-0022](docs/adr/0022-continuous-aamas-research-loop.md)。每批先固定主张、近邻、预算与否定条件；数据资格决定结论范围：722 个旧 BBB 分子只作开发；已经封存并读过的 157 个 fresh confirmation 分子不能重新充当新候选的未读确认。

开源复用沿用 [ADR-0013](docs/adr/0013-open-source-first-composition.md)。当前优先复用 Laya、现代在线路由方法和 Task36 回放；以 [baseline 合同](task/task49-pharma-benchmark-baseline-freeze/baseline_adapters_v0.md) 记录同任务改动。旧 M2/M3 复核门实验保留为历史证据，不承担当前直接选择的方法效果。

## 可复核的最近交付

- [数据合同](task/task49-pharma-benchmark-baseline-freeze/data_contract_v0.md)：真值、provider 粒度、缺失、池外候选及独立复核。
- [方法审查卡](task/task49-pharma-benchmark-baseline-freeze/method_card_v0.md)：固定写入/可学读取的限制，及下一轮表示瓶颈诊断。
- [H7 receipt closure](task/task49-pharma-benchmark-baseline-freeze/fresh_receipt_closure_v1/report.md)：两组 90-stream canonical replay、provenance、restart/reorder 与 fail-closed evidence。
- [Task36 回放报告](task/task36-pharma-provider-replay/report.json)：只暴露已选模型输出、终止后判分；103 条定向测试，47,450 个快照格核对。不是操作系统沙箱，也不是训练效果。
- [模型粒度审计](data/tool_results/divergence/pharma_tool_granularity_audit_v2.json)：家族票与具体模型的区别，含事后可救错误集合。
- [候选资格说明](task/task36-pharma-provider-replay/qualification_notes.json)：标签翻转、真值与模型训练重叠的已知/未知边界。

本机现有环境可运行定向测试，不安装新依赖：

```bash
.srcprobe/beir_venv/bin/python -B -m pytest -q -p no:cacheprovider \
  src/probe_headless/tests/test_pharma_bbb_replay.py \
  src/probe_headless/tests/test_t32_history_isolation.py \
  src/probe_headless/tests/test_t34_isolated_calibration.py
```

该环境不是已发布的可移植训练环境。全仓历史测试、Git 未收口与远端代码/数据交付仍有待办，见任务清单。新预测、安装、远端提交和修改判据继续遵守现有授权边界；不删除旧现场，不覆盖失败结果。

<details>
<summary>历史入口快照：2026-09-13 至 09-17（不代表当前进度与执行顺序）</summary>

以下原文保留供追溯。其中“当前”“下一步”、测试总数及单线交接口径只指当时；本页上方与当前主线文档已给出更新，不照历史段落重新开工。

# benchmark — 制药工具冲突：评测 agent 能不能发现「工具正常返回但结果不可靠」

本仓在造一套基准。要测的失败模式很具体：多个制药性质预测工具都正常返回、格式也对、数值看着也合理，但对同一个分子给出互相矛盾的结论，而 agent 察觉不到，还照着其中一个继续往下做决策。

做法是让多个公开发表的工具对同一批 730 个分子各自预测，把工具之间互相矛盾的分子挑出来——矛盾说明工具在这个分子上不可靠，这种分子才是好考题。一个性质要能出题至少需要两个不同来源的工具，因为一个工具无法与自己矛盾。

配套的训练目标是让 agent 学会**判断「这组预测能不能拿去做决策」**，并据此选择性作答（作答/弃答/上报）。「把对熟悉工具的偏心纠回来 / 在多个工具里选哪个」保留作**评测**目标，不进入训练环。**这一训练口径原与一份已生效的决议（ADR-0028 D1）冲突，已于 2026-09-13 由 [ADR-0006](docs/adr/0006-training-goal-decision-usability.md) 裁定**，见下文「训练目标冲突的裁定」。冲突不影响当前交付，GPU 闸门仍保持关闭。

## 这个仓现在归谁、你从这里开始

本项目自 2026-09-13 起整体移交远程工程师，你是长期唯一执行者。这份 README 是你的入口，读完它你应当知道：项目要做什么、当前交付到哪、怎么把环境跑起来、下一步做什么、哪些事必须先问 owner。

**工具的安装与预测运行全部归你。** 包括本地此前已跑通的工具也要在你的机器上独立部署验证，本地跑出的结果只作对照用的历史输入，不替代你的部署——这样整套结果在一台机器上就能完整复现。

owner 那一侧只做四类事：文档与清单、判据规格、分析脚本、结果整形。四类都不调用任何预测工具、不产生新预测值。牵涉运行环境验证的代码改动也归你，因为判据改得对不对必须拿真实批次验证，而真实批次只在你的机器上。

**按这个顺序读：**

1. [CONTEXT.md](CONTEXT.md)：项目专用词的定义与目录职责。写文档、起工单、命名变量时用它的词，不要漂移到同义词。
2. [docs/目标.md](docs/目标.md)：我们到底要做什么。三层终点、为什么选制药域、六步路线、训练目标冲突的裁定（ADR-0006）、当前到哪一步。
3. [docs/计划_全工具远程跑批与结果库.md](docs/计划_全工具远程跑批与结果库.md)：当前怎么干。开头「执行进度」段记 Step 完成情况与实测数字，§二 十五条已定口径，§四 十步分工，§七 四条停机条件。
4. [docs/远程跑批任务_全部工具.md](docs/远程跑批任务_全部工具.md)：给你的派单，自足。前半是各工具装法与背景，末尾「第二轮批次总派发」一节是现行判据规格与执行顺序，**以该节为准**。

[docs/README.md](docs/README.md) 是全部文档的地图，标了每份的职责、权威级别与状态。同一件事两处说法不同时，以级别高者为准，级别相同者以更新者为准。

## 终点分三层，本轮只交第一层

三层递进，每层有自己的验收，不能混着做。

**第一层，离线结果库。** 清单内全部工具在你的机器上独立部署并验证，预测结果、失败记录、版本信息完整留存，每工具交一份 CSV。验收口径：每个工具要么有双轮全量结果，要么有可核对的失败回执，不存在无归属的工具；每份表的分子集合与冻结输入一致；主表每一个空格在失败清单里有且仅有一条对应记录。

**第二层，工具冲突数据集。** 在第一层之上确认哪些工具测的是同一种性质，再挑出答案不一致的分子。这一层只报「工具互相不一致」，不判谁对谁错。判对错要接实验真值，那是 Step 2 真值清洗的活。

**第三层，可评测 agent 的 benchmark。** 题目、agent 可用工具集、标准答案、评分方法另立设计。第一二层是它的数据地基，不是它本身。

为什么必须分层：第三层一旦提前设计，就会反过来决定第一层收集什么数据，结果是数据集变成题目的附庸。现在先把工具的真实输出原样存住，第三层才有选择余地。

## 当前状态

第一层进行中。计划文档的十步里，本地已完成 Step 1、3、8、9，Step 2 规格已交付你实现，Step 4 到 7 全部在你这边，Step 10 的文档部分已完成、代码部分未做。逐步的实测数字与证据见 [docs/计划_全工具远程跑批与结果库.md](docs/计划_全工具远程跑批与结果库.md) 开头的「执行进度」段，这里只给总数。

| 项 | 数 | 出处 |
| --- | --- | --- |
| 工具总账 | 49 行，归属线分布为跑批线 13、安装线 23、接口线 5、待裁定 5、缺陷记录 3 | `data/tool_property_matrix/LEDGER.tsv` |
| 工具×性质矩阵 | 49 工具 × 45 性质族，TSV 55 行 × 50 列，227 个归属 | `data/tool_property_matrix/AUDIT.json`，2026-09-12 核过 |
| 冻结输入 | 730 分子，行序冻结不得重排，`entity_id` 形如 `z000282` 是全仓对账主键 | `data/tool_results/molecule_pool.csv` |
| 首批正式跑批 | 10 工具闭账：8 工具 38 端点双轮全量交付，2 工具共 12 端点失败闭账。**第三批更正（2026-09-17）**：JPlogP v2 已独立验收并加入正式交付，根 manifest 现为 **9 工具 39 端点**；同批 XLogP3 未取得合格交付、SSL-GCN 阻断未产出，三者不得一并标完成 | `data/tool_results/formal_campaigns/` + `manifest.json`，验收见 `governance/进展_20260913_第三批JPlogP_XLogP3_SSLGCN独立验收裁定.md` |
| 交付表 | 15 份主表 + 2 份失败清单，290 个空格对 290 条失败记录 | `data/tool_tables/`，2026-09-13 实测复核一致 |
| 性质配对表 | 16 族，56 行来源每行带官方定义原文 + DOI 或官方页 + 仓内证据路径，另有 14 行核到出处但判定不可比并写明理由 | `data/tool_property_matrix/property_pairings.json` |
| 分歧分析 | 14 族产出比较，2 族不比较且都打印原因 | `data/tool_results/divergence/summary.json` |
| 重叠端点对照 | 64 组映射、128 条比较序列、明细 93,440 行 | `data/tool_results/reference_comparison.{csv,json}` |
| 测试 | 229 条收集，228 过、1 跳过 | 2026-09-15 在 Linux 实测；此前 macOS 实测有一条路径缺陷会失败，Linux 上不出现，见下文。**2026-09-17 更正**：macOS 实测是 10 条失败、三个根因（非一条）；Linux 因 09-16 环境清空事故应为 227 passed / 1 failed / 1 skipped，见下文更正段 |

**已计分的 9 族分歧数**，分母都是 730：血脑屏障 222、水溶解度 241、心脏毒性 hERG 303、致突变 AMES 364、肝毒性 DILI 424、P-gp 抑制 223、P-gp 底物 236、致癌性 345、皮肤致敏 509。（2026-09-13 按 step9_deferred 第一条重算：`to_label()` 改读来源自带 vocabulary 后，hERG 补入 FPADMET 列从 111 变 303，P-gp 底物从凑不出比较变 236，复核过程见计划文档执行进度段。）

**这些数字现在只能读作「工具互相不一致」，不能读作「某个工具错了」。** 判对错要等 Step 2 把实验真值接上。

另有 5 个连续值族（亲脂性 logP、极性表面积 TPSA、类药性 QED、Caco-2 渗透、血浆蛋白结合）容差待定，分歧数写 `null` 而不是 `0`。**这个 null 不表示工具意见一致**，只表示没拿到官方容差因此分歧不可判定。2 个不比较的族：人肠吸收 HIA 与急性毒性 LD50 进待核实区（P-gp 底物原先因 FPADMET 词汇读不出而跳过，vocabulary 修复后已进入比较）。

## 环境

Python 3.10 起即可，不需要 3.12。依赖见 [requirements.txt](requirements.txt)，钉了版本，注释里写了为什么钉：`mcp` 2.0.0 删掉了 `mcp.server.fastmcp`，而本仓的 MCP server 文件都 `from mcp.server.fastmcp import FastMCP`，不钉 `<2` 今天装到的是跑不起来的版本。

```bash
pip install -r requirements.txt
```

跑测试：

```bash
python -m pytest -q src/probe_headless/tests/
```

2026-09-15 在 Linux 实测 229 条收集、228 过、1 跳过（含 ADR-0005 端点级冒烟新增 4 条、step9 vocabulary 修复新增 10 条、D2-① BBB 方差分解新增 11 条、D2-② 恒答基线新增 14 条、裸答难度交叉新增 20 条）。同一条命令在 macOS 上会多一条失败：`tests/test_numeric_qualification_campaign.py::test_completed_pending_phase_is_published_without_rerunning_worker`，原因是 macOS 把临时目录写成 `/var/folders/...`，而 `pathlib.Path.resolve()` 会解析成软链真实路径 `/private/var/folders/...`，测试用字面路径比对两者就不相等。这是测试自身的平台缺陷，不是被测代码的问题，**在 Linux 上不会出现，仍待修以保跨平台全绿**。修法是比对前对两边都调 `.resolve()`，或者用 `Path.samefile()`。

**更正（2026-09-17）**：上段「在 macOS 上会多一条失败」不成立，当前口径以 [CONTEXT.md](CONTEXT.md)「已知测试缺陷 → 更正（2026-09-17）」为准。2026-09-17 在 macOS 实测全仓是 **10 failed / 218 passed / 1 skipped**，10 条分三个根因：8 条 `tests/test_jplogp_cdk_campaign.py` 是被测代码 `drug_prediction_campaign.py:60` 的 `process_start_ticks()` 只读 `/proc/<pid>/stat`、缺 Darwin 分支（仓内 `tool_conflict_census.py:753` 已有可复用的 `_darwin_process_start_ticks()`）；1 条是上段描述的软链路径缺陷（该条描述本身正确）；1 条 `tests/test_bbb_naked_difficulty_crossref.py::DeterminismTests::test_rerun_byte_identical` 是真实缺陷——task23 逐字节重放的原始出处 `runs_naked/` 已在 2026-09-16 环境清空事故中永久丢失，两个平台都红，处置待 owner 裁（见 problems.md PR-69）。上段「228 过」是 09-15 的数，当时 `runs_naked/` 尚在；事故后 Linux 应为 227 passed / 1 failed / 1 skipped，与 PR-69 登记一致。议题登记见 problems.md PR-70。

定向跑首批验收相关的三个测试文件，预期 67 passed：

```bash
python -m pytest -q \
  src/probe_headless/tests/test_drug_prediction_campaign.py \
  src/probe_headless/tests/test_verify_runtime.py \
  src/probe_headless/tests/test_numeric_qualification_campaign.py
```

2026-09-13 在 Linux 实测 67 passed（ADR-0005 端点级冒烟给这三个文件加了 4 条，从交接文档 §2.7 记的 63 增至 67；交接文档的「63 passed」是改判据前的数）。在 macOS 上这条命令会多出 1 条失败（66 passed 1 failed），失败的是下文那条路径缺陷测试，Linux 上不出现。数量对不上时先看测试清单差在哪，不要只接受「全绿」而忽略少收集了测试。

两个分析脚本都是固定路径、不接命令行参数，直接跑：

```bash
python src/probe_headless/divergence_analysis.py          # 重算分歧，写 data/tool_results/divergence/
python tools/step8_pairing_evidence/build_property_pairings.py  # 重建配对表，写 property_pairings.json
```

两个脚本原先把仓库根写死成 owner 的绝对路径，2026-09-13 改成按 `__file__` 解析，否则 clone 到别处直接跑不起来。改完实测产物逐字节一致：`divergence/` 下当时 15 个文件 md5 全部不变，配对表 md5 `f63a1002dfabb6914b26b280483e6c58` 不变。**确定性保证至今有效**：两个脚本各自连跑两次产物 md5 一致。但配对表的 md5 值本身随后因内容更新前移了一次——2026-09-13 step9 vocabulary 修复把 `to_label()` 那条延期项从 `step9_deferred` 移进 `step9_resolved`，配对表现为 `f1e9326f78e24fd788a7f2da82e8b7c1`（`families`、`same_as`、`threshold` 三块逐字节未变，只有 step9 元数据动了）；同一修复让 P-gp 底物从 skipped 转为进入比较，divergence/ 文件数从 15 增至 16。引用具体 md5 时以当前生成结果为准，不要拿历史值当现值比对。

**产物确定性是硬规矩**：生成的产物不写时间戳，同一份代码连跑两次产物 md5 必须一致。改任何生成器之前先存一份产物 md5，改完逐字节比。

## 下一步

按依赖排序，前两项零机时。完整清单与判据在 [docs/任务清单.md](docs/任务清单.md)。

1. **保护现场。** `.bench_runs/` 是唯一完整原始证据，被 gitignore 排除，git 里的三件套只保存结果与摘要。不要 `git clean`、不要删失败 attempt、不要在原 campaign 里重跑正式阶段。
2. **修那条 macOS 路径缺陷测试**，让全仓测试在你的机器上全绿，作为后续改动的安全网。
3. **改冒烟判据到端点级。** [ADR-0005](docs/adr/0005-endpoint-level-smoke.md) 立了判据但代码没改，三处旧判据的逐行位置登记在该 ADR 的「当前代码状态」一节。补两项测试，现有测试不回归，再在真实批次上复现一次「单分子失败仍可进全量」。
4. **Step 4 到 7**：派发第二轮批次、导出五个安装失败工具与 OCHEM 的证据包、结果收进结果库、总账与覆盖率结账。
5. **第三批工具单独接手。** JPlogP、XLogP3、SSL-GCN 的代码与运行现场都不在本机工作树，也没有纳入首批验收。见 [governance/交接_20260912_首批正式跑批完成状态与后续TODO.md](governance/交接_20260912_首批正式跑批完成状态与后续TODO.md) §3.5 与 P4。

第二批 12 个工具**须 owner 明确授权后才能启动**。CypReact 与 PKSmart 若要新实验，也须 owner 先裁定，任何改法都必须新建 campaign。

## 训练目标冲突的裁定

agent 要被训练去做的那件事，到底是「在多个工具里挑出更可靠的那个」，还是「判断这组预测能不能拿去做决策」——仓里一度两种口径并存。

**已于 2026-09-13 由 [ADR-0006](docs/adr/0006-training-goal-decision-usability.md) 裁定**（owner 授权「按照这个目标自己去裁决」）：可训练主任务为**「判断这组预测能不能拿去做决策」+ 选择性作答**，即维持 [ADR-0028](docs/inherited/0028-calibration管线v1四方评审裁决.md) D1；「选哪个工具 / 纠正选工具偏心」降为**评测**目标，不进入训练环。核心理由是方差分解：选最准工具由交互项 γ_ik 决定、绑定具体工具、换工具即失效，判断可用性由分子主效应 α_i 决定、换工具仍有效——「能迁移」与「有信号」对选工具任务在定义层互斥，而第三层 Step 6/7 的核心主张恰是迁移。`docs/Proposal.md` §3.7 判为未吸收的回退（它入库时 ADR-0028 只在未跟踪的 `archive/`、clone 不可见），补吸收，ADR-0028 不标 Superseded。

**冲突只落在第三层往后的训练闭环，不落在你当前要交的第一层与第二层。** 离线结果库与工具冲突数据集两条口径在任何一份文档里都一致，照常推进。**GPU 闸门保持关闭**：裁定后仍须先过 ADR-0028 D2 的五条零机时检验（顺序不可颠倒）才申请 GPU。

**D2 检验当前进度（全部在 BBB 族，2026-09-15）**：D2-① 前半完成（α_i 占 54.5%、p=0.001，[task21](task/task21-D2一方差分解BBB/report.md)），后半（标签噪声翻转率）需多源真值、归 Step 2；D2-② 恒答半完成（退化策略白嫖不了，c 区间 (0.0804,1.0]，[task22](task/task22-D2二恒答基线BBB/report.md)），裸答半 2026-09-01 已跑第一轮、判 INCONCLUSIVE 且 qwen3.7-max 恰踩作废线 0.80——交叉分析（[task23](task/task23-裸答难度交叉BBB/report.md)）量出该高分几乎全部来自「工具也答对」的易分子（96% vs 难分子 20%），不支持「任务作废」读法。

**题集构造要求已落（[ADR-0007](docs/adr/0007-benchmark-task-requirements.md)，2026-09-15）**：owner 给出两个量化口径——可训题量 ≥5000、易分子（tw≤1）占比 ≤30%——并裁定「要求先于现状」：ADR 落的是达成目标必须满足的 R1–R5 五组要求（现象/取舍/量级/可训练/防伪），现状不满足时以要求为准去达成。关键事实：裸答天花板是难度配比的函数（BBB 自然组成易 80.3% → 投影 0.870 撞穿作废线；易 30% → ~0.55 安全），故难度配比是显式设计要求。可行性已证：5000 题在「分子 × 工具子集」单位下仅 BBB 一族即可达（≈13k 名义题），但跨工具迁移仍需 Step 2 给高密度族接真值。U-8 由此重框为「按配比出题」，实质走选项 (B)，正式销项待 owner 点头；ADR 末尾另有三个待 owner 拍的数（难分子下限、伪共识种子数、选族优先级）。②未收口则③④⑤不得启动。

现行权威以七步稿 [docs/inherited/提案_20260902_七步v2_未入库稿.md](docs/inherited/提案_20260902_七步v2_未入库稿.md) 为骨架，待本地并入 §3.4.1 方差分解推导与 D2-⑤ pass@k 两处缺口（补完前不申请 GPU）。完整的提案血缘、时间线、ADR-0028 D1–D5、九条「七步稿即 v2」的依据，登记在 [docs/目标.md](docs/目标.md) 的「一处待裁的目标冲突」一节（已就地标注裁定结论）与 [docs/inherited/README.md](docs/inherited/README.md)。

## 撞上这四条就停下交 owner

不得自行推进，出处是 [docs/计划_全工具远程跑批与结果库.md](docs/计划_全工具远程跑批与结果库.md) §七。

1. 任何工具需要改动官方程序才能出数。
2. 任何配对核不到官方定义，但从名称看似应当配对。
3. 任何工具需要追加安装预算。
4. 任何已定判据需要再次修改。

另有一批数据口径红线，见 [governance/交接_20260912_首批正式跑批完成状态与后续TODO.md](governance/交接_20260912_首批正式跑批完成状态与后续TODO.md) §0：不平均两轮数值、不舍入来制造一致、不给失败行补值、不把日志措辞补造成模型输出、不通过重跑挑选成功结果、不把标签与概率或 LogD 与 logP 强行比较。

## 目录

逐目录的职责、状态与清理候选见 [CONTEXT.md](CONTEXT.md) 的「目录职责」一节。这里只给导航。

| 路径 | 装什么 |
| --- | --- |
| `docs/` | 目标、提案、执行计划、远程派单、工具调研、ADR、agent 约定、继承决议链 |
| `data/tool_results/` | 工具原样输出、`manifest.json`、分子池、分歧分析产物、正式跑批交付物、重叠端点对照 |
| `data/tool_tables/` | 交付表：15 份主表 + 2 份失败清单 |
| `data/tool_property_matrix/` | 工具总账、工具×性质矩阵、配对表、处置记录、自检报告、证据 |
| `data/bbb_census_v3/` | 早期 BBB 工具冲突普查产物，含预登记、封存件、12 份可重建回执 |
| `src/probe_headless/` | 活代码：分歧分析、交付表生成、两个跑批控制层、运行时核验、三个 BBB 分析脚本（方差分解 / 恒答基线 / 裸答难度交叉），`tests/` 下 13 个文件 |
| `tools/` | 生成器与核验脚本，含配对表生成器与核验日志 |
| `governance/` | 你交回的验收报告与状态汇总。命名规则：`进展_` 是验收报告，`交接_` 是接手清单，`现状_` 是某一时点的全局状态汇总 |
| `reference/`、`research/` | 外部基准的采样与阅读报告、专题调查报告，参考级 |
| `archive/` | 上一版项目的封存材料，1.7GB，git 未跟踪。其中与本项目训练目标相关的决议链已复制进 `docs/inherited/` |

`src/` 其余目录、`src/probe_headless/` 下的 413 个运行产物（`chain9_*`、`sweep*`、`sw_*`、`ws_*`、`err*`、`*.out`、`*.log`）都是上一版项目（六格过程量基准）的，与本项目无关。

## 文档规矩

完整八条见 [docs/README.md](docs/README.md) 末尾。挑最容易踩的四条：

1. 同一件事只在一处写权威版本，别处引用不复制。目标写在 `docs/目标.md`，怎么干写在计划文档，为什么这么判写在 ADR。
2. 决策一事一 ADR，进 `docs/adr/`，编号递增，同步进索引。被取代的标 `Superseded by NNNN`，不就地改写正文。
3. 台账类文档发现错了，追加更正段并写明改了什么、为什么，不改写原文。
4. 数字要能指到文件。不用「预计」「应该」这类措辞。

工单进 `.scratch/<feature-slug>/`，一单一文件，状态写在文件头的 `Status:` 行，约定见 [docs/agents/issue-tracker.md](docs/agents/issue-tracker.md)。

</details>
