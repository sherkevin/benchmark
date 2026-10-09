# Task Result · E1-03 收口 —— 匿名层重建的自检、冻结、交接与 report（Task #25）

> **Status: `done`**

> ⚠️ 本文件自包含：所有结论附实跑命令与输出摘要，外部读者只读本文件即可核验。

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | E1-03 收口（任务板 Task #25）/ `task/E1-后端数据库构造/` |
| **类型** | Implementation（收口：自检 + 冻结 + 交接 + report） |
| **负责人** | E1（数据线，Jay） |
| **起止** | 2026-08-07（1 天） |
| **上游来源** | task-03-匿名层重建.md §4/§5 · 前置三步（Task #22 映射表 / #23 A 侧重建 / #24 材料键）+ Task #26 性质值指纹评估 |
| **下游消费方** | E3（规范化算法/gateway）· E5（造题/gt 交付）· E2（R4 实判）· 监工（PR-31 状态裁定） |

---

## 一句话结论

> E1-03 匿名层重建收口完成：**停止准则四条全部实跑通过**（真实身份零命中、R4 非单调、材料键 5,771/5,771、两脚本退出码 0），CHECKSUMS 统一刷新（主清单 14 + 附录 2，`--verify` 全绿），材料键列按裁定纳入冻结附录，交接说明落盘（E3/E5/E2 三段口径），§4 六项交付标准逐条达标；**收口中发现并修复一处前置路遗漏**（`ingest_open_gt.py` 隔离闸未同步 registry 口径，实为失效闸，已修并实证）。PR-31 的 E1 侧落地结论见 §5（§2 已达标；§3 归 E5 未落地；另存在编号漂移，归监工裁定）。

---

## 1. §4 六项交付标准逐条自检 ★

- [x] **① 映射表**（存在 / dummy 载体 / seed 不公开 / 非单调）——`dag_poc/data/anonymization_mapping.json` 存在（md5=a77b6ef8ce3be434eba36e08a5bb1401，88 标签键 = 87 真实元素 + 保留标签 `*`，dummy 安全上限 65535）；seed 落盘不公开；R4 非单调断言本次收口复跑全绿（见 §2 准则②）。
- [x] **② A 侧四库重建自检断言**（无真实 InChIKey / 分子式 / 元素符号）——四 jsonl 全文 grep InChIKey 正则 0 命中；真实公式/真名黑名单扫描（37,565 式 / 40 名）0 命中；`check_agent_visible_anonymity.py` 复跑四库四道断言全过、退出码 0（见 §2 准则①）。
- [x] **③ 材料侧身份键全覆盖**——5,771/5,771 覆盖复算通过、零缺口；唯一键 5,740，冲突 31 组 62 条全为 3DSC 上游重复录入（4 个 mp-id），如实登记并写进 E5 交接（见 §2 准则③）。
- [x] **④ 交接说明**——`task-03-交接说明.md` 落盘：给 E3（映射表接入口径 + CalcMolFormula 该返什么 + 材料键双接口）、给 E5（匿名域样例 / id 语义不变 / 材料键取用 / 31 组去重 / gt 键形态）、给 E2（R4 转实判口径）。
- [x] **⑤ 隔离与完整性重跑 + CHECKSUMS 刷新**——`test_ab_isolation.py` 四断言全过退出码 0；`verify_sources_integrity.py` 11 库六要素齐全退出码 0；CHECKSUMS 刷新后 `--verify` 全绿（主清单 14 + 附录 2）；根 MANIFEST 与五子库 MANIFEST 交叉一致（md5/bytes/record_count 逐条比对，见 §3）。
- [x] **⑥ report 收口**——本文件（改动清单 + 自检实跑输出 + 遗留项）。

---

## 2. 停止准则四条实跑记录（task-03 §5）

### 准则①：四 A 侧 jsonl 全文无真实 InChIKey 与真实分子式

```
$ for f in <四 A 侧 jsonl>; do grep -cE '[A-Z]{14}-[A-Z]{10}-[A-Z]' "$f"; done
literature_search_A.jsonl: 0 | search_candidates_A.jsonl: 0
get_entity_A.jsonl: 0 | get_property_A.jsonl: 0          ← 全部 0 命中

$ python3 黑名单扫描（judge 侧口径 anonymization.load_identity_blacklist）
黑名单：公式 37,565 / 名字 40
四库逐行扫描：公式命中 0 / 名字命中 0（合计 0）

$ python3 src/data_build/check_agent_visible_anonymity.py
四库均 ✓（InChIKey=0 元素符号=0 真名=0 真实公式=0）→ 退出码 0
```

### 准则②：R4 非单调断言复跑

```
$ python3 src/data_build/check_mapping_nonmonotonic.py
[断言1] 周期序/字母序非严格单调：通过 ✓
[断言2] Spearman ρ：周期序 +0.0323 / 字母序 +0.0029（|ρ|<0.5）：通过 ✓
[断言3] 相邻原子序 label 差符号混合（正 44 / 负 42）：通过 ✓ → 退出码 0
```

