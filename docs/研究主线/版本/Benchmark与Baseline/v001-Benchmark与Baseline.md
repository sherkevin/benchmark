# Benchmark 与 Baseline

- 版本：v001
- 状态：已归档（当前生效版本的历史副本）
- 生效日期：2026-09-28
- 文档职责：唯一记录 benchmark 合同、baseline 集合和实验矩阵；详细运行结果留在 task 报告和 experiments 日志。

## Benchmark 由两层组成

### 三种状态要分开

- **已确认约束**：公共 truth、provider 输出和 routing 评估分层；选择前不能读未选输出；每个实验保存可恢复的逐事件日志。
- **当前候选**：TDC BBB/AMES 作为 truth 锚点，Task36 作为 development replay，33-provider 主菜单和 family-balanced 敏感性分析。
- **未决项**：BBB 冲突组处理、provider checkpoint 重新拟合范围、hERG 是否进入主结果、最终 confirmation 样本。

主评测不是把现有 33 个 provider 结果表直接叫作社区 benchmark，而是由“公共 truth layer + 可审计 provider routing layer”组成。

### Truth layer：公共数据和固定切分

首选 TDC ADMET 的 BBB 和 AMES。已冻结的官方 Group archive SHA-256 是 `bd0005246cb6f1672333c28ad202112a8bf071ab65d847848715030308faf1f1`。审计得到：BBB `train_val/test` 为 1,624/406 行、1,581/394 unique；AMES 为 5,821/1,457 行、5,802/1,453 unique。两端点的官方切分 canonical 和非手性 Bemis--Murcko scaffold 均不交叉。

BBB 仍有 10 个 canonical 冲突组（`train_val` 8、`test` 2），同一 canonical 同时出现 0 和 1。没有找到足够的原始实验出处前，不能按第一行、多数票或模型表现静默合并。当前候选是排除冲突组并记录分母；这是待 Gate 0 确认的协议，不把它写成已冻结结果。

旧 722 条 development panel 与官方 BBB test 有 147 条严格 canonical、235 条 scaffold 重叠。因而旧开发结果不能直接使用官方 test 作 untouched confirmation。方法冻结后，confirmation 至少使用 molecule-disjoint 候选；若声称结构独立，使用审计剩余的 scaffold-disjoint 候选并重新生成 provider outputs。

### Provider layer：具体工具菜单

每个 provider 记录源码 commit、checkpoint hash、端点、阈值、训练来源、运行环境、失败语义、原始输出和生成时间。Task36 的 730×33 BBB 缓存用于 development replay 和压力测试；它不是公认社区 benchmark，也不把 24 个 B3clf 变体当成 24 个独立模型家族。

provider 训练成员审计目前把资格分成 lineage-aware 等级：GraphB3 为 G3，ADMET-AI/DeepPurpose/LGBM-BBB/24 个 B3clf 为 G2，FP-ADMET/ADMETlab 为 G1。正式 model-fresh 结果前，要么在公开 train split 重新拟合并绑定 checkpoint，要么把 lineage 限制写入结论。

## 统一任务合同

每题输入是 $x_t,e_t,A_t$ 和已释放历史 $H_t$。选择前不能读取当前真值、任何未选 provider 输出或当前题目的完整输出面板；调用结束只释放被选 provider 的状态、预测和 correctness；反馈按 delay 释放。失败、缺失和未知真值单独记账，不把 unknown 自动改成 0 或 1。

每次 run 保存逐事件 JSONL：输入 hash、菜单、动作、可见历史、释放时刻、provider 输出、状态、reward、更新耗时和错误。方法冻结后才打开 confirmation；confirmation 的标签不能用于选 epoch、调阈值或修改菜单。

## Baseline 集合

| 角色 | baseline | 作用 |
|---|---|---|
| 下界 | Random provider | 检查任务是否比盲选有信号 |
| 强固定 | fit-only Best Fixed | 只用训练区完整标签选一个固定 provider |
| 上界 | evaluator-only Oracle | 每题看完整面板后的不可部署上界 |
| 静态路由 | Frozen static selector | 检查表示和静态映射能力 |
| 匹配在线 | Online SGD、Replay SGD | 分离普通持续学习和回放的贡献 |
| 现代在线 | Greedy LinUCB、PILOT-adapted、BayesianRouter-adapted | 代表近期在线/先验 router 思路 |
| 现代静态 | GraphRouter（实现可审计后） | 检验图结构静态路由；不得用 confirmation 选 epoch |
| 验证控制 | 固定 confidence threshold、随机同成本 escalation | 检查调用后 verifier 是否真的有用 |
| 任务控制 | direct-property predictor | 检查收益是否只是恢复端点真值，而非学习 provider 差异 |

每个 baseline 都固定论文版本、源码 commit、许可证、适配差异、初始化权限、特征、反馈延迟、预算和随机种子。没有可审计实现的项目只能作为论文行为审计，不能写成已运行 baseline。

## 实验矩阵

| 阶段 | 要回答的问题 | 关键对照 | 通过条件/产物 |
|---|---|---|---|
| Gate 0 | truth、重复、冲突、训练成员和 confirmation 是否合格 | 独立脚本与清单 | 资格报告；不过不开放 confirmation |
| 静态开发 | 分子表示是否能超过固定赢家 | Best Fixed、Frozen、Oracle | OOF accuracy、fold 明细、失败计数 |
| 在线开发 | 延迟 selected-only 反馈能否改善后续选择 | SGD、Replay、LinUCB、PILOT、BayesianRouter | delay 0/1/4，累计错误、反馈数、时间、内存 |
| 菜单/家族 | 结果是否被相关 provider 重复放大 | 33-provider 与 family-balanced 菜单 | family macro、provider exposure、敏感性报告 |
| 调用后验证 | confidence 是否能在相同成本下减少错误 | fixed threshold、random matched、always/never | accuracy、平均调用数、fallback rate、风险覆盖 |
| 独立确认 | 开发结论能否迁移到未读数据 | 冻结方法和一次性 confirmation | 只报告预登记指标；不回流任何标签 |

主指标在开跑前固定为未来 episode correctness；同时报告累计 regret、相对 Best Fixed 的差值、延迟、更新 p50/p95、峰值内存、调用次数和失败率。按 endpoint 和 provider family 分开报告，不能只报 pooled accuracy。

## 当前实验边界

已经恢复的 fingerprint diagnostic、online adapter smoke 和 confidence escalation 是开发证据，分别回答“任务可学性”“实现时序”和“调用后信号”。它们不能替代最终 confirmation，也没有证明一个新的动态 JEV 已经成立。

在 Gate 0、provider checkpoint 绑定和方法冻结前，不追加新的 A800 训练。每个新实验先写 claim、最近邻 comparator、预算和 falsification condition，再写 config、原始 JSONL、恢复脚本和报告。

## 依据和结果位置

- truth 与切分：`research/benchmark_authority_audit_20260928/report.md`、`research/benchmark_authority_audit_20260928/tdc_conflict_semantics_note.md`。
- provider lineage：`research/provider_training_membership_audit_20260929.md`。
- 合同候选：`task/task49-pharma-benchmark-baseline-freeze/public_benchmark_contract_candidate_v1.md`。
- baseline 适配：`task/task49-pharma-benchmark-baseline-freeze/baseline_adapters_v0.md`。
- 原始实验日志：`experiments/logs/task49_*`。
