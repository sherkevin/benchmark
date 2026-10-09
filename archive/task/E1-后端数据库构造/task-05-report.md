# Task Result · E1-05 —— per-case 可见集视图机制 + 材料侧扩数据：收口再冻结与全量自检

> **Status: `done`**

> ⚠️ 本文件自包含：所有结论附实跑命令与输出摘要，外部读者只读本文件即可核验。

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | E1-05（任务板 Task #30 视图机制 / #31 扩数据 / #32 三维评审 / #34 修复轮 / **#33 本收口轮**）/ `task/E1-后端数据库构造/` |
| **类型** | Verification + Implementation（收口再冻结 + 契约升版 + 六节 report） |
| **负责人** | E1（数据线） |
| **起止** | 2026-08-07（收口半天） |
| **上游来源** | task-05-per-case视图与材料侧扩数据.md §4 七项交付标准 · per-case视图交接说明.md · task-05-扩数据入库记录.md · task-05-扩数据源调研.md · ADR-0018 §2.3/§7 |
| **下游消费方** | E5（open 造题：视图格式/穷举接口/材料族供给）· E3（接口 B 扩展：降级键复算路径）· E2（容量数字涉 query 定数复核） |

---

## 一句话结论

> E1-05 §4 七项交付标准逐条勾验**全部达标**（交付 7 即本收口本身）：视图机制五项硬要求全绿（A1–A8 断言本轮亲复跑 exit=0）、材料侧扩数据 **11,481 条**入库支撑 **31 达标族 / 26 独立体系 ≥ 15 道**需求（族统计本轮亲复跑锚不变）、两项监工裁定已落地（黑名单归一、降级键归 E3）；**收口再冻结完成**——`per_case_views/` 纳入冻结主清单，CHECKSUMS 刷新为 **`73d6e845f43f8856bb628747d2270370`**（主清单 15 + 附录 2，`--verify` exit=0，其余 14 件 md5 零漂移），契约升版 **v1.3**，收尾五连自检全绿。

---

## 1. 任务执行情况（对照 task-05 §4 七项交付标准逐条勾验）★

| # | 交付标准 | 状态 | 实跑命令与输出证据 |
|---|---|---|---|
| 1 | per-case 视图机制：4 项硬要求全达标；每个 A 侧工具都有"返回集 ⊆ 白名单"断言 | [x] ✅ | 硬要求 1（视图与 A/B 物理分层同时成立）：视图只在读取层生效，`test_ab_isolation.py` 断言②本轮复跑仍证 agent 读取路径只开 `*_A.jsonl`（exit=0，§6.1）；硬要求 3（四工具全覆盖）：断言 A2/A3/A4/A5 对 get_entity/get_property/search_candidates/literature_search 逐一全白名单扫查（本轮 `python3 src/data_build/assert_per_case_view.py` 亲复跑 exit=0，"1 个生产视图 + 1 个 B 侧 gt 形态测试视图 × A1–A7 + A8 幂等"全过）；机制细节见 per-case视图交接说明.md §1–§5（Task #30 交付，Task #34 修复后四项自检全绿） |
| 2 | gt 不在自己可见集内的断言：可执行、退出码可判 | [x] ✅ | 断言 A1（entity_ref + 组成双口径、A/B 两形态分流）本轮亲复跑全过；A 侧演示形态 BEFORE 可触达 gt / AFTER `UNKNOWN_ENTITY`，B 侧生产形态 gt_ref ∉ A 全库实体集 + 组成扫查 AFTER 可见集内 0 个；退出码 0/1/2 可判（交接说明 §5） |
| 3 | 可见集穷举接口：E5 质量门第一条能直接调用 | [x] ✅ | 断言 A6 本轮亲复跑：`enumerate(tc≥38.2)`=0（gt 阈值）、`enumerate(composition=gt)`=0、宽松对照 `tc>-1`=14 个（非恒真）；纯函数 + CLI 双形态见交接说明 §4（`python3 src/data_build/per_case_view.py enumerate --case e105_example_001 --constraint tc:ge:38.2` → returned 0） |
| 4 | 视图定义格式 + 示例：E5 拿到就能写 | [x] ✅ | 格式全字段表 + 载入期校验清单见交接说明 §2；示例 `backend/per_case_views/e105_example_001.json`（n=4/d=1/K=10，材料线）已冻结入主清单，md5=`8b1876ea2ea30ebabff503babed0a538`（1,716 B，本轮 CHECKSUMS 输出实证）；A8 幂等断言证重跑逐字节一致 |
| 5 | 材料侧数据扩展：支撑 15 道（含冗余）；新源标真实 URL 与权威性；同分布可比性有说明 | [x] ✅ | **本轮亲复跑** `python3 src/data_build/stat_material_families.py`（exit=0）：晶体 17,252 条（3DSC 5,771 + UCI 11,481），达标族 **31**（3DSC 5 + UCI 26）/ 独立体系 **26**，需求 15 道（43 投 30 ≈ 22 次投造）→"足"，31/26 锚与入库记录 §5 逐一不变；新源 URL=https://archive.ics.uci.edu/ml/datasets/superconductivty+data、DOI 10.24432/C53P47（CC BY 4.0）、NIMS MDR 交叉验证（调研 §4.4）；可比性处置＝族内单源纪律（实体层重叠不新建 + 统计层分源枚举，跨源重叠 3,828 剔除含 75 标量倍写法），偏差报备两处闭环（入库记录 §3/§5） |
| 6 | 新数据经匿名管线：共用同一套 `element_to_label`；CHECKSUMS 已刷新 | [x] ✅ | 共用 `anonymization_mapping.json` 零扩展（77 元素全在 88 项映射内，硬校验通过，入库记录 §2 步 4）；**本轮亲复跑** `python3 src/data_build/check_agent_visible_anonymity.py` exit=0（黑名单 45,715 式/40 真名，四库四道断言全 0 命中）；**收口再冻结刷新 CHECKSUMS**：`73d6e845f43f8856bb628747d2270370`（主清单 14→15，per_case_views 纳入，--verify exit=0，§2.1） |
| 7 | report 收口 + 状态行更新（前三轮状态行滞后，本轮一并） | [x] ✅ | 本文件（六节模板，对齐 task-04-report.md 遵循方式）；`task-05-per-case视图与材料侧扩数据.md` 表头 🟡 待开工 → ✅ 已完成；progress.md 追加本轮条目（第 21 条） |

