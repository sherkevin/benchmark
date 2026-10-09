# _deprecated：task-07 手写重做批归档区

**出处**：task-07 手写 150 道 case 方案定稿第 11 项（owner 裁定：150 道
case 全部手写重做，脚本只算答案/验可解性、不生成约束或题面），旧脚本批
37 道按「归档不删除」处置，`git mv` 入本目录保留两层结构
（`_deprecated/<原目录名>/`）。

**归档清单（37 道）**：

| 原目录 | 道数 | 说明 |
|---|---|---|
| `fixed_discovery/` | 29 | task-01/02 脚本批 fixed 发现题（case_id 含实体真名后缀，如 `case_*_risperidone.json`，29 道全列见本目录） |
| `open_discovery/` | 4 | task-03 脚本批 open 发现题（`case_open_*_superconductor.json` 4 道） |
| `constraint_satisfaction/` | 2 | pilot 题：`case_cs_cns_drug.json`、`case_cs_supercon_material.json` |
| `constrained_optimization/` | 2 | pilot 题：`case_co_cns_drug.json`、`case_co_supercon_material.json` |

**口径**：

- 归档 gt 不计入 gt 查重分母（owner 裁定③：查重分母=活跃语料 150 道）。
- 活跃语料现役 150 道 = `constraint_satisfaction/` 60 +
  `constrained_optimization/` 60 + `discovery_haystack/` 30，将由
  task-07 批次①~⑤逐批手写覆盖。
- 本目录不再参与 lint 全量扫描、eval 语料枚举与 b_partition 注册；
  活引用已按路径级改指现役目录（gateway 边表→`constraint_satisfaction/
  template.json`、query_spec 两族条目 `"status": "deprecated"`）。
- 保留原因：历史 trace、研究文档与 E6 样本对账仍需回溯原文；
  删除待 task-07 终冻结后另行裁定。
