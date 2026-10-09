# Provider training-membership audit（2026-09-29）

## 结论先说

这是一次**只读本地证据审计**。没有联网、没有调用预测 API、没有提交训练任务，也没有修改决议文件。审计对象是当前 33-provider BBB panel，使用完整池 730 个分子和开发 panel 722 个分子。

当前结论：**33 个 provider 没有一个可以直接宣称“对 722 个开发分子完全独立”**。原因不是工具身份不清楚；33 个工具的运行资产大多已经有固定版本和哈希。缺口在训练成员：多数工具只知道训练数据集或其来源，拿不到“这个检查点究竟见过哪些分子”的完整名单。

资格等级是本审计自定义的 membership grade，和 `runtime_verification/*.json` 中的运行级 `tier=L3` 不同：

- **G3-paper-backed-observational：本地模型包带有可读的 train/test 分子表，且一手论文的 split 数量/协议与表格一致。** 可以量化当前池与训练/测试表的交集，但若没有 checkpoint→seed/split/member 绑定，不能自动把 test 表升级为最终 benchmark。
- **G2：训练来源和池交集已核实，检查点的精确成员缺失。** 这类工具不能用于声称当前 722 是独立测试集。
- **G1：只有运行资产或结果快照，尚未定位训练来源和分子成员。**

按这个标准，`graphb3` 为 G3-paper-backed-observational；ADMET-AI 两个版本、DeepPurpose 三个模型、LGBM-BBB 和 24 个 B3clf 变体为 G2；FP-ADMET、ADMETlab 3.0 为 G1。**整个 33-provider panel 的“独立测试资格”仍为 0。**

## 1. 数据和匹配规则

- 完整池：`data/tool_results/molecule_pool.csv`，730 行。
- 开发 panel：`task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json`，722 行；排除的 8 个 ID 保持不变。
- 参考真值端点：`BBB_Martins`。本审计不改真值，也不把外部数据标签替换进 panel。
- 主匹配键：保留立体化学的 RDKit canonical SMILES；同时报告去盐/最大分子片段的敏感性匹配。敏感性匹配只说明结构写法不同造成的交集变化，不把它当作额外实验事实。

### 文件哈希（便于复核）

| 文件 | SHA-256 |
|---|---|
| `data/tool_results/molecule_pool.csv` | `cbab39d616162586892855c4b150519b07fdf08c20b37ee9d51dc9d4fc3dcf06` |
| `data/recovered_tmp_evidence_20260916/frozen_assets/BBBP.csv` | `d07a38487aeac5cee5508413e468043ef3097451d2a112701c2d60be9ec6b662` |
| `task/task38-p01-provider-pool/evidence/B3DB_classification.tsv` | `47a160aea1551423ead2aab70b301fb1646aea03cf151fd614eb37c78fa55b82` |
| `research/benchmark_authority_audit_20260928/sources/tdc_bbb_martins.tsv` | `eb32676c7bf6e2bcd98b7625186bbb2ba4e4644c1d1468dfa18b6be665bd5e98` |

## 2. 730/722 与训练来源的交集

| 来源或本地分割表 | 730：严格 canonical | 730：最大分子片段 | 722：严格 canonical | 722：最大分子片段 | 能说明什么 |
|---|---:|---:|---:|---:|---|
| DeepPurpose BBBP/MoleculeNet（`BBBP.csv`） | 703 | 730 | 695 | 722 | 三个 DeepPurpose 动作共享同一个 BBBP 来源；不能由此恢复各检查点成员 |
| TDC `BBB_Martins`（`tdc_bbb_martins.tsv`） | 703 | 730 | 695 | 722 | 是当前真值来源；不等于每个 provider 的训练成员表 |
| B3DB 主表（7,807 行） | 613 | 613 | 609 | 609 | B3clf、LGBM-BBB、GraphB3 的来源线索；不能替代检查点成员 |
| GraphB3 本地 `train_mol_set.xlsx`（6,029 行，2 行结构无效） | 451 | 451 | 448 | 448 | 当前 panel 中 448 行出现在本地训练表 |
| GraphB3 本地 `test_mol_df.xlsx`（1,771 行） | 162 | 162 | 161 | 161 | 当前 panel 中 161 行出现在本地测试表 |

B3DB 的 730/722 交集均为 613/609；722 行中有 27 个 B3DB 主表标签与当前 `BBB_Martins` 参考标签不一致。这个数字是**来源冲突**，不是本审计对真值的裁决，详见 `task/task38-p01-provider-pool/cross_source_label_disagreements.json`（若存在）和原有 B3DB 审计报告。

