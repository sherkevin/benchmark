# PoseBench — 采样说明（深度版）【蛋白-配体结构预测综合 benchmark，内置 PoseBusters 终态反查】

## 论文与来源
- 论文：*PoseBench: A Comprehensive Benchmark for Practical Molecular Modeling*（BioinfoMachineLearning，Nature Machine Intelligence 2024/25）
- 代码：https://github.com/BioinfoMachineLearning/PoseBench ｜ 本地采样：gh raw，2026-07（评测配置 + 真实测试集清单 + README 摘要）

## 它是什么
**蛋白-配体结构预测（docking/共折叠）方法的综合评测平台**：统一跑 11 种方法（DiffDock、FABind、DynamicBind、NeuralPLexer、FlowDock、RFAA、Chai、Boltz、AlphaFold3、Vina、ensemble）× 4 个数据集，对**最终预测结构**做双重 hard 反查：**RMSD（与实验结构偏差）+ PoseBusters 物理合理性套件**。

## 与 PoseBusters 的关系（为什么是两个）
- **PoseBusters** = 一个**检查套件/工具**（输入 pose → RDKit/几何检查 → 布尔+通过率）；
- **PoseBench** = 一个**完整 benchmark**（多方法×多数据集×统一协议），其终态判分**内置调用 PoseBusters 套件**——真实配置 `configs/inference_analysis.yaml` 里 `full_report: full PoseBusters report`、`force_rescore: ...with the PoseBusters software suite` 两行直接证明这一点。

## ★ 评测机制（真实配置 `configs/inference_analysis.yaml`）
- **method**：11 个被测方法任选（`diffdock/fabind/dynamicbind/neuralplexer/flowdock/rfaa/chai-lab/boltz/alphafold3/vina/ensemble`）；
- **dataset**：4 个（`posebusters_benchmark`（428 个无晶体接触复合物）、`astex_diverse`、`dockgen`、`casp15`（真实盲测））；
- **判定**：对每个最终预测结构算 RMSD（vs 实验 holo 结构）+ 跑 PoseBusters 全套检查；`ensemble_ranking_method: consensus/ff` 决定多构象如何取 top1；
- **基线维度**：`pocket_only_baseline`（只给口袋）、`v1_baseline`——控制"给多少先验"的难度轴（类似我们的 L0-L3 思路）。

## 完整 case（真实测试集条目 `test_cases/posebusters_benchmark_ensemble_inputs_head.csv`）
```
protein_input,ligand_smiles,name
data/.../5S8I_2LY_holo_aligned_predicted_protein.pdb,,5S8I_2LY
```
每条 = 一个真实 PDB 复合物（蛋白结构文件 + 配体）——被测方法输出预测 pose，PoseBench 用实验结构算 RMSD、用 PoseBusters 查物理合理性。`posebusters_pdb_ccd_ids.txt` 列出无晶体接触的靶点（5SAK_ZRY、5SB2_1K2、5SD5_HWI…），保证评测不被晶体堆积假象污染——这是它的"质量门"。

## 判分性质
- **hard**：RMSD 数值计算 + PoseBusters 确定性检查，无 LLM、无人工；
- **评终态**：只看最终结构对不对/合不合理，不看推理过程；
- **多方法统一协议**：方法即插即用（input CSV → 输出目录 → 统一 rescoring）——是"终态反查"在结构生物学里的最完整实现。

## 对我们的价值
1. **"统一协议多方法评测"的模板**：我们的引擎对 agent 也是"方法即插即用"（任何 agent 产出 trace+产物即可评）——PoseBench 的 input_csv→rescore 协议与我们 events→grade 协议同构，写论文时可引为同类设计；
2. **质量门借鉴**：`posebusters_pdb_ccd_ids.txt`（剔除晶体接触靶点）= 它的防污染门，对应我们的内容指纹/负例门——benchmark 都需要"哪些样本根本不该进集"的显式清单；
3. **casp15（真实盲测集）**：证明"用未公开答案的盲测集做最终防线"是该领域惯例，对应我们"test 冻结+发布后污染监测"的门。

## 采样内容
- `configs/inference_analysis.yaml` + `_casp.yaml`（评测配置全文，含 11 方法/4 数据集/PoseBusters rescoring 证据）；
- `test_cases/posebusters_benchmark_ensemble_inputs_head.csv`（真实测试条目）、`posebusters_pdb_ccd_ids_head.txt`（无晶体接触靶点清单）、`casp15_ensemble_eval_inputs_head.csv`；
- `README_REPO_head.md`（官方 README 前 15KB）。