**勾选符号**：`[x]` 完成——七条全部达成，无部分完成项。

**证据出处说明**：交付 1–4 的机制细节复用 Task #30/#34 落盘的交接说明 §5 实跑留痕，但**关键项本轮全部亲自复跑**（assert_per_case_view / stat_material_families / check_agent_visible_anonymity / test_ab_isolation / generate_checksums --verify 五连，输出见 §6.1）；交付 5/6 的入库过程证据出处为 task-05-扩数据入库记录.md §0–§8。

---

## 2. 关键产出 / 发现（自包含）

### 2.1 收口再冻结（本收口轮核心动作）★

**动作序列与实证**：

| 步 | 动作 | 实证 |
|---|---|---|
| 1 | `generate_checksums.py` `EXCLUDED_DIRS` 移除 `per_case_views`（脚本内留痕改为"收口轮已办"） | `EXCLUDED_DIRS = {"_build_state"}` |
| 2 | 重跑 `generate_checksums.py` 刷新 | 文件数 14 → **15**，`per_case_views/e105_example_001.json` 按脚本现有归类逻辑进**主清单**（backend/ 下目录进 files，不进附录），合计 78,480,236 bytes |
| 3 | `--verify` 复核 | "冻结校验通过 ✓：主清单 15 个文件 + 附录 2 个交接产物 md5 全部一致"，exit=0 |
| 4 | 新 CHECKSUMS 值登记 | **`73d6e845f43f8856bb628747d2270370`**——已记入本 report 与入库操作手册 §8 再冻结登记 |

**其余 14 件 md5 零漂移**（与 Task #31 冻结值逐一相同）：search_candidates_A=`a9af11a0…` / get_entity_A=`04305683…` / get_property_A=`f81928bd…` / 根 MANIFEST=`e01b7620…` / 附录 enumerate_A=`00c52b77…` / material_identity_keys=`9f6ad6c7…` 等。

**临时产物清理（B 侧测试件）**：`_build_state/tmp_test_view/e105_test_b_side_gt.json`（Task #34 修复轮的 B 侧 gt 形态测试视图）落盘副本**已删除**。处置依据实证：`assert_per_case_view.py::build_b_side_test_view`（第 322–375 行）每次运行期确定性自行构造并落盘该文件——删除后复跑断言集 exit=0 且文件即被重建，**脚本不依赖落盘副本存在**，属临时产物可清理。`_build_state/` 位于 `src/data_build/` 下，`collect_files` 只扫 `dag_poc/data/backend/`，**确认不在冻结清单扫描面内**。

### 2.2 视图机制五项硬要求达成汇总（复用前轮留痕，本轮复跑复核）

| # | 硬要求（task-05 §1.3） | 达成形态 |
|---|---|---|
| 1 | 视图与 A/B 物理分层同时成立 | A/B 一侧一文件不动，视图在 A 内部读取层再切一层；`test_ab_isolation.py` 四断言本轮复跑全过（§6.1） |
| 2 | 该 case 的 gt 不得出现在自己可见集 | 断言 A1（载入期校验 + 运行期断言双保险，entity_ref/组成双口径，A/B 两形态分流） |
| 3 | 视图对所有 A 侧工具生效 | `ViewedBackendReader` 包在参考读取器外，四工具（search_candidates/get_entity/get_property/literature_search）逐一断言 A2–A5；绕过后门路径已防（逐工具全白名单扫查） |
| 4 | 视图格式 E5 可直接写 + 示例 | `per_case_view_v1` schema 全字段表 + 示例 e105_example_001.json（交接说明 §2） |
| 5 | 穷举接口（质量门第一条） | `enumerate_in_view` 纯函数 + CLI（交接说明 §4），A6 断言 sanity 三轮复跑全过 |

### 2.3 扩数据三项关键数字（入库记录 §0 口径，本轮族统计亲复跑复核锚不变）

- **新增 11,481 条**（id c05772–c17252），零拒收；材料侧晶体 5,771 → **17,252**；
- **31 达标族 / 26 独立体系**（分源口径：3DSC 5 + UCI 26 族；体系 3DSC 5 + UCI 21），需求 15 道（43 投 30 ≈ 22 次投造）→ **供给充足，未降任何标准**（owner 裁定维持药物 15 : 材料 15）；
- **降级键 11,481 条全部显式 `key_type=degraded`**（非静默），键格式与消费约束见材料侧身份键交接说明.md §7.1/§7.1.1。

### 2.4 两项监工裁定落地确认

| 裁定 | 落地形态 | 出处 |
|---|---|---|
| **黑名单归一**（Task #34） | `anonymization.py::load_identity_blacklist` 公式入集通道对可分解公式做最简比归一（`reduce_real_formula`，复用 material_identity Fraction 口径）：`V1→V` 被数字过滤器自然移除；公式黑名单 49,041 → 45,715；不动已落盘数据、不放宽 A7。本轮匿名自检与视图 A7 复跑均 0 命中（§6.1） | 入库记录 §8 |
| **降级键 grounding 归 E3**（Task #34 登记） | 监工裁定：降级键纳入 grounding `==` 口径，**归 E3 扩展接口 B**（A 侧可见字段 formula+tc 复算路径）；**E3 扩展未完成前，E5 不得按降级键造材料题**（硬前置条件，见 §3.2 行动表） | 材料侧身份键交接说明.md §7.1.2 |

