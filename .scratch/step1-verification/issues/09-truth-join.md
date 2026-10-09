# 09 · 真值关联与污染排除

Type: task
Status: open
Blocked by: 06

## 顺序铁律

真值关联必须在**冲突名单封条写完之后**。既有预登记契约明文规定 `experimental_truth_in_conflict_selection: false`、`truth_join_stage: "after conflict_set_seal.json is written"`。这个顺序是防止用真值反向挑选冲突样本（那会让冲突集带上选择偏差）。**新一轮沿用同一顺序，不得提前接真值。**

## 真值源

- `archive/dag_poc/data/sources/TDC_ADMET/bbb_martins.tsv`（2040 行，含表头；三列 `Drug_ID / Drug / Y`，Y 为 0/1）
- 入库后的 `get_property_A.jsonl`（sha256 已被预登记锁定）
- **路径不变**：这两个文件不随 02 号 ticket 移动，因为契约按原路径+哈希锁定

## 必须排除的污染

`truth_unresolved_bbb.json` 记录 11 个分子在源数据里同一 canonical SMILES 有互相矛盾的标签，入库逻辑是"后写覆盖"（取值由文件行序决定），**其中 7 个在冲突集内**。样例：阿司匹林 `CC(=O)Oc1ccccc1C(=O)O`，源数据 `acetylsalicylate=0` 与 `aspirin=1` 冲突，入库取了 1。

处置：**按 entity_id 排除出判分，但不从数据集删除**。宽表保留这些行，加一列 `truth_usable: false` 标记（既有宽表已有此列，722 true / 8 false，口径需与 11 的数字对齐核查）。

## 已有的人工核验资产

`truth_manual_verification.json` 对部分分子做过文献人工核验，带 `evidence_grade` 分级（strong / indirect / mixed / insufficient）和原文引文。**条数口径待核**：勘察报告说 15 个分子，我实测顶层 9 个键——用前自己数清，不要照抄任一方。

## 验收

- 每个分子的真值来源、是否 usable、排除原因（若有）逐行可追
- `truth_join_receipt.json`：记录关联时间、源文件 sha256、封条哈希、排除名单
- 排除名单与 `truth_unresolved_bbb.json` 逐条对齐

## Comments
