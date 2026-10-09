# Provider lineage audit v1：预登记与边界

日期：2026-10-04  
状态：已执行；只读审计。

## 目的

把 BBB 固定菜单的 33 个具体 provider 逐一归档为 G1/G2/G3，并记录训练来源、可核对的分子成员证据、当前开发 panel 的 exposure 线索和仍缺的 freshness 证据。这一步只解决资格与主张边界，不比较 selector 效果，也不打开 confirmation。

## 固定输入

- provider cell 回放：`experiments/logs/task49_data_audit_v0_20260926/v1/provider_cells.jsonl`
- 分子池：`data/tool_results/molecule_pool.csv`
- 既有训练成员审计：`research/provider_training_membership_audit_20260929.md`
- GraphB3 绑定审计：`task/task49-pharma-benchmark-baseline-freeze/graphb3_lineage_audit_v1/`
- LGBM 重建审计：`task/task49-pharma-benchmark-baseline-freeze/lgbm_lineage_audit_v1.md`

生成的 `matrix.json` 保存了 provider cell 与分子池 SHA-256；不读取未选 provider 输出来重算标签，不修改任何输入。

## 分级规则

- **G3（observational）**：有 provider-specific 的可读 train/test 分子表，并有一手 split 协议支持；若 checkpoint 没有绑定到具体 seed/split/member，仍只能写观察到的 split。
- **G2**：训练来源、版本或来源交集已经核对，但没有当前 checkpoint 的精确成员表。
- **G1**：运行资产或结果快照可定位，但训练来源或分子成员不能映射。
- **MISSING**：不作为 provider 总等级，而是用于 `missing_evidence` 字段；它表示某项证据缺失。若以后出现没有任何可核对运行身份的动作，才新增 MISSING provider 等级。

等级永远不自动赋予 model-fresh confirmation 资格。打开 confirmation 还需要 checkpoint/member 绑定、独立输入与 split、独立 provider 输出及完整 verifier。

## 预期输出与停止条件

- 输出 33 行 provider 明细、family 计数、等级计数、证据路径存在性和缺口。
- 如果 provider 数量、来源 receipt、family 或路径与既有材料不一致，脚本失败而不是静默生成矩阵。
- 不启动 provider，不训练，不提交 Nebula，不产生新的预测值。

## 执行

```bash
python3 task/task49-pharma-benchmark-baseline-freeze/provider_lineage_audit_v1/build_provider_lineage_audit_v1.py
python3 -m pytest -q task/task49-pharma-benchmark-baseline-freeze/provider_lineage_audit_v1/test_provider_lineage_audit_v1.py -p no:cacheprovider
```
