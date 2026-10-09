# ADR-0021：医药自建 benchmark 作为直接工具选择主任务

日期：2026-09-26

状态：Partially superseded by ADR-0004；本 ADR 的 episode、反馈、最终多端点和 baseline 约束仍生效

## 状态说明

ADR-0004 已把 benchmark 的 truth source 和 routing layer 收敛为公共 TDC truth + Task36 provider routing protocol，取代本 ADR 第 1 条中“直接使用已有医药结果库作为主 benchmark”的来源表述。
本 ADR 第 2--7 条以及“最终证据至少覆盖 BBB、hERG、AMES”的要求继续生效；BBB 是当前开发端点，hERG/AMES 需通过各自资格审计后进入最终 evaluation 或 confirmation。

## 背景

ADR-0014 将 PTRB-v0 定为医药主 benchmark 草案，ADR-0015 随后又把 RouterBench 定为方法论文主 panel。Owner 后续明确收窄论文：不再把长任务、追加复核或通用公开 routing benchmark 作为主问题，而是直接用 JEV 类判别模型选择医药预测工具，并研究它如何根据真实 0/1 正确性反馈高效实时更新。

现有 BBB 资产有 730 个分子、722 个可评分真值、7 个来源家族和 33 个具体输出。它提供真实工具结果和清楚反馈，但 33 个输出不是独立样本，现有 BBB 分子也已被反复用于开发，不能作为未触碰最终确认集。

## 决定

1. 主 benchmark 使用本项目已有医药结果库构造，不再以 RouterBench、LLMRouterBench 或 ASlib 为主任务。ADR-0015 被本 ADR 取代。
2. 主 episode 是单次直接选择：输入为分子、性质、候选 provider 及合法历史；动作是选择一个 provider；终止后释放该选择是否正确的二元反馈。复核、拒答和主 Agent 长轨迹保留为后续扩展，不进入当前主 benchmark 定义。
3. 统计单位是独立分子或预先定义的 scaffold group。多个 provider 输出是同一任务上的候选潜在结果，不增加独立样本量。
4. 当前 BBB 722 分子标记为 development benchmark。最终主证据需要至少 BBB、hERG、AMES 三个带独立实验真值的端点，以及一个方法和 baseline 冻结后才打开的确认集。若其他端点真值未完成，不得用工具共识代替。
5. 外部公开 routing benchmark 只做附录 sanity、接口迁移或跨域补充，不与医药结果合并成总榜。
6. baseline 必须覆盖：训练折 Best Fixed、Oracle 上界、AAAI 2026 Greedy LinUCB、PILOT、BayesianRouter、至少一个静态强 router（RouterDC 或 GraphRouter），以及 Frozen/Online-SGD/Replay-SGD/Proposed 的 matched-backbone 对照。
7. 所有方法共享初始数据、到达顺序、反馈延迟、反馈预算、候选池和调参预算；必须先决策后更新，当前 episode 的未选输出和真值永远不可见。

## 理由

医药自建 benchmark 与问题、数据和目标反馈完全一致，能直接检验实时 JEV，而通用 LLM routing panel 会把研究重新变成文本模型路由。自建 benchmark 的风险不在“不是公开名字”，而在独立样本、真值、泄漏、确认集和强 baseline 是否严格。通过 development、多端点 evaluation 和 frozen confirmation 三层分工，可以保留已有资产，同时避免用反复查看过的 BBB 结果做最终确认。

PILOT 和 BayesianRouter 已覆盖“离线判别先验 + 在线部分反馈”，AAAI 2026 工作覆盖无离线训练的现代 LinUCB。它们是判断动态 JEV 是否有新增量的必要对手。RouterDC/GraphRouter 用于判断在线更新相对强静态 router 是否确有价值；matched-backbone 对照用于排除增益只来自 backbone 或工程预算。

## 影响

- ADR-0015 状态改为 `Superseded by 0021`；ADR-0014 的“医药数据为主、外部 benchmark 分工”继续有效，但当前任务从复核/拒答协议收窄为单次直接选择。
- 现有 BBB 可以继续做开发与消融，不能称为独立最终测试。
- hERG/AMES 真值连接、provider 训练重叠审计和池外确认集成为 benchmark 冻结的前置工作。
- 当前 exploration-only verification-gain 实验属于旧复核支线，不能作为本主任务的方法效果。

## 证据

- [Task49 冻结审查](../../task/task49-pharma-benchmark-baseline-freeze/report.md)
- `data/tool_results/divergence/t1_bbb_characterization.json`
- `data/tool_results/divergence/pharma_tool_granularity_audit_v2.json`
- `task/task36-pharma-provider-replay/qualification_notes.json`