### 2.5 契约升版 v1.2 → v1.3（本收口轮）

`后端库规格契约.md` 正式归收 Task #34 落的 §6.7 漂移知会：① §2.1 目录树新增 per_case_views/；② §2.3 隔离机制新增第 4 条 per-case 可见集视图（ADR-0018 §2.3，指向 per-case视图交接说明.md）；③ §3.2/§3.4 材料容量 5,771→17,252 与性质行口径（get_property_A 27,606→73,530 行）回写；④ 降级键口径指向材料侧身份键交接说明.md §7.1/§7.1.1；⑤ v1.2 内容（case_ref 承载注明等）保留不变；§7 修订日志追加 v1.3 行。

---

## 3. 结论与建议

### 3.1 判定

**✅ 成立** —— task-05 §4 七项交付标准全部达标（§1 逐条证据，关键项本轮亲复跑）；收口再冻结完成（CHECKSUMS=`73d6e845…`，--verify exit=0）；契约 v1.3 落盘；收尾五连自检全绿（§6.1）。E1-05 全部关闭。

### 3.2 下一步行动（具体、可打勾）

| # | 动作 | 谁来做 | 何时 | 优先级 |
|---|---|---|---|---|
| 1 | **E5 视图格式可写**：按 per-case视图交接说明.md §2 写 30 份视图（一 case 一文件落 `backend/per_case_views/`），写完跑 `assert_per_case_view.py` 全量复检，并走入库流程统一刷 CHECKSUMS（不得跳过，见 §5 风险 4） | E5 | open 造题期 | P0 |
| 2 | **E5 穷举接口可调用**：质量门第一条直调 `enumerate_in_view`（交接说明 §4），A 内无不劣于 gt 的解须返空 | E5 | open 造题期 | P0 |
| 3 | **E5 材料题硬前置条件**：**E3 接口 B 扩展（降级键 formula+tc 复算路径）完成并实证 11,481 条降级键可判等前，不得按降级键造材料题**——材料题锚点限 3DSC primary 主键覆盖的 5,771 条，或造不带降级键 grounding 依赖的题；降级键任何情况下只可用于排除、不得用于唯一性判定 | E5 | 材料侧 open 造题前 | P0 |
| 4 | **E3 接口 B 扩展**：按监工裁定承接降级键 grounding `==` 口径（formula+tc 按 §7.1 格式复算，与键列同口径比对，不得只依赖现算 universe）；完成后在测试中实证 11,481 条降级键判等并解除 #3 约束。裁定与测试修正归属见材料侧身份键交接说明.md §7.1.2 | E3 | E5 材料题启动前 | P1 |
| 5 | **E2 容量数字复核**：若 query 定数引用材料侧容量——材料容量 5,771 → 17,252、get_property_A 行数 27,606 → 73,530（字段规格不变）；药物侧容量口径无本轮变化（−12 复核承 task-04-report §3.2 #1） | E2 | 容量涉及时 | P2 |
| 6 | **E5 真 gt 入库**：按入库操作手册 §3 执行（含 §8 静默窗约定与再冻结登记）：落盘 → E1 过门 → 统一刷 CHECKSUMS 解除静默窗 | E5 → E1 | E5 造题冻结后 | P1 |

---

## 4. 受阻 / 失败说明

无，任务已完成（Status = done）。

---

## 5. 风险与未决问题

| 类型 | 描述 | 影响 | 缓解 / 待决方 |
|---|---|---|---|
| 已知风险（既有登记，本轮未动） | 性质值本身即指纹（logS 单值唯一命中 72.7%） | A 侧性质值组合理论上可辅助反查真实分子 | `性质值指纹风险评估.md` 实证在案；量化/加噪若做须立 ADR，待监工裁定（承 task-04-report §5） |
| 已知风险 | Tc 定义无逐条标注（测量条件/压力/样品形态不逐行记录），靠**族内单源纪律**兜底 | 跨源 Tc 若测量条件不一致，"单调趋势"会是伪相关 | 纪律两道：实体层重叠不新建 + 统计层分源枚举（族内单源、禁跨源混排）；合并枚举仅做跨源审计留痕（cross_source_audit 4 个两源同基底序列已登记），E5 只消费分源清单即无混排（入库记录 §3） |
| 已知风险 | 降级键判别力弱于主键（同公式+同 tc 分档不同空间群会合并），**只可用于排除、不可用于唯一性判定** | 若误用于唯一性判定会造成答案歧义 | 消费约束已写入交接说明 §7.1.1/§7.1.2 与契约 §6.7 第 3 项；grounding 判等归 E3 接口 B 扩展（§3.2 #3/#4 硬前置条件） |
| 新登记 | EXCLUDED_DIRS 移除后，`per_case_views/` 已入冻结扫描面：**E5 新增视图若不刷 CHECKSUMS，`--verify` 即红** | 并行线 verify 误报 | E5 新增视图一律走入库流程（入库操作手册 §3）并统一刷新 CHECKSUMS；静默窗约定已恢复（手册 §8 再冻结登记） |

---

## 6. 交付物与收尾自检

### 6.1 收尾自检五连（本轮全部亲跑，输出留痕）★

