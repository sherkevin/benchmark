# E6 · report-02 — task-02 本批收口报告（矩阵设计与统计层）

| 字段 | 值 |
|---|---|
| **日期** | 2026-08-07 |
| **范围** | task-02 交付物 1–7（矩阵设计、估算、统计层、前置门禁、结果表骨架、收口）；**不含正式实验与真实结果** |

---

## 1. 核心结论（先总后分）

**task-02 的 7 项验收标准全部达成**：矩阵三因子设计定稿（M=4 × H=2 × 4 类 fixed template × k=3；M4 格位已登记降级路径，可用性确认后收口为 M=4 或降级 M=3）、总跑测次数与耗时预估（满额 3600 次、并发 8 路约 75 小时墙钟，数字由代码从 `query_spec.json` 动态计算；耗时表为 2026-08-07 01:38 对当时落盘 traces 的复算快照，E4 并行冒烟会重生 traces，正式开跑前须重跑 `sample_trace_durations()` 刷新，见 `matrix_design.md` §6.3 防漂移快照注）、统计层与 fixed/open 显著性区分写进代码并有自查断言跑通、三道门编排器实跑可用（当前如实报告"拒跑"及原因）、三张结果表骨架落盘且占位数据显著标注、收口文档齐备。**正式跑测仍未开**，阻塞清单见 §3——全部是上游交付依赖与待仲裁口径，不是本线欠账。

### 1.1 七项验收对照

| # | 验收标准 | 状态 | 落盘证据 |
|---|---|---|---|
| 1 | 三层矩阵设计定稿（因子水平、k 值、种子策略），定稿（M4 格位已登记降级路径，可用性确认后收口为 M=4 或降级 M=3） | ✅ | `matrix_design.md` §1–§4 |
| 2 | 总跑测次数与总耗时预估（含单次耗时依据） | ✅ | `matrix_design.md` §6（公式 + trace 时间戳采样依据 + 降级四档） |
| 3 | 统计层实现比例检验与置信区间，自查 n=30 ±0.18 / n=10 ±0.31 | ✅ | `src/eval_matrix/stats.py` + `test_stats.py`（Wald 口径锚点断言通过；Wilson CI 一并实现，n=30 时半宽 0.168 略窄于 Wald，两口径在测试中均有断言） |
| 4 | fixed 出显著性、open 只出描述性——写进代码 | ✅ | `stats.report_significance(side, ...)`：阈值从 `query_spec.json → min_publishable` 动态解析（不写死 97），n 不足时 API 级只返回描述性 + 功效不足声明；`test_open_side_n30_only_descriptive_and_power_statement` 等断言强制 |
| 5 | 跑测前置三道门接入，任一不过拒跑并指明哪条 | ✅ | `src/eval_matrix/pre_gates.py`：子进程调现成脚本读退出码；实跑输出 overall=reject 并逐门标注（门 1 fail / 门 2 pass【cached】/ 门 3 fail+blocked）；机器可读 JSON 报告 + `--dry-run` + `--cases` + `--ab-rebuild` |
| 6 | 结果表骨架可渲染，占位数据显著标注且不外流 | ✅ | `result_tables/01_main_table.md`、`02_process_breakdown.md`、`03_ablation_harness.md`：文件头与脚注双重标注「占位数据，非真实结果」；本报告与对外文档不引用任何占位数值 |
| 7 | 收口：列出开跑前还等什么 | ✅ | 本报告 §3（旧"等 E3"项已按 E3 验收通过改写为现状，见 §2） |

### 1.2 已知坑规避情况

| 坑 | 规避动作 |
|---|---|
| 旧 15 点数据（run_all.py 批，PR-26）已作废 | 全文未引用其任何分数；仅取既有 trace 的**时间戳量级**作耗时估算并显式声明不作基线（`matrix_design.md` §6.3） |
| query_lint 退出码 2 ≠ 违规 | 门 1 实现三态分支：0=pass / 1=fail（真违规）/ 2=blocked（打印 pending 明细 + 放行条件：E2-04 转实判后退出码归 0），并有单测断言不得混报 |
| 门 3 回执未就位不得静默放行 | 门 3b 实测输出 blocked「等专家回执」；且 total_questions 非法/分母为 0/回执形状异常均 fail-closed 拒绝判定；单测 `test_gate3_no_receipt_must_block_not_silently_pass`、`test_gate3_invalid_total_questions_refuses_to_judge`、`test_gate3_malformed_receipt_fail_closed_not_traceback` 强制 |
| 门 2 实跑会覆写 E1 产物 | 门 2 默认 **cached 模式**：只读 `_build_state/ab_isolation_result.json` 校验 all_passed 与新鲜度（mtime），不触发重建；实跑重建需显式 `--ab-rebuild`。**E1 迁移期建议一律默认 cached**；报告 `gate2_mode`/`mode` 字段注明本次走的是哪种 |
| 门 3b 粒度错配 | 分母改为 **cases_glob 匹配到的 case 文件数**（与门 1 同范围、机器可判，实测 67 个）；题量粒度（total_questions=150）与 case 粒度的最终口径、连同回执目录 SIGNOFF_DIR 约定，已登记待 owner 仲裁（报告 `granularity_note` 留痕） |
| cp5 口径 gap（query_spec vs evaluate.py） | 不自改实现：结果表与 progress.md 如实登记（type1 按现行实现 cp5 恒含且恒判 0），已报监工请 E3/owner 仲裁 |
| 容量数字订正时点差 | 代码/文档零硬编码：题量读 `total_questions/fixed_total/open_total`、阈值读 `min_publishable`、容量仅注明以 `capacity_basis` 落盘值为准 |
| open n=30 只出描述性 | 代码强制（非注释提醒）：`report_significance` 对 n < 阈值一律拒出检验，open/fixed 同逻辑、阈值统一从 spec 读 |

