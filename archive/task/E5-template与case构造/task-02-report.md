# E5 task-02 收口报告 — fixed 批次 30 道

| 字段 | 值 |
|---|---|
| 任务 | task-02：fixed 类进场批次（先造 30 道，验证造题链路），见 `task-02-fixed批次进场.md` |
| 完成日期 | 2026-08-07 |
| 配套文档 | 造题经验：`造case经验/task-02-fixed批次.md`；进度与阻塞：`progress.md`（E5-01） |

**核心结论：30 道 fixed case 全部交付并通过独立验证，验收 7 条中 6 条完全达标、1 条（strict 退出码 0）因 lint 固有行为字面不可达、按监工批准的达标线达标；三维评审后 task-05 返工全量重生成 30 道（致命问题+唯一性缺口修复，见 §3），重生成后 lint 仍全部达标。**

---

## 1. 交付清单

| # | 交付物 | 路径 / 数量 |
|---|---|---|
| 1 | 药物侧 15 道 | `dag_poc/templates/constraint_satisfaction/case_cs_drug_0{1..8}.json`（类型1 ×8）+ `dag_poc/templates/constrained_optimization/case_co_drug_0{1..7}.json`（类型2 ×7） |
| 2 | 材料侧 15 道 | `dag_poc/templates/constraint_satisfaction/case_cs_material_0{1..8}.json`（类型1 ×8）+ `dag_poc/templates/constrained_optimization/case_co_material_0{1..7}.json`（类型2 ×7） |
| 3 | 造题脚本 | `src/case_build/{common.py, shared_text.py, build_drug_cases.py, build_material_cases.py}`（骨架批量为主，题面措辞人工轮换为辅） |
| 4 | 可解性证明 | 每 case 的 `solvability_verified` / `gold_best_note` 字段：格全部成员 + 目标量 + 最优唯一 |
| 5 | provenance | 30/30 真实论文 + DOI/URL，文献池 10 篇（药物 6 + 材料 4），独立验证方联网抽查 6 篇全部真实 |
| 6 | 造题经验文档 | `造case经验/task-02-fixed批次.md` |

## 2. 验收 7 条标准逐条对照（进场文档 §5）

| # | 标准 | 结果 | 证据 |
|---|---|---|---|
| 1 | 30 道 fixed case（药物 15 + 材料 15；每域类型1 八道 + 类型2 七道） | ✅ 达标 | 30 个文件齐备（见 §1），配比精确 8+7 / 8+7，M13 对称性 8+8 / 7+7 通过 |
| 2 | `query_lint.py --strict` 退出码 0 | ⚠️ 字面不可达，按监工批准达标线达标 | strict 退出码恒 2 系 lint 固有行为（L777-784 无条件 A1 留痕 + L927 pending 即 2），已报 E5-01；监工 2026-08-07 批准达标线「违规 0 + pending 仅 A1 留痕」。实测：精确 glob 非 strict 违规 0 / 退出码 0；`--strict --freeze-batch` 违规 0 / pending 仅 A1 / 退出码 2 |
| 3 | 每 case 落盘穷举证明 | ✅ 达标 | 每 case `solvability_verified` 含格全部成员 + 目标量 + 最优唯一；独立验证方基于当前库（`get_property_A.jsonl` 27,606 行）重算 30/30 通过 |
| 4 | 每 case 合格 provenance（真实论文标题 + DOI/URL） | ✅ 达标 | 30/30 真实论文；10 篇文献池全部联网核实，独立验证方抽查 6 篇全部真实 |
| 5 | 材料侧脚本两条断言（tc>0、题面无 ambient） | ✅ 达标 | `build_material_cases.py` 内置 tc>0 / ambient 禁词 / 目标量非空三条硬断言，写盘前逐条执行 |
| 6 | 一份造题经验文档 | ✅ 达标 | `造case经验/task-02-fixed批次.md`：切分参数实际效果、lint 卡点（含 strict 退出码、M7、M9a）、脚本 vs 手工取舍、库变重验记录 |
| 7 | report 收口，如实报告放弃/返工 | ✅ 达标（即本报告） | task-05 返工全量重生成 30 道已如实上报（见 §3），未为凑数放宽任何标准 |

**lint 关键数字**（task-05 重生成后，批次 `e5_task05`）：M9a 变体率 type1 16/16 = 1.000、type2 13/14 = 0.929（均 ≥0.3）；M9b gt 全库唯一通过；整批精确 glob（`dag_poc/templates/con*/case_c?_*_??.json`）非 strict 违规 0/退出码 0，`--strict --freeze-batch e5_task05` 违规 0/pending 仅 A1/退出码 2（监工批准达标线）。

## 2.5 lint "需人工复核清单"状态（task-05 补录）