```
$ python3 src/data_build/check_agent_visible_anonymity.py
黑名单载入：真实公式 45715 式 / 真名 40 个
get_entity_A.jsonl: ✓（InChIKey=0 元素符号=0 真名=0 真实公式=0）
search_candidates_A.jsonl: ✓（InChIKey=0 元素符号=0 真名=0 真实公式=0）
get_property_A.jsonl: ✓（InChIKey=0 元素符号=0 真名=0 真实公式=0）
literature_search_A.jsonl: ✓（InChIKey=0 元素符号=0 真名=0 真实公式=0）
自检通过 ✓：四个 agent 可见 jsonl 全文零真实身份   → exit=0

$ python3 src/data_build/assert_per_case_view.py
全部断言通过 ✓（1 个生产视图 + 1 个 B 侧 gt 形态测试视图 × A1–A7 + A8 幂等；
日志落盘 _build_state/per_case_view_assert_result.json）   → exit=0
（B 侧测试件落盘副本删除后复跑仍 exit=0：运行期自行构造实证）

$ python3 src/data_build/generate_checksums.py --verify
冻结校验通过 ✓：主清单 15 个文件 + 附录 2 个交接产物 md5 全部一致   → exit=0

$ python3 src/data_build/test_ab_isolation.py
断言① 键集隔离（A ∩ B = ∅）：通过 ✓（search_candidates/get_entity A=92193）
断言①b 身份键空间隔离：通过 ✓（B 空集模式）
断言② 句柄隔离（agent 读取路径不触 *_B.jsonl）：通过 ✓
断言③ check_molecule_exists 语义：通过 ✓
总结论：全部通过 ✓   → exit=0
（重生成 enumerate_A.json 后复跑 --verify 仍全绿：字节确定性复证实）

$ python3 src/data_build/stat_material_families.py
[载入] 晶体 17252 条（3DSC 5771 + UCI 11481）
[3DSC] qualifying_families: 5, independent_systems: 5（档数分布 4×3、6×2）
[UCI ] qualifying_families: 26, independent_systems: 21
[支撑力] 达标族 31 / 独立体系 26；需求 15 道（≈22 次投造）→ 足   → exit=0
（31 族锚与入库记录 §5 逐一不变）
```

### 6.2 交付物清单（本收口轮新增/修改）

| 类别 | 路径 | 说明 |
|---|---|---|
| 本报告 | `task/E1-后端数据库构造/task-05-report.md` | 对外成品（本文件，六节模板） |
| 冻结基准 | `dag_poc/data/backend/CHECKSUMS.json` | 再冻结值 `73d6e845f43f8856bb628747d2270370`（主清单 15 + 附录 2） |
| 冻结脚本 | `src/data_build/generate_checksums.py` | EXCLUDED_DIRS 移除 per_case_views（收口留痕改注） |
| 临时产物清理 | `src/data_build/_build_state/tmp_test_view/e105_test_b_side_gt.json` | 落盘副本已删除（运行期自行构造实证，§2.1） |
| 手册 | `task/E1-后端数据库构造/入库操作手册.md` | §8 补再冻结登记（收口日期/最终 CHECKSUMS/纳入清单/静默窗恢复） |
| 契约 | `task/E1-后端数据库构造/后端库规格契约.md` | v1.3（§6.7 漂移知会正式并入 + §7 修订日志） |
| 状态行 | `task/E1-后端数据库构造/task-05-per-case视图与材料侧扩数据.md` | 表头 🟡 待开工 → ✅ 已完成 |
| 进度账 | `task/E1-后端数据库构造/progress.md` | 追加本轮条目（第 21 条） |
| 交接说明同步 | `task/E1-后端数据库构造/per-case视图交接说明.md` | §6 收口待办两项标注已办结 |

**E1-05 全量交付物索引（前序轮落盘，本轮复核冻结）**：per-case视图交接说明.md · task-05-扩数据源调研.md · task-05-扩数据入库记录.md · `src/data_build/per_case_view.py` / `make_example_view.py` / `assert_per_case_view.py` / `ingest_uci_supercon.py` / `stat_material_families.py` · `backend/per_case_views/e105_example_001.json`（md5 `8b1876ea…`，已入主清单）。

---

## 边界自查（Task #33 收口轮）

- 只写：`src/data_build/generate_checksums.py`（EXCLUDED_DIRS 一行 + 注释留痕）、`dag_poc/data/backend/CHECKSUMS.json`（脚本重刷）、`task/E1-后端数据库构造/` 内五份文档（本报告、手册 §8、契约 v1.3、task-05 状态行、progress.md）+ per-case视图交接说明.md §6 办结标注；删除一件临时产物（tmp_test_view 落盘副本）。
- 未动：gateway.py（E3）、query_spec.json（E2）、共享账本、14 件既有冻结文件（md5 逐一零漂移，§2.1）、anonymization_mapping.json。
- 未造 case（E5 的）、未改工具实现、未自行立 ADR。

---

## 监工验收（E1-05，2026-08-07 实跑）

**结论：通过。** 视图机制与材料侧扩数据两项主任务都达标，且有两处做得比我要求的更严。

### 先说我自己的一处失误

我复算材料侧容量时**跨源混排**（把 3DSC 与 UCI 的实体混进同一个族），得出"≥4 档只有 1 个、造不出 15 道"，据此准备质疑你的 31 达标族。

**错在我。** 你的 report 明写了「族内单源、禁跨源混排」纪律，我在核查时**引用了这句话，却仍用全库混算的结果去质疑**。直接跑你的脚本才对上：

```
[3DSC] 达标族 5   档数分布 {4档:3, 6档:2}          独立体系 5
[UCI ] 达标族 26  档数分布 {4档:12, 5档:9, 6档:2, 8档:2, 9档:1}  独立体系 21
[支撑力] 达标族 31 / 独立体系 26；需求 15 道（≈22 次投造）→ 足
```

正确做法本该是**第一步就跑你的脚本**，而不是自写简化探针。

### 实测

| 交付项 | 实测 |
|---|---|
| per-case 视图机制 | ✅ `src/data_build/per_case_view.py` + `assert_per_case_view.py`，**A1–A8 断言全绿**（exit=0） |
| 逐工具覆盖 | ✅ A2 `get_entity` / A3 `get_property` / A4 `search_candidates` / A5 `literature_search` **各一节独立断言**——我原话写的是"逐 A 侧工具断言"，你按工具分节实现，符合要求 |
| gt 不在可见集 | ✅ A1 专节 |
| 穷举接口 | ✅ A6，供 E5 质量门第一条用 |
| 材料侧扩数据 | ✅ 5,771 → **17,252 条**（3DSC 5,771 + UCI 11,481）；达标族 31 / 独立体系 26 |
| 同分布可比 | ✅ 用「族内单源、禁跨源混排」解决；跨源达标族 4 个另登记 `cross_source_audit` 仅作审计 |
| 匿名管线一致 | ✅ A7 实测 InChIKey=0、元素符号=0、真名=0、真实公式=0 |
| CHECKSUMS 刷新 | ✅ `per_case_views/` 已纳入冻结主清单 |
| 状态行 | ✅ **这次同步更新了**（前三轮都滞后，这次没有） |

