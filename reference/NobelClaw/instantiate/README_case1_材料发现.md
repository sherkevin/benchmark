# 材料发现漏斗题(4 道)

## 场景

想象你是材料筛选工程师,老板给你任务:"从 73,561 个候选材料里,筛出所有超导体(Tc > 4K),并锁定最有希望的几个体系。"

你需要:
1. 初筛:用 Tc > 4K 过滤,剩下 2,319 个
2. 分类:把剩下的分成"确定是超导(positive)/ 确定不是(negative)/ 还说不准(unverified)"三堆
3. 建模:用 positive/negative 训练分类器
4. 锁体系:用分类器筛 unverified,锁定 Hf-Zr-Re 和 Zr-V-Re 两个体系
5. 生成候选:在这两个体系里枚举组成,生成新结构
6. 联合过滤:用稳定性 + 超导性 + 分类置信度联合过滤
7. 排序:综合评分排序,输出 top-K

这就是**漏斗形**——海量候选层层筛,最后剩少量高质量候选。

## 这道题考什么

- **初筛数对不对**(Tc > 4K 筛完剩多少)
- **三分类对不对**(positive/negative/unverified 各多少)
- **分类器准不准**(拿已知超导 MgB2 考它)
- **锁定体系对不对**(Hf-Zr-Re / Zr-V-Re)
- **生成结构合不合法**(pymatgen 解析)
- **生成时跑没跑题**(生成 ∈ 锁定体系)
- **排序对不对**(跟实验 Tc 比 Spearman)
- **最终命中几个真货**(跟实验确认名单比)

## 例子:超导发现(premium 题)

```json
{
  "task_id": "mat1_superconductor_premium",
  "template": "case1_材料发现_超导体",
  "tier": "A路 premium(期刊 + 实验验证)",
  "checkpoint_gt": {
    "cp_screen_count": {
      "type": "numeric",
      "gt": 2319,
      "tol_frac": 0.15,
      "note": "Tc>4K 初筛后剩多少"
    },
    "cp_classify_counts": {
      "type": "numeric_set",
      "gt": {"positive": 158, "negative": 385, "unverified": 981}
    },
    "cp_classifier_sanity": {
      "type": "numeric",
      "gt": {"MgB2_confidence": ">=0.9"},
      "measured": 0.992
    },
    "cp_locked_families": {
      "type": "set",
      "gt": ["Hf-Zr-Re", "Zr-V-Re"]
    },
    "cp_final_hits": {
      "type": "set_with_values",
      "gt": [
        {"formula": "ZrRe2", "tc_real_K": 6.4, "known": true},
        {"formula": "Hf3ZrRe8", "tc_real_K": 5.9, "new_discovery": true},
        ...
      ]
    }
  }
}
```

## 字段含义

| 字段 | 白话 |
|---|---|
| `tier` | A路 premium(论文+实验验证)或 B路(数据库重建) |
| `cp_screen_count` | 初筛后剩多少候选 |
| `cp_classify_counts` | 三分类各多少 |
| `cp_classifier_sanity` | 分类器拿已知超导考,置信度 >=0.9 算过 |
| `cp_locked_families` | 锁定的目标体系 |
| `cp_final_hits` | 最终候选名单 + 实测 Tc |

## 难度标签

- **easy**:候选少(<1000),分类清晰
- **medium**:候选适中(1000-5000)
- **hard**:候选多(>5000),分类模糊

## 正负例标签

- **positive**:正常筛选题,有真货可筛
- **negative_control**:无稳定相或无超导相,答案应该是"筛不到"

## 4 道题一览

- `mat1_superconductor_premium.json` — A路 premium,超导发现(论文+实验)
- `mat1_Li-Fe-O_funnel.json` — B路,锂铁氧体系
- `mat1_Na-Nb-O_funnel.json` — B路,钠铌氧体系
- `mat1_Ca-Si-O_funnel.json` — B路,钙硅氧体系
