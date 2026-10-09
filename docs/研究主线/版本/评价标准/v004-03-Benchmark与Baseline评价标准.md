# Benchmark 与 Baseline 评价标准：投稿前严格闸门

- 版本：v004
- 状态：生效
- 生效日期：2026-09-29
- supersedes：v003
- 性质：本项目的内部 go/no-go 标准，不是 AAMAS 官方评分表，也不保证录用。
- 对应主文档：[Benchmark 与 Baseline](../03-Benchmark与Baseline.md)
- 证据来源：[AAMAS 2027 验收标准来源审计](../../../../research/AAMAS2027_验收标准来源审计_20260928.md)、[逐条溯源矩阵](../../../../research/AAMAS2027_验收标准逐条溯源矩阵_20260928.md)、[benchmark 权威性审计](../../../../research/benchmark_authority_audit_20260928/report.md)、[provider lineage 审计](../../../../research/provider_training_membership_audit_20260929.md)。

## 1. 这份标准审什么

Benchmark 与 baseline 只有在“真值可信、任务匹配、信息公平、最近邻充分、结果可复算”同时成立时，才有资格支撑论文结论。数据量大、provider 多或跑在 A800 上，都不能替代这五件事。

本项目必须把两层写开：

1. **公共 truth layer**：来自可核验的公共数据、版本和标签语义，承担“答案是什么”的权威性；
2. **provider routing layer**：由具体 provider、输出、失败和事件时钟构成的可审计任务/回放层，承担“如何选择工具”的实验性。

第二层可以是本项目新建协议，但不能把它直接叫成已有社区 benchmark，也不能用自建回放层冒充公共 truth。论文必须明确每个结论依赖哪一层。

## 2. 来源层级

| 层级 | 含义 | 用法 |
|---|---|---|
| O | AAMAS 官方对 originality、significance、soundness、reproducibility、clarity、relevance、presentation 和 state of the art 的要求 | 决定 benchmark 必须能支撑准确、相关、公平和可复现的比较 |
| P | 近年 AAMAS 论文用与技术困难匹配的场景、canonical/modern 对照、消融和边界分析，而不是堆数据集 | 决定 baseline 角色和实验矩阵 |
| C | 本项目的 TDC truth、provider lineage、selected-only feedback、delay、family correlation、confirmation freeze 和恢复日志 | 防止自建任务过度承担结论 |

AAMAS 官方没有固定 benchmark 数量、baseline 数量或录用分数。本文件的阈值是项目内部 go/no-go 加严线。

本文件是内部审查标准。论文正文应正面说明公共 truth、routing protocol、比较合同和证据边界，不直接复制内部防守标签。

## 3. 硬性淘汰条件

逐行填写 PASS、FAIL 或 UNKNOWN。只有 FAIL 触发直接不通过；UNKNOWN 先补证据，不能按通过处理。Stage A 与 Stage B 的确认状态分开记录。

| 编号 | PASS 条件 | FAIL 触发条件 | 证据 |
|---|---|---|---|
| H1 | truth 来源、版本、标签语义和 hash 固定 | 运行时下载未固定版本或来源无法追溯 | truth manifest |
| H2 | duplicate、canonical 冲突、别名和 unknown 规则公开且不静默改标签 | 按第一行、多数票或模型表现合并冲突 | label audit |
| H3 | split、confirmation 与 provider training exposure 分开并经过审计 | development/test 或 provider 训练成员重叠未披露 | split/lineage audit |
| H4 | provider 源码、commit、checkpoint、环境和失败语义可追溯 | 只写工具名，无法复现输出或失败 | provider manifest |
| H5 | 选择前不读取真值、当前未选输出或完整 panel | full-panel 排序后冒充 online | event checker |
| H6 | 所有方法共用输入、菜单、selected-only 反馈、delay、预算和初始化权限 | Proposed 拥有额外标签、warm-start 或计算预算 | fairness matrix |
| H7 | 有最强合法近邻、matched control、固定策略和 privileged Oracle，角色边界清楚 | 只和 Random 或过时方法比较 | baseline role matrix |
| H8 | confirmation 在方法、菜单、epoch、阈值和 baseline 冻结后一次性只读 | 看 test 后调参，或 confirmation 标签回流训练 | freeze receipt |
| H9 | 主指标、分母、failure/unknown/delay 统计和不确定性在开跑前冻结 | 结果出来后换指标、删失败或改分母 | preregistration |
| H10 | raw JSONL、config、source/input/checkpoint hash 和 recovery verifier 能独立重算 | 只有最终均值或手工表格 | recovery receipt |

## 4. 评分方法

### 4.1 统一锚点

B1–B15 每项取 $q_i\in\{0,1,2,3,4\}$，总分满分 60。每项必须填写 score、evidence_path、hash 或日志行号、缺口和修复动作。

- **4 分**：预登记合同、独立脚本和原始事件都可复核；有压力或负例，且主张范围明确。
- **3 分**：合同和证据完整，可独立重算；只有不改变结论的局部限制。
- **2 分**：可以运行或开发复现，但依赖作者自报、审计不完整或缺关键边界。
- **1 分**：只有描述、URL 或最终均值，无法证明公平和独立性。
- **0 分**：缺失、矛盾、发生泄漏，或原始证据推翻该项。

