# E1-03 交接说明 —— 匿名层重建完成后的下游接口口径（给 E3 / E5 / E2）

| 字段 | 值 |
|---|---|
| **状态** | 收口交付（E1-03 Task #25，2026-08-07） |
| **上游依据** | ADR-0017（dummy atom 匿名命名空间）· task-03-匿名层重建.md §4 第 4 项 |
| **配套文档** | `材料侧身份键交接说明.md`（材料键双接口细节）· `task-03-映射表实跑留痕.md` · `性质值指纹风险评估.md` |

> 一句话：A 侧四库已完成匿名域重建并冻结（CHECKSUMS --verify 全绿），真实身份全部收归 judge 侧
> （映射表 + registry + provenance 账本）。本文写清 E3/E5/E2 各自该按什么口径接入。

---

## 一、给 E3（规范化算法 / gateway）

### 1. 规范化算法接映射表的口径

| 项 | 口径 |
|---|---|
| **载体** | RDKit dummy atom `[N*]`，同位素编号 N 即匿名标签；**禁止**元素→真实元素映射（价规则失败，实测 `FFO`/`SiSiO` → None） |
| **映射表路径** | `dag_poc/data/anonymization_mapping.json`（md5=a77b6ef8ce3be434eba36e08a5bb1401；88 个标签键 = 87 真实元素 + 保留标签 `*`；seed 落盘不公开） |
| **载入方式** | `from src.data_build.anonymization import load_mapping`——载入前 `guard_agent_path()` 代码断言复核"映射文件不得位于 backend/ 内"。**只许 judge 侧代码 import**（评分、穷举、构建脚本）；gateway / backend_reader / agent 读路径**一律不得 import** |
| **dummy SMILES canonical 判定** | 匿名域 SMILES 必须满足一步不动点：`MolToSmiles(MolFromSmiles(x)) == x`。`anonymize_smiles()` 已迭代重解析至不动点（上限 3 轮，实测最多 2 轮收敛）；E3 做 `normalize(edit_op) == answer` 判定时可直接 `==` 比对，不必再 canonical 化，但若接收外部串仍建议过一遍 `MolToSmiles(MolFromSmiles(·))` 归一 |
| **编号上限** | RDKit isotope 16 位，安全上限 N=65535（100000 会静默截断）；A 侧元素并集仅 87 种（label 1..87），无风险。E3 若引入新元素须先扩映射表（报 E1/监工） |

### 2. CalcMolFormula 该返什么（task-03 §3 指定 E1 给出口径建议）

**红线：不得返真实分子式**（`C15H25NO3` 这类即真实身份，违反 ADR-0017 §2）。两个合规口径，按优先级：

1. **推荐：返 A 侧库记录里已落盘的匿名域分子式**（`formula` 字段，形如 `[7*]3[12*]15[45*]25[80*]3`）。
   实现上即把 `CalcMolFormula` 从"由结构现算真实式"改为"透传库内匿名式"——**不得**拿匿名 SMILES
   现算计数（dummy atom 无隐式 H，现算会丢氢计数，与库内匿名式口径不一致）。
2. **备选：直接删除该返回**（若工具契约允许收窄）。

**理由**：返匿名式保住工具"能问分子组成"的语义、不动已冻结契约结构，且天然满足红线；
现算真实式则无论如何包装都是泄漏。**E3 落地时请在 gateway 加断言：返回值不匹配真实元素符号表
与 InChIKey 正则**（与 E1 自检脚本 `check_agent_visible_anonymity.py` 同口径，ADR-0017 影响分工表给 E3 的第②条）。

### 3. 材料侧接 material_identity_key 的双接口

（细节与等价性实证见 `材料侧身份键交接说明.md`，此处摘要）

- **接口 A（读键列）**：`src/data_build/_build_state/material_identity_keys.jsonl`
  （5,771 行，md5=55922089536b0388c8e343fbc2cd2575，已纳入 CHECKSUMS 冻结附录）；
- **接口 B（纯函数现算）**：`from src.data_build.material_identity import material_identity_key`
  ——`material_identity_key(匿名域formula, 空间群符号) → "[20*]99[32*]50[42*][48*]100_SG139"`；
  纯函数无 IO 无映射表依赖，grounding 可直接 import；
- 双接口已全量等价实证（5,771/5,771）。grounding 条件③的 `==` 判定用本键（ADR-0017 §4）。

---

## 二、给 E5（造题 / case 迁移 / open gt 交付）

### 1. 匿名域数据长什么样（字段样例，实读自冻结库）

`get_entity_A.jsonl` 首条（原 metoprolol，真实身份已收归 judge 侧）：

```json
{"id":"aa","entity_type":"molecule","formula":"[16*]15[19*]25[22*]3[81*]1",
 "masked_structure":"FORMULA:[16*]15[19*]25[22*]3[81*]1;ATOMS:44","atom_count":44,
 "cif":null,
 "provenance":"A 侧匿名域记录不携带 provenance（ADR-0017 §2/§3）；科学出处存 judge 侧账本 _build_state/provenance_ledger_<tool>.jsonl"}
```

变化点：`inchikey` 字段**已删除**；`formula`/`smiles`/`masked_structure` 全部匿名域；
`provenance` 只可能是上述声明串；`cif` 保持 null（将来补齐须先匿名化）；性质值行（tc/family/
condition/logS）原样保留（量化/加噪属监工裁量中，本轮不动，见 `性质值指纹风险评估.md`）。

