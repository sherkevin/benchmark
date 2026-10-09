# Provider-fresh BBB 菜单：资格清单 v2

日期：2026-09-30  
状态：**manifest 与 CPU 训练已独立核验；confirmation 仍关闭。**

## 这次修了什么

v1 有一个真实的分区缺口：同标签重复 canonical 只保留了第一条，但额外的 14 条源行没有进入 `excluded_rows.csv`。因此 1,624 条源行只有 1,610 条能在输出文件中找到。v2 把这些行全部记为 `same_label_duplicate_first_source_row`，并让 verifier 强制检查 train、validation、excluded 恰好覆盖全部源行。

v2 还把 RDKit 版本和 canonical/scaffold 重算写进 manifest，并把 provider train/validation 与 confirmation 的 scaffold overlap 固定成机器检查。development scaffold overlap 仍然存在，已单独记录，不能写成 development scaffold-disjoint。

## 固定输入与 manifest

| 输入 | SHA-256 |
|---|---|
| TDC BBB `train_val` | `5035139072311f4cb0a1a54cffe11bc727004886ace4629b98f6db20c431e4af` |
| selector development panel | `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf` |
| confirmation 候选 | `c0af602bbc4ec49df448b916fb1e54dfce183926965d61c184cb736e68402805` |

manifest：`experiments/logs/task49_provider_fresh_menu_20260930/manifest_v2/manifest.json`  
manifest SHA-256：`b7433036004abdcac7e797db8054fcadb99847d5095c3e59c5f5e1f8807ad0b9`  
独立收据：`experiments/logs/task49_provider_fresh_menu_20260930/verification_v2.json`，状态 `verified`。

canonicalization 使用 RDKit `2025.09.2`：`MolToSmiles(..., isomericSmiles=True)`，scaffold 使用不含手性的 Bemis--Murcko；无效 SMILES 直接失败。

## 成员与隔离

| 项目 | 数量 |
|---|---:|
| 原始源行 | 1,624 |
| canonical 分子 | 1,581 |
| 冲突 canonical group | 8 |
| provider 候选 canonical 分子 | 1,025 |
| provider train | 845 |
| provider validation | 180 |
| excluded | 599 |
| 其中同标签重复额外行 | 14 |
| train scaffold | 505 |
| validation scaffold | 127 |
| train/validation scaffold overlap | 0 |
| candidate/confirmation scaffold overlap | 0 |
| candidate/development scaffold overlap | 118 |

候选与 development/confirmation 没有 canonical overlap；confirmation scaffold 也没有 overlap。候选与 development 有 118 个 scaffold 重合（train 97、validation 21），所以 provider 训练集不是对 development 的 scaffold-disjoint 集合。这是已记录的分布边界，后续 selector 结果不能把它解释成完全结构外推。

## CPU provider training v3 (v2 manifest)

训练器 commit：`1e523fb44ce28aefa4af321d15777f844ff05ff8`。receipt 同时记录 Python 3.9.6、NumPy 1.26.4、scikit-learn 1.6.1、PyTorch 2.8.0、RDKit 2025.9.2、线程设置、工作树状态和四个相关源码的 git-tree blob hash。只读取 845 条 provider-train 和 180 条 provider-validation，未读取 confirmation。输出在：

`experiments/logs/task49_provider_fresh_menu_20260930/cpu_v3/`

四个 checkpoint、720 条 validation prediction 和配置均由独立 verifier 重算通过，最大概率误差为 `4.99×10^-11`，收据：

`experiments/logs/task49_provider_fresh_menu_20260930/cpu_verification_v3.json`

| provider | accuracy | ROC-AUC | BCE |
|---|---:|---:|---:|
| ECFP-LogReg | 0.9333 | 0.9759 | 0.1639 |
| ECFP-MLP | 0.9167 | 0.9355 | 0.2681 |
| ECFP-RandomForest | 0.9111 | 0.9638 | 0.2538 |
| Graph message passing | 0.7611 | 0.8384 | 0.4655 |

这些只是 provider 自身的 validation sanity metrics，不是 selector 结果，也不是 confirmation 结果。图模型表现较弱保留在证据中；没有换 split 或删模型追分。

## 当前资格边界

这一步闭合了：

- 源行完整分区和可恢复性；
- provider train/validation 成员与 hash；
- train/validation scaffold 隔离；
- confirmation canonical/scaffold 隔离；
- 四个 checkpoint 的逐行前向可重算。

这一步仍没有闭合：

- 将当前环境进一步固化为可直接重建的依赖 lock 文件（receipt 已记录版本和源码 blob，但还不是完整 lock）；
- 四个 provider 是否都进入最终 confirmation 菜单；
- selector 的统一 feature/warm-start/update budget/cost 合同；
- 一次性冻结 confirmation。

因此 Gate A/B/C 仍是 `HOLD`，不打开 confirmation，也不把四个候选写成已经通过资格审查的最终 baseline。
