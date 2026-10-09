# hERG 端点语义补充核验

日期：2026-09-30

这份补充只回答一个问题：当前 hERG 候选能不能把不同工具的 0/1 输出当成同一份真实标签。结论先写在前面：**还不能**。现有材料足以确认若干来源关系，但还不足以把 hERG 放进正式主 benchmark 或 confirmation。

## 已核实的来源关系

1. TDC 的 `hERG` 数据文件把来源指向 Wang et al. 2016（DOI [10.1021/acs.molpharmaceut.6b00471](https://pubs.acs.org/doi/10.1021/acs.molpharmaceut.6b00471)）。该论文摘要说明任务是区分 hERG active/inactive，并报告外部测试集结果；摘要没有给出可用于跨数据集对齐的单一 IC50 cutoff。当前材料因此只能证明“来源论文身份”，不能证明 TDC 标签与其他工具标签同 assay、同 cutoff。

2. Siramshetty et al. 2020（DOI [10.1021/acs.jcim.0c00884](https://pubs.acs.org/doi/10.1021/acs.jcim.0c00884)）的摘要明确说训练集约 9,000 个分子，整合 ChEMBL 与自有高通量 thallium-flux assay；约 840 个自有分子用于前瞻性验证。作者归档仓库的固定 README 与 CSV 已保存到本地，实际解析得到 8,154 行 training 和 839 行 validation。这个 8,993 行材料可以作为作者项目的可追溯复现资产，但不能直接声称它等于 FP-ADMET 表中写的 9,204 个训练分子。

3. FP-ADMET SI 的端点段落把两个任务放在同一节但明确分开：hERG liability 使用 ChEMBL 加 thallium-flux 数据；另一个 hERG blocker 数据集来自 Cai et al. 2019，整合 patch-clamp、radioligand binding、hERG K+ binding 和文献数据。SI 对后一个 blocker 数据集明确写了 `IC50 ≤ 10 µM` 视为 blocker。这个 cutoff 不能反向套到 TDC Wang 数据，也不能证明 FP-ADMET liability 模型和 blocker 模型共享相同标签。

4. FP-ADMET 本仓当前 endpoint map 的编号 33 读取 `predict_hergliability.R` 和 `model_hergblocker_pubchem.rds`，所以缓存列的模型身份是 liability 这一支；它不是 endpoint map 中的 cardiotoxicity 模型。ADMETlab 的公开端点定义另给出 `IC50 ≤ 10 µM` 或 10 µM 下抑制率 `≥ 50%` 的判定条件，但这仍不足以证明它与 TDC Wang 文件或 FP-ADMET liability 训练源逐分子同源。

## 对 benchmark 的决定性影响

- 当前 43 个严格 identity overlap 分子只能作为**端点对齐候选集**，不能作为正式跨工具真值集。
- hERG 不能承担 confirmation；不能把不同来源的 0/1 结果多数投票后称为独立真值。
- 若继续保留 hERG，正式资格至少需要两条新证据：
  1. 逐分子把工具输入、训练曝光、原始 assay 类型和二元化规则连到同一来源谱系；
  2. 对冲突标签规定先验规则，且规则在看到工具结果前冻结。
- 在这两条证据完成前，主实验仍以已完成 authority audit 的 BBB development routing 为主；hERG 只作为候选资格审查，不扩大论文 claim。

## 一手材料与本地校验

- Wang 2016 PubMed：[https://pubmed.ncbi.nlm.nih.gov/27379394/](https://pubmed.ncbi.nlm.nih.gov/27379394/)
- Wang 2016 ACS：[https://pubs.acs.org/doi/10.1021/acs.molpharmaceut.6b00471](https://pubs.acs.org/doi/10.1021/acs.molpharmaceut.6b00471)
- Siramshetty 2020 PubMed：[https://pubmed.ncbi.nlm.nih.gov/33259212/](https://pubmed.ncbi.nlm.nih.gov/33259212/)
- Siramshetty 2020 作者归档仓库：[https://github.com/ncats/herg-ml/tree/183a1ac54a43253375fc35e0f5cc7fce02eceb55](https://github.com/ncats/herg-ml/tree/183a1ac54a43253375fc35e0f5cc7fce02eceb55)

本轮保存材料的 SHA-256：

- `sources/web_20260930/pubmed_hERG_sources.txt`: `f8ff87c424330e465f1b5fe83845fac7a305ee8e8323e751d4db5e5c90f7824b`
- `sources/fpadmet_si.txt`: `4e81d11728db2e10c1013cb9a291e0ddc6e8756fa12e609fd51b87ed127127c9`
- `sources/ncats_readme.md`: `c10bb855521d68f147ca8b2d4e76385be01a7183344590c5fb28ec3f336b7d5a`
- `sources/ncats_training_set.csv`: `867b88ca2bb6526c34e45d21fea6045e3bb7f85f55375e1acb9dab167e761ab0`
- `sources/ncats_validation_set.csv`: `637587843b0fdde01e8754fa2242da0df642a3b92f7ccb5f2d192c97e1ed4ed3`

## 未完成事项

ACS SI 的完整原始 assay 表和 TDC Wang 文件的逐分子 cutoff/assay 映射仍未获得；FP-ADMET 的 9,204 训练源与 NCATS 8,993 行归档之间的差异也没有闭合。两项未闭合前，不把 hERG 从“候选”升级为 benchmark 主端点。
