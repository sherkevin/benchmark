# Task49 数据合同 v0

日期：2026-09-26  
状态：审计完成，不能作为最终 benchmark 发布版。

这份合同只回答一个问题：现有结果库哪些行可以作为“具体 provider 选择后，释放一个 0/1 正确性反馈”的分析样本。它不证明 provider 的训练集隔离，也不证明方法有效。

## 分析单位和数据来源

- 基本统计单位是一个独立分子；同一分子上的多个 provider 输出是候选结果，不增加样本数。
- scaffold 数只作描述。当前审计使用含手性和不含手性的 Bemis--Murcko 分组，但没有把它们冻结成 split。
- 输入池是 `data/tool_results/molecule_pool.csv` 的 730 个 `entity_id`。池中有 730 个严格 canonical SMILES 和 730 个 parent SMILES；描述性 scaffold 数为 401。
- 真值来源是公开 TDC ADMET 文件。文件、下载地址和 SHA-256 已写入 `experiments/logs/task49_data_audit_v0_20260926/v1/source_manifest.json` 与 `task/task49-pharma-benchmark-baseline-freeze/data_audit/sources/`。

| 端点 | 真值来源 | 有参考标签的分子 | 具体 provider | 输出完整且有参考标签 | 结论 |
|---|---|---:|---:|---:|---|
| BBB | `bbb_martins.tsv`；文件 SHA `eb32676c…5e98` | 722 | 33（7 family） | 716 | 可作 development；不能作 untouched confirmation |
| hERG | `herg.tsv`；文件 SHA `dfce97b6…a379` | 43 | 3（3 family） | 43 | 只能作小规模资格候选 |
| AMES | `ames.tab`；文件 SHA `825c2b4c…3e16` | 129 | 4（4 family） | 129 | 只能作小规模资格候选 |

三端点交集是 12 个分子；BBB 与 AMES 交集 127，BBB 与 hERG 交集 43，hERG 与 AMES 交集 12。端点的 provider 池不同，不能把三个端点拼成一条同分布的样本流。

BBB 的 722 个真值来自现有 ledger：8 个冲突真值被排除。严格 source join 直接解析到 695 行；另外 27 行只在 parent-fragment join 解析到，故保留为 ledger-resolved 并单独记录，不能把“严格 join 695”写成“只有 695 个可评分”。BBB 有 724 个 33-output 完整行，其中 716 个同时有可用真值；另外 6 个有 B3clf 输出缺失。历史数字 652 指七家族汇总票齐全且可评分，不是 33-output 完整数。

## 输出字段与反馈

BBB 的 33 个具体 provider 包括 ADMET-AI 1.4.0/2.0.1、DeepPurpose 三种编码器、GraphB3、LGBM、24 个 B3clf 变体、FPADMET 和 ADMETlab 3.0。hERG 有 ADMET-AI、ADMETlab、FPADMET；AMES 有 ADMET-AI、ADMETlab、FPADMET、Toxtree。B3clf 24 列共享一个文件和模型谱系，不能当作 24 个独立来源；同样的依赖关系在统计分析中必须按 family 或 provider lineage 说明。

每个 provider cell 保留原始值、状态、来源行和 provider id。概率 provider 按冻结的 0.5 阈值转成二元预测；词汇 provider 按 `providers.json` 的 vocabulary 映射。缺失、失败和未知真值不填成 0。

一次 episode 的合法信息是分子表示、性质 key、候选 provider 菜单、已经释放的历史反馈和预算。选择发生在本题真值和未选 provider 输出之前；终止后才释放被选 provider 的正确性标签。由于二分类预测和正确性标签结合后可以恢复该题的性质标签，实验不能声称“反馈不包含真值信息”；必须加入同一标签访问条件下的直接性质预测器控制。

## 可复核实例

以下用真实缓存行实例化三个端点；BBB/AMES 是 `z000282`，hERG 是 `z021687`：

1. BBB ledger label 为 1。DeepPurpose-CNN 的原始概率为 `0.9352496862411499`，转成预测 1，反馈为 1；GraphB3 概率为 `0.02379637211561203`，转成预测 0，若被选择反馈为 0。
2. hERG 实验标签为 0。ADMET-AI 概率为 `0.01582832634449005`，预测为 0；FPADMET 原始词为 `NonBlocker`，按 vocabulary 映射为 0。
3. AMES 实验标签为 0。ADMETlab 概率为 `1.1848568419736694e-06`，预测为 0；FPADMET 为 `inactive`，Toxtree 为 `YES`，二者按各自登记的 vocabulary 都映射为 0。

这些行只说明字段怎样实例化，不是方法结果，也不证明端点来源的训练隔离。Toxtree 的交付列来自 “No alerts for S. typhimurium mutagenicity”，因此 YES→0、NO→1；证据为 `data/tool_property_matrix/evidence/toxtree/delivered_column_provenance.txt`。它是结构警示预测，不能当作独立实验真值。ADMETlab 的 hERG 定义包含 IC50 ≤10μM 或 10μM 时抑制率 ≥50%；TDC 与 FPADMET 的具体 assay/cutoff 尚待核对，连接到同名端点不等于已证明可比。

## 当前资格边界

1. BBB 只能作 development benchmark。旧 BBB 已被反复用于选择问题、阈值和方法讨论；不能把 scaffold re-split 后的行称为 model-fresh 或 untouched confirmation。
2. hERG/AMES 的覆盖分别只有 43/129 个分子，当前审计没有完成 provider 训练成员审计、独立 snapshot 或最终 split，因此不满足三端点 confirmation 的条件。
3. 来源表有重复和冲突记录：BBB strict source 有 10 个冲突组、64 条重复额外行；hERG 有 3 个冲突组、7 条重复额外行；AMES 无冲突但有 23 条重复额外行。去重规则必须固定在发布版中，不能由模型表现反推。
4. 当前审计没有产生新的科学模型预测，没有训练，没有远程任务，也没有置信区间。`audit_validation/run_v1/summary.json` 的 `all_checks_passed=true` 只表示保存的 join、source row 和 provider cell 可复核。

## 可以复用的数据与尚缺的资格

公开来源在 parent 去重并排除冲突后，还有池外候选：BBB 1,233、hERG 595、AMES 7,126。它们可用于准备扩展采集，但当前没有对应的新 provider 输出，也没有证明上游训练隔离或最终确认资格。现有缓存还可核查 ADMET-AI 2.0.1 的 hERG/AMES（增加版本，不增加 family）及 BayesHerg 的 730 个输出（原生 label 为空，先核语义）。

BBB development 可开展已授权、预登记的诊断；正式多端点与 confirmation 仍需 provider 训练曝光清单、端点原始定义、划分和独立输出。公共再分发权限尚未审计。若这些条件不能满足，记录缺口和补齐成本，回到 ADR-0021 的范围决策；本合同不自行取消多端点与最终确认要求，也不用工具共识补真值。

证据：`experiments/logs/task49_data_audit_v0_20260926/v1/summary.json`、`task/task49-pharma-benchmark-baseline-freeze/audit_validation/run_v1/summary.json`、`audit_validation/run_v1/cases.json`。
