# BBB 33-provider 训练归属、模型谱系与 freshness 审计 v1

日期：2026-10-04  
状态：**只读审计完成；没有 provider 重跑、训练或 GPU 任务。**

## 结论

这次审计把固定 BBB 菜单的 33 个具体 provider 逐一落到机器可读矩阵。等级只描述训练归属证据的强弱，不代表预测性能，也不代表独立测试资格。结果为：**G3 1 个、G2 30 个、G1 2 个；整体没有一个 provider 可以据此进入 model-fresh confirmation。**

“missing”按证据维度记录在 `missing_evidence`；本菜单没有完全没有运行身份的 provider，因此没有把 G1 误写成 MISSING。现有 730 行 provider 回放身份仍可核对，但训练成员、checkpoint 绑定与独立性不能由数据集名字推断。

## 分级口径

| 等级 | 允许的陈述 | 不能陈述 |
|---|---|---|
| G3 | 有可读的 provider-specific train/test 表，并有一手 split 协议支持；可写观察到的 split/exposure | 不能把未绑定的发布 checkpoint 写成严格 model-fresh |
| G2 | 训练来源、版本或来源交集已核对；可作 lineage-aware operational routing | 不能声称开发分子对该 checkpoint 独立，不能把同源 provider 当独立来源 |
| G1 | 运行资产或结果快照可定位；可作已部署菜单压力测试 | 不能声称训练来源、成员或 model-fresh 泛化 |

## 总体统计

| 项目 | 数值 |
|---|---:|
| BBB provider | 33 |
| 每 provider 回放行数 | 730 |
| G3 / G2 / G1 | 1 / 30 / 2 |
| 独立 confirmation 资格 | 0 |
| 证据路径在审计时缺失 | 0 |

## 逐 provider 矩阵

| provider | family | 等级 | source / membership 摘要 | panel exposure | freshness 结论 | 主要缺口 |
|---|---|---|---|---|---|---|
| `admet_ai_1_4_0` | `admet_ai` | **G2** | TDC/PyTDC BBB_Martins; five seed checkpoints are hashed, exact molecule members are not shipped. | TDC/BBBP source covers the 722 development rows under max-fragment matching; this is source overlap, not checkpoint membership. | not model-fresh: complete checkpoint train/validation/test member manifests are missing | `checkpoint_to_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `admet_ai_2_0_1` | `admet_ai` | **G2** | PyTDC 1.1.15; v2 was retrained from scratch, so v1 member claims cannot be reused. | TDC/BBBP source covers the 722 development rows under max-fragment matching; exact v2 checkpoint members are unavailable. | not model-fresh: v2 checkpoint-specific member manifest and split binding are missing | `checkpoint_to_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `admetlab_3_0` | `admetlab` | **G1** | Only a fixed BBB output snapshot and endpoint-pairing record were found; no checkpoint, training source, or member list was located. | unknown: no training-member evidence is available for a join. | not model-fresh: training source, checkpoint identity and molecule members are missing | `training_source, checkpoint_identity, checkpoint_to_member_manifest, panel_exposure_join, scaffold_disjoint_confirmation` |
| `b3clf_dtree_borderline_smote` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_dtree_classic_adasyn` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_dtree_classic_randundersampling` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_dtree_classic_smote` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_dtree_common` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_dtree_kmeans_smote` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_knn_borderline_smote` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_knn_classic_adasyn` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_knn_classic_randundersampling` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_knn_classic_smote` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_knn_common` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_knn_kmeans_smote` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_logreg_borderline_smote` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_logreg_classic_adasyn` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_logreg_classic_randundersampling` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_logreg_classic_smote` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_logreg_common` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_logreg_kmeans_smote` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_xgb_borderline_smote` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_xgb_classic_adasyn` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_xgb_classic_randundersampling` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_xgb_classic_smote` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_xgb_common` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `b3clf_xgb_kmeans_smote` | `b3clf` | **G2** | B3DB source (7,407 rows in the published B3clf asset); this action is one of 24 algorithm/sampling variants sharing the same source family. | B3DB source intersects 609 of the 722 development rows under strict canonical matching; the variant's exact fitted members are not mapped. | not model-fresh: no variant-specific fitted-member manifest or split binding | `fitted_input_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `deeppurpose_bbb_molnet_cnn` | `deeppurpose` | **G2** | MoleculeNet/DeepPurpose BBBP; no split/member manifest bound to this CNN checkpoint. | BBBP source covers the 722 development rows under max-fragment matching; model-specific members are unknown. | not model-fresh: model-specific split and checkpoint member list missing | `checkpoint_to_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `deeppurpose_bbb_molnet_morgan` | `deeppurpose` | **G2** | MoleculeNet/DeepPurpose BBBP; no split/member manifest bound to this Morgan checkpoint. | BBBP source covers the 722 development rows under max-fragment matching; model-specific members are unknown. | not model-fresh: model-specific split and checkpoint member list missing | `checkpoint_to_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `deeppurpose_bbb_molnet_mpnn` | `deeppurpose` | **G2** | MoleculeNet/DeepPurpose BBBP; no split/member manifest bound to this MPNN checkpoint. | BBBP source covers the 722 development rows under max-fragment matching; model-specific members are unknown. | not model-fresh: model-specific split and checkpoint member list missing | `checkpoint_to_member_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |
| `fpadmet` | `fpadmet` | **G1** | BBB RDS exposes a 7,364 x 882 training matrix, but row names are anonymous and no public molecule-member mapping was found. | unknown: the anonymous training matrix cannot be joined to the development molecules. | not model-fresh: training source version and molecule members are not machine-mappable | `training_source_version, checkpoint_to_member_manifest, panel_exposure_join, scaffold_disjoint_confirmation` |
| `graphb3` | `graphb3` | **G3** | B3DB train_mol_set.xlsx (6,029) and test_mol_df.xlsx (1,771) are readable, non-overlapping, and agree with the primary paper's split protocol. | 448 development rows occur in the local train table and 161 in the local test table; this is an exposure audit, not checkpoint binding. | observational only: released checkpoint is not mapped to a seed/split/member manifest | `checkpoint_to_seed_split_binding, checkpoint_specific_member_manifest, test_use_for_model_selection_exclusion` |
| `lgbm_bbb` | `lgbm_bbb` | **G2** | B3DB source, Mordred filtering and 59-feature LightGBM checkpoint are documented; the training intermediate CSV is absent. | B3DB source intersects 609 of the 722 rows under strict canonical matching; exact filtered checkpoint members are unknown. | not model-fresh: reconstruction yields 7,296 rows versus documented 7,298 and cannot bind the published checkpoint | `filtered_input_manifest, checkpoint_specific_split_binding, scaffold_disjoint_confirmation` |

