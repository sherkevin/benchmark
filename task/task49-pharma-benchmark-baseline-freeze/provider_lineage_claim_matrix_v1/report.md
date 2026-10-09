# Provider lineage 与主张边界矩阵 v1

日期：2026-09-29
状态：Gate A 审计补充；不改变 provider 权重、不产生新预测。

## 1. 目的

当前任务测的是“固定已部署医药工具池上的在线选择”，不是重新评估每个 provider 在未见分子上的泛化能力。因此，provider 训练成员未知不会自动让 routing benchmark 无效；真正的要求是：公开 lineage 证据、按 family 处理相关性、禁止把固定池结果写成 model-fresh 泛化。

## 2. 资格分层

| 层级 | provider | 可以支撑的主张 | 禁止的主张 |
|---|---|---|---|
| G3 | GraphB3 | 固定菜单中的 lineage-aware operational routing；可报告观察到的本地 train/test split | 不能直接写成 checkpoint 已证明 model-fresh，除非补齐训练脚本与 checkpoint 绑定 |
| G2 | ADMET-AI 1.4.0/2.0.1；DeepPurpose CNN/Morgan/MPNN；LGBM-BBB；24 个 B3clf 变体 | 固定菜单上的工具选择、selected-only 反馈和 family-sensitive 结果 | 不能写成独立训练来源；24 个 B3clf 不能当 24 个独立 family，3 个 DeepPurpose 不能当 3 个独立数据来源 |
| G1 | FP-ADMET；ADMETlab 3.0 | 已部署菜单中的 lineage-aware stress control，前提是输出、失败和版本可复核 | 不能写成独立 confirmation provider 或 model-fresh 泛化证据 |

## 2026-09-30 LGBM-BBB 重建边界

LGBM 的 B3DB 来源、notebook 和发布模型可追溯，但当前 runtime 重建出 7,296 行而文档目标为 7,298 行；实际训练中间文件 `dcorr_filtered_BBB_0.25.csv` 未随包发布。LGBM 仍为 G2，重建 split 不进入独立 confirmation。

## 2026-09-30 GraphB3 绑定审计

新增 `task/task49-pharma-benchmark-baseline-freeze/graphb3_lineage_audit_v1/report.md`。本地 train/test 表分别为 6,029/1,771 行，canonical 交集为 0；检查点有 14 个 tensor 且与推理脚本的 33→128 结构一致。但包内没有训练脚本、优化器、日志或 checkpoint→split 元数据，因此 GraphB3 升级为 `G3-paper-backed-observational`：论文和本地表格的 split 数量/协议一致，但 checkpoint-specific seed/member binding 仍缺，不能写成 model-fresh 独立测试证据。

## 3. 统计和报告约束

1. 33 个 provider 是同一题上的候选动作，不是 33 个独立样本。
2. 主表至少同时报告 all-provider、family-macro 和 lineage-stratified 结果；24 个 B3clf 先按 B3DB family 聚合。
3. 旧 BBB development panel 只能支撑 development/operational routing claim；157 条 scaffold-disjoint candidate 也不能自动解决 provider training exposure。
4. 如果论文要声称 model-fresh 或跨 provider 泛化，必须另建 provider checkpoint 可绑定的确认菜单；当前矩阵不支持该主张。
5. 任何 selector 结果都必须同时标注 endpoint、menu、lineage grade、feedback delay 和是否为 controlled event。

## 4. 结论

现有 provider lineage 审计已经足以支持“固定已部署菜单上的在线工具选择”这一收窄后的任务定义，但不足以支持“33 个独立 provider 的 model-fresh 泛化”。这应当反映在 Gate A A4 的评分中：证据边界已建立，独立资格仍未建立。

证据：`research/provider_training_membership_audit_20260929.md`、`task/task49-pharma-benchmark-baseline-freeze/data_contract_v0.md`、`docs/决议/0004-主Benchmark收敛为TDC与Task36医药工具路由.md`。
