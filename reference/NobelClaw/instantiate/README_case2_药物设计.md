# 药物设计门控流水线题(5 道)

## 场景

想象你是药厂 AI 项目经理,老板给你任务:"为 GLP-1R 靶点设计几个新药分子,要求成药性可接受、不跟已上市药(如司美格鲁肽)重复。"

你需要按**门控流水线**走:
1. **靶点验证**(Gate 1):确认 GLP-1R 是成药靶点,了解结合口袋
2. **分子设计 + ADMET 筛选**(Gate 2):生成 50 个候选多肽 → 查 novelty → 预测 ADMET(吸收/分布/代谢/排泄/毒性)
3. **3D 结合验证**(Gate 3):对 ADMET 过的候选做分子对接,预测结合力 Kd
4. **虚拟细胞模拟**(Gate 4):对高亲和候选做细胞模拟,预测通路扰动
5. **聚合报告**(Gate 5):输出 top-N 多目标排序 + 实验建议

每关做完要**停下汇报**,确认了再进下一关。

## 这道题考什么

- **靶点找对没**(UniProt P43220)
- **目标定对没**(做新分子、跟参照药比)
- **novelty 检查偷工没**(查了全部候选)
- **ADMET 数值离不离谱**(毒性/溶解度在合理范围)
- **Kd 数值离不离谱**(物理上靠不靠谱)
- **对接跳过 ADMET 没**(对接的候选 ⊆ ADMET 过的)
- **细胞反应方向对不对**(胰岛素分泌 ↑)
- **模拟跟实验对照对不对得上**
- **报告数字前后自洽吗**(50 ≥ 新颖 ≥ ADMET过 ≥ 对接过 ≥ top-N)
- **报告有没有吹牛**(把模拟 1 个说成确认 3 个)

## 例子:GLP-1R 设计

```json
{
  "task_id": "drug2_GLP-1R",
  "template": "case2_药物设计_门控流水线",
  "root": {
    "type": "target",
    "value": "GLP-1R",
    "uniprot": "P43220",
    "chembl_id": "CHEMBL1784"
  },
  "checkpoint_gt": {
    "cp1_target_id": {
      "type": "structure",
      "gt": {"uniprot": "P43220", "chembl_id": "CHEMBL1784"}
    },
    "cp4_admet_reasonable": {
      "type": "rule+range",
      "pchembl_range": [6.37, 8.96]
    },
    "cp5_affinity_plausible": {
      "type": "numeric_range",
      "pchembl_range": [4.0, 9.96]
    },
    "cp8_actives_set": {
      "type": "set",
      "gt": ["CHEMBL410972", "CHEMBL412288", ...],
      "note": "活性配体集(pChembl >= 6.0)"
    }
  },
  "final_answer": {
    "top_ligands": [
      {"mol_id": "CHEMBL410972", "pChembl": 9.85},
      ...
    ]
  }
}
```

## 字段含义

| 字段 | 白话 |
|---|---|
| `root.value` | 这道题考的靶点(如 GLP-1R) |
| `root.uniprot` | 靶点的 UniProt 编号 |
| `root.chembl_id` | 靶点的 ChEMBL 编号 |
| `cp1_target_id` | agent 报的靶点编号跟真值比 |
| `cp4_admet_reasonable` | ADMET 预测值在合理范围 |
| `cp5_affinity_plausible` | 结合力预测值在合理量级 |
| `cp8_actives_set` | 已知活性配体集合 |
| `final_answer.top_ligands` | 最终推荐的 top 配体 |

## 难度标签

- **easy**:活性数据少(<80 条),容易筛完
- **medium**:活性数据适中(80-500 条)
- **hard**:活性数据多(>500 条),分布宽

## 正负例标签

- **positive**:正常设计题,有活性配体可找
- **negative_control**:无活性配体或数据不足,答案应该是"设计不出"

## 5 道题一览

- `drug2_GLP-1R.json` — GLP-1R(340 活性,medium,positive)
- `drug2_DPP-4.json` — DPP-4(997 活性,hard,positive)
- `drug2_EGFR.json` — EGFR(1000 活性,hard,positive)
- `drug2_BACE1.json` — BACE1(1000 活性,hard,positive)
- `drug2_mTOR.json` — mTOR(61 活性,easy,positive)
