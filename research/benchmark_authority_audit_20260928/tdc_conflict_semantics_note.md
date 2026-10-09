# TDC BBB 冲突标签的语义审查

日期：2026-09-29
状态：审查记录；没有据此替换 TDC 标签，也没有把冲突组纳入训练。

## 已确认的本地事实

TDC ADMET Group archive 的 `BBB_Martins` 文件在 canonical SMILES 层存在 10 个标签冲突组：`train_val` 8 组，`test` 2 组。每个冲突组在公开文件里同时出现 `Y=0` 和 `Y=1`。冲突分子和原始行保存在：

`experiments/logs/task49_tdc_admet_split_audit_20260929/bbb_v4/conflicting_groups.json`

这不是 train/test overlap：官方 split 的 canonical 和非手性 Bemis--Murcko scaffold 交集仍为 0。与完整 TDC `bbb_martins.tsv` 对照后，10 组都能映射到两个不同的 `Drug_ID`（例如 `acetylsalicylate`/`aspirin`、`levodopa` 的大小写或别名），但该文件也没有提供每条记录的原始实验出处，因此别名映射仍不能决定应保留哪个标签。原始行和 ID 也写入冲突收据。

## 外部来源核对

1. TDC Dataverse file metadata 把 archive 标为 `admet_group.zip`，公开、非 restricted，创建日期为 2021-02-26，最后更新时间为 2025-08-27；metadata API：<https://dataverse.harvard.edu/api/files/4426004>。
2. TDC BBB 原始文件 metadata 把 `bbb_martins.tab` 标为 `bbb_martins.csv` 的公开 tabular 版本；metadata API：<https://dataverse.harvard.edu/api/files/4259566>。
3. B3DB 原始论文说明其来源数据包含重复 InChI，并通过结构标准化和清洗构建数据集，但没有给出 TDC Group split 这 10 个具体冲突组的实验重复解释：<https://www.nature.com/articles/s41597-021-01069-5>。
4. 现有公开资料没有证明这些 TDC 行是同一实验的重复测量，也没有证明 `0/1` 中哪一个是可优先采用的标签。因此不能从数据行本身推断“重复测量”或“标注错误”。

## 对 benchmark 的处理

在语义被原始实验来源补齐前，Gate 0 不允许静默合并冲突组。候选处理只有两类：

- 将冲突 canonical group 从训练和确认评分中排除，并报告排除数量；
- 找到原始实验/来源记录后，把同一分子的多次测量建模为重复观测，预先规定聚合、权重和不确定性。

按第一行、按多数票、按模型正确率或按当前方法结果挑标签，都没有来源依据。该审查结论只约束数据合同，不改变 TDC 的原始文件。

## 可复用结论

“官方 benchmark”只能说明来源和 split 可追溯，不能自动说明 canonical 去重后的标签语义无冲突。后续 provider 训练、selector 反馈和 confirmation 必须引用冲突处理 manifest；如果冲突组被排除，所有分子数量、标签比例和 provider 输出都要从排除后的 manifest 重新生成。
