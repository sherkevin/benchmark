# hERG 端点来源核对：当前能确认到哪里

2026-09-26。这里只追来源与模型身份；不发布新的 hERG 准确率，也没有把同名端点默认合并。

## 已确认

| 对象 | 一手证据 | 对 benchmark 的影响 |
|---|---|---|
| TDC `hERG`，648 条原始记录 | TDC 官方归档明确引 Wang et al., 2016，DOI `10.1021/acs.molpharmaceut.6b00471` | 不能套用 `hERG_Central` 或别的 hERG 文件的阈值；当前严格连接的 43 个分子仍只是候选 |
| FP-ADMET | 2021 原论文 Table 2 分开列 cardiotoxicity（7,889）和 liability（9,204），分别引 Cai 2019 与 Siramshetty 2020 | 两套模型不能因名字相近被合成同一个工具版本 |
| 本仓 FPADMET hERG 缓存的映射 | 归档 endpoint map 的编号 33 是 `predict_hergliability.R`；固定源码读取 `MODELS/model_hergblocker_pubchem.rds` | 当前列对应 liability，不是 cardiotoxicity(8) |
| ADMETlab hERG | 官方端点定义为 IC50 ≤10 µM，或 10 µM 下抑制 ≥50% | 来源中“hERG”标签需要 assay/cutoff 核对，不能仅用 0/1 数值对齐 |
| liability 引用的数据来源 | Siramshetty et al., 2020，DOI `10.1021/acs.jcim.0c00884`；作者 NCATS 仓库 README 自证与论文的联系 | 原始数据已拉到本地，可继续做训练曝光核查与池外分子来源审计 |

NCATS 固定 commit 为 `183a1ac54a43253375fc35e0f5cc7fce02eceb55`。仓库已归档；本轮只取静态数据与说明，不安装其旧预测程序。`training_set.csv` 有 8,154 行，`validation_set.csv` 有 839 行，字段为 smiles/activity/source；这些数量来自实际解析，不是声称与 FP-ADMET 的 9,204 个训练源分子逐行等价。

## 2026-09-30 补充核验

补充核验见 [semantics_update_20260930.md](semantics_update_20260930.md)。一手摘要确认 Wang 2016 的来源身份，但没有给出可跨数据集复用的单一 cutoff；Siramshetty 2020 确认约 9,000 个 ChEMBL+thallium-flux 训练分子和约 840 个前瞻性验证分子；作者归档实际保存的 train+validation 为 8,993 行，不能直接当作 FP-ADMET 的 9,204 个训练成员。FP-ADMET SI 只明确给出 Cai blocker 数据的 `IC50 ≤ 10 µM` 规则，并把 liability 与 blocker 数据分开。

因此 hERG 仍是资格审查候选，不进入正式主 benchmark 或 confirmation；43 个 identity overlap 只用于后续逐分子 assay/cutoff/训练谱系核验。

## 还缺哪条证据

目前仍未取得 Wang 2016 与 Siramshetty 2020 的可核验完整 assay/cutoff 说明及 FP-ADMET 训练集具体映射。TDC 当前连接、ADMET-AI 输出和 FPADMET 输出能否共用同一份二元真值尚未完成资格证明。不能据当前结果冻结 hERG 正式评价。

FP-ADMET 原文给出 9,204，NCATS 已下载 train+validation 是 8,993；这个差异需要追溯，不能把下载的 NCATS 列表直接当 FP-ADMET 精确训练成员。原始二元标签 CSV 不含 IC50，不可能从 0/1 反推出阈值。

## 可复用资产已经落盘

`sources/` 保存 FP-ADMET 原文与 SI、Table 1–3、TDC/ADMETlab 本地官方归档副本、实际 liability 预测脚本、NCATS commit/tree/README 和两个原始 CSV。每个获取结果及失败都记在 `source_manifest.jsonl`，包括地址、日期、SHA-256 和重定向。

TDC TLS、ACS/figshare/chemrxiv 403 及 EuropePMC 500 均保留；没有把请求失败记成已读全文。接下来先基于原始作者材料继续找 cutoff 和训练映射，并准备 identity-level overlap 核验。两份 NCATS CSV 当前是来源审计材料，未打开作性能测试或 confirmation，也未授予再分发资格。

本轮代理曾开始来源核查，留下上述前半材料；随后代理句柄不再存在，主执行者续取 NCATS 固定版本与完成本报告。它不构成已完成的独立审稿意见。

## 回看主线

任务仍是实际工具的选择正确性。这里消除的是“同名 hERG 是否同一标签”的疑问，而不是扩大端点数量。可比性证据不足时保留候选身份；不为三端点的形式要求制造不成立的对比。
