# Task46：动态 Jev 开源复用与集成路线

Status: SPECIFIED（功能合同、benchmark/baseline 候选和开源组件审查已完成；尚未进入真实学习实验）

## 目标

把三层主线落成可执行的复用方案：

1. 固化“动态工具适应模块 + 实时学习动态 Jev + 主 Agent 可靠调用”的功能合同。
2. 固化主 benchmark、信息条件、数据切分、指标和 baseline 族。
3. 为每项功能找到许可证可核、接口可接的开源实现，并标出需要自研的最小胶水。
4. 保存官方仓库、许可证、固定 commit 和源码摘录，避免依赖未经核验的项目描述。

## 范围

- 包含：AnyJev、Laya、SemIf-OpenJev、jev-benchmarks、jevcal、bayesianbandits、contextualbandits、MABWiser、River、Vowpal Wabbit、OBP、tau2-bench、Inspect AI、jev-ultrafast、AgentDojo、ToolSandbox 等候选。
- 包含：PTRB-v0 主 benchmark 草案、T0–T3 任务、B0–B8 baseline、延迟反馈和日志合同。
- 不包含：训练基础模型、运行新的医药预测、申请/解除 Task38 资格闸门、远端提交或 Git 提交。

## 验收

- [x] 故事线和三层方法写入 `docs/开源复用与集成路线_动态Jev.md`。
- [x] 开源优先组合策略写入 ADR-0013 并加入 ADR 索引。
- [x] 主 benchmark 和 baseline 形成同一信息合同下的可比较矩阵。
- [x] 保存候选仓库 metadata、README、LICENSE 和固定分支信息。
- [ ] 生成统一 `ToolAdapter`、`PendingFeedback`、`Trace` 的代码接口和合同测试。
- [ ] 经过外部资格放行后，运行最小真实批次并完成动态 Jev/模块/Agent 三层结果。

## 当前边界

Task38 的 33 个具体模型/722 分子池仍有外部资格限制。当前交付是规格、开源审查和集成路线；没有把开源项目的自报 benchmark 结果写成医药效果，也没有运行训练或新预测。