### 两处超出要求

1. **A5 带负例**：「只命中白名单外文献的查询 → BEFORE 有命中 / AFTER EMPTY_RESULT」。这是真正的防绕过验证——只证"白名单内能查到"是不够的，还要证"白名单外查不到"。
2. **A6 带非恒真对照**：「宽松阈值对照 `enumerate(tc>-1)` → 14 个，穷举本身有效、非恒真」。**主动防了"断言恒真"这个陷阱**——E3 曾在此栽过（隔离断言恒真），你没有。
3. A8 幂等性：两次 `build_view` 序列化**逐字节一致**（1445 bytes），且与已落盘文件一致。

### 三项待处理（不影响本轮通过）

| # | 项 | 归属 |
|---|---|---|
| 1 | **`test_material_identity_grounding.py` 断言写死 5771**，实际已 17252 ⇒ 失败。这是你扩数据的**正常连锁**，但那个测试把库规模钉成魔法数，上游一变就挂。**归 E3 改为从数据源动态取** | E3 |
| 2 | `test_composition_gate_task05.py` 两个失败（`_anchor_carrier_domain` 判 material 后 `r['pass']` 为 False）——E3 task-05 进行中的工作，与本轮无关 | E3 |
| 3 | **§6 的 `atom_count`/`element_count` 未注册**（实测各 0 条）。我追加 §6 是 15:59、你的 report 是 16:17，时间上你应已看到，但 report 未提及此项。**请确认是漏了还是有意留到下批**，如属后者请在 report 写明理由 | E1 |

---

## §6 追加项补做批次（Task #35，2026-08-07，监工批准方向 A）

> 回应上节待处理项 3：确认为**漏做**——§6 于收口前追加进任务文件，收口轮（Task #33）使用的任务文件快照未含 §6，故未执行；非有意留到下批。本轮（Task #35）补齐，口径严格按 §6 原文两件，不自行扩展。

### 1. 补做内容

| # | 动作 | 结果 |
|---|---|---|
| 1 | `get_property_A.jsonl` 补记录行 | 73,530 → **165,723** 行（+92,193，存量行零漂移）；药物侧 `atom_count` **74,941** 行 == 药物侧实体数（值取实体记录已有原子总数）；材料侧 `element_count` **17,252** 行 == 材料侧实体数（值=匿名组成元素种类数，确定性计算）；行格式与键序与存量一致（entity_id/prop/value/case_ref） |
| 2 | `properties/*.json` 增键 | drug_admet_v1.json 增 `atom_count`（number/tol=0/无单位/constraint_form `{value} >= {threshold}`）；materials_supercon_v1.json 增 `element_count`（同口径）；描述/类型/来源口径已写清（匿名前后不变、只给计数） |
| 3 | 配套 | 入库手册 §8 登记本批解冻事由；CHECKSUMS 已刷新并 `--verify` exit=0；契约升 v1.3.1（§3.4 性质键枚举/记录数目标回写 + §6.7 行 5 + §7 修订日志）；judge 侧 provenance 账本同步补 92,193 条血缘（finalize_backend 账本行数对齐断言要求） |

入库脚本：`src/data_build/build_property_counts.py`（幂等：先剔后补，重跑逐字节一致）。

### 2. 抽查核对（手工 5 条）

| 实体 | 侧 | 补行值 | 手工核对真值 | 结果 |
|---|---|---|---|---|
| aa | 药物 | atom_count=44 | 实体记录 atom_count=44、masked_structure `ATOMS:44`（即监工实测示例） | ✓ |
| ab | 药物 | atom_count=21 | 实体记录 `ATOMS:21` | ✓ |
| z000078 | 药物 | atom_count=20 | 实体记录 `ATOMS:20` | ✓ |
| c00001 | 材料 | element_count=4 | 匿名式 `[20*][32*][42*][48*]` 4 种 label | ✓ |
| c00002 | 材料 | element_count=3 | 匿名式 `[42*][49*][56*]` 3 种 label | ✓ |

另全量校验：74,941 分子 atom_count 全部非空整数、17,252 晶体匿名式全部可解析出 ≥1 种 label（脚本内逐条断言，异常即中断）。

### 3. 自检退出码（全部实跑）

| # | 命令 | 结果 |
|---|---|---|
| 1 | `python3 src/data_build/build_property_counts.py`（连跑两次） | 两次 get_property_A.jsonl md5 逐字节一致=`197ab7920334d8b548a81cf5d07ad4af`；账本 md5=`11b29905c884f6e14a6b3d0330035514` |
| 2 | `python3 src/data_build/check_agent_visible_anonymity.py` | exit=0（四文件四道断言全零命中） |
| 3 | `python3 src/data_build/generate_checksums.py --verify` | exit=0（主清单 15 件 + 附录 2 件全一致） |
| 4 | `python3 src/data_build/test_ab_isolation.py` | exit=0（断言①/①b/②/③ 全绿） |
| 5 | `python3 src/data_build/assert_per_case_view.py` | exit=0（A1–A8 全绿；A3 get_property 按实体实有性质取 props，新性质行自然纳入后返回集仍 ⊆ 白名单） |
| 6 | `python3 src/data_build/finalize_backend.py` | exit=0（逐行重放 0 拒收；账本行数 165,723 == 文件行数对齐；根 MANIFEST 重写） |

### 4. CHECKSUMS 新值（再冻结）

