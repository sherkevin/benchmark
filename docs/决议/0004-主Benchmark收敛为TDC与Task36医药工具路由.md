# 0004 主 Benchmark 收敛为 TDC truth + Task36 医药工具路由

- 状态：生效
- 确认日期：2026-09-29
- 相关主文档：[Benchmark 与 Baseline v002](../研究主线/生效/03-Benchmark与Baseline.md)
- supersedes：ADR-0021 第 1 条中的 benchmark 来源表述；保留 ADR-0021 第 2--7 条及 BBB/hERG/AMES 最终多端点要求

## 背景

项目早期的 benchmark 方案同时保留了 LLMRouterBench、ASlib SAT12-ALL 和 BBB。这个组合可以覆盖一般模型路由、经典算法选择和医药工具，但三者的输入、反馈、工具版本和动态更新合同不同。继续把它们并列为 main，会让论文同时讲三个问题，实验矩阵也无法用同一信息预算解释。

当前研究要回答的是一个更具体的问题：给定分子、性质和候选医药 provider，方法在选择前看不到未选工具结果，只能在调用后收到 selected-only、可能延迟的 correctness feedback，并据此更新后续选择。

## 决定

1. 本项目唯一的主 benchmark 收敛为：**公共 TDC truth layer + Task36 provider routing protocol**。
2. 主开发端点为 TDC BBB；Task36 的 33-provider 菜单是主 routing panel。AMES 和 hERG 只有在端点语义、覆盖、冲突和 provider lineage 审计通过后，才能加入最终的多端点 evaluation 或 confirmation；它们不自动成为并列的独立 benchmark。
3. TDC truth 负责标签语义、版本、切分和可追溯性；Task36 routing protocol 负责 provider 输出、selected-only 反馈、延迟、失败和事件日志。论文必须明确这两层的职责，不能把 Task36 回放层称为已有社区 benchmark。
4. LLMRouterBench 和 ASlib SAT12-ALL 从主 benchmark 中移出，保留为历史候选。若未来需要证明跨任务迁移，只能另设 external transfer/appendix 实验，不能和医药主结果混成一张主表。

## 理由

- 任务、反馈和方法创新终于落在同一个问题上，减少 benchmark、baseline 和实验矩阵之间的口径漂移。
- TDC 提供公共 truth 的权威来源，Task36 保留真实 provider 输出和可恢复的在线反馈过程，既不把自建协议冒充社区 benchmark，也不丢掉本项目最有价值的工具调用场景。
- 主结果可以用同一 selected-only、delay、failure、menu、family 和预算合同比较 Frozen、在线更新和现代 router baseline。
- 代价是不能直接声称一般算法选择或跨领域泛化；这些结论必须另做外部 transfer 实验，或者明确不主张。

## 不改变的边界

- 该决议不认定当前 benchmark 已经完成最终资格审查。BBB canonical 冲突、旧开发 panel 与官方 test 的 overlap、provider training membership 和 confirmation 样本仍需按 Gate A 修复。
- 该决议不取消 ADR-0021 对最终多端点证据的要求；它只明确 BBB 是当前开发主端点，AMES/hERG 需要先完成资格审计再进入最终 evaluation。
- 该决议不认定任何动态 JEV 方法已经有效。已有 CPU 负结果和 controlled version event 仍按各自报告的范围解释。
- 该决议不把 33 个 provider 当作 33 个独立训练来源；family 相关性和 lineage 必须分层报告。

## 后果

- 后续 benchmark、baseline 和实验矩阵文档以这一口径为准；旧 benchmark 选择报告标记为历史，不再作为当前执行依据。
- 下一步顺序是先完成 Gate A 的 truth/overlap/lineage 收口，再完成统一 baseline runner，最后才进入 confirmation。
