# task-05 · UCI SuperCon 材料侧扩数据入库记录（E1-05 · Task #31）

> 执行日 2026-08-07。静默窗解冻授权已登记（入库操作手册 §8，E1-05 授权 2026-08-07）。
> 本文是 task-05-扩数据源调研.md §6.1 八步入库要点的逐条执行留痕与统计报告段。

## 0. 结论（先读此节）

- **新增实体 11,481 条**（id c05772–c17252），零拒收；材料侧晶体总量 5,771 → **17,252**。
- **造题支撑力达标**：分源口径达标族（同族 + Tc 严格单调 + R²≥0.90 + ≥4 档）共 **31 族**（3DSC 5 + UCI 26），独立化学体系 **26 个**（3DSC 5 + UCI 21）；需求 15 道（43 投 30 口径 ≈ 22 次投造），**供给充足（31/26 ≥ 15/22）**。
- **降级键报备**：UCI 无晶体结构/空间群，11,481 条走交接说明 §7.1 降级替代键（cif=null），逐行显式标记 `key_type=degraded`，非静默；tc 分档口径差异（§7.1 引用的"已冻结口径"实际未冻结）按整 K 向下取档落地并已报备（见 §6）。
- **CHECKSUMS 已刷新并 `--verify` exit=0**（新值见 §7）。
- 与调研预估的两处偏差均已实测闭环并报备：跨源重叠 3,753 → **3,828**（新增拦 75 个标量倍写法组成，§3）；UCI 达标族 29 → **26**（纪律去重拆散 6 族、残留点重组 3 族，§5）。

## 1. 新源与权威性

| 项 | 值 |
|---|---|
| 源 | UCI ML Repo #464 SuperCon 抽取（Hamidieh 2018），NIMS SuperCon 谱系 |
| 数据文件 | `dag_poc/data/sources/UCI_SuperCon/unique_m.csv`（冻结落盘，不重新下载） |
| 源 md5 | `1b3a0c521d38ba602a4b2041c8c11090`（与 sources/UCI_SuperCon/MANIFEST.json 双断言） |
| URL | https://archive.ics.uci.edu/ml/datasets/superconductivty+data |
| DOI | 10.24432/C53P47（CC BY 4.0），配套论文 Hamidieh 2018（arXiv:1803.10260） |
| 交叉验证源 | NIMS MDR SuperCon（https://doi.org/10.48505/nims.3739），同谱系非叠加（调研 §4.4） |
| 源规模 | 21,263 条全 Tc>0 超导体，88 列（86 元素计量列 + critical_temp + material） |
| 白名单登记 | `common.DATA_FILE_ALLOWED_SOURCES` 新增 `UCI_SuperCon`（先修白名单再过门，入库操作手册 §2 闸 1 断言三） |

## 2. §6.1 八步入库要点逐条执行对照

| 步 | 要点 | 执行 |
|---|---|---|
| 1 | 只读冻结源 | md5 断言 + MANIFEST 双校验，不下载 |
| 2 | 组成去重（冻结口径） | 同组成取最高 Tc（max）；互异 Tc 多测量命中 5,954 次（§7 不可保证项 2 留痕）；口径写入 provenance note |
| 3 | 实体去重 | 与 3DSC 按**最简整数比规范键**比对（raw 向量超集，拦标量倍），重叠不新建实体（§3 详述） |
| 4 | 匿名化 | 共用 `anonymization_mapping.json` element_to_label，**零扩展**（77 元素全在 88 项映射内，硬校验通过；映射外元素会硬停 exit=3） |
| 5 | 结构字段 | cif=null；masked_structure 仅化学式级 `FORMULA:<匿名式>`（契约 §3.3 允许）；身份键降级见 §6 |
| 6 | family | UCI 无 sc_class 列，不做 NIMS 通式回填（不猜测分类），记"未知"（§6.1 途径②） |
| 7 | condition | 走 `condition_annotations.lookup_condition`：命中 1 条（H1Pd1 → ambient，PdH 规则带 DOI 人工标注），其余 11,480 条记"未知"，**未默认 ambient**（E1-02 红线）；高压候选已被选型整体排除 |
| 8 | CHECKSUMS | `generate_checksums.py` 刷新 + `--verify` exit=0（§7）；EXCLUDED_DIRS 的 per_case_views 排除按指令保持不动（收口归后续轮） |

## 3. 可比性处置与去重（含与调研预估的偏差报备）

**纪律**：族内单源、禁跨源混排（调研 §3.5）。落地两道：

