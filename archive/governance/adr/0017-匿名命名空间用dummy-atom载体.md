# ADR 0017 · 匿名命名空间用 dummy atom 载体；A 侧不得返回真实化学身份

| 字段 | 值 |
|---|---|
| **状态** | **Superseded by [[0023]]**（2026-08-09）——已取消匿名，本 ADR 全部作废；保留作决策史 |
| **日期** | 2026-08-06 |
| **决议人** | **owner（jingwu）** —— 监工出三选项，owner 选定方案② |
| **领域** | 防污染（等价匿名变换的技术载体） |
| **关联** | 修复 `problems.md` **PR-31**（红线级）· 落实 `goal.md` §4 支柱5 与 §5 红线5 · 解除 [[0015]] §3 遗留的命名空间未裁定问题 · 约束 E1（映射表）/E3（规范化算法）/E5（case_id 命名） |

## 背景：红线 5 已被实际违反，且 RDKit 与元素级匿名数学不兼容

### 事实一：agent 可见的数据带真实化学身份

实测 `dag_poc/data/backend/get_entity/get_entity_A.jsonl` 首条：

```
id: aa
formula:  C15H25NO3                      ← 真实分子式
inchikey: IUBSYMUCCVWXPE-UHFFFAOYSA-N    ← metoprolol 的真实 InChIKey
```

配合三个事实，防污染完全失效：

| 事实 | 实测 |
|---|---|
| 匿名映射表 | `dag_poc/data/anonymization_mapping.json` **不存在**；规范 §6.1 的 R1/R5 **零实现**（`query_lint` 的 R4 断言因此永久 pending） |
| `case_id` | **直接带真名**：`case_antibiotic_sulfamethoxazole`、`case_anticoagulant_warfarin`、`case_acute_cns_risperidone` |
| gateway | 用 `CalcMolFormula` 把**真实分子式**返给 agent |

**后果**：真实 InChIKey + 分子式 + 性质值足以在 PubChem 反查真名 ⇒ 等价匿名变换失效 ⇒ 记忆型模型可直接命中 ⇒ **benchmark 的防污染主张不成立**。

**监工失职留痕**：验收 E1 时把"数据真实（真 InChIKey）"写进了记功项，未意识到——对数据集而言真实是优点，**对 benchmark 而言 agent 可见的真实化学身份就是漏洞**。

### 事实二：RDKit 无法解析元素级匿名（实测）

| 输入 | `MolFromSmiles` → `MolToSmiles` |
|---|---|
| `CCO`（真实分子） | `CCO` ✓ |
| `a2b`（规范 §6.1 R1 举的 H→a、O→b 例子） | **None** |
| `[Aa]CC` | **None** |
| `FFO`（真元素但价规则失败） | **None** |
| `[1*]CC`（dummy atom） | `[1*]CC` ✓ |
| `[2*][1*]` | `[1*][2*]` ✓（canonical 有效） |

匿名符号不是合法元素 ⇒ 无 canonical SMILES ⇒ InChIKey 亦不存在。**故"接 RDKit 提升 grounding 判别力"与"元素级匿名"不可兼得**，必须裁定命名空间归属。

## 决策：采用方案② —— 匿名符号改用 dummy atom 作载体

### 1. 匿名表示法（写死）

- 匿名符号用 **RDKit dummy atom** 承载：`[1*]`、`[2*]`、`[3*]` …，同位素编号即匿名标签；
- **禁止**把元素映射到其他真实元素（实测 `FFO` / `SiSiO` 均因价规则失败返 None）；
- 映射为**全局唯一 + 密钥派生的确定性置换**（承规范 §6.1 R4：seed 落盘、不公开、可复现；**不得与元素周期序/字母序单调相关**）。

**保留的判别能力**：图同构级（canonical 有效、可做 `==` 判定），足以支撑 grounding 条件③ `normalize(edit_op) == answer`。
**放弃的能力**：芳香性、价规则、真实化学合理性判断——这些在匿名空间本就无意义。

### 2. A 侧返回值的硬约束（这条是修复红线的关键）

**agent 可见的任何返回值中不得出现真实化学身份**：

| 字段 | 处置 |
|---|---|
| `inchikey` | **从 A 侧返回中删除**（judge 侧保留，供评分与穷举用） |
| `formula` | **改为匿名域分子式**（按 dummy atom 计数，如 `[1*]15[2*]25[3*]3`）或**删除**；不得返回 `C15H25NO3` |
| `masked_structure` | 只允许匿名域信息（原子计数、匿名骨架），不得含真实元素符号 |
| `cif`（材料） | 同理，元素符号须匿名化后才可返回 |

**判定方式**：脚本断言 A 侧任一返回值不匹配真实元素符号表与 InChIKey 正则（`[A-Z]{14}-[A-Z]{10}-[A-Z]`）。

### 3. `case_id` 与元数据去真名

- `case_id` 不得含分子/材料真名（现存 `sulfamethoxazole`/`warfarin`/`risperidone` 等须重命名为语义中性 ID）；
- `provenance` 中的题目背书（论文标题/DOI）**保留**——那是给评审看的科学出处，不是给 agent 的；但**不得出现在 agent 可见的任何字段中**。

### 4. 材料侧另定规范化口径（本 ADR 补齐 [[0015]] §3 的缺口）

5,771 条晶体**无 SMILES**，canonical SMILES 与 InChIKey 对周期性晶体**无定义**。故：

| 项 | 材料侧口径 |
|---|---|
| 身份键 | `pymatgen` 的 `Composition.reduced_formula` + 空间群（或 `StructureMatcher`），**在匿名域计算** |
| 封闭算子集 | 须另定**材料版** `edit_op`（元素替换 / 掺杂比例调整），不能复用分子版 |
| grounding 条件③ | 用材料侧身份键做 `==` 判定 |

**若材料版算子集与身份键在第一版无法就位**：按 `goal.md` §2b「质量门两条任一不成立即废弃」，**15 道 open 材料题须废弃**，此时 open 的 1:1 领域对称破掉，须由 owner 决定是降 open 题量还是延后 open 材料线。**不得**在判据不可执行的情况下强行出题。

## 影响与分工

| 对象 | 动作 |
|---|---|
| **E1** | ① 建 `dag_poc/data/anonymization_mapping.json`（全局唯一、密钥派生、seed 落盘不公开）② 重建 A 侧库使返回值符合 §2 硬约束（删 inchikey、formula 匿名化）③ 材料侧提供匿名域身份键（pymatgen 口径） |
| **E3** | ① `normalize_structure` / `edit_molecule` 改用 RDKit 处理 dummy atom SMILES（替代现有字符串拼接）② 加 A 侧返回值的真实身份断言 ③ 材料侧接身份键 |
| **E5** | ① `case_id` 与全部 case 元数据去真名 ② open 材料题在材料版算子集就位前**不造** |
| **E2** | `query_lint.py` 的 R4 断言从 pending 转为实判（映射表就位后）；增"A 侧返回无真实身份"的断言 |
| **E6** | 跑测前置检查加"匿名映射表存在且 R4 断言通过" |

## 备案

**本 ADR 由 owner 直接决议**（监工出 ①匿名降实体层 / ②dummy atom / ③匿名图 WL hash 三选项，owner 选 ②）。

PR-31 在 §2、§3 两项落地并通过断言后方可转 CLOSED。
