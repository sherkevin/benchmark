# Gate A：BBB confirmation 候选清单 v1

日期：2026-09-29  
状态：审计产物；不是已打开的 confirmation，也没有运行 provider。

## 目的

把 Gate A 中最容易混淆的三件事分开：TDC 官方 test 的标签冲突、旧 development panel 的分子/骨架重叠、以及未来 confirmation 候选的实际分母。这个步骤只生成候选清单，不训练 selector、不调用 provider、不把 test 标签暴露给任何方法。

## 固定输入

| 输入 | SHA-256 |
|---|---|
| TDC BBB test CSV | `82d4fa96f671a8382e9d8e6de3dde8385b9f337854c83e6fde2e69a7be93754f` |
| 已读过的 development panel | `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf` |
| TDC 冲突组收据 | `9e528562616ec9be1065830bc3104f1919f6da8418b0d00911303987386bd6a6` |

完整命令和 Python/RDKit 版本在 `experiments/logs/task49_gate_a_20260929/bbb_confirmation_candidate_manifest_v1/config.json` 与 `result.json`。

## 预登记的候选规则

对 TDC BBB official test：

1. 排除 10 个 canonical 冲突组；
2. 排除与已读 development panel 具有严格 canonical 分子重叠的条目；
3. 排除与已读 development panel 共享非手性 Bemis--Murcko scaffold 的条目；
4. 按 isomeric canonical SMILES 去重；只有标签一致的重复行才允许合并。

第 1 条是必要的标签安全规则；第 2--3 条把候选分成 molecule-disjoint 和更严格的 scaffold-disjoint 两种口径。本次产物采用更严格的 scaffold-disjoint 口径。

## 结果

| 项目 | 数量 |
|---|---:|
| TDC test 原始行 | 406 |
| TDC test canonical 分子 | 394 |
| 冲突 canonical group | 10（test 中 2 个） |
| 排除冲突行 | 4 |
| 排除旧 panel 严格分子重叠行 | 151 |
| 排除旧 panel 骨架重叠行 | 92 |
| 合并的同标签重复行 | 2 |
| scaffold-disjoint 候选 | **157** |
| 候选标签 0 / 1 | **36 / 121** |
| 候选内部不同 scaffold | 124 |

清单文件：`experiments/logs/task49_gate_a_20260929/bbb_confirmation_candidate_manifest_v1/manifest.csv`。SHA-256：`c0af602bbc4ec49df448b916fb1e54dfce183926965d61c184cb736e68402805`。独立检查确认 157 条候选没有冲突标签、没有旧 panel canonical overlap，也没有旧 panel scaffold overlap；同一输入重复生成的 manifest 字节一致。

## 这一步解决了什么

- 旧 722 panel 不能再被误写成 untouched confirmation；这一点现在有可直接使用的 157 条候选清单和固定 hash。
- TDC test 的冲突标签不会被多数票、第一行或模型结果静默覆盖。
- 后续 provider 输出重跑可以直接绑定到这份 manifest，而不是重新从 test 文件临时筛选。

## 还没有解决什么

这份清单还不是 confirmation。157 个分子仍然需要：

1. 冻结方法、baseline、菜单、delay、seed 和指标；
2. 决定采用旧 provider 的 lineage-aware stress，还是用可绑定训练成员的 provider 子菜单重新生成输出；
3. 由 evaluator 私有保存新 provider 输出，确认 selector 在决策前看不到 test 标签和未选输出；
4. 通过 Gate A--C 后才能一次性运行 confirmation。

因此当前 Gate A 仍保持 HOLD；本产物只是把 overlap/分母问题从“待处理”推进到可执行输入。