### 4.2 通过条件

同时满足以下条件才算“benchmark/baseline 通过”：

1. H1–H10 全部 PASS；
2. B1–B8 每项至少 3 分；
3. B9–B15 每项至少 2 分且不得为 0；
4. 总分至少 49/60；
5. Stage A 只要求 E1–E5 有预登记配置、原始日志和结果报告，并输出 ELIGIBLE_FOR_CONFIRMATION 或 BLOCKED；
6. Stage B 才执行 E6 confirmation。完成后才允许把 B14/B15 的最终分数用于 CONFIRMED 状态；任何阶段都不能让 confirmation 标签回流。

总分 49/60 以上只表示“达到内部投稿审查线”，不表示 AAMAS 会录用。任何 H 失败时，分数不能抵消。

## 5. confirmation 阶段

Stage A 是冻结前资格，Stage B 是冻结后确认；评分表在下一节。

## 6. 逐项评分表

| ID | 0 分 | 1 分 | 2 分 | 3 分 | 4 分 | 证据要求 |
|---|---|---|---|---|---|---|
| **B1 truth 权威性 / 4** | 自造 truth 或无来源 | 只有 URL | 有冻结源/hash，无独立审计 | primary/public source、版本/hash、下载清单、重跑脚本和独立核对齐全 | 另有 source sensitivity、primary provenance 和冲突报告，明确 routing layer 不是公共 benchmark | truth manifest、source hash、audit report |
| **B2 标签语义 / 4** | endpoint 或标签含义错误 | 只有 0/1 名称 | endpoint/threshold 写了，unknown、timeout、conflict 缺失 | endpoint、阈值、duplicate、unknown、failure 和冲突规则均固定，重新生成一致 | 逐字节复现标签生成，并报告 TDC 与 B3DB/PharmaBench 差异，不静默合并 | label contract、regeneration receipt |
| **B3 split 和独立性 / 4** | 有 overlap 或泄漏 | 只报随机 split | molecule split 有了，scaffold 或 provider exposure 不明 | 预登记 molecule/scaffold split、development/confirmation 隔离和 provider member risk 分层 | 独立审计确认集，包含旧 panel 与 test 的 overlap 清理收据 | split audit、overlap report |
| **B4 provider lineage / 4** | 只有工具名 | 有 repo 或论文 | commit/checkpoint/env/failure 齐全但训练成员未知 | lineage-aware/model-fresh 分层，输出和失败可重现 | 模型 hash、训练来源、随机性、许可证、独立 rerun 或 deterministic receipt 齐全；未证明独立时收窄 claim | provider manifest、checkpoint audit |
| **B5 任务匹配 / 4** | benchmark 测的不是论文主张 | 只定义 property prediction | 有 molecule/provider/action/reward，无终止、delay 或 lifecycle | 完整 selector contract：输入、menu、action、selected feedback、delay、budget、failure/unknown | 机器校验 contract、反例测试，并能对应每条 claim | task contract、schema validator |
| **B6 信息与因果公平 / 4** | pre-choice 读取 truth/full panel/未选输出 | 只写“不泄漏” | selected-only 意图存在，无事件证据 | 每条 run 有 visible history/release time，validator 抽查通过 | independent replay checker 能拒绝非法 trace，并证明 confirmation label 未进任何 state | event log、leakage checker |
| **B7 动态事件覆盖 / 4** | 静态 full-panel 结果冒充 online | 只有单一无延迟回放 | 记录 delay/failure，但 declared dynamic_scope 与压力覆盖不完整 | feedback-adaptive 至少有预登记 d∈{0,1,4}、pending queue、selected-only release；environment-nonstationary 另有 arrival/retire/version/shift | 在对应 scope 下有 recovery/forgetting、负结果和 transfer/held-out 边界，或明确缩小 claim | lifecycle trace、delay matrix |
| **B8 资源和初始化公平 / 4** | 方法拥有额外标签、预算或算力 | 只写 same setting | menu/feedback 相同，warm-start、feature、compute 不清 | 逐项固定 input/menu/feature/feedback/delay/budget/seed/compute，并分 cold/common-warm/online | independent fairness checker 通过；Oracle/privileged control 明确不进可部署排名 | baseline matrix、fairness receipt |
| **B9 baseline 角色完整 / 4** | 只有 Random 或过时方法 | 只有一个现代 baseline | 有 Random/Fixed/Oracle，缺最近邻或 matched control | 有 canonical、strong recent nearest、matched Frozen/Online/Replay、Best Fixed 和 Oracle，各自回答不同问题 | 另有 direct-property、verification control、family sensitivity；不为堆数量添加无关 baseline | roster、role matrix |
| **B10 baseline fidelity / 4** | baseline 被改坏或不可运行 | 只写方法名 | 能运行但版本、适配差异或 license 不清 | 固定论文版本、源码 commit、适配差异、license、超参和 tuning budget，统一接口 | 数值/单元测试对齐官方实现或论文例子，独立 reviewer 可重跑并报告限制 | source audit、adapter tests |
| **B11 provider family correlation / 4** | 33 provider pooled 当独立证据 | 只口头说相关 | 有 family 标签，无敏感性 | 报 family-macro、all-provider、exposure/lineage 分层和敏感性 | 按 family/seed 重跑并给 paired uncertainty，不对单条 online 日志行 bootstrap | family report、paired analysis |
| **B12 metrics 和 utility / 4** | 只有 final accuracy | 只有 accuracy 和一张均值表 | accuracy/regret 有了，无成本或 failure accounting | 预登记 future correctness、累计 error/regret、calls、latency p50/p95、update cost/memory、failure/unknown | 多目标或非劣准则冻结，按 endpoint/family/time-slice 分解，并报告 verifier/abstain 代价（若主张涉及） | metric spec、results JSON |
| **B13 statistics / 4** | 无 seed、方差或分母 | 单 seed/单均值 | 多 seed，无 paired CI/effect | 预登记 seed、paired episode、95% CI/variance/effect size、failure accounting，并区分策略随机性和 scaffold 泛化 | independent raw-log recovery、null/negative settings 全报告，不看结果选指标或 seed | stats script、CI report |
| **B14 experiment coverage / 4** | 只有一张 static table | 只有主比较 | 有主比较但缺机制、边界或成本 | E1–E5 完成并映射 RQ：任务有效性、主比较、机制消融、压力、错误/成本 | 加入冻结 confirmation E6 或明确缩小 claim，并保留失败和停止规则 | experiment matrix、preregistration |
| **B15 reproducibility 和 confirmation / 4** | 无 raw logs 或测试调参 | 只有 summary | raw JSONL 存在，无 recovery | config、命令、环境、GPU、hash、逐事件日志、checkpoint、recovery script 齐全；若进入 Stage B，再增加独立 confirmation receipt | 外部机器或独立脚本重算一致，artifact 可审计，负结果保留；Stage B 的确认只读且不回流 | raw logs、recovery receipt、freeze record |

