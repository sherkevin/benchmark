# 公共 benchmark 权威性审计

日期：2026-09-28

## 结论先说

当前 BBB development 面板的真值不是项目内部凭空构造的。722 条分子来自仓库已有的 TDC ADMET `BBB_Martins` 真值链；按现有最大碎片 canonical SMILES 匹配，722/722 条标签与 TDC 数据一致。TDC 原始文件有 2,039 行、1,975 个 canonical 分子，这与 TDC ADMET benchmark 公布的 BBB 数据规模一致。

因此，当前任务可以被准确地描述为：

> 以公开、可追溯的 TDC BBB 真值为 benchmark anchor，在同一批分子上比较多个实际可调用 provider 的直接选择与反馈适应。

这里的“工具选择任务层”是我们基于公开分子 benchmark 和 Task36 provider 回放构造的评测协议；它不是把内部 722 条结果冒充成一个已有的社区 benchmark。论文中必须把公共真值来源和我们新增的 routing protocol 分开写。

## 审计方法

- 输入：冻结 fingerprint diagnostic 的 722 条 BBB development 分子和标签。
- 对每个公开数据源，用 RDKit canonical SMILES 做完整分子匹配；再用最大碎片匹配复现仓库 provenance ledger 的盐形式处理。
- 比较标签，不用公开模型预测替代实验标签。
- 所有原始文件、下载 URL、SHA-256 和运行结果均保存；没有新 provider 调用。

## 结果

| 公共来源 | 原始规模 | 完整分子重叠 | 最大碎片重叠 | 标签一致率 | 用途判断 |
| --- | ---: | ---: | ---: | ---: | --- |
| TDC ADMET `BBB_Martins` | 2,039 行 / 1,975 unique | 695 | 722 | 100% | 主真值锚点 |
| B3DB classification | 7,807 行 / 7,805 unique | 609 | 609 | 95.57% | 外部敏感性检查，不合并真值 |
| PharmaBench BBB | 8,301 | 559 | 559 | 90.16% | 外部敏感性检查，不合并真值 |

B3DB 与 PharmaBench 的差异不是 SMILES 格式问题：在重叠分子上仍分别有 27 和 55 条标签不同。这说明不同公开数据集的来源、冲突处理或阈值定义不能在没有端点审计的情况下混合。

AMES 的同源审计得到：TDC AMES 有 7,278 行、7,255 个有效 canonical 分子；与 PharmaBench AMES 的 9,139 行逐分子重叠 7,255 条，其中 7,222 条一致、33 条为 `TDC=0 -> PharmaBench=1`，没有反向差异。这个结果支持把 TDC AMES 作为另一个公共真值锚点，但也说明 PharmaBench AMES 不能直接和 TDC AMES 合并。

## 官方 Group split 的冻结审计

从 TDC Dataverse 的 ADMET Group archive（下载文件 SHA-256：`bd0005246cb6f1672333c28ad202112a8bf071ab65d847848715030308faf1f1`）提取并冻结了 `BBB_Martins` 与 `AMES` 的 `train_val/test` 文件。审计结果如下：

| endpoint | train_val | test | combined unique | train/test canonical overlap | scaffold overlap |
| --- | ---: | ---: | ---: | ---: | ---: |
| BBB_Martins | 1,624 行 / 1,581 unique | 406 行 / 394 unique | 1,975 | 0 | 0 |
| AMES | 5,821 行 / 5,802 unique | 1,457 行 / 1,453 unique | 7,255 | 0 | 0 |

两个 endpoint 都没有无效 SMILES，也没有非手性 Bemis--Murcko scaffold 交集。文件内部存在 canonical duplicate rows（BBB 合计 55 行，AMES 合计 23 行），因此后续 benchmark 必须显式决定按公开行保留还是按 canonical molecule 去重，不能默默去重。更关键的是，BBB `train_val` 有 8 个 canonical 冲突组、`test` 有 2 个冲突组，同一 canonical 分子同时出现 `0` 和 `1`；AMES 两个 split 没有标签冲突。BBB 的冲突组不能通过“保留第一行”或“按多数票”静默解决，必须在 benchmark 发布合同中定义排除或重复测量语义。

这份官方 split 不能直接作为旧 722 条开发 panel 的 untouched confirmation：严格 canonical 匹配有 147 条，最大碎片匹配有 152 条。更严格地按旧 panel 的非手性 Bemis--Murcko scaffold 过滤时，清理 BBB test 的 392 个 unique 分子中有 235 个共享 scaffold；排除冲突组和旧 panel 分子后有 245 个 molecule-disjoint candidate，排除共享 scaffold 后只剩 157 个更强的 scaffold-disjoint candidate。旧 panel 已经被 selector 开发读取，因此这些重叠必须从新的 confirmation 声称中排除，或在新的方法冻结后重新定义未读取的确认集。该重叠是数据审计发现，不改变官方 split 本身 train/test 的 scaffold-disjoint 性质；具体计数见 `experiments/logs/task49_tdc_admet_split_audit_20260929/bbb_v4/fresh_confirmation_overlap.json`。

