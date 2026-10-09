# station45_report · task25 站4 零API基线 + 站5 预登记

> 角色：benchmark 仓库工程师 subagent（站4+站5）。零 API、零下载、零机时；不动冻结输入；不修改任何既有脚本；不 commit。
> 交付落点：`task/task25-pipeline小流量全链路演示/experiment/`（唯一允许落点）。

## 结论（先说答案）

1. **三条零成本基线全部落盘**（`baselines.json`），每个数字从冻结原始数据脚本复算、与既有冻结输出逐项核对一致：恒答基线在 722 分子 usable 池上算出 c 有效区间 **(0.0804, 1.0]**、恒答 y=1 期望损失 0.2867 / y=0 0.7133 / 一律弃答 = c / 参照多数票 0.0804；裸答投影（演示批 易2/中2/难2 配比）qwen3.7-max **~0.596（投影非实测）**；方差分解引用 α_i 54.5% vs β_k 0.56%（p≈0.001）。
2. **预登记已落盘**（`preregistration.json`，self_sha256 封存 `8f2f999c…`，落盘于任何计分 API run 之前；已绑定站1 sample.json md5 与 6 个 entity_id）：批次构成 ≤21 run（12 带工具 + 6 裸答 + 冒烟余额 2 = 实际最多 20）、判分口径 L 公式 + c 网格 {0.0804, 0.2, 0.5, 1.0} + 主报告 c=0.5、演示批成功/失败判据（**无统计显著性判据**）、task.md §4 四条停止条件、数据落盘路径约定、执行顺序（裸答批先行以喂 preflight 闸门6 的裸答留痕）。
3. **preflight 未对演示 case 实跑**——站2/3 产物（cases/ 题面+judge）尚未落盘，无 case_dir 可查（站1 sample.json 已到达）。已留占位记录 + 确切命令（`preflight_record.json`），并对存量 case `case_dr_qed_01` 做了一次零 API 可行性探测证明工具链可运行。**这是本站唯一未实跑项。**
4. **难分子计数双口径已登记不裁决**（baselines.json `hard_molecule_count_discrepancy`）：56（ADR-0007 台账，排除真值矛盾分子）vs 59（分歧 CSV 全量直算），差集 3 个分子实测**全部在 truth_unresolved 排除名单内**——差因是「是否排除真值矛盾分子」这一个口径开关，**与平票家族处理无关**（协调者假设已实测否证，单独登记）。裁决走 ADR-0009。
5. **两处口径疑问停下报告、不拍板**（见 §5）：ADR-0007 投影表官方数值（~0.55 / 0.295）无法用任何显式公式从落盘数据精确复算；「一律答可信/不可信」在 BBB 演示题面上尚无官方字段定义。

## 1. 产出清单（路径 + md5 前 8 位）

| 文件（experiment/ 下） | md5 | 说明 |
|---|---|---|
| `baselines.json` | `b223a795` | 三部分基线 + 难分子双口径节 + 6 分子 demo 子集恒答基线（sample.json 到达后已补算）；连跑两次字节一致 |
| `preregistration.json` | `bb92138c` | 闸6 预登记；self_sha256 `8f2f999c…`（`--check` 可验）；已绑定 sample.json md5（`b934c039`）与 6 个 entity_id |
| `preflight_record.json` | `d3f58ce6` | 占位 + 确切命令 + 工具链可行性探测记录 |
| `station45_baselines.py` | `4fe97ee5` | baselines.json 生成 wrapper（import 复用既有脚本，不改原脚本） |
| `station45_prereg.py` | `2f8227ee` | preregistration.json 生成器（确定性、self-sha 封存） |
| `constant_answer_baseline_rerun.json` | `2890a8cc` | 原脚本 `--out` 重定向重跑，**与冻结输出 md5 相同**（`data/tool_results/divergence/constant_answer_baseline_bbb.json`） |
| `variance_decomposition_rerun.json` | `95d45867` | 同上，与冻结输出 md5 相同 |
| `preflight_probe_qed01.log` | `6bfe5236` | 存量 case 可行性探测 stdout 全量留痕 |
| `station45_report.md` | （本文件） | — |

复算命令（仓库根运行，全部零 API）：