### 2. entity id 语义不变声明

**id 不变、语义不变**：`aa/ab/ac…` 占位符原样保留，`enumerate_A.json` md5（55ba749cf2a3dad680926a15ff387754）
重建前后逐位一致——按 id 引用实体的全部既有链路（评分、穷举、gt members）无需改动。
变化的是 id 背后的**身份字段**（已匿名），不是 id 本身。历史 63 份 trace 按 ADR-0014 修订 R1
不迁移、作 fixture 保留。

### 3. 材料键列取用

`src/data_build/_build_state/material_identity_keys.jsonl`：每行 `{"id", "material_identity_key", ...}`，
5,771 行全覆盖、唯一键 5,740。造 open 材料题时以 `material_identity_key` 做答案唯一性判定与
grounding `==` 判定；材料版 edit_op 算子集未就位前，**open 材料题不造**（ADR-0017 §4 备案）。

### 4. 31 组重复晶体的造题去重提醒

5,771 条晶体中 **31 组 62 条** material_identity_key 冲突——全部是 3DSC 上游重复录入
（4 个 mp-id 各录入两次，不同 entity id 同一材料）。**造题时同一键只能视为一个材料**：
不得把冲突组内两个 id 同时用作同一题的"不同候选"，也不得用其中一条做答案、另一条做干扰项
（会让"答对"歧义）。处置口径与明细见 `材料侧身份键交接说明.md`。

### 5. open gt 入库与键形态要求（ingest 链路影响说明）

- **B 侧隔离键空间现已以 judge 侧 registry 为准**：E1-03 §2.2 匿名重建后 A 侧 jsonl 已无真实键，
  `ingest_open_gt.py` 的隔离闸 `load_a_identity_keyspace()` 已改读
  `_build_state/entity_registry.jsonl`（InChIKey 74,283 / canonical SMILES 74,941，
  与 `test_ab_isolation` 断言①b 同口径）——本次收口修复，实跑验证键数量一致。
- **E5 真 gt 交付的键形态**：gt 文件 `g_b` 记录仍按契约 §3.5 三字段白名单
  （`inchikey` / `canonical_smiles` / `provenance`，多一个字段即拒），且须提供**真实**
  InChIKey/canonical SMILES——隔离闸拿它们与 registry 真实键比对，拦"gt 与 A 池重叠"；
  交付匿名域键将无法比对、隔离闸失效。`case_ref` 承载于 `provenance.note`
  （格式 `case_ref=<case_id>；…`，裁定二口径）。
- **题面与 case 元数据**：引用 A 侧实体一律用匿名 entity id；case_id 去真名归 E5
  （ADR-0017 §3，现存 `case_antibiotic_sulfamethoxazole` 等须重命名）。
- **gt 池准入规则（Task #28 评审修复新增，E5 必读）**：候选 gt 实体 **heavy atom ≤ 1**
  （水（O）/质子（[H+]）/单重原子离子（[Na+]/[Cl-]/[OH-]/[NH4+] 等））**不得入 open 题 gt 池**——
  此类实体无独立溶解/存在语义，其 logS 匹配源必为整盐记录（语义退化），已在 E1-04 从
  get_property_A 清理 12 条（口径函数 `build_get_property.py::is_degenerate_logS_entity`，
  可直接 import 复用）；E5 冻结 G_B 前请按同口径自查候选集。
- **穷举质量门基线漂移知会（Task #28）**：`enumerate_A.json` 的 get_property 实体数
  由 9,198 → **9,186**（E1-04 退化匹配清理 −12），新冻结 md5=`6ecb47404adb9e72968d0d5d1205354d`
  （已入 CHECKSUMS 附录）；穷举质量门一律以 CHECKSUMS 附录的当前冻结值为准，
  不得引用旧文档中的 9,198。

---

## 三、给 E2（query_lint R4 断言转实判）

- **映射表已就位**：`dag_poc/data/anonymization_mapping.json`（规范 §6.1 R1/R5 的前置条件满足），
  R4 断言可由 pending 转实判。
- **实判口径（可直接复用）**：`src/data_build/check_mapping_nonmonotonic.py`——三条断言：
  ①映射与周期序/字母序均非严格单调；②Spearman ρ：周期序 +0.0323 / 字母序 +0.0029，
  均满足 |ρ| < 0.5；③相邻原子序 label 差符号混合。退出码 0 即通过（本次收口复跑全绿）。
- **附加建议**（ADR-0017 影响分工表给 E2 的第②条）：query_lint 可增"A 侧返回无真实身份"断言，
  口径参考 `src/data_build/check_agent_visible_anonymity.py`（真实公式/真名黑名单 + 元素符号
  公式语境判定 + InChIKey 正则）。

---

## 四、分界总原则（对所有下游）

judge/agent 分界在**代码与目录结构里**，不靠"记得别返回"：

1. 映射表在 `dag_poc/data/` 根下、backend/ 之外（backend_reader 白名单结构不可达），
   `guard_agent_path()` 载入时断言复核；
2. 真实身份单一判据源 = 映射表 judge 段（id→真实身份）+ registry + 四 provenance 账本
   （`_build_state/provenance_ledger_<tool>.jsonl`），均在 backend/ 之外；
3. agent 可见 = 仅 `dag_poc/data/backend/` 白名单文件，其中 A 侧 jsonl 已全部匿名域
   （CHECKSUMS 冻结，--verify 全绿）。
