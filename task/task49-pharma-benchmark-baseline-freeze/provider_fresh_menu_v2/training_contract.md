# Provider-fresh BBB 菜单：统一训练合同 v2

日期：2026-09-30  
状态：训练/验证已完成；confirmation 预测仍禁止。

## 数据边界

- 唯一训练成员：`experiments/logs/task49_provider_fresh_menu_20260930/manifest_v2/provider_train.csv`。
- 唯一 checkpoint 选择成员：`manifest_v2/provider_validation.csv`。
- `excluded_rows.csv` 只用于审计，不进入训练。
- 不读取 selector development 的 provider 输出，不读取 confirmation 标签或 provider 输出。
- manifest verifier 必须证明 1,624 条源行恰好分到 train、validation 或 excluded，各源行只出现一次。

## 共同输入与输出

- 输入是 canonical SMILES 与二值 BBB 标签。
- ECFP 使用 RDKit Morgan radius=2、nBits=1024、useChirality=True。
- 图模型使用 RDKit 生成原子特征与 self-loop normalized adjacency；不使用 provider 输出或 confirmation 信息。
- 输出为概率、`p≥0.5` 二值预测和状态字段。异常必须记录为 error，不能静默改成 0。
- 训练收据必须保存 manifest/hash、源码 commit、随机种子、最佳 epoch、checkpoint hash、逐行 validation prediction 和独立 verifier 结果。

## 候选菜单

| provider | family | 训练规则 | checkpoint 选择 |
|---|---|---|---|
| `ecfp_logreg` | fingerprint linear | Logistic Regression，C=1，liblinear，balanced，max_iter=2000 | 固定拟合，validation 只报告 |
| `ecfp_mlp` | fingerprint neural | 1024→128→64→1，ReLU，Adam，lr=1e-3，weight decay=1e-4，200 epoch | validation BCE，平局取最早 |
| `ecfp_random_forest` | fingerprint tree | 256 棵树，balanced_subsample，seed=20260930，单线程 | 固定拟合，validation 只报告 |
| `graph_message_passing` | molecular graph | 两层 normalized GCN，hidden=64，mean pooling，Adam，lr=1e-3，weight decay=1e-4，100 epoch | validation BCE，平局取最早 |

validation sanity metrics 只用于发现训练故障，不用于事后删模型、换 split 或打开 confirmation。