```bash
python3 src/probe_headless/bbb_constant_answer_baseline.py --out task/task25-pipeline小流量全链路演示/experiment/constant_answer_baseline_rerun.json
python3 src/probe_headless/bbb_variance_decomposition.py --out task/task25-pipeline小流量全链路演示/experiment/variance_decomposition_rerun.json
python3 task/task25-pipeline小流量全链路演示/experiment/station45_baselines.py --check   # 断言：重跑 md5 == 冻结输出、错误率逐项一致
python3 task/task25-pipeline小流量全链路演示/experiment/station45_baselines.py           # 重生成 baselines.json（字节一致）
python3 task/task25-pipeline小流量全链路演示/experiment/station45_prereg.py --check      # 验 preregistration self_sha256 + 冻结输入 md5
```

## 2. 三个基线的关键数字（每个数指得到出处）

### 2a. 恒答基线（part_a，730 池 usable 722 分子，全池口径、与分子选择无关）

| 策略 | 期望损失 L（never_abstain 口径） | 弃答率 | c 依赖 |
|---|---|---|---|
| 一律弃答（always-abstain） | **L = c** | 1.0 | 是 |
| 恒答 y=1（BBB 可透过） | **0.2867**（=少数类占比 207/722） | 0.0 | 否 |
| 恒答 y=0（BBB 不可透过） | **0.7133**（515/722） | 0.0 | 否 |
| 参照：集成多数票（平票 13 个排除不猜） | **0.0804**（57/709） | 0.0 | 否（oracle_yield 档 = c×0.0804） |

- c 网格 {0.0804, 0.2, 0.5, 1.0} 全部报数（baselines.json `strategies.*.loss_by_c*`）；c 有效区间 **(0.0804, 1.0]**，下界即多数票错误率（低于它一律弃答占优）、上界 1.0（≥1 时连 oracle 都不弃答）。与方向7 ESCALATE 口径 (0.157,1.59] **不可互换**（出处字段原样携带）。
- 出处：脚本 `src/probe_headless/bbb_constant_answer_baseline.py`（md5 eb765b1f…，未修改，import 复用）；冻结输出 `data/tool_results/divergence/constant_answer_baseline_bbb.json`（md5 2890a8cc…）；本站从逐分子原始数据（CSV 730 行 + ledger 730 + unresolved 8）重算并 assert 与冻结输出逐项一致。
- **每条损失与弃答率成对报告**（入库版 §3.4.6 硬约束）。
- **6 分子演示子集恒答基线（sample.json 已到达，已补算**，baselines.json `demo_subset_6molecules`，entity_ids = z000282/z000364/z000553/z000555/z000565/z000846，全部在 usable-722 池内）：恒答 y=1 子集错误率 **1/6=0.167**、恒答 y=0 **5/6=0.833**、一律弃答 = c、参照多数票子集错误率 **2/6=0.333**。⚠️ 如实登记两个 n=6 观察（无功效主张）：① 子集真值 5 阳 1 阴，类不平衡远大于池级（71:29），恒答 y=1 在子集上比多数票还好——**这正是恒答基线必须随弃答率、随配比同报的原因**；② 子集多数票 2 错均落在带工具的难分子上。复算命令：`python3 …/station45_baselines.py --sample-json …/experiment/sample.json`。

### 2b. 裸答投影（part_b —— **全部字段带 projection 字样 + 非实测/样本量/出处三要素**）

投影公式：`projection = Σ_band w_band × raw_accuracy_band(model, n1)`，难度带 = tw（易≤1/中2–3/难≥4，ADR-0007 统一口径）。数据源：冻结 `naked_difficulty_crossref_bbb.json`（md5 32196ba9…）逐模型×逐层 correct/total 计数——**这是可复算的最细粒度**（runs_naked 逐样本答卷已随 T-278 环境清空丢失，task23 report 追加段）。

qwen3.7-max n1 分层原始率：易 26/27=0.963、中 5/8=0.625、难 1/5=0.200。

| 配比 | 权重 | 投影值（qwen3.7-max） |
|---|---|---|
| **演示批 易2/中2/难2**（易 33.3%） | 1/3, 1/3, 1/3 | **0.5960**（5 个有效模型区间 0.409–0.680） |
| ADR-0007 R5.2 易30%+中35/难35 | 0.30, 0.35, 0.35 | 0.5776 |
| ADR-0007 R5.2 易30%+中难按池比例 86:56 | 0.30, 0.424, 0.276 | 0.6091 |
| （对照）自然组成 池份额 580:86:56 | 0.803, 0.119, 0.078 | 0.8635 |
| （对照）易50%+中25/难25 | 0.50, 0.25, 0.25 | 0.6877 |