### 准则③：材料键覆盖率复算

```
$ python3 复算（键列 id 集 vs registry 晶体 id 集）
键列行数=5771 唯一id=5771 晶体总数=5771 覆盖=5771/5771 缺口=[] ✓
键列 md5=55922089536b0388c8e343fbc2cd2575（与 progress 第 17 条一致）
```

### 准则④：两个既有验证脚本退出码 0

```
$ python3 src/data_build/test_ab_isolation.py
断言① 键集隔离 ✓（literature 309 / search 80712 / entity 80712 / property 9198，交集 0；
  沙箱负向自检：注入重叠键 aa → 被抓 ✓）
断言①b 身份键空间隔离 ✓（A 池键从 judge 侧 registry 载入：InChIKey 74,283 / SMILES 74,941）
断言② 句柄隔离 ✓（无非法 _B 句柄）；断言③ 存在性语义 ✓ → 退出码 0

$ python3 src/data_download/verify_sources_integrity.py
库目录 11，六要素齐全且 complete=true：11，[PASS] → 退出码 0
```

---

## 3. CHECKSUMS 统一刷新与 MANIFEST 交叉核验

**裁定执行**：材料键列 `material_identity_keys.jsonl` 纳入冻结附录（judge 侧交接物，与 enumerate_A 同例）——`generate_checksums.py` 的 `HANDOVER_ARTIFACT_PATHS` 已增列。

执行顺序：`finalize_backend.py` 终审（逐文件重放复核全过、写根 MANIFEST 189,351 条 / 67,249,539 B）→ `generate_checksums.py` 刷新 → `--verify`。

```
CHECKSUMS.json：主清单 14 文件（合计 67,259,817 B）+ 附录 2 交接产物
  附录 enumerate_A.json                 55ba749cf2a3dad680926a15ff387754  1,859,077 B（未变）
  附录 material_identity_keys.jsonl     55922089536b0388c8e343fbc2cd2575    843,072 B（新增）
$ python3 generate_checksums.py --verify
冻结校验通过 ✓：主清单 14 + 附录 2 md5 全部一致 → 退出码 0
```

四 A 侧 jsonl md5 与 progress 第 16 条逐位一致：search_candidates_A=`2c071ee6…` / get_entity_A=`02ba808c…` / get_property_A=`c5d95bb3…` / literature_search_A=`993989f1…`。

**MANIFEST 交叉核验**：五子库 MANIFEST → 根 MANIFEST 逐条比对（`tool/file` 拼相对路径，md5/bytes/record_count 三字段），8/8 文件条目全一致 ✓（根 MANIFEST：total_records=189,351 / total_bytes=67,249,539）。注：progress 第 16 条过期清单所列"四 A 侧 MANIFEST/根 MANIFEST"在前置路已刷新，本次复核实测全部一致，CHECKSUMS 以本次落盘为准（根 MANIFEST md5=9f276ae1f9e1f091b8e76053945b7027，与第 16 条 9f9913… 的差异来自 generated_at 时间戳重生成）。

---

## 4. 改动清单（本次收口）

**脚本（src/data_build/）**

| 文件 | 改动 |
|---|---|
| `generate_checksums.py` | `HANDOVER_ARTIFACT_PATHS` 增列 `material_identity_keys.jsonl`（裁定纳入冻结附录） |
| `ingest_open_gt.py` | **修复隔离闸失效**：`load_a_identity_keyspace()` 原从 A 侧 jsonl 读 inchikey/smiles——匿名重建后该处 inchikey 已删、smiles 已匿名，闸实为空转（返回空集，任何 gt 都能过）。改为读 judge 侧 `_build_state/entity_registry.jsonl`（与 `test_ab_isolation` 断言①b 同口径），实跑验证 InChIKey 74,283 / SMILES 74,941 |

**数据（dag_poc/data/backend/）**

| 文件 | 状态 |
|---|---|
| `CHECKSUMS.json` | 统一刷新（主清单 14 + 附录 2），`--verify` 全绿 |
| 根 `MANIFEST.json` | finalize_backend 终审重生成（189,351 条 / 67,249,539 B） |
| 五子库数据 jsonl / 子 MANIFEST | **未改动**（边界约束；实测 md5 与冻结一致） |

**文档（task/E1-后端数据库构造/）**

| 文件 | 说明 |
|---|---|
| `task-03-交接说明.md` | 新增：E3/E5/E2 三段交接口径（§4 第 4 项交付） |
| `task-03-report.md` | 新增：本收口报告 |
| `progress.md` | 解除阻塞第 16 条 + 追加收口段 |
| `task-03-匿名层重建.md` | 表头状态行确认（已为 ✅ 完成，无需改动） |

---

## 5. PR-31 落地状态（ADR-0017 备案核验，E1 侧结论）