GraphB3 的 train/test 表共有 6,027/6,029 和 1,771/1,771 行能与 B3DB canonical 匹配，匹配行标签全部一致；两张表 canonical 交集为 0。这给出了本地分割证据，但 README 只说 `data` 目录包含“用于训练”的 BBB 数据，没有写出完整的检查点训练命令和成员清单，论文进一步支持 split 数量/协议，但由于 checkpoint seed/member 绑定仍缺，保留 G3-paper-backed-observational 的限定语气。

## 3. 33-provider qualification matrix

`evidence` 列给出可复核的本地路径。G2/G1 的等级只说明审计到的证据强度，不表示模型性能，也不表示工具一定污染了某一行。

| provider | family | membership grade | training/source finding | evidence |
|---|---|---|---|---|
| `admet_ai_1_4_0` | ADMET-AI | G2 | TDC/PyTDC BBB_Martins；5个随机80/10/10集成检查点，完整成员缺失 | data/bbb_census_v3/runtime_verification/admet_ai_1_4_0.json；task/task38-p01-provider-pool/report.md |
| `admet_ai_2_0_1` | ADMET-AI | G2 | PyTDC 1.1.15；v2重新训练，旧版成员名单不可沿用 | data/bbb_census_v3/runtime_verification/admet_ai_2_0_1.json；task/task38-p01-provider-pool/report.md |
| `deeppurpose_bbb_molnet_cnn` | DeepPurpose | G2 | BBBP/MoleculeNet BBBP；没有对应模型的划分成员表 | data/bbb_census_v3/runtime_verification/deeppurpose_bbb_molnet_cnn.json；task/task38-p01-provider-pool/evidence/dataverse_cnn_search.json；data/recovered_tmp_evidence_20260916/frozen_assets/BBBP.csv |
| `deeppurpose_bbb_molnet_morgan` | DeepPurpose | G2 | BBBP/MoleculeNet BBBP；没有对应模型的划分成员表 | data/bbb_census_v3/runtime_verification/deeppurpose_bbb_molnet_morgan.json；task/task38-p01-provider-pool/evidence/dataverse_morgan_search.json；data/recovered_tmp_evidence_20260916/frozen_assets/BBBP.csv |
| `deeppurpose_bbb_molnet_mpnn` | DeepPurpose | G2 | BBBP/MoleculeNet BBBP；没有对应模型的划分成员表 | data/bbb_census_v3/runtime_verification/deeppurpose_bbb_molnet_mpnn.json；task/task38-p01-provider-pool/evidence/dataverse_mpnn_search.json；data/recovered_tmp_evidence_20260916/frozen_assets/BBBP.csv |
| `graphb3` | GraphB3 | G3-paper-backed-observational | 论文报告的 B3DB split 数量/协议与本地 train_mol_set.xlsx/test_mol_df.xlsx 一致；两表与B3DB匹配且标签一致，但 checkpoint seed/member 绑定缺失 | data/bbb_census_v3/runtime_verification/graphb3.json；/Users/jingwu/.local/share/benchmark-runtimes/graphb3/source/README.md；/Users/jingwu/.local/share/benchmark-runtimes/graphb3/source/data/{train_mol_set,test_mol_df}.xlsx |
| `lgbm_bbb` | LGBM-BBB | G2 | README声明B3DB 7,807行，过滤后7,298行；训练划分成员未随检查点提供 | data/bbb_census_v3/runtime_verification/lgbm_bbb.json；/Users/jingwu/.local/share/benchmark-runtimes/lgbm_bbb/source/README.md；/Users/jingwu/.local/share/benchmark-runtimes/lgbm_bbb/source/Predict_BBB/04_model_training_and_evaluation..ipynb |
| `b3clf_dtree_common` | B3clf | G2 | B3DB 7,407行；dtree+common；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_dtree_common.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_dtree_classic_randundersampling` | B3clf | G2 | B3DB 7,407行；dtree+classic_randundersampling；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_dtree_classic_randundersampling.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_dtree_classic_smote` | B3clf | G2 | B3DB 7,407行；dtree+classic_smote；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_dtree_classic_smote.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_dtree_borderline_smote` | B3clf | G2 | B3DB 7,407行；dtree+borderline_smote；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_dtree_borderline_smote.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_dtree_kmeans_smote` | B3clf | G2 | B3DB 7,407行；dtree+kmeans_smote；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_dtree_kmeans_smote.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_dtree_classic_adasyn` | B3clf | G2 | B3DB 7,407行；dtree+classic_adasyn；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_dtree_classic_adasyn.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_knn_common` | B3clf | G2 | B3DB 7,407行；knn+common；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_knn_common.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_knn_classic_randundersampling` | B3clf | G2 | B3DB 7,407行；knn+classic_randundersampling；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_knn_classic_randundersampling.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_knn_classic_smote` | B3clf | G2 | B3DB 7,407行；knn+classic_smote；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_knn_classic_smote.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_knn_borderline_smote` | B3clf | G2 | B3DB 7,407行；knn+borderline_smote；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_knn_borderline_smote.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_knn_kmeans_smote` | B3clf | G2 | B3DB 7,407行；knn+kmeans_smote；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_knn_kmeans_smote.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_knn_classic_adasyn` | B3clf | G2 | B3DB 7,407行；knn+classic_adasyn；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_knn_classic_adasyn.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_logreg_common` | B3clf | G2 | B3DB 7,407行；logreg+common；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_logreg_common.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_logreg_classic_randundersampling` | B3clf | G2 | B3DB 7,407行；logreg+classic_randundersampling；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_logreg_classic_randundersampling.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_logreg_classic_smote` | B3clf | G2 | B3DB 7,407行；logreg+classic_smote；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_logreg_classic_smote.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_logreg_borderline_smote` | B3clf | G2 | B3DB 7,407行；logreg+borderline_smote；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_logreg_borderline_smote.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_logreg_kmeans_smote` | B3clf | G2 | B3DB 7,407行；logreg+kmeans_smote；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_logreg_kmeans_smote.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_logreg_classic_adasyn` | B3clf | G2 | B3DB 7,407行；logreg+classic_adasyn；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_logreg_classic_adasyn.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_xgb_common` | B3clf | G2 | B3DB 7,407行；xgb+common；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_xgb_common.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_xgb_classic_randundersampling` | B3clf | G2 | B3DB 7,407行；xgb+classic_randundersampling；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_xgb_classic_randundersampling.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_xgb_classic_smote` | B3clf | G2 | B3DB 7,407行；xgb+classic_smote；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_xgb_classic_smote.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_xgb_borderline_smote` | B3clf | G2 | B3DB 7,407行；xgb+borderline_smote；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_xgb_borderline_smote.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_xgb_kmeans_smote` | B3clf | G2 | B3DB 7,407行；xgb+kmeans_smote；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_xgb_kmeans_smote.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `b3clf_xgb_classic_adasyn` | B3clf | G2 | B3DB 7,407行；xgb+classic_adasyn；24个动作共享同一数据来源，分子成员映射缺失 | data/bbb_census_v3/runtime_verification/b3clf_xgb_classic_adasyn.json；task/task38-p01-provider-pool/evidence/b3clf_readme.txt；task/task38-p01-provider-pool/evidence/b3clf_commit.json |
| `fpadmet` | FP-ADMET | G1 | BBB RDS可读出 trainingData=7,364×882，但行名匿名；未定位公开分子成员/数据版本 | /Users/jingwu/.local/share/benchmark-runtimes/_sources/FPADMET/PREDICTORS/predict_bbbp.R；/Users/jingwu/.local/share/benchmark-runtimes/_sources/FPADMET/MODELS/model_bbb_pubchem.rds；data/tool_results/fpadmet.csv |
| `admetlab_3_0` | ADMETlab 3.0 | G1 | 本轮只有结果快照，未找到对应 runtime receipt、检查点或训练成员表 | data/tool_results/admetlab_3_0.csv；data/tool_property_matrix/property_pairings.json |