## 7. 最小实验包

| 实验 | 必答问题 | 必须有的对照和证据 |
|---|---|---|
| E1 任务有效性 | provider 是否真的有 context-dependent 差异？ | Random、fit-only Best Fixed、evaluator-only Oracle；区分 static panel 与 online action |
| E2 主比较 | 方法是否改善未来 selected correctness 或任务 utility？ | strongest legal nearest、matched Frozen/Online/Replay；同一 stream、同一 feedback clock |
| E3 机制消融 | 收益来自新增机制还是 backbone、预算或 full-panel 权限？ | 去掉每个新增状态/更新；保存反事实配置和原始事件 |
| E4 动态与压力 | 在 declared dynamic_scope 下，delay、failure、menu、family、lifecycle 或分布变化时是否仍成立？ | feedback-adaptive 至少覆盖 d∈{0,1,4} 和 pending/failure；environment-nonstationary 才要求 arrival/retire/version/shift；报告负结果 |
| E5 错误、成本和恢复 | 方法什么时候错、要付出什么成本、能否恢复？ | failure/unknown/refusal、calls、p50/p95、memory、crash/replay receipt |
| E6 冻结确认 | 开发结论能否迁移到未读数据？ | 方法、菜单、阈值、epoch、baseline 冻结后一次性只读 confirmation；标签不回流 |

full-panel 只允许用于 fit-only warm start、evaluator-only Oracle、离线审计或 provider 资格审计。任何 full-panel static score 必须与 online cumulative score 分表，不能改名为实时效果。

## 8. 双人评分和复核规则

只对当前生效 benchmark/baseline 文档和标明版本的实验合同评分。benchmark/data auditor 与 paper writer 独立填写：

ID | 维度 | 作者分 | 复核分 | 生效分=min | 证据路径 | 缺口/修复动作

没有文件、hash、日志行号或可重跑脚本的项目不能给 3 或 4 分。两人分差达到 2 分时先补证据再重评；发现泄漏、未知训练成员、confirmation 回流或恢复失败时直接触发对应 H。Oracle、privileged full-panel 和不可部署控制不能被排在可部署方法中。

## 9. 评分结果的动作

- H1–H5 触发：冻结所有效果结论，先重建 truth、label、split 和 provider 资格；
- H6–H10 触发：停止比较数字，先重跑公平合同、冻结流程和恢复链；
- B1–B8 任一低于 3：只能报告 development/stress，不得写主结果；
- B9–B15 为 0：停止对应 claim，补 baseline、统计、错误或恢复证据；
- 总分 49/60 以下：不进入 confirmation；
- 总分达到 49/60 且所有门槛通过：可进入故事线、方法和 benchmark 联合复核，不等于录用预测。

当前 33-provider 菜单的独立 model-fresh 资格尚未全部证明时，主张必须限定为“固定已部署菜单上的在线选择”。不得把 family 相关的重复变体当成独立模型数量，也不得把可运行 adapter 的存在写成 baseline 已完成效果。