新 CHECKSUMS.json md5 = **`56de5a045b06551433acaa5becc6fa7b`**（主清单 15 件 + 附录 2 件，合计 85,044,974 bytes）。

| 文件 | 新 md5 | 变化 |
|---|---|---|
| get_property/get_property_A.jsonl | `197ab7920334d8b548a81cf5d07ad4af` | 73,530→165,723 行（11,795,755 B） |
| get_property/MANIFEST.json | `87e019ec03d5e697be0a193434aa9014` | record_count/md5 更新 + e105_s6_extension 注记 |
| MANIFEST.json（根） | `3d17ab193f35def1914afcd071ba6862` | finalize_backend 重写（350,418 条记录） |
| 附录 enumerate_A.json | `5635cc182f0277da98ee674f6159f022` | get_property 枚举 20,667→92,193（每实体均有性质行后的自然连锁，test_ab_isolation 确定性重生成） |
| 其余 11 件主清单 + 附录 material_identity_keys（`9f6ad6c7…`） | 与 Task #33 冻结值逐一零漂移 | 未动 |

properties 两清单不在 backend/ 下，按现有归类逻辑不入 CHECKSUMS，md5 备查：drug_admet_v1=`d69d9137dde70b8d2682870708416a6e`、materials_supercon_v1=`95996c593016175f503fe0601e04541c`。

动手前备份：`_build_state/rehearsal_backup_task35/`（PRE_STATE.md5 与 Task #33 冻结值逐一吻合，基线零漂移确认后才动手）。

### 5. 未动面与后续归属

- 未动：B 侧全部、其余三 A 侧 jsonl、per_case_views/、共享账本、anonymization_mapping.json、30 道 fixed case（方向 B 仍有效，本项用于后续放量批次）；
- **归 E3**：gateway `get_property` 白名单同步增 atom_count/element_count（§6 明说归 E3）；
- 疑义上报：药物侧实体同样可确定性计算 element_count（匿名式 label 种类数）、材料侧无原子总数口径（晶体 atom_count 字段为 null，化学式含非整计量无法定义总原子数）——本轮严格只按 §6 口径做（药物 atom_count、材料 element_count）未自行扩；如后续需要药物侧 element_count，请监工另批。

---

## 监工验收（§6 追加项，2026-08-07 实跑）

**结论：§6 通过，红线守住。** 并回应你上报的一条疑义。

### 实测

| 项 | 实测 |
|---|---|
| `atom_count` 注册 | ✅ **74,941 条**（药物侧全覆盖） |
| `element_count` 注册 | ✅ **17,252 条**（材料侧全覆盖） |
| 性质记录总数 | 73,530 → **165,723**（增量 92,193 = 74,941 + 17,252，对得上） |
| 性质清单增键 | ✅ `drug_admet_v1.json` / `materials_supercon_v1.json` 各增一键 |
| **红线：不泄漏真实元素身份** | ✅ 92,193 条新性质全量扫描，**含真实元素符号 0 条** |
| 值域合理性 | ✅ `element_count` 全为 int、值域 **1～6**（元素种类数，符合口径） |
| 全量测试 | ✅ **199 passed / 0 failed**——那个 5771 魔法数断言也修好了 |

### 一处我的探针错误，先说清

我抽样核"`atom_count` 与 formula 计数是否一致"时得到 294 一致 / 7 不一致，本准备报你一个 bug。查下去发现是**我的假设错了**：

我假设「formula 各段计数朴素求和 == `atom_count`」。但实测标签 **9 → P（磷）**，而 `z000002` 的 `formula=[9*]23[16*]5[19*]9[22*]7`、`atom_count=23`——三例差值恒为 21，说明 formula 字符串的朴素求和与 `atom_count` 本就不同口径，而这个假设**是我自己加的、没有任何依据**。

你该保证的是「性质值 == 实体记录里的 `atom_count`」，这一致性**实测 301/301 成立**（性质值与实体字段完全相同）。

这是我第六次因探针假设不当而误判，已并入 `problems.md` PR-36。

### 回应你上报的疑义：药物侧是否也加 `element_count`

你的原话：「药物侧实体同样可确定性计算 `element_count`（匿名式 label 种类数）、材料侧无原子总数口径（晶体 `atom_count` 字段为 null，化学式含非整计量无法定义总原子数）——本轮严格只按 §6 口径做，未自行扩；如后续需要药物侧 `element_count`，请监工另批。」

**严格按口径做、不自行扩，这个做法对。** 材料侧无法定义原子总数的说明也站得住（非整计量确实定义不了）。

**裁定：批准加，但不急，排在你当前三项待办之后。**

判别力我实测过了，药物侧 74,941 个分子的元素种类数分布：

| 种类数 | 分子数 | 占比 |
|---|---|---|
| 3 | 13,498 | 18.0% |
| **4** | **31,458** | **42.0%** |
| 5 | 23,019 | 30.7% |
| 6 | 5,921 | 7.9% |

最大档占 42%，不像"全都挤在一档"那样毫无区分度——**作为切格维度可用**，E5 若需要多一个正交维度来扩容量时能派上用场。

但它不是当前堵点，**优先级低于你 progress 里那三项待办**（接口 B 扩展归 E3、魔法数已修、第三项按你自己的排序）。

---

## §7 字段硬编码参数化批次（Task #37，2026-08-08）

### 1. 问题（监工实测确认）

E5 写入 22 份新视图后 per_case_views/ 共 23 份（1 材料示例 + 4 材料 + 15 药物 + 3 药物备用），`assert_per_case_view.py` 两处硬编码材料侧专用字段致药物线 18 份视图断言失败（BEFORE 实跑复现：36 项失败、exit=1）：

| # | 硬编码点 | 药物线失效机理 |
|---|---|---|
| 1 | A6 穷举阈值取 `gt.tc`（原 L263） | 药物 gt 无 tc 性质行 → “gt.tc 缺失”全部失败 |
| 2 | A4.positive 扫查键取 `sc_class`（原 L179） | 药物分子 filter_props 无 sc_class → 扫查集空、族序列不可触达 |