**三要素声明**（baselines.json part_b `three_elements` 原文携带）：① 非实测——是 2026-09-01 裸答 n1 轮（40 分子）分层率按目标配比重加权的外推；② 样本量——难层 n=5、伪共识 n=1、只收 1/5 轮（预登记判 INCONCLUSIVE）、5 个有效模型；③ 出处——冻结 crossref JSON + 生成脚本 + ADR-0007。**演示批 n=6 同理：站6 裸答实测出来后与投影并排展示，但 n=6 不得用于校验或修正投影公式，无任何功效主张。**

### 2c. 方差分解引用（part_c，纯引用零新计算）

α_i（分子主效应）**54.46%** vs β_k（工具偏置）**0.555%**，残差 44.99%；置换检验 p≈0.000999（1000 次，seed=0，null mean 0.1428 = 自由度基线自洽）。敏感性：去 deeppurpose α=57.1%、非平衡面板 α=50.7%——三个 run 都是 α≫β。⇒ **分子间方差主导，演示批按分子难度（tw）分层有意义**。出处：冻结 `variance_decomposition_bbb.json`（md5 95d45867…），本站重跑副本字节一致；caveats（二值响应 ANOVA 近似、γ/ε 混淆等）原样携带。

### 2d. 难分子计数双口径（协调者补充要求②，只登记不裁决）

- **56**（ADR-0007 R3.3 台账：19 阳+37 阴）＝ usable 池口径（730 ∩ 有真值 − truth_unresolved 排除 8）tw≥4；本站重算逐位一致。
- **59**（分歧 CSV 直算）＝ 730 全量只要求有真值标签、不排除矛盾分子；差集 3 个 = **z007002, z015731, z034653**，实测**全部在 truth_unresolved_bbb.json 排除名单内**（同药异名标签矛盾：aspirin/acetylsalicylate、indomethacin 两名、loperamide/BRL53080）。
- **平票假设已实测否证**：差集里只有 z015731 一个存在 tied_families_no_vote（admet_ai 平票、6 票在场），另两个 7 票齐全；若改「缺票计为错」口径 hard=70/73，既非 56 也非 59 ⇒ 56 vs 59 之差**不是**平票处理差异，是排除名单开关。第三变体（平衡 652 子集）hard=48 一并登记，供 ADR-0009 裁决参照。
- 复算：`station45_baselines.py --check`（差集逐分子明细含票数、排除原因、source_rows 冲突原文都在 baselines.json）。

## 3. 投影 vs 实测的区别声明

本站所有裸答相关数值（0.5960 / 0.5776 / 0.6091 / 0.8635 / 0.6877）**全部是投影**：它们把 n1 轮 40 分子的分层率外推到目标配比，**不是在演示批 6 分子上的实测**。演示批实测由站6 裸答批（qwen3.7-max × 6）产出，届时在 scores.json / report.md 中与投影并排。ADR-0007 官方表里的 ~0.55 / 0.295 / 0.870 / 0.690 同为投影（见 §5 疑问 1：其中两个无法精确复算）。凡 baselines.json 引用投影值的字段名一律带 `projection` 字样并附三要素——已按协调者要求①落实。

## 4. preflight 状态

- **状态：占位（PLACEHOLDER_PENDING_CASE_DIRS）**——演示 case 目录尚不存在（站2/3 并行进行中，`experiment/cases/` 目前为空；站1 sample.json 已到达），`preflight.py <case_dir>` 无从实跑。这是 task25 交付中**唯一未实跑项**。
- 已做：① 工具链可行性探测——对存量 case `cases/drug/atom/fixed_some_tools/case_dr_qed_01`（只读）实跑 preflight：闸门1/2/4/5 通过或警告，闸门6 FAIL（该存量判分器缺 `baselines()`，历史遗留，与演示批无关），exit=1，stdout 全量落 `preflight_probe_qed01.log`；证明 preflight 在本机（python3 3.10.14 + rdkit 2026.03.5 + PYTHONPATH=src/probe_headless）可运行。② 占位记录含**确切命令**（逐 case 与批量两条）、日志落盘约定、对演示 case 每道闸门的预期行为（含闸门1 对 BBB 题型的已知覆盖局限、闸门2 小样本 WARN 属既定行为）。
- **命名消歧已登记**：preflight「闸门N」≠ BENCH_SPEC 造题「闸N」≠ task25 所称「闸6（预登记）」；且 **preflight.py 实际没有闸门3**（源码仅闸门1/2/4/5/6 五道），task.md 写「闸门1~6」按现有实现即指这五道。
- **对站2 的硬约束（已写进 preregistration 执行顺序）**：preflight 闸门6 的裸答基线**只认实测留痕**（None 即 FAIL、手挑先验不算，坑⑨）⇒ 演示批必须**裸答 6 run 先行**，留痕回填 `judge.baselines()` 后再过 preflight、再开带工具 12 run。这不是本站新增的口径，是 preflight 闸门6 既有实现。

