# 逆合成树搜索题(7 道)

## 场景

想象你是有机化学家,老板给你任务:"给你一个目标药物分子(SMILES),规划一条可行的全合成路线:一步步把它拆解到可以买到的起始原料,每一步给出反应类型和它在已知反应里的先例依据。"

你需要:
1. **策略**:解析目标骨架,定逆合成策略(找可断键/官能团互变)
2. **首层拆解**:提出目标的直接前体 + 反应类型
3. **递归拆解**:对每个前体继续拆,直到可购原料
4. **终止判定**:所有叶子都是可购原料则停
5. **提交路线**:完整步骤序列 + 每步条件
6. **报告**:标注哪步有先例 / 哪步是新提议

这就是**树搜索**——递归拆解成一棵树,从根(目标)到叶子(可购原料)。

## 这道题考什么

- **策略靠不靠谱**(有没有说清"倒推拆解 → 拆到可购原料 → 每步有先例")
- **每步反应合不合法**(RDKit 解析 + 反应模板匹配)
- **路径接不接得上**(每步产物 = 下一步反应物,树连通)
- **叶子真能买到吗**(叶子 ∈ ZINC 可购库)
- **有没有绕圈/退化**(合成树不能有环)
- **反应类型有没有先例**(反应类型 ∈ USPTO-50k 模板库)
- **路线长短合理吗**(步数落在合理区间)
- **关键中间体命中已知路线吗**(中间体 ∩ 真实路线)
- **报告诚实吗**(有没有如实标"这步有先例 / 这步是我新提的")

## 例子:aspirin 逆合成

```json
{
  "task_id": "retroE_aspirin",
  "template": "caseE_逆合成树搜索",
  "root": {
    "type": "target_molecule",
    "name": "aspirin",
    "smiles": "CC(=O)Oc1ccccc1C(=O)O"
  },
  "checkpoint_gt": {
    "cp2_reaction_validity": {
      "type": "rule",
      "note": "每步反应用 RDKit 校验合法性"
    },
    "cp3_path_connectivity": {
      "type": "graph",
      "mode": "path_connected",
      "note": "每步产物=下一步反应物,树连通"
    },
    "cp4_leaf_reachability": {
      "type": "set",
      "gt_buyable": ["Oc1ccccc1C(=O)O"],
      "note": "叶子原料 ∈ ZINC 可购库"
    },
    "cp6_reaction_precedent": {
      "type": "set",
      "n_uspto_precedents": 100,
      "note": "反应类型 ∈ USPTO-50k 模板库"
    },
    "cp7_reasonable_depth": {
      "type": "numeric",
      "gt_steps": 2,
      "tolerance": 2
    },
    "cp8_hit_known_route": {
      "type": "set",
      "gt_intermediates": ["Oc1ccccc1C(=O)O"]
    }
  },
  "known_route": {
    "steps": 2,
    "key_intermediates": ["Oc1ccccc1C(=O)O"]
  }
}
```

## 字段含义

| 字段 | 白话 |
|---|---|
| `root.smiles` | 目标分子的 SMILES 表示 |
| `cp2_reaction_validity` | 每步反应在化学上成不成立(RDKit 校验) |
| `cp3_path_connectivity` | 路径连通性(每步产物 = 下一步反应物) |
| `cp4_leaf_reachability` | 叶子原料能不能买到 |
| `cp6_reaction_precedent` | 反应类型有没有先例 |
| `cp7_reasonable_depth` | 总步数落在合理区间 |
| `cp8_hit_known_route` | 关键中间体命中已知路线 |
| `known_route.steps` | 已知路线的步数 |
| `known_route.key_intermediates` | 已知路线的关键中间体 |

## 难度标签

- **easy**:已知路线短(2 步),如 aspirin
- **medium**:已知路线适中(3-5 步),如 paracetamol
- **hard**:已知路线长(6+ 步),如 ibuprofen

## 正负例标签

- **positive**:正常逆合成题,有已知路线可对照
- **negative_control**:无已知路线或无反应先例,答案应该是"合成不出"

## 7 道题一览

- `retroE_aspirin.json` — 阿司匹林(2 步,easy)
- `retroE_paracetamol.json` — 扑热息痛(3 步,medium)
- `retroE_lidocaine.json` — 利多卡因(4 步,medium)
- `retroE_caffeine.json` — 咖啡因(5 步,medium)
- `retroE_warfarin.json` — 华法林(3 步,medium)
- `retroE_ibuprofen.json` — 布洛芬(6 步,hard)
- `retroE_metformin.json` — 二甲双胍(2 步,easy)

## 杀手检查点:cp3 路径连通

这是逆合成题的杀手锏:agent 可能写出"步骤齐全但中间断链"的路线(如第 2 步产物写成 X,第 3 步反应物却用了 Y ≠ X),纯读文字看不出问题,但把它画成图校验,发现中间冒出对不上的中间体 → 立刻露馅。