---

## 2. 依赖现状改写（旧"等 E3"项 → 现状）

**E3 已验收通过**，旧 task 文档里的"等 E3 修完才能开跑"不再是等待项，现状如下：

| 原等待项 | 现状（2026-08-07） |
|---|---|
| `no_path_violation` 判据方向错误（evidence_time 取 max 误杀迭代式 agent） | ✅ 已修：`gateway.py` evidence_time 改取 min，"结论后回访文献"不再判为路径违例 |
| `open_score` 双因子缺失 | ✅ 已就位：`hit_gb` × `evaluate_grounding`，`aggregate()` 输出 open_gate/open_score/grounding_mode |
| `evaluate.py` 对 open case KeyError | ✅ 已修，open 判分链路可跑 |

判分链路可信后，本线剩余等待全部转为**上游交付依赖**（§3），与判分质量无关。

---

## 3. 开正式跑测前还等什么（MECE：题集 / 背书 / harness 三项）

| # | 等什么 | 归属 | 对本线的影响 | 解除信号 |
|---|---|---|---|---|
| 1 | **E5 30 道验证批题集**（含 type1/type2 迁移与重写） | E5 | 无合格题集则矩阵无题可跑；当前门 1 对存量 37 case 实跑为 fail（M10 违规 37 条等，旧 case 未迁移所致） | `query_lint --strict` 对目标题集退出码归 0 |
| 2 | **专家回执**（专家覆盖 100%，ADR-0016 修订 R1 新口径——旧"抽审 ≥30%"条款已作废） | owner 发起 / E2-E5 落盘 | 门 3b 显式 blocked，不得静默放行 | 回执按约定落盘 `task/E2-query风格与题量定数/expert_signoff/*.json`（字段：expert/batch/date/signed_off_case_ids），并集 ≥ 当前 cases_glob 匹配到的 case 文件数（门 3b 现行分母；case 粒度 vs 题量粒度 total_questions 的最终口径待 owner 仲裁） |
| 3 | **E4 多 harness 就绪度**：H2 真实 harness 端到端（CLI → adapter → evaluate.py 吃通） | E4 | 不就绪则矩阵降为 H=1，消融表冻结（不以 H1 自比冒充消融） | E4 二期验收：同 case 双 harness 可比 |
| 附 | 容量订正 E2-04 落盘（156/154/91 → 155/153/92） | E2 | **不阻塞**：代码动态读取；仅"引用容量数字"类结论待其落盘 | `query_spec.json → capacity_basis` 更新 |

**门禁编排器已就绪**：上述条件满足后，跑 `python3 src/eval_matrix/pre_gates.py` 退出码归 0 即开门；本批实跑记录（overall=reject：门 1 fail（M10 等真违规）/ 门 2 pass（**cached 模式**，只读产物 mtime 留痕，未触发重建）/ 门 3a fail（M10 违规 37 条）+ 门 3b blocked（等专家回执，分母 = cases_glob 匹配的 case 文件数 67 个，signed=0））如实反映当前差距，无任何一门被静默放行。

---

## 4. 产出文件清单（绝对路径）

- `/Users/jingwu/work/benchmark/task/E6-三层矩阵评测/matrix_design.md`
- `/Users/jingwu/work/benchmark/task/E6-三层矩阵评测/result_tables/01_main_table.md`
- `/Users/jingwu/work/benchmark/task/E6-三层矩阵评测/result_tables/02_process_breakdown.md`
- `/Users/jingwu/work/benchmark/task/E6-三层矩阵评测/result_tables/03_ablation_harness.md`
- `/Users/jingwu/work/benchmark/src/eval_matrix/__init__.py`
- `/Users/jingwu/work/benchmark/src/eval_matrix/stats.py`
- `/Users/jingwu/work/benchmark/src/eval_matrix/pre_gates.py`
- `/Users/jingwu/work/benchmark/src/eval_matrix/test_stats.py`
- `/Users/jingwu/work/benchmark/src/eval_matrix/test_pre_gates.py`
- `/Users/jingwu/work/benchmark/task/E6-三层矩阵评测/progress.md`
- `/Users/jingwu/work/benchmark/task/E6-三层矩阵评测/report-02.md`（本报告）

**验证记录**（评审修复后复验）：`python3 -m pytest src/eval_matrix/ -q` → **42 passed**（新增用例覆盖 R1/R2/R3/D1/D2/D3/R6/R7 边界）；`python3 src/eval_matrix/pre_gates.py --dry-run` → 退出码 0（gate2_mode=cached）；`python3 src/eval_matrix/pre_gates.py` → 退出码 1（如实拒跑，逐门原因见 §3）；`--cases <glob>` 与 `--ab-rebuild` 新 CLI 参数有单测覆盖。

---

## 5. 遗留风险

1. **M4 弱档模型名单未定**：矩阵设计已留格位与降级路径（M=4→3），但开跑前必须确认，否则能力谱系缺下界锚点；
2. **并发度未实测标定**：耗时预估的并发 8/16 行是公式推演，开跑前须 10 次并发冒烟实测 API 限流，不得直接按表承诺排期；
3. **回执目录是新立约定**：`expert_signoff/` 路径与 JSON 字段是本门禁定义的约定，需与 E2/owner 对齐一次，若他们另有落盘习惯，改 `SIGNOFF_DIR` 常量即可（单点修改）；**连带待仲裁**：门 3b 分母的 case 粒度 vs 题量粒度口径（见 §1.2 已知坑表）；
4. **存量 37 case 全量未迁移**：门 1 的 fail 主要由旧 case 贡献；E5 迁移完成前，门禁对全目录 glob 会持续拒跑——这是设计意图（防旧题混入正式矩阵），不是 bug。
