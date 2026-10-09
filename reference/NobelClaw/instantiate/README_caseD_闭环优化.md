# 材料闭环优化题(12 道)

## 场景

想象你是材料科学家,老板给你任务:"在 Ba-Ti-O 这个体系里,找出最稳定的晶体结构。"

你需要:
1. 生成一批候选结构
2. 预测每个结构的稳定性(Ehull 值,越小越稳)
3. 挑出最稳的几个
4. 围绕它们再生成、再预测、再挑
5. 反复迭代,直到找不到更稳的了,停手

这就是**闭环优化**——反复改进、逼近最优,直到收敛。

## 这道题考什么

- **能不能越改越好**(best-so-far 单调递减)
- **会不会把找到的好结果弄丢**(第 3 轮 best 比第 2 轮大 = 状态丢失)
- **什么时候停**(收敛后还狂生成 = 浪费预算)
- **最终答案对不对**(找到的稳定相是不是真的在凸包上)

## 例子:Ba-Ti-O 体系

```json
{
  "task_id": "matD_Ba-Ti-O",
  "root": {
    "type": "chemical_system",
    "value": "Ba-Ti-O",
    "elements": ["Ba", "Ti", "O"]
  },
  "final_answer": {
    "stable_phases": [
      {"formula": "BaO3Ti", "ehull": 0.0, "spacegroup": "Ima2"},
      {"formula": "BaO5Ti2", "ehull": 0.0, "spacegroup": "Pnma"},
      ...
    ],
    "n_stable": 7
  },
  "checkpoint_gt": {
    "cp6_ehull_value": {
      "type": "numeric",
      "tol_eV_per_atom": 0.05,
      "note": "agent 预测的 Ehull 跟真值比,容差 0.05 内算对"
    },
    "cp8_stable_set": {
      "type": "set",
      "gt": ["BaO3Ti", "BaO5Ti2", ...],
      "note": "agent 提交的 top-K 跟这个集合求交集,算命中率"
    }
  }
}
```

## 字段含义

| 字段 | 白话 |
|---|---|
| `root.value` | 这道题考的材料体系(如 Ba-Ti-O = 钡-钛-氧) |
| `final_answer.stable_phases` | 最终答案:最稳定的晶体结构列表 |
| `ehull` | 稳定性指标,0 = 最稳(在凸包上),越大越不稳 |
| `spacegroup` | 晶体对称性(专业术语,不用懂) |
| `checkpoint_gt` | 中间每一步的标准答案 |
| `cp6_ehull_value` | agent 预测的 Ehull 值跟真值比,容差 0.05 eV/atom |
| `cp8_stable_set` | agent 提交的稳定相集合跟真值集合求交集 |

## 难度标签

- **easy**:稳定相少(<3 个),容易找全
- **medium**:稳定相适中(3-6 个)
- **hard**:稳定相多(>6 个)或分布密集

## 正负例标签

- **positive**:正常优化题,有稳定相可找
- **negative_control**:无稳定相(如 Zn-Sn-O),答案应该是"找不到"

## 怎么用

1. 给 agent 任务:"在 Ba-Ti-O 里找最稳晶体"
2. agent 输出每一步的预测结果
3. 用 `checkpoint_gt` 里的真值逐步骤判分
4. 最终答案跟 `final_answer.stable_phases` 比

## 12 道题一览

- Ba-Ti-O(钡钛氧,7 稳定相,medium)
- Sr-Ti-O(锶钛氧,8 稳定相,hard)
- Li-Fe-O(锂铁氧,5 稳定相,medium)
- Na-Nb-O(钠铌氧,7 稳定相,hard)
- ...
