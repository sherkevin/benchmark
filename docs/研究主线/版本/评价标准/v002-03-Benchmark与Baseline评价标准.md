# Benchmark 与 Baseline 评价标准：投稿前严格闸门

- 版本：v002
- 状态：已归档（当前生效版本的历史副本）
- 生效日期：2026-09-28
- 性质：本项目的内部 go/no-go 标准，不是 AAMAS 官方评分表，也不保证录用。
- 对应主文档：[Benchmark 与 Baseline](../03-Benchmark与Baseline.md)
- 逐条来源：[验收标准逐条溯源矩阵](../../../../research/AAMAS2027_验收标准逐条溯源矩阵_20260928.md)

## 先说结论

Benchmark 和 baseline 只有在“真值可信、任务匹配、信息公平、近邻充分、结果可复算”同时成立时，才有资格支撑论文结论。数据量大、provider 多或跑在 A800 上，都不能替代这五件事。

## 1. 硬性淘汰条件

| 编号 | 硬闸门问题 | 不通过例子 |
|---|---|---|
| H1 | truth 的来源、版本、标签语义和哈希是否固定？ | 运行时下载最新版数据 |
| H2 | duplicate、冲突标签、别名和 scaffold overlap 是否明确处理？ | 按第一行或多数票静默改标签 |
| H3 | provider 版本、checkpoint、训练来源和失败语义是否可追溯？ | 只写工具名，不知道实际权重 |
| H4 | 选择前是否读取了当前真值、未选输出或完整 panel？ | offline full-panel 结果被当 online feedback |
| H5 | confirmation 是否在方法和 baseline 冻结后才打开，且不回流调参？ | 看 test 后选阈值/epoch |
| H6 | 所有 baseline 是否完成同一动作、使用同一反馈权限和预算？ | Proposed 看全标签，baseline 只看 selected label |
| H7 | 是否至少有最强合法近邻、matched control、固定策略和 Oracle？ | 只和 Random 比 |
| H8 | 是否保存逐事件原始日志并能独立重算？ | 只有一张最终结果表 |

任一项为“是”，benchmark 只能作为开发材料，不能支撑主结果。

## 2. 十三项评分

统一评分：`0` 缺失/有泄漏；`1` 记录不完整或只适合开发；`2` 可复核并与主张匹配。

| ID | 检查项 | 2 分必须看到什么 | 证据记录 |
|---|---|---|---|
| B1 | 公共 truth 权威性 | 原始来源、版本、下载哈希、标签定义和独立审计 | truth manifest |
| B2 | 标签语义 | endpoint、阈值、unknown、冲突和 duplicate 规则固定 | label contract |
| B3 | split 独立性 | molecule/scaffold overlap、开发/确认边界和分母可复算 | split audit |
| B4 | provider lineage | 源码、权重、训练数据、checkpoint、运行环境和失败记录 | provider audit |
| B5 | 任务匹配 | benchmark 能直接回答论文主张，而不是只测上游预测准确率 | task contract |
| B6 | 信息公平 | 所有方法同一 $x_t,A_t,H_t$、同一反馈释放和同一调用预算 | event log |
| B7 | baseline 先进性 | 有最近邻现代方法、matched backbone、经典强控制和不可部署 Oracle | baseline matrix |
| B8 | baseline 适配诚实 | 论文原问题、适配改动、许可证和未复现部分清楚 | source audit |
| B9 | provider family 相关性 | 相关变体合并或 family macro 结果，不能把重复模型当独立证据 | family report |
| B10 | 指标完整 | accuracy/error、regret、调用数、延迟、内存、失败率和不确定性都定义 | metric spec |
| B11 | 实验覆盖 | 主结果、机制消融、延迟/shift/菜单压力和错误分析齐全 | experiment matrix |
| B12 | 统计与恢复 | seed、置信区间/方差、逐样本结果和独立恢复一致 | raw JSONL + recovery |
| B13 | confirmation 可信 | 方法、菜单、阈值和 baseline 冻结后一次性只读确认 | confirmation receipt |

### 通过阈值

- H1–H8 全部为“否”；
- B1–B9 每项必须为 `2`；
- B10–B13 至少 3 项为 `2`，其余不得为 `0`；
- 总分至少 `23/26`；
- 主表至少回答四个问题：是否超过强固定、是否超过最近邻、收益来自什么机制、在哪些条件下失败。

## 3. 最小实验包

| 实验 | 必答问题 | 不能缺的对照 |
|---|---|---|
| E1 任务有效性 | provider 是否真的有 context-dependent 差异？ | Random、Best Fixed、Oracle |
| E2 主比较 | 方法是否改善目标指标？ | 最强现代近邻、matched Frozen/Online/Replay |
| E3 机制消融 | 收益来自新增机制还是 backbone/预算？ | 去掉每个新增状态或更新 |
| E4 鲁棒性 | 延迟、菜单粒度、family 相关性或分布变化时是否仍成立？ | 预登记压力条件 |
| E5 错误分析 | 方法什么时候错、成本如何、是否需要拒答/复核？ | failure、unknown、成本对照 |

缺少 E2 或 E3，不能声称方法有效；缺少 E4 或 E5，不能声称适用范围广。

## 4. Reviewer 六问

1. 这个真值为什么可信，冲突标签怎么处理？
2. provider 是否见过这些分子或标签？
3. 每个 baseline 是否拥有同样的信息和预算？
4. 33 个 provider 是否只是同一个 family 的重复变体？
5. 主结果的提升是否在多个 seed/端点/压力条件下稳定？
6. 如果把最有利的 provider、split 或阈值拿掉，结论还成立吗？

任何一问没有证据，相关评分项不能给 2 分。

## 5. 评价标准的来源

- **O 官方要求**：AAMAS 要求 originality、significance、soundness、reproducibility、clarity、relevance、presentation 和 state of the art；官方没有规定必须使用哪个 benchmark 或固定 baseline 数量。见 [Main Track Call](https://warwick.ac.uk/fac/sci/dcs/aamas2027/calls/call-for-main-track/)。
- **P 代表性规律**：近年论文用与技术困难匹配的场景、canonical/modern 对照、消融和边界分析，不是无目的堆数据集；来源矩阵列出具体样本。
- **C 项目加严**：TDC truth、provider lineage、selected-only feedback、family 相关性、独立恢复和 confirmation freeze 是本项目防止自建回放过度承担结论的内部门槛。

## 6. 不通过后的动作

- H1–H5：冻结所有效果结论，先重建 truth/split/confirmation 隔离；
- H6–H8：不能继续比较数字，先重跑公平合同和恢复链；
- B1–B9 不足：只能报告 development/stress，不得写主结果；
- B10–B13 不足：缩小 claim，补统计、错误分析或可复现材料。

## 7. 评分执行规则

benchmark 和 baseline 由写作者、数据审计者各自打分。任一人发现信息泄漏、训练成员不明或恢复失败，直接触发硬闸门；分数不取平均，较低分生效。
