# TDC + PMO — 采样说明（深度版）

## 论文与来源
- TDC：*Therapeutics Data Commons*（Harvard，NeurIPS 2021 D&B）｜ https://tdcommons.ai/functions/oracles/ ｜ github.com/mims-harvard/TDC
- PMO：*Sample Efficiency Matters: A Benchmark for Practical Molecular Optimization*（Gao et al., NeurIPS 2022），用 TDC oracle 搭 23 个优化任务
- 本地采样：gh raw，2026-07（`oracle.py` 40KB 全 oracle 实现 + filter/docking + molgen）

## 它是什么
一个**药物发现 oracle 库**：给一个 SMILES，调 `oracle(smiles) -> 分数`，返回成药性/活性/毒性/可合成性等性质分。PMO 把 23 个 oracle 包成"限预算（1万次调用）下的分子优化任务"。

## ★ 关键机制：oracle 分两档，hard 程度不同（真实代码 `oracle.py`）
这是 TDC 最该说清的——**它的 oracle 不是一类东西，"硬"的程度分两级**：

**A 类·确定性化学公式（纯 hard，无模型误差）**
```python
def qed(smiles):                      # 579行
    mol = Chem.MolFromSmiles(smiles)
    return QED.qed(mol)               # 594行：RDKit 内置 QED 公式，确定性
def SA(s):                            # 641行
    return sascorer.calculateScore(mol)  # 合成可及性 sascorer 公式（339-436行：环大小/大环/对数惩罚，最后钳到 [1,10]）
```
- `qed` / `SA` / `logP` / `相似度`：RDKit 现场算，**同样输入永远同样输出，无误差**。

**B 类·加载的 ML 预测器（确定但有模型误差，非物理真值）**
```python
def load_drd2_model():  return load_pickled_model(name)   # 467行
def drd2(smile):        return drd2_model(featurize(smile))  # 488-501行：神经网络分类器
def load_gsk3b_model(): return load_pickled_model(gsk3_model_path)  # 660行
def jnk3(smiles):       # 706行：load_pickled_model(jnk3_model_path)
```
- `DRD2` / `GSK3β` / `JNK3`（靶点活性）、`hERG/ClinTox/Tox21/LD50`（毒性）：都是在**真实实验数据（ChEMBL/TDC 数据集）上预训练的神经网络**，pickle 加载后对分子打分。**确定性但对真实活性的逼近有模型误差**。

**一句话**：TDC 的"反查接口"里，QED/SA/合成性是**公式级 hard**；活性/毒性是**预测器级**（可作 oracle 但非物理真值）。

## 完整 case（PMO 优化任务）
任务形态："优化分子使 oracle 分最高"。如 `GSK3β`：造分子让 GSK3β 活性预测分最高；`Osimertinib_MPO`：同时满足多个物化约束（MPO 多目标）。agent/算法在 1 万次 oracle 调用预算内迭代生成分子。

## 怎么测 / 怎么打分
- 判分器 = oracle 本身（写一次，全体分子复用）：`oracle(smiles)->score`；
- PMO 指标 = **top-10 平均分 vs oracle 调用次数的 AUC**（既看最终分，也看样本效率）；
- **无中间过程卡点**：评的是优化轨迹的"终点高度+效率"，不判中间每步对错。

## checkpoint / 金标准怎么来
- 无需人工标答案——oracle 现算（A类公式 / B类预测器）；
- 人力 vs 自动：人力=挑数据集训预测器 + 定 MPO 组合（一次性）；每个分子评分全自动。

## 对我们的价值
- **可复用的"毒性/成药性反查接口"最全库**：QED/SA/docking（确定性）可直接当我们卡点的 target；
- **一个警醒**：做严肃 hard 卡点时，优先用 A 类公式 oracle 或 Tartarus 的物理模拟；B 类活性/毒性预测器当 oracle 要在论文里说明"这是预测器而非真值"。

## 采样内容
`oracle_src/oracle.py`（1266行，QED/SA/DRD2/GSK3β/JNK3/docking 等真实实现）、`filter.py`、`docking.py`、`molgen.py`、`REPO_README.md`。
