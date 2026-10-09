# Task46 报告：动态 Jev 开源复用与集成路线

日期：2026-09-24

## 结论

当前最小可行组合是：

```text
Laya（冻结冷启动/静态基线）
        + AnyJev（typed head 与 observe 周期重拟合骨架）
        + bayesianbandits（selected-arm 在线后验主实现候选）
        + contextualbandits / MABWiser（LinUCB/LinTS 强基线）
        + Task36 replay → tau2-bench 自定义 pharma domain（交互环境）
        + Inspect AI / jev-benchmarks / OBP（运行、指标、OPE 与报告）
```

这条组合把新增代码限制在 provider adapter、延迟反馈队列、typed feedback head、轨迹日志和医药 evaluator。P1 先用 bayesianbandits 做动态 selected-arm 后验；River 保留为可替换实现和稳健性基线。AnyJev 的 `observe(question, state, label)` 已具备收到标签后在 30、60、120… 个样本重新拟合 L2 head 的在线骨架；它按题目保存全部 observations，默认不是 selected-only、多 provider reward 或延迟真值系统。Laya 当前适合冻结 baseline/候选表示，不能直接称为实时学习实现。

## 调研证据

### 动态 Jev/Jev 工程

- AnyJev：Apache-2.0；`Decider` 有 `observe`、`fit_head`、artifact 导出、L0/L1/L2；L2 是每题 closed-form head，需要本地 hidden states。源码和 README 已保存于 `reference/anyjev/`。
- Laya：Apache-2.0；已有 typed decision 输出，但没有 partial-fit/observe 在线反馈接口。既有源码快照见 `task/task45-dynamic-decision-model/reference/laya-8465935/`。
- jev-benchmarks：Apache-2.0；提供概率向量校验、Brier/NLL/ECE、coverage-risk、p50/p95 latency、bootstrap CI、固定 revision 和 JSONL 工件结构。
- jev-ultrafast：MIT；浏览器领域实现了候选约束、执行前 freshness/fingerprint guard、执行后重新观察和 DONE 后独立验证。只借状态机模式，不借 hosted web-agent 结果。

### 在线学习和 bandit

- bayesianbandits：MIT；`pull/update`、Thompson/UCB、动态 arm、decay 等接口适合包成 selected-arm 后验。
- contextualbandits：BSD-2-Clause；含 LinUCB、Linear/Logistic Thompson、partial_fit、DR/OPE，README 明确 contextual bandit 只观察 chosen reward。
- MABWiser：Apache-2.0；scikit-learn 风格 `fit/partial_fit/predict`，适合轻量基线；仓库较久未更新，需锁 commit。
- River：BSD-3-Clause；`river.bandit` 提供 LinUCB、Thompson、BayesUCB 等流式实现，`learn_one` 与 offline replay 可复用；延迟 feedback 仍需外层 queue。
- Vowpal Wabbit：LICENSE 文本为 BSD 风格条款，GitHub SPDX 显示 NOASSERTION；ADF/propensity 能处理动态候选，但先过人工许可证审查再纳入可分发依赖。
- OBP：Apache-2.0；可复用 replay、IPW/DR/MRDR 和时序 OPE，不替代真实工具真值。

### 交互 benchmark 和运行器

- tau2-bench：MIT；可注册 domain、Tool/ToolKit、任务、用户模拟器、Gym 环境和完整 trajectory。默认 customer-service DB reward 不等于医药 provider correctness，需要自定义 evaluator。
- Inspect AI：MIT；提供多轮 tool usage、custom solver/scorer、sandbox 和日志，适合作为统一 runner。
- AgentDojo：MIT；可作为恶意/错误工具输入的鲁棒性切片，不作为主医药 benchmark。
- ToolSandbox：GitHub license metadata 为 Other/Apple 自定义授权；只借 milestone DAG 与 insufficient-information 评分思想，不复制代码/数据。

## Benchmark 与 baseline 决策

PTRB-v0 是本项目拟构建的领域 benchmark，不是已有公开标准。它复用 Task36 的真实 pharma replay 和 BBB 结果，再由本项目定义 episode、信息条件、T0–T3、数据切分、延迟反馈和评分。将来需要多轮交互时，把同一合同适配到 tau2 domain。B0–B8 详见 `docs/开源复用与集成路线_动态Jev.md`，主表包含 random、fixed best、static Laya/AnyJev、full-information upper bound、LinUCB、LinTS、AnyJev observe、dynamic Jev + posterior、base Agent 三种输入。公开发布和独立复核完成前，不把 PTRB-v0 称为社区标准。

所有策略共享候选菜单、预算、feature 版本和日志字段。未调用工具不生成负标签；OPE 使用真实 propensity 并报告支持范围、ESS 和方差。AnyJev/jev-benchmarks 的 teacher agreement 只能做接口 sanity check，不能替代药理真值。

## 未完成

1. 把 provider adapter、pending feedback、trace 和 evaluator 写成最小代码接口及合同测试。
2. 在 Task38 资格放行后冻结真实 split、执行参数和第一批实验。
3. 验证 AnyJev head 与 Laya/医药 state 的接口兼容性；若不兼容，保留 AnyJev 为 baseline，动态 head 仍以 bayesianbandits 为主实现，River 为替换基线。
4. 逐项审核 Vowpal Wabbit、ToolSandbox、openJev-verdict 权重和依赖的再分发条件。

## 复现材料

- 候选仓库 metadata/README/LICENSE：`reference/`
- 候选来源清单：`sources.jsonl`
- 组件核验摘要：`verification.json`
- 总体集成方案：[docs/开源复用与集成路线_动态Jev.md](../../docs/开源复用与集成路线_动态Jev.md)
