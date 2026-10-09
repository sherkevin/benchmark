# 后端库配置报告 · check_molecule_exists

| 字段 | 值 |
|---|---|
| 工具规范名 | `check_molecule_exists` |
| alias | 无 |
| source_tier | existence-only（唯一可触达 B 成员关系的通道） |
| 预算 | existence_budget（max_attempts 上限 10，超限该 case 记 0 分，attempts 计入指标） |
| 后端来源决策 | **自建**（调研报告 §3 决策表行 7：61 家无存在性接口先例；比对架构借 tau-bench dict 内存比对） |
| 契约依据 | `后端库规格契约.md` v1.1 §3.5、§6.5 |
| 支撑 checkpoint | open 类三件套之一（existence 存在性接口） |

## 1. 数据数量（实测）

| 侧 | 文件 | 记录数 | 体积 |
|---|---|---:|---|
| **仅 B 侧**（本工具定义域就是 m ∈ B，无 A 侧文件） | `dag_poc/data/backend/check_molecule_exists/check_molecule_exists_B.jsonl` | **0**（空占位 + MANIFEST 说明） | 0 B |

数据内容 ＝ 全部 open case 的 G_B 合格集分子并集（每 case ≥1，随 E5 造题冻结增长，由 E5 落盘、E1 过门入库，评分侧持有）。当前无造题 gt 集，空文件占位；**不得编造 gt**。构造脚本 `build_check_molecule_exists.py` 已内置三字段白名单守门（多一个字段即违约）。

## 2. 功能

open 类存在性接口：判定 agent 推断的分子是否存在。**返值严格等价 `m ∈ B`**——A 内分子与库外分子一律返 false（若对 A 内分子返 true，agent 直接交 A 中分子即过、整个设计崩塌）。**只返单个 bool**——output 结构上不含任何性质/相似度/距离字段（`additionalProperties=false` + B 侧文件只含两列身份键，结构上双重封死泄漏）。三个概念分离：停止信号（exists=true → agent 可结束）≠ 接口语义（定义域 m∈B）≠ 评分依据（推断分子 ∈ G_B 且经 B 侧性质复核满足题面窗口，评分由评测机在 B 侧执行，不经本工具）。

## 3. 调用方式（契约 §6.5 读取接口签名）

```
load:   check_molecule_exists/check_molecule_exists_B.jsonl → set(inchikey) ∪ set(canonical_smiles)
exists(molecule_id: str) → bool     # 先经 normalize_structure 同口径规范化，再作集合成员判定
```

- 读取对象只有两个身份键集合（inchikey / canonical_smiles），**无任何性质字段可读**；
- 本文件是契约声明的**唯一允许触 B 的读取通道**（§2.3 规则 1 白名单例外）；
- 预算计数写死为服务端口径：每次真实调用 existence_used += 1，超限返 `exists=false` + `ATTEMPTS_EXCEEDED`（该 case 记 0 分）；入参 `attempt` 仅作可选元数据回显，防 agent 自报小值无限枚举。

## 4. 出入参含义（对照 tools_schema.json）

**入参**：

| 参数 | 类型 | 必填 | 含义 |
|---|---|---|---|
| `molecule_id` | string | 是 | agent 推断出的分子标识（匿名域规范化后的结构标识；服务端先按 normalize_structure 同口径规范化再作集合成员判定） |
| `attempt` | integer（1–10） | 否 | agent 自报的第几次尝试，仅回显不参与预算（预算由服务端按真实调用次数计数） |

**出参**：

| 键 | 类型 | 含义 |
|---|---|---|
| `exists` | boolean | 严格等价 m ∈ B；A 内分子与库外分子一律 false |
| `attempt` | integer | 服务端真实计数（回显口径） |
| `attempts_remaining` | integer（≥0） | 剩余尝试次数 |

**错误码**：`INVALID_ARGUMENT` / `ATTEMPTS_EXCEEDED`。

## 5. 子库记录字段与 provenance 写法

**记录字段（只许三个，多一个字段即违约）**：`inchikey`（27 位标准哈希，主查询键）/ `canonical_smiles`（与 normalize_structure 同口径规范化后的唯一形式，次查询键）/ `provenance`（source 记「open case gt 冻结集」）。

provenance 写法示例（格式预置，待 gt 集入库时填真实值）：

```json
{"inchikey":"<27位标准哈希>","canonical_smiles":"<匿名域规范化 SMILES>",
 "provenance":{"source_db":"open case gt 冻结集","source_url":"<gt 实体身份档来源 URL>",
  "source_paper_doi":"<身份来源 DOI>","note":"case_ref=<case 标识>；E5 造题冻结、E1 过门入库"}}
```

## 6. 性能与隔离实测（引自 `性能与隔离实测报告.md` §2.3、§3）

- B 侧空占位时全表扫描 0.000 s、内存峰值 0（无记录）；
- **断言③ 语义实测通过**：抽样 A 内分子身份键 50 个（get_entity_A 的 InChIKey，随机种子 20260806）+ 库外入参 3 个，全部返 `exists=false`；exists 全部为纯 bool；返回结构严格为 `{exists, attempt, attempts_remaining}` 三键，无任何性质/相似度/距离字段。

## 7. 已知限制

1. **当前为空库**：gt 集待 E5 造题冻结后入库；入库后隔离断言与语义测试按一般形式自动生效（Task #8 沙箱负向自检已证明，无需改测试代码）；入库后须重跑 `generate_checksums.py` 刷新 CHECKSUMS.json 冻结基准；
2. B 侧空时「对 A 内分子返 false」是**正确行为而非缺陷**（goal §2b⑤ 写死语义）；
3. 本工具不含 A 侧文件：agent 永远无法通过本工具探测 A 池成员关系。
