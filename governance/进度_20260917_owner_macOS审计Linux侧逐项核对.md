# owner macOS 侧独立审计 × Linux 侧逐项核对登记

> 日期：2026-09-17 · 分支 `master` · 性质：**一次性快照留痕**，不再更新。
> 起因：owner 从 macOS（`U-CGPCVWR0-0024.local`，`/Users/jingwu/work/benchmark`）发来一份独立项目审计（未改项目、未跑批），主会话按仓规「不凭记忆断言」在 Linux 侧对审计的每个关键数字实跑核对。
> 结论先行：**审计的关键数字 10/10 核验吻合或更精确；审计发现的四个缺口全部成立**；其中「缺口一」被 Linux 侧实跑**进一步实证**——BBB 族内难分子计数已存在 56 vs 59 两种口径，跨族合并前必须钉死口径（已交 ADR-0009 (d) 节）。

## 1. 逐项核对表（审计声称 vs Linux 实跑）

| # | 审计声称 | Linux 实跑核对 | 判定 |
|---|---|---|---|
| 1 | 冻结分子池 730 行 730 唯一 entity_id | `data/tool_results/molecule_pool.csv` 实跑：730 行、col0 唯一 730 | ✅ 吻合 |
| 2 | 工具总账 49 条 | `data/tool_property_matrix/LEDGER.tsv` 实跑：49 行 | ✅ 吻合 |
| 3 | 根 manifest 正式交付 9 工具 39 端点（含 JPlogP） | `data/tool_results/manifest.json` `formal_deliveries` 实跑：9 条、endpoint_count 求和 = 39；`jplogp_cdk_2_13` 在列 | ✅ 吻合。**README:47 与 docs/任务清单.md:129 仍写「首批 8 工具 38 端点」——历史行本身没错（说的是首批），但第三批交付未同步入口，本次已补行** |
| 4 | 多数票 err 8.04%（709 可形成多数） | `constant_answer_baseline_bbb.json` 实跑：`ensemble:majority_vote` err=0.080394…，errors=57（57/709=8.04% 精确；errors/730=7.8% 不是这个数——**口径是 709 分母**，平票 13 不计） | ✅ 吻合 |
| 5 | 恒答阳/阴 err 28.67% / 71.33% | 同文件实跑：0.28670 / 0.71329 | ✅ 吻合 |
| 6 | c ∈ (0.0804, 1.0] | 同文件 `c_valid_interval` 实跑：下界 0.08039、上界 1.0 | ✅ 吻合 |
| 7 | α_i 54.5% / β_k 0.56%、伪共识 14 | `task/task21-D2一方差分解BBB/report.md`：54.5%/0.56%、全票一致 492 中 14 错；**主会话另从 730 全池独立重算「7 家族票全一致且 ≠ 真值」= 14，精确复现** | ✅ 吻合 |
| 8 | BBB 难分子（tw≥4）= 56（19阳+37阴），引 ADR-0007 | ADR-0007 R3.3 原文确有「56（19阳+37阴）」；**但 Linux 用分歧 CSV 7 列直算得 59**（tw 分布 {0:494, 1:89, 2:51, 3:37, 4:20, 5:12, 6:13, 7:14}，tw≥4 = 59）。差 3 疑似 `tied_families_no_vote`（平票家族计不计入分母）口径差 | ⚠️ **审计数字与台账一致，但盘面存在双口径**——实证审计「缺口一」的口径警告。已同步 A/B/D 三路 subagent：采样以 56 的权威口径为准并落「口径出处」字段；双口径登记进 baselines.json；统一规则进 ADR-0009 (d) |
| 9 | task23 = 40 分子×6 模型（5 有效）、INCONCLUSIVE、qwen3.7-max 0.800 恰触阈、易 26/27 难 1/5 | `task23 report.md` + `naked_baseline/results.json`（missing_slots=25、verdict=INCONCLUSIVE）实跑核对 | ✅ 吻合。审计的纪律提醒成立：**难层 n=5、样本内伪共识 n=1，0.55 是投影非实测** |
| 10 | `.bench_runs/` 不存在；Linux 测试 227/1/1 与 PR-70 macOS 10 failed 自洽 | Linux 实跑：`.bench_runs` 同样不存在；PR-70 已登记 macOS 10 failed 三根因，Linux 侧数字逐位吻合 | ✅ 吻合 |

## 2. 审计四个缺口的 Linux 侧确认与处置

| 缺口 | 确认 | 处置 |
|---|---|---|
| 一：题集有效信息量未达标（难分子 56 vs ≥500、伪共识 14 vs ≥50） | ✅ 成立，且族内已有 56/59 双口径 | **跨族难度口径与去重规则 → ADR-0009 (d) 节**（已发 subagent D）；hERG/AMES 真值候选表（task24 交付物2）回答「难分子从哪来」；task25 演示批不假装达标（n=6 明示无功效） |
| 二：D2 未完整通过（①半项、②裸答未收口、③④⑤未做） | ✅ 成立 | GPU 闸门维持关闭；task23 处置收敛为 (a)/(b) 待 owner 裁（U-9 裁定①）；D2-③④⑤ 不自行启动（ADR-0028 顺序铁律） |
| 三：原始证据链受损（逐样本裸答输入丢失、.bench_runs 缺） | ✅ 成立（ADR-0008 已客观证死逐字节重放） | U-9 裁定②（备份甲/乙）待 owner；**task25 起所有新 run 现场按 ADR-0008 四类边界入库**（防复发） |
| 四：状态清单滞后 | ✅ 成立（README:47/任务清单:129 第三批未同步） | 本次已补同步行；ADR-0007 文末三个数已裁（T-277 登记），摘要旧问句待 ADR-0009 一并订正 |

## 3. 与 task25 演示批的关系（审计不改变演示批，但收紧三处纪律）

审计判断「当前重心应在阶段 3 真值/难样本 + 阶段 4 可执行验收，而非提前训练」——与 task25 的定位**一致**（演示批就是阶段 3/4/5 的最小闭环，GPU 闸门未动）。据审计收紧：

1. **投影纪律**：演示批所有汇总数字（含 ~0.55 引用）必须带 `projection` 标记 + 样本量 + 出处三要素（已发 subagent B）。
2. **口径纪律**：采样用 56 权威口径并落口径出处；找不到 56 算法就停下报差集，不自拍（已发 subagent A）。
3. **证据纪律**：演示批 run 现场全部入库（answer.json + tool_calls.jsonl + 判分记录），不再产生「聚合在、原始丢」的新欠账。

## 4. 出处

owner macOS 审计原文（本会话 09-17 消息）· `data/tool_results/manifest.json` · `data/tool_results/molecule_pool.csv` · `data/tool_property_matrix/LEDGER.tsv` · `data/tool_results/divergence/constant_answer_baseline_bbb.json` · `data/tool_results/divergence/naked_difficulty_crossref_bbb.json` · `data/tool_results/divergence/血脑屏障 BBB.csv` · `cases/_methodology/tool_conflict_census/bbb_authoritative_v2/screening_ledger.jsonl` · `docs/adr/0007-benchmark-task-requirements.md:53,79-80,99` · `task/task21-…/report.md:44` · `task/task23-…/report.md` · `governance/进展_20260913_第三批JPlogP_XLogP3_SSLGCN独立验收裁定.md` · problems.md PR-70 · ADR-0008