## 2026-09-30 LGBM-BBB 行成员重建审计

按发布 notebook 和当前固定 runtime 做了只读重建，完整计算 Mordred 1,613 个描述符后得到 7,296 行 MW 150–800 数据；文档目标是 7,298 行。训练 notebook 实际读取的 `dcorr_filtered_BBB_0.25.csv` 不在发布包内，因此无法把 5,836/1,460 的重建 split 当成发布 checkpoint 的精确成员。机器收据：`experiments/logs/task49_lgbm_lineage_audit_20260930/run_v2/`；报告：`task/task49-pharma-benchmark-baseline-freeze/lgbm_lineage_audit_v1.md`。LGBM-BBB 继续为 G2。

## 2026-09-30 GraphB3 只读绑定审计

对 GraphB3 固定包做了只读 checkpoint–split 核对，机器收据为 `experiments/logs/task49_graphb3_lineage_audit_20260930/run_v1/results.json`，人读报告为 `task/task49-pharma-benchmark-baseline-freeze/graphb3_lineage_audit_v1/report.md`。train/test 表为 6,029/1,771 行，canonical 交集为 0；检查点 14 个 tensor 与当前 33→128 推理结构一致。但包内没有训练脚本、优化器、日志或 checkpoint→split 元数据。因此 GraphB3 由 G3-observational 更新为 `G3-paper-backed-observational`：论文报告的 6,029/1,771 split protocol 与本地表格一致，但该具体 checkpoint 的 seed/split/member binding 仍未证明，不能升级为 checkpoint-specific model-fresh 独立测试资格。

