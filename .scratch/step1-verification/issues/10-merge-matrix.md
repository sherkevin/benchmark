# 10 · 合并宽表：新工具与既有 4 个模型同表可比

Type: task
Status: open
Blocked by: 06, 07, 09

## 目标产物

`data/bbb_census_v3/matrix_v3.tsv` —— 一张表，730 行（每分子一行），列结构：

**标识列**：`entity_id`、`canonical_smiles`、`drug_name`

**L3 工具列**（每工具每轮三列）：
```
<round>__<tool_id>__value           float，可空
<round>__<tool_id>__status          success|install_failed|runtime_error|timeout|not_attempted
<round>__<tool_id>__failure_reason  text，可空
```

**W 档列**（前缀隔离，绝不与 L3 混）：
```
w__<tool_id>__value / __status / __fetched_at_utc / __http_code / __evidence_path
```

**真值列**：`experimental_label`、`truth_usable`、`truth_verification_status`、`evidence_grade`

**派生列**：逐工具 `__label`（按端点 action_rule 算）、`__action_correct`

## 与既有数据的接合

既有宽表是 731 行 × 64 列（我实测；勘察报告说 62 列，以实测为准），含 4 个模型 × 4 轮（discovery/replay1/replay2/live_2026_09_02）+ 一列 morgan 逻辑回归探针基线。

接合规则：
1. 既有 4 个模型的列**原样保留**，不重算、不改名
2. 新工具列追加在后
3. `entity_id` 为唯一连接键，730 行数量必须一致，任何行数变化都要停下来查

## 硬约束

- 数值列不许出现哨兵值（`-999` 之类）。失败一律走 `status` 列（ADR-0003 §三）
- 二分类端点与回归端点**分开计分**，不混算准确率
- DeepPurpose 的三个编码器是**同一体系**，跨体系冲突口径只算不同 family 之间的对，不把三个编码器当三个独立工具（既有 `formal_authoritative_scope: "cross_family"` 已如此规定）

## 验收

- `matrix_v3.tsv` 行数 = 730（+1 表头）
- 抽 3 个分子（含胆碱 `z000282`、阿司匹林）手工逐列核对，与 receipt 里的原始值一致
- 一份 `matrix_v3_schema.md` 说明每列含义与取值域
- `summary_v3.json`：工具数、L3 通过数、W 档数、V 档数、冲突对数（按 family 分组）、真值可用数

## Comments