1. **实体层**：与 3DSC 重叠组成不新建实体。
2. **统计层**：族按源分开枚举，供给数字只取分源结果；合并枚举仅做跨源审计留痕（`cross_source_audit`，4 个两源同基底序列合法存在，E5 只消费分源清单即无混排）。

**去重条数（实测，均已入 stats_uci_ingest.json）**：

| 项 | 条数 |
|---|---:|
| 源行数 | 21,263 |
| 组成级去重后唯一组成（max 口径） | 15,309 |
| 与 3DSC 重叠剔除组成 | **3,828** |
| └ raw 向量恒等（调研 §6.1 预估口径） | 3,753 |
| └ **标量倍写法（入库实测新增拦截，报备）** | **75** |
| 对应唯一最简比键 | 3,756 |
| **新增实体** | **11,481**（c05772–c17252） |
| 拒收 | 0 |

**偏差报备①（重叠数 3,753 → 3,828）**：调研预估 3,753 基于 raw 计量向量恒等；入库实测发现另有 75 个 UCI 唯一组成与 3DSC 是**最简整数比恒等的标量倍写法**（如 UCI `Al1V3` vs 3DSC `V3Al` 系、`C60`/`C70` vs 3DSC 碳系、`V66.6Zr33.3` vs `V2Zr1` 系）。raw 口径漏拦会造成同材料双实体并诱发跨源族混排（违反 §3.5），故跨源重叠判定升级为最简比规范键（`ingest_uci_supercon.reduced_key`，Fraction 精确），剔除组成为调研预估的超集。已在脚本注释、provenance note 与本文报备。

## 4. 入库产物清单（写面均在授权范围内）

| 产物 | 变化 | md5 |
|---|---:|---|
| search_candidates_A.jsonl | 80,712 → 92,193 行（+11,481） | a9af11a081994824903f0520180f2c04 |
| get_entity_A.jsonl | 80,712 → 92,193 行（+11,481） | 04305683b9a0b1a35bcf1fe5e986acc7 |
| get_property_A.jsonl | 27,606 → 73,530 行（+45,924 = 11,481×4 性质行） | f81928bdb25d8261ec630b7da70b406b |
| entity_registry.jsonl（judge 侧真值） | +11,481 条（source=UCI_SuperCon） | — |
| provenance_ledger_{sc,ge,gp}.jsonl | 与对应 A 侧文件行数逐一相等（finalize 复核前提） | — |
| material_identity_keys.jsonl（`src/data_build/_build_state/`，CHECKSUMS 附录按此路径登记） | 5,771 → 17,252 行（+11,481 降级键行） | 9f6ad6c78c8769541430f09017019eef |
| enumerate_A.json（test_ab_isolation 重生成） | 92,193/92,193/20,667 id | 00c52b779adf86875d3b07e3fbad208b |
| stats_uci_ingest.json / rejections_uci_supercon.jsonl | 统计与拒收留痕（拒收 0 条） | — |
| stats_material_families.json | 族统计全量产物（§5） | — |

新记录每实体 4 性质行（顺序与 3DSC 一致）：`tc`（max 去重值）/ `family`（"未知"）/ `structure_in_db`（0，事实推导）/ `condition`（"未知"，唯 H1Pd1=ambient）。

**幂等性（交付要求）**：整链重跑两轮，14 个产物（3×A 侧 jsonl、registry、3×账本、键列、2×stats、rejections、3×目录 MANIFEST）**md5 逐一相同、逐字节一致**。入库脚本 `ingest_uci_supercon.py` 幂等机制：按 `source=UCI_SuperCon` 标记清出上一轮产物后按确定性顺序（组成规范键排序）重建；既有行保留原始行文本不重新序列化；目录 MANIFEST generated_at 锚定常量 `2026-08-07T16:00:00Z`。

## 5. 造题支撑力实测（硬性要求 5）

**口径**：调研报告 §4.1/附录 A 同款——单元素计数变动族 + 严格单调 + R²≥0.90 + ≥4 档；独立体系按族内元素集合去重；Tc 取入库后 get_property_A tc 行；族内单源（分源枚举）。落盘脚本 `src/data_build/stat_material_families.py`。

**实跑命令**：

```
python3 src/data_build/stat_material_families.py
```

**实跑输出（摘要）**：