## 5. 口径疑问（遇到无法核到官方定义的配对——停下写报告，不自己拍板）

1. **ADR-0007 投影表官方值无法精确复算。** 用 crossref 冻结分层数据 + 显式公式重算：易50% 行 0.6877≈0.690 ✓、自然组成行 0.8635–0.8761≈0.870 ✓（视逐带/逐层加权），但 **易≤30% 行官方 ~0.55 与本站任何显式权重的 0.578–0.609 对不上、tw≥2 行官方 0.295 与本站 0.448–0.462 对不上**；全仓检索无任何脚本产出这四个数（`0.55`/`0.295`/`0.690`/`0.870` 组合无代码出处）。投影公式未落盘 ⇒ 无法核到官方定义。**处置**：本站只报自己公式下的可复算值并登记差异（baselines.json `adr0007_official_projection_reference.discrepancy_note`），不改 ADR、不裁决哪个对；建议随难分子双口径一并交 ADR-0009 / owner 裁定官方投影公式并落盘脚本。
2. **「一律答可信/不可信」在 BBB 演示题面上无官方字段定义。** task.md 要求的三种退化策略里「一律答可信」「一律答不可信」有两种读法：恒答标签 y∈{0,1}（本站采用，与冻结脚本 constant:* 预测器一一对应），或恒答「这组工具预测可信/不可信」（可用性判断口径，期望损失取决于所抽分子的工具错误率与真值关系）。站2 题面未产出、无法核到官方映射 ⇒ 本站按恒答标签口径报数并把 caveat 原文写进 baselines.json（`label_mapping_caveat`）；若站2 采用可用性口径，用 `--sample-json` 在 6 分子上补算（命令已留）。**不自行拍板选哪个映射。**
3. **投影指标口径**：crossref 分层表只有 raw accuracy（无逐层类别混淆），本站投影是 raw-accuracy 口径；ADR-0007 表头写「平衡准确率」。二者在层内类别不平衡时不等价。已登记于 baselines.json `method.metric_caveat`。
4. **preregistration 的冻结输入含 `baselines.json` 与 `sample.json` 的 md5**（已处置）：sample.json 到达后重生成 baselines.json（补 demo 子集节）→ 重跑 `station45_prereg.py` 刷新 md5 并把 sample.json（md5 `b934c039`）+ 6 个 entity_id 封进预登记——两次刷新都发生在**任何计分 run 之前**，属同一工作单元内的机械更新，此处留痕。**预登记不可变性自首个计分 run（裸答批 run 1）起生效**：此后 sample.json / baselines.json / 本预登记任何一个 md5 变动都构成停止条件 4 类事件，须停下交 owner，不得静默重刷。

## 6. 小样本纪律声明（坑⑦）

演示批 n=6、裸答投影上游难层 n=5：**本文件与两份 JSON 不含任何「必然/恒定/结构性」断言，不含任何统计显著性判据**。preregistration 的成功判据 S3（易/难档可见分离）明文标注为观察性读数；失败判据 F1/F2 均为「如实记录、交 owner」，负结果就是结果（决议_20260831）。池级数字（722/652/730）是既有落盘结论的引用与复算，不是演示批的新主张。

## 7. 遗留 / 交接

- [x] ~~待 sample.json：demo 子集恒答基线补算~~ **已完成**（sample.json 于站1 落盘后补算并刷新预登记，见 §2a 与 §5 疑问 4）。
- [ ] **待站2 cases/**：preflight 逐 case 实跑，命令与日志约定在 `preflight_record.json`；结果回填其 `real_run_results` 键。
- [ ] **待站2 judge.py**：CHECKPOINTS + baselines()（裸答槽位等站6 裸答批留痕回填）——preflight 闸门6 会硬卡。
- [ ] §5 疑问 1–3 交协调者/owner 裁定（宜随 ADR-0009 一并收口）；难分子双口径 56 vs 59 同样只登记待 ADR-0009 裁决。
- 冒烟已花 1 run（站6，非计分）；preregistration 内 time_anchor 已诚实登记该并行顺序事实。
- **给站6/7 的指针**：预登记的执行顺序（裸答批先行 → 留痕回填 judge.baselines() → preflight → 带工具批）与 run 目录命名、剔除口径、重试政策全部在 `preregistration.json`；判分聚合规则（批级 L = 6 题算术平均、逐 c 档、聚合数必须可从逐 run 复算）也在其中，站7 直接照此实现。
