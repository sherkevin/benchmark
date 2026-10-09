# MOSES — 采样说明（深度版）

## 论文与来源
- 论文：*Molecular Sets (MOSES): A Benchmarking Platform for Molecular Generation Models*（Front. Pharmacol. 2020）
- 代码：https://github.com/molecularsets/moses ｜ 本地采样：gh raw，2026-07（metrics.py 全指标 + 样例输出）

## 它是什么
分子**生成模型**的标准化评测：给一批生成的 SMILES，算**分布层面**的质量指标——不针对单分子优化，而看"这批分子整体像不像真药物、够不够多样/新颖"。

## ★ 指标全表（真实代码 `metrics.py` 的 `get_all_metrics`，17–60行）
对生成的分子集 `gen`，对照参考集（ZINC 子集）算这些：
- **valid**：RDKit 能解析的比例（`get_mol` 解析，= 化学合法性/存在性）；
- **unique@1k/10k**：前 k 个里不重复比例；
- **novelty**：不在训练集里的比例（创新性）；
- **FCD**（Fréchet ChemNet Distance）：生成分布 vs 参考分布的 **ChemNet 特征距离**（`fcd_torch.FCD`，越像越小）；
- **Frag / Scaf**：片段/骨架相似度（`compute_fragments`/`compute_scaffolds`）；
- **SNN**（Similarity to Nearest Neighbour）：生成分子与参考集最近邻的 Tanimoto 相似度（`average_agg_tanimoto`）；
- **IntDiv / IntDiv2**：内部多样性（分子间 Tanimoto 的均值/均方根）；
- **Filters**：通过 PAINS 等化学过滤器的比例；
- **分布差**：logP / SA / QED / weight 与生成分布的 Wasserstein 距离。

## 怎么测 / 怎么打分
- **判分器**：一套通用指标函数（`get_all_metrics(gen, k, ...)`，写一次）；
- **金标准/target**：**现算**——validity/unique/novelty 是集合统计，FCD/Frag/Scaf/SNN 对照参考分子集（ZINC），全确定性；FCD 用预训练 ChemNet 特征；
- **hard 吗**：是（统计 + RDKit + 预训练特征）；
- **对象**：对"**一批分子**"的**分布**打分，既非单分子优化也非过程卡点。

## 与 GuacaMol 的分工（分子生成评测的两条主线）
- **MOSES = 分布学习**：这批分子像不像真药、够不够多样/新颖（看分布）；
- **GuacaMol = 目标导向**：能不能把分子往某个化学目标优化（看单目标分）。

## 对我们的价值
"分布层面质量指标"代表。对本项目参考价值较低（我们要**单任务过程卡点**，非分布指标），收录用于厘清分子生成评测的分布学习 vs 目标导向两条主线。

## 采样内容
`metrics.py`（全部指标真实实现）、`sample_metrics_char_rnn_1.csv`（某模型样例指标输出）、`REPO_README.md`。
