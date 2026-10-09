# E1-05 · per-case 可见集视图机制 — 交接说明（供 E5 与收口轮）

| 字段 | 值 |
|---|---|
| 任务 | Task #30（task-05 §1 主任务一）；依据 ADR-0018 §2.3/§3/§9 交付标准 1/4 |
| 交付方 | E1 数据线（2026-08-07） |
| 消费方 | **E5**（按 §2 格式写 30 份视图、按 §4 调穷举接口过质量门）· **收口轮**（§6 CHECKSUMS 待办） |
| 断言实跑 | `assert_per_case_view.py` 全过，退出码 0（§5 附命令与关键输出；Task #37 §7 字段硬编码参数化后复跑，23 份生产视图 + B 侧 gt 形态测试视图全绿） |

**一句话机制**：A/B 物理分层不动（一侧一文件，读取层只开 `*_A.jsonl`）；per-case 视图是在 **A 内部再切一层**——每个 open case 一份可见集白名单（`backend/per_case_views/<case_id>.json`），agent 在该 case 下的四个 A 侧工具查询一律被限制在白名单内，gt 结构上不可达。

---

## 1. 交付物清单

| # | 文件 | 角色 |
|---|---|---|
| 1 | `dag_poc/data/backend/per_case_views/e105_example_001.json` | 完整示例视图（n=4/d=1/K=10 + gt 声明），E5 照抄 |
| 2 | `src/data_build/per_case_view.py` | 视图载入/校验 + `ViewedBackendReader` 视图化读取器 + `enumerate_in_view` 穷举纯函数 + CLI |
| 3 | `src/data_build/make_example_view.py` | 示例生成器（确定性选料，重跑逐字节一致，md5=`8b1876ea2ea30ebabff503babed0a538`） |
| 4 | `src/data_build/assert_per_case_view.py` | 可执行断言集（A1–A8，退出码 0/1/2 可判） |
| 5 | `src/data_build/generate_checksums.py` | 静默窗内曾暂排除 `per_case_views/`；**Task #33 收口轮已移除排除并纳入主清单冻结**（脚本内留痕可查） |

未动：`gateway.py`、`query_spec.json`、共享账本、既有 14 件冻结文件（md5 复验一致）。

---

## 2. 视图定义格式（E5 直接按此写）

落盘路径：`dag_poc/data/backend/per_case_views/<case_id>.json`，一 case 一文件。

| 字段 | 类型 | 约束 |
|---|---|---|
| `schema` | string | 必须 `"per_case_view_v1"` |
| `case_id` | string | 与文件名一致 |
| `gradient` | object | `n`≥1（族内样本数）；`d`∈1..2（外推距离，goal §2b④ 禁 ≥3）；`K`≥10（干扰项，ADR-0018 §3.2 强制，默认 20～30）——三值显式记录即 ADR-0018 §9 交付标准 2 |
| `target` | object | **必含目标性质键 `prop`**（Task #37 §7 格式要求，供后续视图作者）：`prop` 为 A6 穷举质量门与 A4 正例分流的目标性质键（如 tc/logS），必须在断言集支持集 `SUPPORTED_TARGET_PROPS={"tc","logS"}`（`assert_per_case_view.py` 顶部常量）内——新增目标性质键的视图线入库前须先扩支持集并评审其 A4/A6 断言语义，缺失或越集断言集以清晰错误报出（A6.target_prop_valid）而非 KeyError 裸抛；另含 `direction`（gt 相对方向，higher/maximize 或 lower/minimize，A6 据此分流判据）+ 自由 note |
| `family_sequence` | string[] | 族内序列实体匿名 ID，**沿外推轴有序**，长度必须 == n |
| `distractors` | string[] | 干扰项实体匿名 ID，长度必须 == K，与 family_sequence 不相交 |
| `gt` | object | `side` 必须 `"B"`；`entity_ref`（B 侧 gt 匿名 ID；生产 case 经 ingest_open_gt 入库后填 B 侧 ID）与 `composition`（匿名域元素计数多重集 `{标签号: 计数}`，ADR-0018 §5 组成判法）不得双空；**gt 一律不得进可见集**（载入期即拦） |
| `literature_doc_ids` | string[] | 该 case 可见的文献 doc_id 白名单（可空；空则 literature_search 一律 EMPTY_RESULT） |
| `visible_entity_ids` | string[] | 可选；若写则必须 == family_sequence ∪ distractors 的排序去重集 |
| `provenance` | object | 建议带 generated_by/basis/note |