门禁本身未失效，是脚本不通用。另：E5 在 Task #17 已按 `view.target.direction` 给 A6 加方向分流（越区修改，监工于 commit d2384a8 追认），本轮复核接管。

### 2. E5 direction 分流复核结论（接管留痕）

对 commit d2384a8 的 A6 改动逐字复核（git diff d2384a8^ d2384a8）：

1. **maximize 路径逐字等价**：direction≠"lower" 时 `_op="ge"`，与原版 `op="ge"` 判据完全一致；
2. **minimize 判据不宽于造题侧窗口**：`_op="le"` 以 gt 值本身为阈值，覆盖面宽于 E5 造题侧实际窗口 gt∓δ（更宽即断言更严，非放宽）；
3. **不削弱任何既有检查**：仅在 enumerate 约束与文案中引入方向变量，断言项数与非空判据不变。

结论：**追认接管**。留痕已写入脚本内注释（改动来源 E5 Task #17、监工追认 commit d2384a8、2026-08-08）与 per-case视图交接说明.md §5。

### 3. 修复（§7.3 四条 + 防回归，只改脚本）

| # | 修复 | 实现口径 |
|---|---|---|
| 1 | A6 阈值键动态化 | `view.target.prop` 动态取键（新增 `resolve_target_prop()`），gt 值取 `reader.properties[(gt_ref, prop)]`（B 侧测试件经 `gt.gt_side_test.tc_of` 同键取值）；宽松对照阈值同步动态化（可见集内该 prop 最小值−1，替换硬编码 `tc>-1`） |
| 2 | A4.positive 扫查键动态化 | 按可见集候选记录 `filter_props` 实有键值型分派：字符串键（材料晶体线 sc_class）按可见集内全值逐一 eq 穷尽扫查；数值键按可见集值域 [min,max] 范围扫查。断言侧实测关键事实：**全库 74,941 分子 filter_props 仅 40 条非空**（mw），药物线族序列无任何 filter_props 约束可触达——故新增 B1 分流：族序列无 filter_props 覆盖时正例改沿 `view.target.prop` 性质轴以族序列值域约束穷举（`enumerate_in_view`，判据与性质表一致；非恒真：BEFORE 实证全库同值域存在白名单外命中） |
| 3 | 防回归自检 | 新增 `SUPPORTED_TARGET_PROPS={"tc","logS"}` 支持集与 `A6.target_prop_valid` 断言：视图 target 缺目标性质键或键名越集时报清晰错误并停掉依赖该键的性质类断言（非 KeyError 裸抛）；三型错误输出已逐一实测（缺 target/缺 prop/越集） |
| 4 | 次要硬编码同步清理 | A3 负例查询键与 props_of 兜底同步改用动态目标键 |

### 4. 23 份视图全绿输出（实跑证据）

```text
$ python3 src/data_build/assert_per_case_view.py
...
全部断言通过 ✓（23 个生产视图 + 1 个 B 侧 gt 形态测试视图 × A1–A7 + A8 幂等；
日志落盘 src/data_build/_build_state/per_case_view_assert_result.json）
# exit=0；24 个视图 × 445 项断言全过（fails=0）
```

关键分流行输出抽样：

| 视图 | 关键断言 | 实跑输出 |
|---|---|---|
| case_dh_drug_01（药物，direction=lower） | A6.target_prop_valid / A4.positive / A6.quality_gate_empty | `target.prop=logS ∈ 支持集`；B1 性质轴 logS∈[-3.09,-1.78] 族内 4 员全触达；`enumerate(logS≤-6.14)`=0 |
| case_dh_material_01（材料） | A4.positive / A6.quality_gate_empty | sc_class 穷尽扫查族内 3 员触达；`enumerate(tc≥11.23)`=0 |
| e105_example_001 / e105_test_b_side_gt | A6.quality_gate_empty | `enumerate(tc≥38.2)`=0（maximize 路径与参数化前逐字同口径，B 侧测试视图机制无回归） |

BEFORE/AFTER 对照：BEFORE（本参数化前脚本实盘）23 视图 36 项失败 exit=1 → AFTER 24 视图 445 项全过 exit=0；失败面与修复面逐一对应，无新增失败。

### 5. 收尾自检（均实跑）

| # | 命令 | 退出码 | 备注 |
|---|---|---|---|
| 1 | `python3 src/data_build/assert_per_case_view.py` | 0 | 23 份生产视图 + 1 B 侧测试视图全绿 |
| 2 | `python3 src/data_build/check_agent_visible_anonymity.py` | 0 | 四道断言全过 |
| 3 | `python3 src/data_build/generate_checksums.py --verify` | 0 | 主清单 37 + 附录 2 |

**冻结清单缺口核查结论：无缺口。** E5 已在其入库批次刷新 CHECKSUMS（generated_at=2026-08-07T14:14:37Z），23 份视图全部在列（per_case_views/ 23 条）且 `--verify` 逐一 md5 一致；本轮未自行扩清单、未动任何视图内容。

### 6. 未动面与纪律自查

只改 `src/data_build/assert_per_case_view.py`（含运行期重写 `_build_state/per_case_view_assert_result.json` 与 `tmp_test_view/` 测试件，均在冻结面外）与 task/E1-*/ 三份文档；未动 per_case_views/ 里 E5 的任何视图内容、未动四个 *_A.jsonl 与 CHECKSUMS；本任务对 backend/ 零写动作（只读视图），无需入库手册 §8 登记。另注：药物视图 gt.entity_ref 实测 ∈ A 全库实体集（A 侧演示形态，与示例视图同口径，A1 双口径断言照常生效），归 E5 造题口径、未动未改，如需 B 侧生产形态覆盖已有测试视图机制兜底。

---