hERG 的 TDC 文件有 655 行、648 个有效 canonical 分子，包含 3 个 canonical 冲突组。它与当前 730 分子池严格匹配 43 条，最大碎片匹配 44 条；这与现有数据合同中的 43 条可评分候选一致。TDC hERG 与 NCATS/Siramshetty lineage 文件有 128 条训练集 canonical overlap、5 条验证集 overlap，但这只能说明来源有交集，不能证明 FP-ADMET 的精确训练成员或 assay 等价性。hERG 仍保留为资格候选，不进入当前主结果。

## 独立性与 provider 泄漏

“公开”不等于“对 provider 独立”。现有 provider 的来源审计显示：

- `DeepPurpose` 的 BBBP/MoleculeNet 路线与当前 BBB 分子池高度重叠；
- `B3clf` 的 README 明确说明模型使用 B3DB 分子训练，因此 B3DB 不能作为这些 provider 的独立测试集；
- ADMET-AI 的公开 BBB 训练来源也包含 `BBB_Martins` 相关数据。

因此，TDC 的 scaffold test 可以作为公开 benchmark 的真值与切分标准，但在把 provider 输出放进同一评测前，仍必须逐 provider 做训练分子成员审计，并记录模型版本、训练来源、checkpoint 和生成时间。未能证明 test 分子未见过的 provider，只能列为 lineage-aware control，不能被称为独立泛化结果。

PharmaBench 也不能直接作为“第二份独立真值”。其公开预处理流程合并了 B3DB、TDC Martins、TDC Adenot 和 ChEMBL，并对冲突样本做筛选；其中 TDC Martins 的原始 `0` 值被按 `x == -1` 逻辑误映射为正类，和当前 TDC 标签语义不一致。BBB 重叠审计得到 503 条 `1->1`、55 条 `0->1`、1 条 `0->0`，没有 `1->0`；AMES 重叠又得到 33 条 `0->1`。这些结果说明不能把 PharmaBench 与 TDC 标签逐条合并。PharmaBench 更适合作为单独端点的规模扩展或敏感性分析。

## 权威来源

1. TDC ADMET Benchmark Group：<https://tdcommons.ai/benchmark/admet_group/overview/>。页面列出 BBB 数据集、二分类任务、scaffold split 和 1,975 条规模。
2. TDC `BBB_Martins` 原始可下载文件：<https://dataverse.harvard.edu/api/access/datafile/4259566>。
3. TDC 代码与 benchmark 实现：<https://github.com/mims-harvard/TDC>。
4. B3DB 原始数据与论文：<https://github.com/theochem/B3DB>，<https://doi.org/10.1038/s41597-021-01069-5>。
5. PharmaBench 数据与论文：<https://github.com/mindrank-ai/PharmaBench>，<https://doi.org/10.1038/s41597-024-03793-0>。

## 对当前决议的影响

这份审计支持把当前 722 条 BBB development 重新表述为“公共 TDC benchmark 的可复现交集 + provider routing 评测层”。它解决了“真值是否权威”的问题，但没有自动解决以下问题：

- 工具选择协议本身是否应被命名为一个新的 benchmark；
- hERG 和 AMES 是否也能用同样严格的公共真值链；
- hERG 和 AMES 的 endpoint 语义、标签阈值和独立真值链；
- provider 的训练数据是否与公开真值存在分子级泄漏（目前已经发现多个明确的 lineage overlap，需要逐 provider 完成成员审计）；
- 论文是否必须加入一个现成的通用 tool-use benchmark 作为外部迁移测试。

官方 split 已经解决了公开切分和 scaffold 隔离的可复核性，但没有解决 provider 独立性；当前 33 个 provider 的训练成员审计仍显示独立资格为 0，见 `research/provider_training_membership_audit_20260929.md`。

所以不把这份审计直接写成“benchmark 已完全确定”的结论。它只确认：BBB 主开发集的真值来源满足公共、可追溯、可复核的最低要求。

## 复现产物

- 来源清单与哈希：`research/benchmark_authority_audit_20260928/source_manifest.json`
- 下载的只读源文件：`research/benchmark_authority_audit_20260928/sources/`
- 审计脚本：`src/probe_headless/audit_public_benchmark_authority.py`
- 结构化结果：`experiments/logs/task49_benchmark_authority_audit_20260928/run_v1/results.json`
- 确定性重跑：`run_v1/results.json` 与 `run_v2/results.json` 逐字节一致。
- AMES 对照脚本：`src/probe_headless/audit_public_ames_authority.py`
- AMES 对照结果：`experiments/logs/task49_ames_authority_audit_20260928/run_v1/results.json`（`run_v2` 为重复运行）。
- hERG 覆盖与 lineage 审计脚本：`src/probe_headless/audit_public_herg_authority.py`
- hERG 审计结果：`experiments/logs/task49_herg_authority_audit_20260928/run_v1/results.json`（`run_v2` 为重复运行）。
- 官方 split 审计脚本：`src/probe_headless/audit_tdc_admet_group_split.py`
- 官方 split 审计结果：`experiments/logs/task49_tdc_admet_split_audit_20260929/{bbb_v1,bbb_v2,bbb_v3,bbb_v4,ames_v1,ames_v2,ames_v3}/results.json`；`bbb_v3/v4` 额外记录了旧开发 panel 与官方 test 的 147/152 条重叠，`bbb_v4/ames_v3` 记录了 canonical duplicate 与标签冲突组。
- 官方 split 文件及其 archive/file hash：`research/benchmark_authority_audit_20260928/sources/tdc_admet_group_split_v2/`、`source_manifest.json`。
