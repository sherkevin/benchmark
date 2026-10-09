# GraphB3 checkpoint–split lineage 审计 v1

日期：2026-09-30

## 结论

GraphB3 的本地包确实包含可读的训练表和测试表，且两张表在保留立体化学的 canonical SMILES 上没有交集；当前发表论文还报告了与这两张表吻合的 B3DB split protocol（6029 条 train-validation、1771 条最终 test，并进一步拆成 4823/1206）。检查点也能被当前推理脚本严格加载，14 个 tensor 的形状与脚本定义一致。但公开包没有把 `best_mcc_model_weights.pth` 绑定到 10 个 seed/split 中的哪一个，也没有提供精确成员 manifest、训练脚本、优化器配置或训练日志。因此本次把 GraphB3 记为 **G3-paper-backed-observational**：可以报告论文支持的 split provenance 和固定菜单上的运行身份，不能把该具体检查点写成已经完成 checkpoint-specific model-fresh 证明的独立测试模型。

## 只读审计范围

审计读取了以下固定资产，没有运行 GraphB3 推理、没有训练、没有生成新预测：

- `README.md`
- `graphb3.py`
- `deepchem_utils.py`
- `best_mcc_model_weights.pth`
- `data/train_mol_set.xlsx`
- `data/test_mol_df.xlsx`

机器收据：`experiments/logs/task49_graphb3_lineage_audit_20260930/run_v1/results.json`，SHA-256=`6d0dcee12be5c080ad4c380a0f4195851ae932bc175aac8cebded5056d1919d7`；论文证据摘要：`paper_evidence.json`（更新后 hash 见该文件本身；全文 HTML 不入库）。

## 论文层面的新增证据

GraphB3 的一手论文（Kumar et al., DOI [10.1093/bib/bbaf679](https://doi.org/10.1093/bib/bbaf679)，全文 [PMC12710471](https://pmc.ncbi.nlm.nih.gov/articles/PMC12710471/)）说明：模型基于 B3DB，采用分层划分；先得到 6,029 条 train-validation 和 1,508 条候选 test，再加入 263 条按分子量保留的 OOD 分子，形成 1,771 条最终 test；train-validation 再拆成 4,823 train 与 1,206 validation。论文还说明使用 10 个随机 seed 和不同 train-validation split state，并按 validation MCC 选择 checkpoint。

这解决了“本地表格是否与论文报告的 split 数量一致”这一问题，但没有解决“当前 `best_mcc_model_weights.pth` 对应哪一个 seed/split、它的精确成员是谁”这一问题。

## 观察结果

| 检查项 | 结果 |
|---|---:|
| train 表行数 | 6,029（含 2 行无效 SMILES） |
| test 表行数 | 1,771（无无效 SMILES） |
| train/test canonical 交集 | 0 |
| train 重复 canonical 行 | 1 |
| test 重复 canonical 行 | 0 |
| 检查点 tensor 数 | 14 |
| `initial_conv` 输入维度 | 33 |
| 隐藏维度 | 128 |
| 当前推理脚本 strict state-dict load | 已在既有 runtime receipt 中通过 |
| 包内训练脚本/训练日志 | 未发现 |
| checkpoint → split 元数据绑定 | 未发现 |

README 只说 `data` 目录包含“用于训练”的 BBB 数据；`graphb3.py` 是加载检查点、推理和保存结果的脚本，没有 optimizer、loss、backward、数据划分或 checkpoint 保存代码。结合论文，当前证据支持“这些表格与论文报告的 B3DB split protocol 一致”这一中等强度说法；仍不支持“当前具体检查点已经绑定到某个已知 seed/split 和精确训练成员”这一强说法。

## 对 benchmark 的影响

- GraphB3 仍可作为固定 provider menu 的可追溯运行动作，并且其表级 split 有论文数量和协议支持。
- 其本地 test 表中的候选分子不能直接作为 model-fresh confirmation；需要拿到训练脚本或作者明确的 checkpoint–split 证明后才可升级。
- current 722 分子与本地 GraphB3 表的交集仍只能解释为 exposure audit 线索：448 行出现在 train 表，161 行出现在 test 表；这不改变“整个 33-provider panel 的独立资格尚未闭合”的结论。
- 论文中可写 lineage-aware operational routing；暂不能写 GraphB3 提供了严格独立的泛化 confirmation。

## 下一步

1. 继续查作者仓库的原始训练 commit、release asset 或补充材料，寻找当前 checkpoint 对应的 seed/split 与成员 manifest。
2. 若仍找不到，固定 G3-paper-backed-observational 标签，并设计不依赖该具体 checkpoint 独立性的冻结 confirmation 来源。
3. 不因这次表格不重叠就提前关闭 Gate A；它解决的是表级 split 观察，不是 checkpoint provenance。
