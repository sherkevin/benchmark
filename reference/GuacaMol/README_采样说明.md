# GuacaMol — 采样说明（深度版）

## 论文与来源
- 论文：*GuacaMol: Benchmarking Models for de Novo Molecular Design*（BenevolentAI，J. Chem. Inf. Model. 2019，arXiv:1811.09621）
- 代码：https://github.com/BenevolentAI/guacamol ｜ 本地采样：gh raw，2026-07（6 个判分源码）

## 它是什么
经典 de novo 分子生成 benchmark，两大类：**目标导向**（优化一组打分目标）+ **分布学习**（学训练集分布）。

## 完整 case（目标导向，真实打分函数 `common_scoring_functions.py`）
任务形态："优化分子使某个打分函数最大化"。打分函数全是 RDKit 确定性计算：
- **相似度目标**：`TanimotoScoringFunction`（32–56行）——给定一个 target 分子，算候选分子与其的**分子指纹 Tanimoto 相似度**（越像越高分），即"重新发现某药物分子"；
- **MPO 多目标**：`CNS_MPO_ScoringFunction`（59–85行）——同时把 5 个物化量（logP/分子量/tPSA/氢键供体）用**高斯修饰函数**压到 [0,1] 再平均，即"要一个各方面都达标的 CNS 药"；
- **公式约束**：`IsomerScoringFunction`（88行起）——分子必须符合给定分子式（同分异构）；
- **物化描述符**：`RdkitScoringFunction`（14–29行）——QED/logP 等 RDKit 描述符。

## 怎么测 / 怎么打分
- **目标导向**（`goal_directed_benchmark.py`）：给一个 scoring objective，生成器产出一批分子，取 top-k 平均分；
- **分布学习**（`distribution_learning_benchmark.py`/`frechet_benchmark.py`）：validity（RDKit 能否解析）、uniqueness、novelty（不在训练集）、**FCD**（Fréchet ChemNet Distance）、KL 散度。

## ★ 判分机制与 hard 程度
- **判分器**：一组通用打分函数（写一次，全体分子复用）；
- **金标准/target**：**RDKit 现场算**——Tanimoto 指纹、分子式、QED/logP/tPSA 都是确定性化学计算，FCD 用预训练 ChemNet 特征；无人工标答案、无 LLM judge；
- **hard 吗**：是（RDKit 确定性）；
- **局限**：纯**终态打分**（对生成分子打分），无中间过程卡点；且只测**药物相似度**，**不含材料、不含物理稳定性**。

## 与 Tartarus 的区别（定位）
GuacaMol/TDC/Tartarus 都是"目标导向优化 + oracle 打分"，但 GuacaMol 的 oracle 是**纯 RDKit 公式（物化/相似度）**，Tartarus 是**物理模拟（对接/量子化学）**，TDC 是**公式+神经网络混合**。GuacaMol 最简单、最"化学式"，无材料任务。

## 对我们的价值
"确定性化学打分函数库"的经典范式；QED/Tanimoto/Isomer 这类 RDKit 打分可直接当 hard 卡点 target。但它只测药物终态、无材料、无过程卡点。

## 采样内容
`src/`：`benchmark_suites.py`、`common_scoring_functions.py`、`scoring_function.py`、`goal_directed_benchmark.py`、`distribution_learning_benchmark.py`、`frechet_benchmark.py`（全真实判分源码）；`REPO_README.md`。