## 4. 各组证据解释

### ADMET-AI 1.4.0 / 2.0.1

1.4.0 检查点内保存的是多任务分类模型配置；已有审计读出 5 个随机种子、80/10/10 训练参数和 28,619 条多任务训练行，但没有恢复 5 个子模型完整的 train/validation/test 分子名单。2.0.1 README 明确是重新训练版本，数据版本是 PyTDC 1.1.15；不能沿用 1.4.0 的旧名单。当前 730/722 与 TDC `BBB_Martins` 的最大分子片段交集为 730/722，但这仍是来源交集，不是检查点成员证明。

### DeepPurpose CNN / Morgan / MPNN

三个模型都对应固定 DeepPurpose BBB_MolNet/MoleculeNet 资产，使用同一 BBBP 数据源，只是编码器不同。BBBP 文件在最大分子片段规则下覆盖当前 730/722 全部样本；各模型的公开包、配置和 Dataverse 文件清单没有提供与实际权重绑定的成员表。因此三个 provider 不能当作三个独立训练数据集，也不能把当前 panel 当作它们的独立测试集。

### B3clf 24 个变体

README 声明 7,407 个 B3DB 分子，四种算法乘六种重采样。24 个动作共享 B3DB 来源；重采样策略改变拟合过程，不创造新的原始分子来源。已核对检查点和固定 commit，但没有 7,407 行到分子 ID 的映射，也没有每个变体的训练/验证成员表，因此全部为 G2。

### LGBM-BBB

本地 README 写明 B3DB 7,807 行，经 MW/缺失值/方差等处理后为 7,298 行，特征选择到 59 个描述符；notebook 采用 `train_test_split(test_size=0.2, random_state=42, stratify=y)`。检查点没有随附处理后分子表，所以只能确认 B3DB 来源和潜在交集，不能恢复精确训练成员。

### GraphB3

本地包含 `train_mol_set.xlsx`（6,029 行）和 `test_mol_df.xlsx`（1,771 行），两表无 canonical 交集。当前 722 panel 有 448 行在 train、161 行在 test。两表与 B3DB 的匹配标签全部一致，说明本地分割表确实与 B3DB 来源相符。需要在最终论文前补一条证据：把 checkpoint 的训练脚本、数据读取路径和这两张表的哈希绑定起来；在此之前只能称为“观察到的本地 split”，不能直接写成严格外部测试资格。

### FP-ADMET / ADMETlab 3.0

FP-ADMET 的 BBB RDS 可以看到训练数据矩阵规模（7,364×882），但行名匿名，不能映射到分子。ADMETlab 3.0 当前只有结果快照，未在本地找到对应 runtime receipt、检查点或训练成员清单。因此两者不参与任何“独立测试”声称。

## 5. 对 benchmark 和 baseline 的直接影响

1. 722 开发 panel 适合作为**工具选择训练/开发数据**，不适合作为声称对所有 provider 独立的最终测试集。
2. 对最终效果，应保留一个冻结 confirmation set，且需要逐 provider 做成员资格核查；不能只按 BBBP、B3DB 或 TDC 数据集名字做补集推断。
3. 24 个 B3clf 变体应在统计上按一个 B3DB family 分组汇报，不能把 24 个变体描述成 24 个独立数据来源。
4. DeepPurpose 三个编码器是同一 BBBP 来源的不同模型；ADMET-AI 两版本也不能因为版本号不同就视为训练数据独立。
5. GraphB3 的 161 行本地 test 交集是最接近可复核的 provider-specific held-out 候选，但在 checkpoint-to-split 绑定完成前仍只能作为审计线索。

## 6. 明确的后续证据缺口

- ADMET-AI：取得两个版本各自完整的 train/validation/test 成员表，并逐模型绑定到 checkpoint 哈希。
- DeepPurpose：取得 CNN/Morgan/MPNN 三个 registry file 对应的 split/member 文件和数据版本。
- B3clf：取得 7,407 行拟合矩阵到原始 B3DB 分子 ID 的映射；同时记录每个重采样变体的实际训练成员。
- LGBM-BBB：取得过滤后 7,298 行和 80/20 split 的分子 ID 列表。
- GraphB3：论文已闭合 split 数量/协议与本地表格的一致性；仍需把当前 checkpoint、训练 seed/split 和两张本地 split 表做哈希绑定，并确认 test 表没有被用于模型选择。
- FP-ADMET、ADMETlab 3.0：补足训练来源、检查点版本和分子成员证据；在补齐前不列入独立 confirmation 结论。

**审计边界：**本文件没有运行 provider，没有生成新预测，没有修改 `data/` 中任何输入或真值。它只整理已有本地 evidence，供 benchmark/baseline 决策和后续外部资格审查使用。