**载入期校验**（`per_case_view.load_view`，违规即 raise，全清单回显）：schema/梯度参数/长度一致性/集合不相交/gt 不在白名单/doc_id 无重复/显式白名单与推导集一致。

**完整示例**：见 `backend/per_case_views/e105_example_001.json`（材料线：family=Other，沿匿名标签 68 计数 0.5→0.6→0.7→0.8 且 tc 严格递增，gt=c01360（计数 0.9、tc=38.2）不在可见集；干扰项 10 个取自其余超导体族；文献白名单 5 篇）。重生成命令见 §5。

**注意**：示例的 `gt.entity_ref` 指向 A 库内实体仅是机制演示（证明"存在于全库但视图拦得住"）；生产 case 的 gt 属 B 侧，entity_ref 填 B 侧匿名 ID，同时建议填 `composition` 供组成判分与穷举用。

---

## 3. 视图化读取器（四工具生效的机制载体）

`per_case_view.ViewedBackendReader(case_id=…)` 包在 E1 参考读取器 `backend_reader.BackendReader`（契约 §6 实现）外，四工具行为：

| 工具 | 视图行为 |
|---|---|
| `get_entity(id)` | id ∉ 白名单 → `UNKNOWN_ENTITY`（与"不存在"同码，防源探测） |
| `get_property(id, props)` | 同上 |
| `search_candidates(constraints, k)` | 命中集 ∩ 白名单；空则 `EMPTY_RESULT`（禁 fallback 语义不变） |
| `literature_search(query)` | papers ∩ `literature_doc_ids`；空则 `EMPTY_RESULT` |

E3 后续把视图接入 gateway 时只需在 agent 会话初始化处用本读取器替换裸读取（接口签名与契约 §6.2–6.4 一致），不改工具语义。

---

## 4. 穷举接口（E5 质量门第一条直调）

**纯函数**：

```python
from src.data_build.per_case_view import load_view, enumerate_in_view
hits = enumerate_in_view(view, property_lookup, entity_formulas,
                         constraints=[{"property": "tc", "op": "ge", "threshold": gt_tc}])
assert hits == []   # A 内无不劣于 gt 的解 → 质量门第一条通过
```

- `property_lookup`：`(entity_id, prop) → value`（即 `BackendReader.properties`）；
- `entity_formulas`：`{entity_id: 匿名式}`（仅 composition 约束需要）；
- `constraints`：`[{property, op, threshold}]`，op ∈ lt/le/gt/ge/eq/ne，与 search_candidates 同口径；性质缺失判不满足；
- `composition`：`{标签号: 计数}` 多重集全等判定（匿名式小数计数容差 1e-9）；
- 返回按 id 升序 `[{"id", "values"}]`；空即无解。

**CLI**（同一口径）：

```bash
python3 src/data_build/per_case_view.py list
python3 src/data_build/per_case_view.py show --case e105_example_001
python3 src/data_build/per_case_view.py enumerate --case e105_example_001 \
    --constraint tc:ge:38.2                       # → returned 0（质量门口径）
python3 src/data_build/per_case_view.py enumerate --case e105_example_001 \
    --constraint tc:ge:3 --composition "[45*]2[68*]0.7"   # → c01352
```

实测：`tc≥38.2`（gt 值）返 0 个；宽松对照 `tc>-1` 返 14 个（穷举非恒真）。

---

## 5. 断言清单与实跑证据