## 证据路径

每一行的完整证据路径、路径存在性、回放状态和 `independent_confirmation_eligible=false` 保存在 [`matrix.json`](matrix.json)。以下是按 family 归并的核心材料：

- 共同输入与 provider cell 回放：`experiments/logs/task49_data_audit_v0_20260926/v1/provider_cells.jsonl`。730 行来自固定 730 分子池；B3clf 每个动作 724 `ok` + 6 `error`，其他 provider 为 730 `ok`。
- 既有 33-provider 归属总审计：`research/provider_training_membership_audit_20260929.md`。
- GraphB3：`graphb3_lineage_audit_v1/report.md` 与 `paper_evidence.json`；6,029/1,771 train/test 表无 canonical 交集，但 checkpoint→seed/split/member 绑定缺失，所以等级为 G3-paper-backed-observational。
- LGBM-BBB：`lgbm_lineage_audit_v1.md`；重建 7,296 行而发布材料写 7,298 行，不能把近似 split 当 checkpoint 成员。
- ADMET-AI / DeepPurpose / B3clf 的 runtime receipt 位于 `data/bbb_census_v3/runtime_verification/`；它们记录版本、检查点和推理身份，但多数没有具体训练成员表。
- FP-ADMET / ADMETlab：只读读取本地 RDS/脚本或输出快照；匿名训练矩阵或缺少训练资产，不能做 panel exposure join。

## 对 benchmark 和 baseline 的直接影响

1. 当前 722 个有参考标签的 BBB 分子仍只能作为 development/operational routing 数据，不能写成对 33 个 checkpoint 全部独立的最终测试集。
2. 33 个动作不是 33 个独立训练来源：24 个 B3clf 共享 B3DB family；3 个 DeepPurpose 共享 BBBP；ADMET-AI 两个版本也不能仅凭版本号当成独立来源。
3. GraphB3 的本地 test 表是最接近可核对的 provider-specific exposure 证据，但在 checkpoint 绑定缺失前不升级为 confirmation。
4. 后续若要打开 frozen confirmation，必须逐 provider 补齐 checkpoint/member manifest 或建立明确的 provider-fresh 重训菜单，并独立冻结输入、split 和输出；本矩阵不替代这些步骤。

## 本审计不支持的推断

- 不把 source dataset 与当前 checkpoint 的成员等同。
- 不把 output `status=ok` 等同于训练隔离。
- 不把 G3 写成严格 model-fresh；不把 G1/G2 写成“模型一定污染了某行”。
- 不把 family 内的模型变体作为相互独立的样本或证据。

## 可复核命令

```bash
python3 task/task49-pharma-benchmark-baseline-freeze/provider_lineage_audit_v1/build_provider_lineage_audit_v1.py
```

生成器只读取固定 receipt 和已有证据，输出 `matrix.json`；没有调用预测 API，不改动 `data/` 输入。
