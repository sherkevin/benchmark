# BBB fresh split 审计

日期：2026-09-25

## 结论

仓库里没有能够证明“所有 provider 在训练时都没有见过”的 fresh BBB 分子池。

已有一个可复现的预注册 scaffold split，但它只能作为 selector/conflict-screening 的结构留出集，不能支持 provider 的未见分子泛化结论。

## 已有的规范 split

`data/bbb_census_v3/tool_conflict_discovery/preregistration.json` 记录了唯一明确的预注册切分：

- `GroupShuffleSplit(n_splits=1, test_size=0.2, random_state=20260901)`。
- 分组键是带手性的 Bemis–Murcko scaffold。
- 730 个源分子划为 train 619、test 111。
- train hash：`1f918be323ef06e603a79234aa7cfe6a3231cc8ae629eef878ff810fe88a52af`。
- test hash：`a9578e5476df87816b67bc4e76095d9cfaaa2735a2e9d1eb79839b4c88578426`。
- train/test scaffold 交集为 0。
- 8 个 unresolved entity 全在 train，test 的 111 个分子均可评分。

这个切分比当前 hash scaffold split 的 616/106 更合适，后续正式回放应优先复现这 111 个 test entity。

## 为什么不能称为 model-fresh

DeepPurpose worker 明确记录其 BBB checkpoint 依据 MoleculeNet BBBP 数据训练。仓库中的 `BBBP.csv` 与 730 个回放分子按 RDKit canonical isomeric SMILES 去重后重叠 703 个，约 96.3%。预注册 test 的 111 个分子中有 107 个与 BBBP 重叠。

因此，scaffold 没有交集只说明 selector 的校准/冲突筛选没有跨 scaffold 泄漏；它没有排除 provider checkpoint 已经见过同一分子的可能。其余 provider 也没有完整、可核验的训练成员清单。

## 不能从当前仓库得到的切分

`molecule_pool.csv` 只有 `entity_id` 和 `canonical_smiles`，没有测量日期、采样批次或外部数据集来源字段。因此无法从现有文件构造可信的时间切分或新批次切分。按 entity ID 排序或随机抽样都不能替代真实时间/来源隔离。

## 可执行的 fresh data 进入条件

1. 先冻结 provider checkpoint、版本、推理代码和 selector 配置，并记录文件哈希。
2. 从独立外部来源取得新分子和独立真值；保留来源、测量日期、实验批次和原始证据。
3. 用与 provider 相同的 canonical isomeric SMILES 做 exact dedup，排除与所有训练/调参数据及已知 BBBP 训练成员的重叠。
4. 再做 Bemis–Murcko scaffold exclusion；新池 scaffold 不得出现在训练、校准、阈值选择或开发集。
5. 在运行前写死 pool/split 清单和 SHA-256；fresh labels 只能用于最终一次评估，不能用于选 provider、调阈值或改方法。
6. 若无法获得 provider 的训练成员清单，只能把结果标为“结构留出回放”，不能写成“未见分子泛化”。

## 对当前实验的处理

当前 722 resolved entity 的结果保留为 post-freeze retrospective。可以用预注册的 111 test entity 重跑并报告 selector 的 held-out 行为，但论文中应明确它不是 model-fresh confirmation。要支持更强结论，必须另建 external fresh pool 并完成上述审计。

