# 靶点验证多证据题(6 道)

## 场景

想象你是药厂投资决策委员会成员,老板给你任务:"评估把 EGFR 作为治疗 NSCLC 的药物靶点是否可行:系统收集遗传、表达、通路、安全性等各类证据,逐项权衡,最后给出 go / no-go 结论 + 置信度 + 依据。"

你需要:
1. **策略**:列要查的证据类型 + 整合打分法
2. **逐类取证**:遗传 → 表达 → 通路 → 安全性,按信息价值排序去查
3. **各证据核验**:每类证据的强度/方向
4. **安全性/反靶**:查副作用风险
5. **整合下结论**:go/no-go + 置信度
6. **报告**:分证据强弱诚实标注

这就是**多证据序贯推理**——逐类取证、权衡、整合,最后下判断。

## 这道题考什么

- **策略靠不靠谱**(有没有规划"多类证据 → 权衡 → 结论 + 置信")
- **关键证据类型齐不齐**(遗传/表达/通路/安全 4 类核心证据都查了没)
- **先查高价值证据吗**(遗传关联应优先,而不是先纠结边角料)
- **遗传关联强度对不对**(Open Targets 遗传关联分 vs 真值)
- **表达方向/通路对不对**(Open Targets 表达/通路证据 vs 真值)
- **安全性/反靶查了没**(有没有查副作用风险)
- **结论跟证据一致吗**(综合证据分 < 0.4 不得给 go,这是杀手检查点)
- **最终结论命中真值吗**(go/no-go vs Open Targets overall score)
- **报告诚实吗**(有没有如实区分"证据支持 / 不确定 / 反对")

## 例子:EGFR × NSCLC

```json
{
  "task_id": "targetF_EGFR-NSCLC",
  "template": "caseF_靶点验证多证据",
  "root": {
    "type": "target-disease",
    "target": "EGFR",
    "target_ensembl": "ENSG00000146648",
    "disease": "non-small cell lung carcinoma",
    "disease_efo": "EFO_0003060"
  },
  "checkpoint_gt": {
    "cp2_evidence_coverage": {
      "type": "tool_grounding",
      "required_evidence_types": ["genetic", "expression", "pathway", "safety"]
    },
    "cp3_evidence_order": {
      "type": "ranking",
      "info_value_order": ["genetic", "pathway", "expression", "known_drug", "literature", "animal"]
    },
    "cp4_genetic_score": {
      "type": "numeric",
      "gt": 0.85,
      "tol": 0.15
    },
    "cp5_expression_pathway": {
      "type": "numeric_set",
      "gt": {"expression": 0.72, "pathway": 0.91}
    },
    "cp6_safety_checked": {
      "type": "tool_grounding",
      "required": ["safety_evidence"],
      "gt_safety_records": 21
    },
    "cp7_conclusion_evidence_consistency": {
      "type": "rule",
      "rule": "综合证据分<0.4 时不得给 go;综合证据分>0.7 时不得给 no-go",
      "gt_overall": 0.852
    },
    "cp8_final_conclusion": {
      "type": "numeric",
      "gt": 0.852,
      "thresholds": {"go": 0.5, "no_go": 0.3}
    }
  },
  "final_answer": {
    "go_no_go": "go",
    "overall_score": 0.852,
    "confidence": "high"
  }
}
```

## 字段含义

| 字段 | 白话 |
|---|---|
| `root.target` | 这道题考的靶点(如 EGFR) |
| `root.disease` | 这道题考的疾病(如 NSCLC) |
| `cp2_evidence_coverage` | 必查的 4 类证据都查了没 |
| `cp3_evidence_order` | 取证顺序(信息价值高的优先) |
| `cp4_genetic_score` | 遗传关联分 vs 真值 |
| `cp5_expression_pathway` | 表达 + 通路证据 vs 真值 |
| `cp6_safety_checked` | 安全证据查了没 |
| `cp7_conclusion_evidence_consistency` | 结论跟证据一致吗(杀手检查点) |
| `cp8_final_conclusion` | 最终结论 vs Open Targets overall |
| `final_answer.go_no_go` | 标准答案:go / no-go / uncertain |
| `final_answer.overall_score` | Open Targets 总体关联分 |
| `final_answer.confidence` | 置信度(high/medium/low) |

## 难度标签

- **easy**:证据维度少(<4),证据量少(<30 条)
- **medium**:证据维度适中(4-5),证据量适中(30-100 条)
- **hard**:证据维度多(6+),证据量大(>100 条)

## 正负例标签

- **positive**:正常验证题,overall >= 0.5,答案是 go
- **negative_control**:overall < 0.3 或数据不足,答案是 no-go

## 6 道题一览

- `targetF_EGFR-NSCLC.json` — EGFR × NSCLC(overall 0.852,go,positive)
- `targetF_ALK-NSCLC.json` — ALK × NSCLC(overall 0.756,go,positive)
- `targetF_BRAF-Melanoma.json` — BRAF × 黑色素瘤(overall 0.819,go,positive)
- `targetF_ERBB2-BreastCancer.json` — ERBB2 × 乳腺癌(overall 0.696,go,positive)
- `targetF_BTK-CLL.json` — BTK × CLL(overall 0.720,go,positive)
- `targetF_ABL1-CML.json` — ABL1 × CML(overall 0.291,no-go,negative_control)

## 杀手检查点:cp7 结论-证据一致

这是多证据题的杀手锏:agent 可能"证据摆一堆,结论却是拍脑袋的"(如综合证据分 0.3 却拍板 go),纯读文字看不出问题,但把"综合证据强度"和"最终结论"对齐校验,发现证据弱却 go → 立刻露馅。