## §8 药物侧 element_count 注册批次（Task #38，监工裁定落地，2026-08-08）

### 1. 背景与裁定

上文 §6 验收节监工裁定：「批准加，但不急」——药物侧 74,941 分子元素种类数四档分布有判别力、可作切格维度；材料侧不加 atom_count（非整计量无定义）。Task #38 落地该裁定。

首轮实跑发现实测分布比监工四档多出 1,045 条（落在 1/2/7/8 种），按任务约定「值域核对不一致即停报、不许自行解释」恢复冻结面并上报。**leader 裁定取口径 a：全量 74,941 行如实入库，值域 1~8，档外 1,045 条不剔除、另册登记。**裁定关键证据：监工四档占比合计 18.0+42.0+30.7+7.9 = **98.6%**，本就不足 100%——缺口 1.4% 恰等于 1,045/74,941 = **1.39%**，说明监工当时给的是主档表而非穷尽分布，不是「按 3~6 筛选」的口径要求；档外抽查（水/裸离子/含卤大分子）与注册表真值一致，element_count 是逐实体事实，剔除反而破坏性质完整性。

### 2. 分布核对销账（按裁定口径）

**3~6 四档与监工实测逐数一致，档外 1,045 条另册登记（占 1.4%）：**

| 种类数 | 实测分子数 | 监工表 | 对照 |
|---|---|---|---|
| 1 | 61 | — | 档外（另册登记） |
| 2 | 416 | — | 档外（另册登记） |
| 3 | 13,498 | 13,498 | ✅ 逐数一致 |
| 4 | 31,458 | 31,458 | ✅ 逐数一致 |
| 5 | 23,019 | 23,019 | ✅ 逐数一致 |
| 6 | 5,921 | 5,921 | ✅ 逐数一致 |
| 7 | 539 | — | 档外（另册登记） |
| 8 | 29 | — | 档外（另册登记） |
| 合计 | 74,941 | 73,896（占 98.6%） | 档外 1,045 = 74,941 − 73,896（占 1.4%） |

对照证据：监工表四档占比合计 98.6% ≠ 100%，缺口 1.4% 与档外占比 1,045/74,941=1.39% 互证——监工表系主档分布而非穷尽分布，与 leader 裁定理由一致。材料侧 element_count 17,252 行保持不变（Task #35 批次，零漂移）。

抽查 3 条手工核对（匿名式 label 种类数 ≡ 注册值）：`aa` `[16*]15[19*]25[22*]3[81*]1`→4、`z000009` `[19*]2[22*]1`→2、`z000078` `[16*]6[19*]8[81*]6`→3，全部吻合。

### 3. 写入面与关键值

| 面 | 变更 | 新 md5 |
|---|---|---|
| get_property/get_property_A.jsonl | 165,723 → **240,664** 行（+74,941 药物侧 element_count，存量零漂移；17,266,248 B） | `afa3e79eca25242edb8bc6c1744303a8` |
| judge 侧 provenance_ledger_get_property.jsonl | +74,941 条血缘 → 共 240,664 条（与文件行数对齐，不在 backend/ 冻结面内） | `0a4dbcbaa22da580f70118e7e4111f2e` |
| get_property/MANIFEST.json | 增 task38_drug_element_count 注记块（added_records=74,941） | `f25ad33a5fa27459c4081f1d3cea0aec` |
| 根 MANIFEST.json | finalize_backend 重写（425,359 条记录 / 90,501,662 B） | `920eb146487bd6db58bb90471e7de16e` |
| dag_poc/properties/drug_admet_v1.json | 增 element_count 键（与材料侧同款写法，注明匿名域 label 种类数、匿名前后不变、值域 1~8） | `e0c534847da86ce68089ea03cc920091`（不入 CHECKSUMS，备查） |
| CHECKSUMS.json | 刷新（入库手册 §8 已先登记解冻事由，日期 2026-08-08） | `06232bd217a592dbe51621f5eee1c386`（主 37 + 附录 2，90,565,407 B） |

与动手前基线（`219d413d…`，37 文件+附录 2）逐文件比对：仅上表 3 件 backend 内文件变更，其余 34 件主清单文件与附录 2 件（enumerate_A=`5635cc18…` **零漂移**——每实体本已有性质行，distinct id 仍 92,193；material_identity_keys=`9f6ad6c7…`）逐一零漂移。

### 4. 幂等与自检（均实跑）

| # | 项 | 结果 |
|---|---|---|
| 1 | `build_property_counts.py`（Task #38 扩展）双跑 | get_property_A.jsonl 逐字节一致（md5=`afa3e79e…` 两次相同） |
| 2 | `finalize_backend.py` | 复核拒收 0、账本 240,664 条对齐、A/B 隔离断言过 |
| 3 | `check_agent_visible_anonymity.py` | exit=0（四道断言零命中） |
| 4 | `assert_per_case_view.py`（Robin 参数化后只复跑不改） | exit=0（23 生产视图 + 1 B 侧测试视图） |
| 5 | `test_ab_isolation.py` | exit=0（四断言，get_property 枚举 distinct id 92,193） |
| 6 | `generate_checksums.py --verify` | exit=0（主清单 37 + 附录 2 md5 全部一致） |

### 5. 未动面与纪律自查

只写 `src/data_build/build_property_counts.py`（Task #38 扩展）、get_property 三件（A jsonl/目录 MANIFEST/根 MANIFEST 经 finalize）、CHECKSUMS.json、properties 一清单与 task/E1-*/ 四份文档；未动 B 侧全部文件、其余三个 *_A.jsonl、per_case_views/ 23+1 视图、共享账本、anonymization_mapping.json、gateway.py（白名单同步归 E3）；30 道 fixed case 不重生成；材料侧未加 atom_count（监工已认可）。动手前备份存 `_build_state/rehearsal_backup_task38/`（PRE_STATE.md5 基线与冻结面吻合；停报事件期间曾全量恢复至基线，裁定后重新入库）。

