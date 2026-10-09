# PoseBusters — 采样说明（深度版）【终态 3D 构象 → 物理/化学合理性接口反查】

## 论文与来源
- 论文：*PoseBusters: AI-based docking methods fail to generate physically valid poses or generalise to novel sequences*（Buttenschoen, Morris, Deane；arXiv:2308.05777，Chem. Sci. 2024，500+ 引）
- 代码：https://github.com/maabuu/posebusters ｜ 本地采样：gh raw，2026-07（主程序 + 5 个检查模块 + 2 套检查配置 + 检查项文档 + 真实配体样例）

## 它是什么
一个**"终态反查"工具/benchmark 套件**：拿 AI 对接/构象生成方法**最终产出的分子 3D 构象**，用 RDKit 跑一整套物理/化学合理性检查，逐项给布尔判定。它实测发现"AI 对接方法常生成物理上无效的构象"，由此成为 docking 评测的标配附加层（PoseBench 内置于其评测）。

## ★ 核心机制：对最终 pose 跑哪些"接口检查"（真实配置 `config/mol.yml`，单分子模式）
| 检查组 | 检查项（真实名称） | 查什么 |
|---|---|---|
| Loading | `mol_pred_loaded` | 分子文件能否被加载 |
| Chemistry | `passes_rdkit_sanity_checks` / `inchi_convertible` / `all_atoms_connected` / `no_radicals` | RDKit 化学净化、可转 InChI、原子全连通、无自由基 |
| Geometry | `bond_lengths_within_bounds` / `bond_angles_within_bounds` / `no_internal_clash` | **键长/键角是否在物理界限内、分子内部无立体冲突**（距离几何界限矩阵，`modules/distance_geometry.py` 12.6KB） |
| Ring flatness | `flatness_passes`（环平坦/非平坦/双键平坦三档） | 芳香环该平的平、该翘的翘（`modules/flatness.py`） |
| Energy ratio | `energy_ratio_passes` | 构象能量相对参考的比率不过高（`modules/energy_ratio.py`） |
| （dock 模式附加，`config/dock.yml`） | 蛋白-配体分子间距离/冲突（`modules/intermolecular_distance.py`）、体积重叠、RMSD、SuCOS 药效团重叠 | pose 与蛋白是否穿插、与实验构象偏差 |

## 完整 case（真实样例 `sample_pose/1ia1_ligand.sdf`）
PDB 1IA1 的配体 TQ3（19 原子/21 键，带 3D 坐标）。用法（`posebusters_main.py`）：`PoseBusters(config="mol").bust(mol_file)` → 输出上表每项检查的 True/False + 汇总分。**这就是"对最后结果调接口、得到值"的标准形态：输入=最终产物文件，接口=RDKit 物理化学检查，值=逐项布尔+通过率。**

## 判分性质
- **hard**：全部 RDKit/几何/能量确定性计算，无 LLM、无人工；
- **评终态**：只看最终 pose 合不合物理，**完全不看生成过程**（生成方法对它是个黑盒）——与我们发现的"终态反查"家族（Tartarus/TDC/TOMG-Bench）同族，但检查维度是**几何/物理合理性**（它们没有的一档）；
- 发现能力强：论文实证多个深度学习对接方法在物理有效性上翻车——"分高但构象物理无效"被它直接抓出。

## 对我们的价值
1. **可直接当我们 Mode A 的"几何级"验证器**：我们 Mode A 目前只有性质级检查（带隙/ehull/合成性），缺 3D 物理合理性这一档；PoseBusters 的检查套件（尤其 distance_geometry 界限矩阵 + energy_ratio）正好补上，医药/3D 任务的 cp 可以直接调它当 oracle；
2. **范式佐证**：它证明"终态产物→物理接口逐项 hard 检查"在医药界已是公认刚需——我们的 Mode A 与此同构，差异化在 Mode B（过程锚定），那是它没有也不做的。

## 采样内容
- `posebusters_main.py`（主程序）、`modules/`（sanity/distance_geometry/intermolecular_distance/energy_ratio/flatness 五个检查模块真实实现）；
- `config/mol.yml` + `config/dock.yml`（两套检查清单配置）；
- `checks_documentation.rst`（检查项文档）、`datasets_pdb.csv`（PDB 列表）；
- `sample_pose/1ia1_ligand.sdf`（真实配体 3D 构象）、`molecule_table.csv`。
