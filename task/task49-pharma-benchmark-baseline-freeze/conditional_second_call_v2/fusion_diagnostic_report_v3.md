# B-only 终局融合诊断 v1：最终核验报告（v6）

日期：2026-10-08。状态：**按冻结卡执行完成，独立核验通过；仅更新开发优先级。**

## 目的和边界

这次诊断只区分三个竞争解释：第二输出没有可用信息、当前终局融合没有正确使用第二输出、以及 gain 价值模型没有把可用变化排出来。它使用已经查看过的 B 回执和冻结的每折模型，不读取 C outcome、策略汇总、bootstrap、provider/API、GPU 或 confirmation。它不是新确认实验，不改变生效方法，不把结果写入论文主表。

冻结卡 [`fusion_diagnostic_card_v1.md`](fusion_diagnostic_card_v1.md) SHA-256：
`b0da0f3f72fbe08d3beacd8073a2f2754f38554d67283fe387e2548aee9fc319`。
原始实现缺陷保存在 `real_v1/contract_failure.json`、`real_v2/`、`real_v3/`、`real_v4/` 和 `real_v5/`；最终版本为 `real_v6/`。失败版本没有被删除，也没有用于选择分支。

## 输入、规模和恢复

- 读取白名单只有两份 B 回执和十份每折冻结模型元数据；两份 B hash 与两个 artifact manifest 一致，v2 每折模型的 `old_model_sha256` 与 v1 模型一致。
- 506 个 fold–molecule 记录、每记录 3 个候选第二工具；S 面板 1,518 行，U12 面板 6,072 行，U10 面板 5,060 行（每分子 4 singleton + 6 unordered pair），分子级 gain 审计 506 行。
- S 每个候选权重 $1/3$；U12 每个有向 pair 权重 $1/12$；U10 每个 mask 权重 $0.1$。U 面板是离线 evaluator 展示面板，不是实际 acquisition 分布。
- 生成完整的 fold/all、selected/not-selected、actual-first、ordered/unordered pair、first-action 和 ordered-pair/first-action 统计，共 2,232 组；6 个 unordered pair 在 all、四个 actual-first 子集和 fold/all 上的 native probability、logit、`.5` 标签不一致和错误指示相关性共 180 条。动作转移表另有 324 个固定格（含空格）。
- 运行耗时 0.91 秒，输出 9.47 MB，峰值 RSS 112,640,000 bytes；均在冻结卡的 2 分钟、1 GiB、50 MB 限制内。
- 独立 verifier 不导入 runner 的输入加载函数，重新读取 v1 B 回执和 verifier 系数，重建 6,072 个有向 pair，并核对选中分组、U10 行数、完整统计/相关性行数和 branch decision。独立核验 SHA：`6ce7265ea97fc33e68e6fc5dcd83a0344f9b6460a09e725bd46f6b6c3b0646d0`。

所有 JSONL、配置、输入 hash、完整分组、相关性、资源和独立核验在 [`real_v6/`](../../../experiments/logs/task49_conditional_fusion_diagnostic_20261008/real_v6/)；执行代码是 [`analyze_conditional_fusion_diagnostic_v1.py`](../../../src/probe_headless/analyze_conditional_fusion_diagnostic_v1.py)。

## 结果和机械分流

| 分支 | 结果 | 卡内含义 |
|---|---|---|
| 分支 1：S 变差、U 不变差 | 只有一个 fold 的一个组满足 | 不触发选择条件转移干预 |
| **分支 2：S、U 同时变差** | **`K=(a=0,b=3,d1=拒答)` 在 fold 0、1、3 同时满足；每个 S/U 支持均 ≥10，跨 fold 去重 entity=63** | **优先冻结“只改终局融合规则”的单因素干预** |
| 分支 3：大多数动作不变且仍有可见概率移动 | fold 0、1、3 满足，但按优先级不先处理 | 保留为竞争解释 |
| 分支 4：完整 S 面板不退化、base16 漏选正候选 | 只有 fold 1、4 满足，少于 3 个 fold | 不触发 gain 优先干预 |

分支 2 选中组的描述性读数如下；正号表示第二输出后的量增加，因而风险或 Brier 变差：

| fold | panel | 去重 entity | $L_2-L_1$ | Brier$_2$−Brier$_1$ |
|---:|---|---:|---:|---:|
| 0 | S | 12 | +0.08333 | +0.01301 |
| 0 | U | 30 | +0.05000 | +0.01286 |
| 1 | S | 27 | +0.04630 | +0.02896 |
| 1 | U | 41 | +0.00610 | +0.01596 |
| 3 | S | 28 | +0.10714 | +0.02606 |
| 3 | U | 43 | +0.06395 | +0.01983 |

同一 `K`、同一 fold、同一首动作下，S 和 U 两个面板都满足风险差和 Brier 差严格为正；跨 fold 的 entity 并集为 63，超过卡内 30 的最低支持。分支 2 是开发分流条件，不是显著性检验，也不证明真实 posterior 不一致。

整体 S 面板风险差为 `+0.000659`、Brier 差为 `−0.002522`；U12 整体风险差为 `−0.014287`。整体量与条件组方向不同，正是不能用总体均值代替条件诊断的原因。

## 下一步

按执行前固定的优先级，下一张实验卡只比较终局融合规则，保持以下内容不变：

1. 首工具选择器、首工具输出和已取得的第二工具 pair 流；
2. provider 菜单、selected-only 反馈、延迟、调用预算、成本 $\kappa=0.01$ 和三动作损失；
3. 原 masked verifier 作为 `ignore-second` 控制；
4. 首动作阈值、gain learner、训练/评价切分和 baseline 信息权限。

融合候选的分布假设、参数量、训练权限、同预算控制和反驳条件必须另写实验卡。若它不能超过 `ignore-second`、同预算 confidence cascade 和当前 strongest legal control，则停止该候选；若只改善 Brier 而不改善终局 utility，也不通过方法验收。不能同时修改 verifier、gain learner 和反馈更新。

## 不能宣称

这次没有证明第二工具在一般情况下有信息价值，也没有证明联合模型必然更好；没有产生实时学习效果、agent 复核/拒答闭环效果或新 confirmation 结果。三份科学投稿 gate 仍保持 HOLD。负分支、未触发分支、旧实现失败和全部中间产物均已保留。