ADR-0017 备案：「PR-31 在 §2、§3 两项落地并通过断言后方可转 CLOSED」。逐项核验：

| ADR-0017 条件 | 归属 | 落地状态 |
|---|---|---|
| **§2 A 侧返回值硬约束**（删 inchikey / formula 匿名 / masked_structure 匿名 / cif） | E1 | ✅ **已落地并通过断言**：四库全量扫描四道断言零命中（本报告 §2 准则①）；监工验收实跑确认（task-03 §8） |
| **§3 case_id 与元数据去真名** | **E5** | ❌ 未落地（不在 E1 范围；现存 `case_antibiotic_sulfamethoxazole` 等待 E5 重命名） |

**E1 侧结论**：备案两项条件中 E1 负责的 §2 已达标且断言通过；§3 属 E5 职责未落地——**按备案字面口径，CLOSED 条件尚未齐备**，是否/何时转 CLOSED 归监工裁定（task-03 §8 监工已写"红线 PR-31 已闭合"，与备案字面口径的衔接请监工确认）。

**意外发现（编号漂移，如实登记）**：`problems.md` 现存 PR-31 条目为「query 出题侧四道关缺三道半」（出题质量问题，OPEN），与 ADR-0017/task-03 所引"红线级匿名身份泄漏问题"**不是同一事项**——两处 PR-31 编号指向不同问题，疑似编号复用/漂移。E1 按边界不改 problems.md，**报监工裁定编号归一**。

---

## 6. 性质值指纹评估结论与监工裁定（必须写进 report 项）

- **评估结论**（Task #26，`性质值指纹风险评估.md`）：**风险实证成立**——logS 精确单值在 AqSolDB 内唯一命中率 72.7%；双值组合（logS+mw 等）唯一命中率 ≥91.5%。即使删了 InChIKey，性质值本身即指纹，可反查实体。
- **评估建议**：选项 B 量化（药物侧 1 位小数 / tc 整 K）。
- **监工裁定**：**本轮性质值原样保留、风险如实登记；量化/加噪另待裁定——若做须立 ADR**（影响判分容差，涉及 E2/E4）。本轮重建已按裁定对性质值行不做任何改动（get_property_A 的 tc/family/condition/logS 原样）。

---

## 7. 风险与遗留项

| 类型 | 描述 | 影响 | 缓解 / 待决方 |
|---|---|---|---|
| 已知风险 | 性质值指纹（logS 72.7% 唯一命中）原样保留 | 记忆型模型可能经性质值组合反查实体 | 监工裁定另待裁定；若做须立 ADR |
| 已修复 | `ingest_open_gt.py` 隔离闸在匿名重建后失效（未同步 registry 口径） | 若不修，E5 真 gt 入库时"gt 与 A 池重叠"无人拦截（goal §2b⑤ 红线） | 本次收口已修并实证（§4）；E5 交付前建议跑一次 `--dry-run` 演练复核 |
| 未决问题 | problems.md PR-31 与 ADR-0017 PR-31 编号漂移 | 红线闭合状态无法从共享账本单点确认 | 需监工裁定编号归一 |
| 未决问题 | PR-31 CLOSED 条件之 §3（case_id 去真名）未落地 | 备案字面口径未齐备 | 归 E5 执行、监工裁定状态 |
| 遗留 | 31 组 62 条 3DSC 上游重复晶体 | 造题若不去重会产生歧义题 | 交接说明已写明 E5 去重口径 |
| 遗留 | 材料版 edit_op 算子集未就位 | open 材料题不可造（ADR-0017 §4 备案） | 归后续任务；不就位则 15 道 open 材料题废弃待 owner 裁定 |
| 说明 | task-03 §8 验收记录写"附录 1"，本次收口后为"附录 2" | 仅记录差异 | 以本报告 §3 与 CHECKSUMS 落盘为准 |

---

## 8. 交付物清单

| 类别 | 路径 | 说明 |
|---|---|---|
| 本报告 | `task/E1-后端数据库构造/task-03-report.md` | 对外成品（本文件） |
| 交接说明 | `task/E1-后端数据库构造/task-03-交接说明.md` | §4 第 4 项交付（E3/E5/E2） |
| 冻结基准 | `dag_poc/data/backend/CHECKSUMS.json` | 主清单 14 + 附录 2，--verify 全绿 |
| 映射表 | `dag_poc/data/anonymization_mapping.json` | judge 侧，md5=a77b6ef8… |
| 材料键列 | `src/data_build/_build_state/material_identity_keys.jsonl` | 5,771 行，md5=55922089…，冻结附录 |
| 修复脚本 | `src/data_build/ingest_open_gt.py` · `generate_checksums.py` | 见 §4 改动清单 |
| 上游 | `task/E1-后端数据库构造/task-03-匿名层重建.md` · ADR-0017 | 任务书与决议依据 |
