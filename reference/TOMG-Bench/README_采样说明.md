# TOMG-Bench — 采样说明【原生测 LLM + RDKit 接口反查分子】

## 论文与来源
- 论文：*TOMG-Bench: Evaluating LLMs on Text-based Open Molecule Generation*（arXiv:2412.14642）
- 代码/数据：https://github.com/phenixace/tomg-bench （test.csv 为 Git-LFS）
- 本地采样通道：判分脚本走 gh raw；真实 case 走 media.githubusercontent LFS（各采前 ~30 行），2026-07

## 与需求的匹配
- **被测者 = LLM 本身**（论文实测 25 个 LLM），不是遗传算法/优化器；
- **对 LLM 产出的分子调 RDKit 接口反查**：合法性(validity)、属性是否达成(accuracy)、相似度/新颖性；
- 判分纯 hard（RDKit 确定性计算，无 LLM judge）。正是"测智能体 + 调接口反查分子"。

## 三类任务（真实 case，采样自 sample_cases/）
- **MolCustom（定制生成）**：按要求造分子。真实 case（AtomNum）：
  `"Please generate a molecule with 8 carbon atoms, 1 nitrogen atom, and 3 oxygen atoms."` → 目标列 carbon=8,nitrogen=1,oxygen=3。FunctionalGroup 子任务：`"...1 halo group, and 2 benzene rings"`。
- **MolEdit（编辑）**：真实 case（DelComponent）：
  `"Please remove a benzene_ring from the molecule O=C(Cc1ccc(Cl)cc1)NC[C@](O)(c1ccccc1)C(F)(F)F."` → 给定原分子 + 要删的基团。
- **MolOpt（优化）**：真实 case（LogP）：
  `"Modify the molecule Cc1ccnc(...)...[N+](=O)[O-] to decrease its LogP value."` → 给定原分子(logP=3.42) + 优化方向。

## LLM 真实输出长什么样（sample_prediction/）
biot5-base 对 AtomNum 的真实产出（一列 `outputs`，每行一个分子）：
`N#[O+1]` / `[18OH2]` / `CCCCCCCCCCCCCC` …（SELFIES/SMILES 串）。

## 怎么判分（真实代码，两步）
1. **抽取**（`correct.py`）：从 LLM 原始输出里正则抠出分子串（处理 JSON/箭头/SELFIES 解码）。
2. **RDKit 反查**（`evaluate.py`，以 AtomNum 为例，真实逻辑）：
```python
if mol_prop(out, "validity"):                 # ① RDKit 能否解析=分子是否合法/存在得了
    flag = 1
    for atom in atom_type:                     # ② 逐元素数原子
        if mol_prop(out, "num_"+atom) != int(data[atom][idx]):
            flag = 0; break                    # 有一个数目不对就判错
print("Accuracy:", sum(flags)/len(flags))      # 达成率
print("Validity:", len(valid)/len(flags))      # 合法率
```
- **MolOpt**：用 RDKit 算编辑后分子的 LogP/QED/MR，判断是否按要求方向变化；
- **MolEdit**：判断目标基团是否被正确增删，且与原分子保持高相似度；
- 汇总指标：**加权平均准确率**（accuracy × novelty/similarity 权重）。

## 判分性质
- **hard**：全 RDKit 确定性计算 + 数值/计数比对，无 LLM judge、无人工；
- **单轮为主**：给指令→出分子→反查，本身不含多轮过程卡点（多轮迭代版见 ICIMBench）；
- 数据来源：从 Zinc-250K 采样真实分子、用 RDKit 反推指令（每子任务 5000 题）。

## 采样内容
- `evaluate.py`/`correct.py`/`generation.py`：判分、抽取、出题脚本（真实源码）；
- `task_readme/{MolCustom,MolEdit,MolOpt}.md`：三类任务与指标定义；
- `sample_cases/`：6 个子任务各 ~30 条真实 case；
- `sample_prediction/`：biot5-base 的真实输出样例。

## 对我们的价值
这是最贴"测 LLM agent + 调接口反查分子"的现成 benchmark：**RDKit 就是那个"反查接口"**（查合法性=存在性、查原子/基团/属性=要求达成）。要变"过程量"，叠加多轮（ICIMBench 式）即可。
