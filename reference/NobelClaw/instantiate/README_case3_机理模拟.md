# 机理模拟对照题(5 道)

## 场景

想象你是药理学家,老板给你任务:"模拟 Dasatinib 作用于 CML 细胞后的动态响应:它扰动了哪些通路、细胞表型怎么变、跟已知实验是否一致、机理上怎么解释。"

你需要:
1. **识别靶点**:Dasatinib 主要靶点是 BCR-ABL(致癌蛋白)
2. **预测扰动**:Dasatinib 抑制 BCR-ABL 激酶活性
3. **模拟通路级联**:BCR-ABL 关了 → 下游 pCRKL↓、pSTAT5↓ → 增殖↓、凋亡↑
4. **对照实验**:跟已发表的 CML 实验数据比(pCRKL 抑制程度、IC50 量级)
5. **机理解释**:输出因果链,标注哪些经模拟、哪些有文献、哪些仍是假设

这就是**机理模拟对照**——预测一个动态过程,并用实验数据验证。

## 这道题考什么

- **找对致病主靶点没**(BCR-ABL,不是 EGFR)
- **机理假设方向对不对**(抑制 → 增殖↓)
- **扰动节点有没有乱点**(ABL 激酶活性被关掉,不是无关节点)
- **有没有如实说耐药**(T315I 突变致 Dasatinib 失效)
- **模拟的信号变化方向对不对**(pCRKL↓、pSTAT5↓)
- **模拟的细胞结局方向对不对**(增殖↓、凋亡↑)
- **模拟结论跟真实验对不对得上**
- **因果链每一环接得上吗**(靶点 → 节点 → 信号 → 结局,图连通校验)
- **有没有把猜的说成证实的**

## 例子:Dasatinib 在 CML 中

```json
{
  "task_id": "mech3_Dasatinib-CML",
  "template": "case3_机理模拟-对照",
  "root": {
    "type": "drug-disease",
    "drug": "Dasatinib",
    "disease": "CML",
    "drug_chembl": "CHEMBL1421",
    "disease_efo": "EFO_0000349"
  },
  "checkpoint_gt": {
    "cp1_targets": {
      "type": "set",
      "gt": ["BCR-ABL", "SRC", "KIT"]
    },
    "cp3_perturbation_nodes": {
      "type": "set",
      "gt": ["BCR-ABL", "SRC", "CRKL", "STAT5", ...],
      "note": "通路中该药可能作用的分子白名单"
    },
    "cp5_cascade_direction": {
      "type": "set",
      "gt": ["ABL1", "SRC", "CRKL", "STAT5"],
      "note": "信号传导路径中的关键节点"
    },
    "cp6_phenotype_signs": {
      "type": "structure",
      "gt": {"proliferation": "↓", "apoptosis": "↑"}
    },
    "cp8_causal_chain_connectivity": {
      "type": "graph",
      "mode": "path_connected",
      "note": "因果链连通性校验"
    }
  }
}
```

## 字段含义

| 字段 | 白话 |
|---|---|
| `root.drug` | 这道题考的药物 |
| `root.disease` | 这道题考的疾病 |
| `cp1_targets` | 药物真实靶点清单 |
| `cp3_perturbation_nodes` | 通路中该药可能作用的分子白名单 |
| `cp5_cascade_direction` | 信号传导路径中的关键节点 |
| `cp6_phenotype_signs` | 细胞结局方向(增殖↓、凋亡↑) |
| `cp8_causal_chain_connectivity` | 因果链连通性(靶点→节点→信号→结局,不能断) |

## 难度标签

- **easy**:通路简单(<15 蛋白),单靶点
- **medium**:通路适中(15-30 蛋白)或多靶点
- **hard**:通路复杂(>30 蛋白)且多靶点

## 正负例标签

- **positive**:正常机理题,有明确通路可模拟
- **negative_control**:无明确通路或数据不足,答案应该是"模拟不出"

## 5 道题一览

- `mech3_Dasatinib-CML.json` — Dasatinib × CML(BCR-ABL 抑制)
- `mech3_Vemurafenib-BRAF_V600E_melanoma.json` — Vemurafenib × 黑色素瘤(BRAF 抑制)
- `mech3_Gefitinib-EGFR+_NSCLC.json` — Gefitinib × NSCLC(EGFR 抑制)
- `mech3_Crizotinib-ALK+_NSCLC.json` — Crizotinib × NSCLC(ALK 抑制)
- `mech3_Trastuzumab-HER2+_breast_cancer.json` — Trastuzumab × 乳腺癌(HER2 抑制)

## 杀手检查点:cp8 因果链连通

这是机理题的杀手锏:agent 可能写出"逻辑通顺但根基错"的因果链(如 Dasatinib → EGFR → 凋亡↓ → 治愈 CML),纯读文字看不出问题,但把它画成图校验,发现 EGFR 不在 Dasatinib 真实靶点连通图里 → 立刻露馅。