`python3 src/data_build/assert_per_case_view.py`（2026-08-08 Task #37 §7 字段硬编码参数化后复跑，退出码 0；日志 `_build_state/per_case_view_assert_result.json`；覆盖 23 份生产视图：1 材料示例 + 4 材料 + 15 药物 + 3 药物备用 + 1 B 侧 gt 形态测试视图）：

**动态取键口径（Task #37 §7，替换两处材料侧硬编码）**：
- **A6 阈值键**：由 `view.target.prop` 动态取（材料 tc、药物 logS），gt 值取 `reader.properties[(gt_ref, prop)]`（B 侧测试件经 `gt.gt_side_test.tc_of`）；宽松对照阈值同步动态化（可见集内该 prop 最小值−1，替换硬编码 `tc>-1`）。
- **A4 扫查键**：按可见集候选记录 `filter_props` 实有键值型分派——字符串键（材料晶体线 sc_class）按可见集内全值逐一 eq 穷尽扫查；数值键按可见集值域 [min,max] 范围扫查；若族序列无 filter_props 约束可覆盖（药物分子线现状：全库 74,941 分子 filter_props 仅 40 条非空，断言侧实测），正例分流走 `enumerate_in_view` 沿 `view.target.prop` 性质轴以族序列值域约束穷举（非恒真：BEFORE 实证全库同值域存在白名单外命中）。
- **防回归自检**：新增 `A6.target_prop_valid`——视图 target 缺目标性质键或键名不在支持集时报清晰错误并停掉依赖该键的性质类断言（非 KeyError 裸抛）。
- **E5 direction 分流接管留痕**：A6 按 `view.target.direction` 分流（higher→op=ge / lower→op=le）的改动来源 E5 Task #17（越区修改，监工于 commit d2384a8 追认，2026-08-08），Task #37 复核接管；复核结论：maximize 路径与原版判据逐字等价、minimize 判据以 gt 值本身为阈值宽于造题侧窗口 gt∓δ（断言更严非放宽）、不削弱任何既有检查。脚本内注释已同步留痕。

| # | 断言 | 正例/负例 | 实跑关键输出 |
|---|---|---|---|
| A1 | gt 不在自己可见集（entity_ref + 组成双口径；两形态分流） | A 侧演示形态：gt=c01360 全库存在，BEFORE 可触达 / AFTER UNKNOWN_ENTITY；B 侧生产形态（测试视图）：gt_ref ∉ A 全库实体集，BEFORE 组成扫查命中 1 个 / AFTER 可见集内 0 个 | 双形态均过 |
| A2 | get_entity ⊆ 白名单 | 正例 c01350 查得到；负例 aa（全库∉白名单）UNKNOWN_ENTITY | 白名单 14 实体逐一取件全过 |
| A3 | get_property ⊆ 白名单 | 正例 c01350 返 2 值；负例 aa UNKNOWN_ENTITY | 14 实体逐一查证全过 |
| A4 | search_candidates ⊆ 白名单（扫查键动态化） | 材料线正例：sc_class 穷尽扫查族内全员触达；药物线正例（B1 分流）：如 case_dh_drug_01 沿 logS∈[-3.09,-1.78] 穷举触达族内 4 员 / BEFORE 同值域有白名单外命中；负例：按 gt.composition 组成约束扫查，BEFORE 命中 / AFTER 返空 | 两形态统一口径，无恒真分支 |
| A5 | literature_search ⊆ 文献白名单 | 正例 'bcs theory' 命中白名单文献；负例 'herg' BEFORE 命中 20 篇 / AFTER EMPTY_RESULT | 双日志实证（底层扫查已取消 20 硬上限，Task #34 重要 3） |
| A6 | 穷举质量门 sanity（阈值键动态取 view.target.prop + direction 分流） | 材料线：`enumerate(tc≥38.2)`=0；药物线：如 case_dh_drug_01 `enumerate(logS≤-6.14)`=0（direction=lower）；宽松对照与组成约束均过 | 全过 |
| A7 | 视图文件匿名红线（沿用 check_agent_visible_anonymity 四道口径） | InChIKey/元素符号/真名/真实公式 全 0 | ✓（Task #34 黑名单归一后复跑，原 `v1` 误命中已除） |
| A8 | 幂等性 | build_view 两次序列化逐字节一致，且与落盘文件一致 | md5 `8b1876ea…` 两次相同 |

