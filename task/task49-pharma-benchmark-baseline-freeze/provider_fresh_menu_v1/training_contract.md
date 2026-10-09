# Provider-fresh BBB 菜单：统一训练合同 v1

日期：2026-09-30  
状态：训练前合同；不包含 confirmation 预测。

## 目标

把四个候选 BBB provider 训练成一组可绑定的、彼此输入相同但模型 family 不同的工具。这里的目标是补齐 provider lineage，不是先追求某个模型的最高 BBB 分数。

## 数据边界

- 唯一训练输入：`provider_fresh_menu_v1/manifest.json` 指向的 `provider_train.csv`。
- 唯一 checkpoint 选择输入：`provider_validation.csv`。它只用于选择停止轮或 checkpoint，不回流训练。
- 不读取 selector development panel 的 provider 输出，不读取 157 条 confirmation 分子的标签或 provider 输出。
- 冲突标签、同 canonical 重复、development overlap 和 confirmation overlap 已在 manifest 阶段处理；训练器遇到不在 manifest 的行必须失败关闭。

## 共同输入与输出

- 输入：canonical SMILES，标签 `Y∈{0,1}`。
- 指纹模型使用 RDKit Morgan/ECFP：radius=2、nBits=1024、useChirality=True，按 canonical SMILES 逐行生成；不做从 confirmation 学习的标准化。
- 图模型使用同一 canonical SMILES；原子和键特征由 RDKit 确定性生成，不能把标签或 provider 输出作为节点/边特征。
- 输出：概率 `p(BBB+)`、固定阈值 `p≥0.5` 的二值预测、`status=ok`。异常分子记录 `status=error`，不能静默填 0。
- 每个 provider 额外保存训练/验证 manifest hash、输入特征版本、源码 commit、依赖锁、随机种子、最佳 epoch、checkpoint SHA-256 和逐行验证输出。

## 四个候选 provider

| provider | family | 固定训练规则 | 选择规则 |
|---|---|---|---|
| `ecfp_logreg` | fingerprint linear | Logistic Regression，`C=1`，`solver=liblinear`，`class_weight=balanced`，`max_iter=2000` | 训练完成后直接保存；validation 只作报告 |
| `ecfp_mlp` | fingerprint neural | 1024→128→64→1，ReLU，Adam，lr=`1e-3`，weight decay=`1e-4`，seed=`20260930`，最多 200 epoch | 只按 validation BCE 选最佳 epoch，平局取最早 |
| `ecfp_random_forest` | fingerprint tree | 256 棵树，`class_weight=balanced_subsample`，`random_state=20260930`，`n_jobs=1` | 固定树数，不按 confirmation 调参；validation 只作报告 |
| `graph_message_passing` | molecular graph | 两层 self-loop normalized GCN，hidden=64，mean pooling，Adam，lr=`1e-3`，weight decay=`1e-4`，seed=`20260930`，最多 100 epoch | 只按 validation BCE 选最佳 epoch，平局取最早 |

MLP 与 GCN 的训练日志要逐 epoch 写出 train/validation loss；LogReg 与 Random Forest 写出拟合配置、验证预测和输入/输出 hash。所有训练单线程，避免并行非确定性。

## 资格闸门

只有同时满足以下条件，provider 才能进入“可绑定 confirmation 菜单”候选：

1. 训练器拒绝越界行，且独立 verifier 能从 manifest 重建成员；
2. 训练和验证 scaffold 完全分离；
3. checkpoint 能由声明 commit 和 manifest hash 重新定位；
4. 验证输出逐行可重算，状态和失败不被改写；
5. 不生成或读取 confirmation 预测，直到 selector 的方法、baseline、预算和 confirmation manifest 全部冻结。

验证集上的准确率不是 provider lineage 的唯一门槛，也不是 selector 优势证据。若某 family 训练失败，保留失败回执，不用旧缓存 provider 静默替代。