```
[载入] 晶体 17252 条（3DSC 5771 + UCI 11481）
[去重] 3DSC：统计层组成归并 45 条（max 口径）   # 3DSC 上游重复录入，仅统计层归一不改库
[去重] UCI_SuperCon：统计层组成归并 0 条（入库期已去重）
[纪律] 合并枚举存在跨源达标族 4 个（两源同基底序列，合法存在；已登记 cross_source_audit）
[校准] 3DSC 调研锚 + UCI 纪律执行锚全部复现（调研全量锚 29/24 的偏差因果链已闭环）
[3DSC] {"candidate_groups": 426, "monotone": 210, "monotone_r2_090": 204,
        "qualifying_families": 5, "qualifying_step_distribution": {"4": 3, "6": 2},
        "independent_systems": 5}
[UCI ] {"candidate_groups": 1053, "monotone": 701, "monotone_r2_090": 675,
        "qualifying_families": 26, "qualifying_step_distribution":
        {"4": 12, "5": 9, "6": 2, "8": 2, "9": 1}, "independent_systems": 21}
[支撑力] 达标族 31 / 独立体系 26；需求 15 道（≈22 次投造）→ 足
```

**可用族数汇总（分源，族内单源纪律口径）**：

| 指标 | 3DSC | UCI | 合计 |
|---|---:|---:|---:|
| ≥2 档候选组 | 426 | 1,053 | — |
| 严格单调 | 210 | 701 | — |
| 单调且 R²≥0.90 | 204 | 675 | — |
| **达标（+≥4 档）** | **5** | **26** | **31** |
| 其中 ≥5 档 | 2 | 14 | 16 |
| **独立化学体系** | **5** | **21** | **26** |

校准自检：3DSC 426/210/204/5 全量复现调研 §4.2 锚（含档数分布 4 档×3、6 档×2）。

**偏差报备②（UCI 29/24 → 26/21）**：调研锚 29 族/24 体系基于 UCI 全量 15,309 组成扫描；入库按 §3.5 剔除 3,828 个与 3DSC 重叠组成后，其中 6 个达标族被拆散（I-Bi2212 碘插层 / Na-WO3 / Pb-Hg / Al-MgB2 / La-Mo6Se8 / N-NbC0.5，成员组成归 3DSC 侧），拆散后残留点另行构成 3 个新 ≥4 档族，故 29−6+3=26、24−6+3=21。已用独立脚本对源 CSV 全量复扫（29/24 复现）并逐族归因核对，因果链闭环，写入脚本校准锚 B。

**支撑力判定**：需求 15 道、43 投 30 口径约 22 次投造；供给 31 族（≥15）且 26 独立体系（≥22），**充足**，未降任何标准。

## 6. 降级报备清单（硬性要求 3，不得静默降级）

| 项 | 内容 |
|---|---|
| 降级事实 | UCI SuperCon 为纯组成表：无晶体结构/空间群/晶格，cif=null |
| 降级范围 | 新增 11,481 条晶体全部走降级替代键（无一例外） |
| 键格式 | `<匿名 reduced_formula>_ALT_<tc 分档>_K<元素种类数>`（交接说明 §7.1） |
| **tc 分档口径差异报备** | §7.1 原文要求"取 get_property 已冻结的 tc 分档口径"，该口径实际未冻结（性质值量化/加噪方案仍在监工裁量，性质值指纹风险评估.md 选项 B）；本轮按选项 B **整 K 向下取档 floor(tc)** 落地（`material_identity.tc_tier_floor`）。监工若裁定其他分档，降级键须重建 |
| 产物显式标记 | 键列逐行 `key_type:"degraded"` + `degradation_reason`（全文报备串）；脚本 docstring、入库 provenance note、交接说明 §7.1/§7.1.1 同步更新 |
| 消费约束 | E5 造题降级键只能用于排除、不得用于唯一性判定；E3 可用 A 侧可见字段（formula+tc）独立复算（S6 全量实测通过） |
| 自检 | build_material_identity_keys.py S1–S6 全过：主键 5,771 + 降级 11,481 = 17,252/17,252 覆盖；唯一键 17,170；冲突组 82/164 条（含 3DSC 既有 31 组上游重复录入，同键判等为正确行为） |

## 7. CHECKSUMS 新值（`--verify` exit=0）

主清单（14 文件，合计 78,478,520 bytes）中被本任务改动者：

| 文件 | md5 |
|---|---|
| search_candidates/search_candidates_A.jsonl | a9af11a081994824903f0520180f2c04 |
| get_entity/get_entity_A.jsonl | 04305683b9a0b1a35bcf1fe5e986acc7 |
| get_property/get_property_A.jsonl | f81928bdb25d8261ec630b7da70b406b |
| search_candidates/MANIFEST.json | 585db2d240311ff5043db5ff928cfe4c |
| get_entity/MANIFEST.json | a992ef08095e7fea7231238858acd743 |
| get_property/MANIFEST.json | f4f529f5a8252c621377750f40bed8db |
| MANIFEST.json（根，finalize 重写） | e01b76208f835131bcf7c162e53d342b |
| CHECKSUMS.json 本体 | 见文件（verify exit=0） |