**B 侧 gt 形态覆盖（Task #34 重要 2）**：断言集按 gt 形态分流——B 侧生产形态（gt.entity_ref ∉ A 全库）时 A1 改断言 gt_ref ∉ A 全库实体集 + 组成全等口径（与侧无关）；A4 负例两形态统一为组成约束扫查。覆盖实证：生产示例视图（A 侧演示形态）+ 断言运行期确定性构造的 B 侧 gt 形态测试视图（case_id=`e105_test_b_side_gt`，落盘 `_build_state/tmp_test_view/`，明确标注测试件，不入 backend/per_case_views/），两视图 A1–A7 均全绿。

复跑命令（E5 新增视图后同命令即全量复检）：

```bash
python3 src/data_build/make_example_view.py      # 幂等：重跑 md5 不变
python3 src/data_build/assert_per_case_view.py   # 全视图断言；退出码 0/1/2
python3 src/data_build/generate_checksums.py --verify   # 冻结基线保持绿（已验证）
```

---

## 6. 留给收口轮的待办（两项，**已于 Task #33 收口轮办结，2026-08-07**）

1. ~~**CHECKSUMS 刷新**~~ **已办结**：`generate_checksums.py` `EXCLUDED_DIRS` 的 `per_case_views` 排除已移除（脚本内留痕已改），重跑刷新 + `--verify` exit=0；`per_case_views/e105_example_001.json` 入主清单，新 CHECKSUMS=`73d6e845f43f8856bb628747d2270370`（主清单 15 + 附录 2，见入库操作手册 §8 再冻结登记与 task-05-report.md §2.1）。
2. ~~**E5 真视图入库后纳入清单**~~ **机制已就绪**：per_case_views/ 已在冻结扫描面内，E5 新增视图后统一刷新即自动纳入；`assert_per_case_view.py` 已列入收口复跑项（task-05-report.md §6.1 五连之一）。E5 新增视图一律走入库流程并刷 CHECKSUMS（入库操作手册 §8 静默窗约定）。

**已知边界**：`test_ab_isolation.py` 本轮未重跑（其会重写冻结附录 `enumerate_A.json`，静默窗内不碰）；本轮未改 backend_reader.py 与任何 *_A.jsonl，隔离前提无变化。

---

## 7. 归 E3 待办指路（Task #36 汇总，两处互指）

E1-05 轮后再冻结（CHECKSUMS=`56de5a045b06551433acaa5becc6fa7b`）后，归 E3 线的三项待办（①接口 B 扩展纳入降级键、②grounding 测试库规模魔法数动态化、③gateway 性质白名单同步 atom_count/element_count）已统一汇总至 **[`材料侧身份键交接说明.md`](材料侧身份键交接说明.md) §10「归 E3 待办汇总」**——每项含事项、依据节号、前置/后置关系与 E1 侧已就绪条件，E3 接手请直接读该节，内容以该节为准；本节仅作指路，不重复展开。Task #41 后 §10 增补第④项（gt 迁移手术 E3 交接）指路至 [`task-06-E3手术交接说明.md`](task-06-E3手术交接说明.md)，与视图相关的 E5 影响面见本文 §8。

---

## 8. gt 迁移手术（方案 B）E5 知会节（Task #41 备战登记，等监工通知后执行）

> **状态**：监工已裁定方案 B（2026-08-08），E1 侧备战完毕（手术脚本 `src/data_build/migrate_gt_to_B.py` dry-run 全验证通过，手术计划 12 件落盘 `_build_state/gt_migration_plan/`）；**执行顺序 E3→E1→E5，本节所列 E5 动作一律等 E3 完工 + 监工通知后执行，当前不动手**。数字与断链明细均取自 Sam 调研报告（task-06-gt迁移影响面调研.md §3）全量实算。