- lint 输出对 30 道均列 M2（术语裸用语义判断）、M3（A 动作段语义判断）、M10（背书真实性专家审）三条需人工复核项——**agent 不可执行人工复核**，E5 侧只能保证机器可判规则全绿。
- M10 背书支撑性深审（ADR-0016 修订 R1，二级深审 ≥30%）：当前仅完成 **6/30 联网真实性抽查**（独立验证方执行，全部真实），余下 24 道的支撑性深审**待 owner 发审题邀请**后由人工专家执行。

## 2.6 E6 取数口径登记（task-05 新增，judge 侧必读）

- **约束可行性完整口径 = constraints（get_property 可查）∧ domain_knowledge 结构判据**：
  - 药物侧结构判据：`atom_count` 区间 ← **get_entity 实体字段**（不经性质接口）；
  - 材料侧结构判据：`element_count` 值 ← **formula 互异 [N*] 计数派生**（正则 `\[\d+\*\]` token 个数）。
- **judge 侧不可只按 constraints 求值**：只按 constraints 会包含跨格候选（药物多 atom 段、材料多元素种类数），合格集/归一化基准会偏大。每 case 的 `solvability_verified`/`gold_best_note` 首句已明示完整过滤口径，复算按此口径。
- 备选方向 A（把 atom_count/element_count 注册为正式可查性质）涉 backend，归 E1/E3，已登记 `progress.md` E5-02；若获批后续批次可把结构判据移回 constraints。

## 3. 放弃 / 返工的题数与原因

- **放弃：0 道。返工：30 道全量重生成（task-05，如实上报）。**
- task-02 首次交付时报告"返工 0"，三维评审后 task-05 返工：① 致命：constraints/gt.properties 含后端不可查性质键（药物 atom_count、材料 element_count），按方向 B 修复（可查约束留 constraints、结构判据移 dk、gt 瘦身）；② 唯一性：材料 type1 三道多成员格（case_cs_material_03/05/07）重选为单成员格；③ 脚本加固 3 项（round 预检、全精度穷举、TYPE2_OPENERS 扩 14）。修复后重跑脚本全量重生成 30 道并重过 lint，达标线不变。详见经验文档 §6。
- 首次交付时选格阶段对个别候选格做过弃用（格内最优值并列等），属正常选格不计入返工。
- 会话期间后端库快照变动触发过一次**全量重验**（非返工）：独立验证方按当前库重算 30/30 通过。

## 4. 遗留问题

| # | 问题 | 状态 |
|---|---|---|
| 1 | **E5-01**：`query_lint.py --strict` 退出码 0 字面不可达（lint 口径 bug 疑似） | 已报 `progress.md`，**待监工裁决**（修法建议已附：A1 留痕移出 pending 通道或 L927 区分 pending.kind）。本批按监工批准达标线交付 |
| 2 | **E5-02**：方向 A 备选（atom_count/element_count 注册为正式可查性质，涉 backend） | 已登记 `progress.md`，归 E1/E3 线，待监工批准；批准前结构判据一律走 dk |
| 3 | 4 个 legacy case 迁移（重命名去真名 + 修 M4/M7/M10 违规） | **未做，属本批边界外**——进场文档边界只覆盖新造 case；宽 glob lint 时需避开 legacy（已用精确 glob 规避） |
| 4 | open 类本批一道未造 | 进场文档明令（§4/§7）：本批目的为验证 fixed 造题链路；材料侧 open 的质量门依赖 E1-04 身份键（未交付），故 open 未开 |

## 5. 给下一批的建议（放量 90 道 fixed + open 15+15）

1. **骨架脚本直接复用**：`src/case_build/` 的格子切分、穷举证明、硬断言、写盘前 lint 镜像自检已在本批验证零结构性返工；放量主要新增工作是**扩句式轮换表**（M9a：type1 至少需 ceil(题数×0.3) 种开头两词，当前 16+10 种不够 90 道用）。
2. **provenance 文献池需扩容**：现池 10 篇支撑 30 道循环引用尚可，120 道规模需扩池以免背书同质化；新文献入库前先联网核实 DOI/URL。
3. **库快照纪律**：造题前记录 `get_property_A.jsonl` 行数/hash（当前 27,606 行），造题期间一旦变动立即全量重验，不攒到最后。
4. **lint 口径**：新批次沿用精确 glob + `--freeze-batch <批次名>`；strict 验收按 E5-01 裁决结果执行，裁决前继续用「违规 0 + pending 仅 A1」达标线。
5. **open 批前置**：等 E1-04 材料身份键交付后再开材料 open；药物 open 可先行，但需确认 E3 的 `check_molecule_exists` 判分链路就绪，避免造了无法判分。
