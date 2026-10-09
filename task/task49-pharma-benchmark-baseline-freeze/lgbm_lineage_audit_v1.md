# LGBM-BBB 训练行谱系重建审计 v1

日期：2026-09-30

## 结论

LGBM-BBB 的公开包包含 B3DB 原始表、特征处理 notebook 和发布的模型文件，因此比只有结果快照的 provider 更容易追溯。但按当前 runtime 重放 notebook 的“有效 SMILES + Mordred MW 150–800 + `train_test_split(test_size=0.2, random_state=42, stratify=y)`”只能得到 7,296 行，而 README/notebook 叙述的是 7,298 行。包内也没有 `dcorr_filtered_BBB_0.25.csv` 这个实际喂给训练 notebook 的中间文件。

因此这次没有得到可以绑定到发布 checkpoint 的精确 train/test molecule manifest；LGBM-BBB 继续保持 **G2**。不把近似重建的 split 当成独立 confirmation 证据。

## 审计范围

只读读取：

- `Predict_BBB/B3DB_classification.tsv`
- `Predict_BBB/02_feature_selection.ipynb.ipynb`
- `Predict_BBB/04_model_training_and_evaluation..ipynb`
- `lgbm_model.pkl`

第一次运行 `run_v1` 因 Mordred 默认多进程和临时脚本缺少 `if __name__ == '__main__'` 保护，在进程启动阶段失败；失败原始日志保留，未把它当成结果。第二次 `run_v2` 改为单进程，完整算完 Mordred 1,613 个描述符，但只作为重建审计，未 fit LightGBM、未运行 provider。

机器证据：`experiments/logs/task49_lgbm_lineage_audit_20260930/run_v1/`、`run_v2/`。

## v2 结果

| 步骤 | 行数 |
|---|---:|
| B3DB 原始行 | 7,807 |
| RDKit 有效行 | 7,805 |
| Mordred MW 150–800 | 7,296 |
| 重建 train（80%，stratified, seed 42） | 5,836 |
| 重建 test（20%，stratified, seed 42） | 1,460 |

README 和 notebook 记载的目标是 7,298 行；因此当前 runtime 重建与原始训练输入存在 2 行差异。最可能的来源是 Mordred/RDKit 版本或未发布的中间清洗文件，但没有证据可以在这次审计里选择其中一个解释。

## 对 benchmark 的影响

- 可以确认 LGBM-BBB 使用 B3DB 来源、150–800 MW 过滤和随机分割式训练流程；不能确认发布 checkpoint 看过哪些具体 molecule。
- 当前 722 development panel 与 LGBM 的暴露仍只能按 G2 处理；不把重建 split 的补集当成 model-fresh confirmation。
- 若要升级资格，必须获得 `dcorr_filtered_BBB_0.25.csv` 或作者保存的行 ID manifest，并将其 hash 与 `lgbm_model.pkl`、最终 59 特征列表和训练 commit 绑定。
- 这次失败和版本差异提高了审计透明度，但没有改变 Gate A 分数，也没有打开 confirmation。

## 下一步

优先寻找作者发布的中间 CSV、Google Drive 数据快照或训练 commit；如果拿不到，LGBM-BBB 保持 G2，并把它作为 lineage-aware operational baseline，不能作为独立 provider confirmation。