### 8.1 判废 7 题清单（裁定一：判废不重建，配比重裁归监工）

5 个 G_B 成员同时是他案造题素材，物理迁出与这 7 道已冻结题面不可兼得（视图载入校验对 family 长度==n、distractors 长度==K 违规即 raise）：

| 判废 case | 断链实体（G_B 归属）| 断链精确位置（Sam §3.3）|
|---|---|---|
| case_dh_drug_04 | z002195（drug_08）| `/anchor`、`/edit_op/anchor`、`/evidence_set[4]`、`/family_sequence[4]`、visible_set——锚点迁出后 edit_molecule 锚点判 ∉A（ANCHOR_NOT_IN_A）整题立断（最重）|
| case_dh_drug_03 | z063321（spare_02）| `/distractors[21]`——K 减员，长度==K 校验 raise |
| case_dh_drug_05 | z065584（drug_11）| `/distractors[23]`——同上 |
| case_dh_drug_06 | z065584（drug_11）| `/distractors[24]`——同上 |
| case_dh_drug_11 | z063321（spare_02）| `/distractors[22]`——同上 |
| case_dh_material_01 | c04551（material_02）| `/evidence_set[0]`、`/family_sequence[0]`、visible_set——族序列首员缺失，单调链断 |
| case_dh_material_02 | c04592（material_01）| `/evidence_set[2]`、`/family_sequence[2]`、visible_set——同上（material_01/02 互为素材，成对判废）|

**模板与视图处置方案（E1 只出方案，执行归 E5）**：templates（templates/discovery_haystack/ 下 7 份）建议**标记废弃不删除**（顶层增 `deprecated` 字段记裁定与断链证据，保审计链；反对删除因配比重裁需回查原始题面）；判废 7 份视图（均已冻结）随手术批移除或标记废弃，涉 CHECKSUMS 解冻登记。两案均须 E5 署名执行，详见 `gt_migration_plan/deprecated_cases_plan.json`。

### 8.2 保留 26 道视图需改 gt.entity_ref（33 生产 − 7 判废 = 26）

迁出后 gt 属 B：视图 `/gt/entity_ref` 现值均为 A 侧匿名 ID（z*/c*），须改为 B 侧引用；`composition` 字段与侧无关不改。**候选新值预览**（药物→注册 InChIKey、材料→material_identity_key）已逐道生成于 `gt_migration_plan/view_entity_ref_rewrites.json`（含 entity_ref_now/候选新值/键型）。**未决点**：B 侧注册键终形（结构键 vs ID 复用 vs 新发 B 侧 ID，Sam §9 R9）待 E3 契约升版定形——若直接落结构键，与视图匿名红线 A7（InChIKey 0 命中）相撞，需先解决；E5 执行以裁定为准。验收：`assert_per_case_view.py` A1–A8（B 侧生产形态分流已就绪，见 §5 Task #34 重要 2）。另：材料 05–15 共 11 份视图尚未冻结（git untracked），随批改写后首次入册（Sam §3.1/E5-4）。

### 8.3 执行时点与前置链（裁定三：E3 先行、零断裂窗）

```
E3 判分侧改造（task-06-E3手术交接说明.md 五项）+ greenlight 六字段置真
  → 监工执行通知（配比重裁完成后）
  → E1 手术（migrate_gt_to_B.py --execute：三库删 486 行、B 侧注册、血缘 486 行、b_partition 清占位、再冻结）
  → E5 视图改写（本节 8.1/8.2）+ templates 同步 + 复跑质量门
  → 收口：统一冻结 + 全量冒烟（default_b_partition_reader 切换点）+ E2 query_lint O4
```

**当前约束**：E5 在收到监工通知前，不要按本节动手改任何视图/templates；新增视图仍可照常入库（材料 05–15 欠的冻结动作不阻塞，但 gt.entity_ref 的 B 侧改写留到手术批）。