附录 handover_artifacts（2 项）：

| 文件 | md5 |
|---|---|
| enumerate_A.json | 00c52b779adf86875d3b07e3fbad208b |
| material_identity_keys.jsonl | 9f6ad6c78c8769541430f09017019eef |

未改动文件（literature_search、check_molecule_exists、各 *_B.jsonl）md5 与上轮冻结一致。EXCLUDED_DIRS 含 per_case_views 的临时排除按指令保持不动，收口归后续轮。

## 8. 匿名红线核验（硬性要求 2）

`python3 src/data_build/check_agent_visible_anonymity.py` 全量重跑（黑名单：真实公式 49,041 式 + 真名 40 个）：

| 文件 | InChIKey | 元素符号 | 真名 | 真实公式 |
|---|---|---|---|---|
| get_entity_A.jsonl | 0 | 0 | 0 | 0 ✓ |
| search_candidates_A.jsonl | 0 | 0 | 0 | 0 ✓ |
| get_property_A.jsonl | 0 | 0 | 0 | 0 ✓ |
| literature_search_A.jsonl | 0 | 0 | 0 | **14（归因已订正，见下）** |

**归因订正（Task #34 三维评审修复，2026-08-07）**：上表 14 处命中原文订正为“预存问题、与 UCI 入库无关、移交后续轮”——**现订正为：本轮 UCI 入库新增所致**。因果链：UCI 源纯钒材料记法 `V1` 经实体 c17248 入 registry 后进 judge 侧黑名单，token `v1` 误命中文献 DOI 版本段（literature_search_A.jsonl 14 处）与视图 schema 串 `per_case_view_v1`——**文献文件内容未变、变的是黑名单**，与 per-case 视图门禁 A7 误报同根因。**本任务新入库的三子库四道口径 0 命中，匿名红线保持；数据本体零泄漏，坏的是门禁口径。**

**修复（监工裁定：黑名单归一）**：在黑名单构造侧（`anonymization.py::load_identity_blacklist` 的公式入集通道）对可分解公式做最简比归一（复用 `material_identity.reduced_formula_anon` 的 Fraction 口径，`reduce_real_formula`）：`V1→V`（无数字，被“黑名单式必含数字”过滤器自然移除），同类单质标量倍记法一并归一；不动已落盘数据、不动 anonymization_mapping.json、不放宽 A7、不豁免视图文件。

**黑名单条数变化留痕**：公式黑名单 49,041 → **45,715**（净减 6,988：单质标量倍记法归一后变纯字母被数字过滤器移除 136 条 + 归一后与既有最简比条目合并去重；真名 40 不变）。

**修复后绿态证据（同批实跑，均 exit=0）**：

```
$ python3 src/data_build/check_agent_visible_anonymity.py
黑名单载入：真实公式 45715 式 / 真名 40 个
get_entity_A.jsonl: ✓（InChIKey=0 元素符号=0 真名=0 真实公式=0）
search_candidates_A.jsonl: ✓（InChIKey=0 元素符号=0 真名=0 真实公式=0）
get_property_A.jsonl: ✓（InChIKey=0 元素符号=0 真名=0 真实公式=0）
literature_search_A.jsonl: ✓（InChIKey=0 元素符号=0 真名=0 真实公式=0）
自检通过 ✓：四个 agent 可见 jsonl 全文零真实身份   → exit=0

$ python3 src/data_build/assert_per_case_view.py
全部断言通过 ✓（A1–A8，含视图 A7 真实公式=0）   → exit=0
```

## 9. 交付物索引

| 交付 | 路径 |
|---|---|
| 入库脚本（幂等可重跑） | `src/data_build/ingest_uci_supercon.py` |
| 族统计脚本（口径落盘版） | `src/data_build/stat_material_families.py` |
| 降级键实现 | `src/data_build/material_identity.py`（tc_tier_floor / degraded_material_identity_key） |
| 键列脚本扩展（S6 自检） | `src/data_build/build_material_identity_keys.py` |
| 白名单登记 / 闸断言 | `src/data_build/common.py` / `src/data_build/ingest_gate.py`（uci_crystal kind） |
| 统计产物 | `src/data_build/_build_state/stats_uci_ingest.json`、`stats_material_families.json` |
| 交接说明同步 | `task/E1-后端数据库构造/材料侧身份键交接说明.md` §7.1/§7.1.1 |

——记录完——
