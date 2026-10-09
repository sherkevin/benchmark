# ADR-0013：按功能拼装开源组件，拒绝从零重做

日期：2026-09-24

状态：Accepted（执行策略）；具体依赖版本、最终实验基线和论文贡献仍需通过合同测试与真实验证。

## 背景

当前主线不是再造一个通用 Agent 框架，而是验证一条可靠工具调用闭环：主 Agent 面对多个具体工具版本，能选择、读取、复核、验证，并在证据不足时拒答；外挂模块需要在真实反馈到达后实时适应。完整系统同时涉及 typed decision、在线后验、延迟反馈、交互环境、轨迹记录和概率评估。全部从零实现会把工程风险和论文风险混在一起，也无法说明每个模块相对已有工作的增量。

Owner 已明确采用“先写清功能，再为每个功能找成熟开源实现，最后只写必要适配层”的路线。开源代码只能作为可复核的工程材料，不能把其自报结果或论文主张直接当成医药工具真值。

## 决定

1. **先冻结功能合同和 benchmark，再选依赖。** 每个组件必须对应一个明确接口、一个验收测试和一个替代方案；没有接口的“明星项目”不进入主线。
2. **动态 Jev 采用组合式冷启动。** Laya 保留为静态 typed-decision/冷启动基线；AnyJev 作为在线闭式 head 的候选骨架。两者都不直接等同于本项目最终方法，provider/action-set/selected-only 反馈需要适配。
3. **在线选择采用成熟 bandit 库。** P1 主实现候选固定为 `bayesianbandits`（MIT），用 `contextualbandits`（BSD-2-Clause）和 River（BSD-3-Clause）做透明交叉基线；MABWiser（Apache-2.0）做轻量基线；Vowpal Wabbit 的 ADF/propensity 能力只在许可证和接口验收后加入。
4. **交互环境和评估复用现成骨架。** 用现有 Task36 回放先做最小可复现实验；需要多轮 Agent 环境时适配 `tau2-bench`（MIT）自定义 pharma domain，用 `Inspect AI`（MIT）承载 runner、scorer 和日志；用 `jev-benchmarks`（Apache-2.0）与 OBP（Apache-2.0）复用概率指标、bootstrap/OPE 和结果工件结构。
5. **可信调用控制借接口，不照搬领域实现。** `jev-ultrafast` 的候选约束、freshness/fingerprint guard、执行前后重新观察和 DONE 后独立验证可作为主 Agent 状态机参考；其 hosted web-agent 依赖和浏览器领域结果不作为医药证据。
6. **本项目只自研四类胶水。** 具体 provider/version adapter；延迟真值的 pending/feedback queue；Jev 建议与 Agent 实际动作的结构化日志；医药真值、拒答和验证收益的 evaluator。不得为了“完整”重写基础模型、通用 bandit 数学、通用 Agent runtime 或通用指标库。
7. **许可证和版本是进入主线的硬门槛。** 必须保存仓库 URL、许可证文件、固定 commit、依赖版本和修改说明。GitHub 显示 `NOASSERTION` 或自定义许可证的项目只能借鉴接口，不能在未审查前作为可分发依赖。

## 结果与代价

- 工程量集中在适配层和反馈合同，能把论文中的新增部分与已有组件清楚分开。
- 版本早期或依赖 hosted 服务的项目可能被降级为 baseline/reference；组合后仍需做端到端一致性测试。
- 借用开源库不减少科学验证要求：未调用工具不能补负标签，AnyJev 的 teacher agreement 不能当医药真值，tau² 的数据库终态奖励不能直接当工具正确性。
- 论文中必须逐项报告“借用什么、改了什么、哪些结果来自原项目、哪些是本项目实测”。

## 相关文件

- [动态Jev可信工具调用质量规则](../论文质量规则_动态Jev可信工具调用.md)
- [开源复用与集成路线](../开源复用与集成路线_动态Jev.md)
- [ADR-0012：三层闭环](0012-three-layer-dynamic-jev-agent-loop.md)
