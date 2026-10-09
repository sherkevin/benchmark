# ADR-0027：AAMAS 2027 长期目标与五道研究闸门

- 状态：Accepted
- 日期：2026-09-27
- 依据：ADR-0022、当前项目持续执行授权

## 目标

在 AAMAS 2027 截止前，完成一套可复现、可审查的医药 direct-selection benchmark 和动态 JEV 方法。目标是形成真实实验支撑的论文与复现材料；不把投稿接收当成可以保证的结果。

## 五道闸门

1. **数据资格**：医药端点、分子身份、工具输出、独立真值、失败和缺失都能追溯；BBB development 与最终 confirmation 分开。
2. **方法与信息隔离**：输入、selected-only 反馈、延迟队列、未选输出边界和训练权限写成合同；baseline 与候选方法公平共享预算。
3. **真实远程运行**：Nebula A800 任务一次只跑一条，代码、输入、配置和 OSS 证据可复现。
4. **独立恢复**：从全量脱敏日志恢复事件、权重或状态、逐样本反馈和指标；恢复失败时不报结果。
5. **冻结后确认**：方法、backbone、baseline、预算和主要指标冻结后，才打开 confirmation；confirmation 不回流调参。

## 工作方式

每完成一个小任务，都检查故事线、方法、benchmark、baseline 和证据是否仍然一致。负结果保留；如果近邻方法已经解释现象，就收窄主张或停止该机制，不靠继续调参维持故事。

## 当前落点

匹配的 Frozen/Online-SGD/Replay-SGD 已完成完整 development 对照；普通 selected-only 更新虽改善 Frozen warm start，仍低于 Best Fixed。provider-transfer、BayesianRouter-style 和校准后的 context memory 候选也已按预登记闸门停止，confirmation 仍关闭。Task36 的 round1/round2 已审查为重复性/执行漂移证据，动态主线暂以 delayed selected-only outcome feedback 为来源，不把工具版本时间漂移写进主张。当前下一小目标是提出一个统计语义自洽的新更新机制，先用已有真实 development evidence 做 CPU 级可证伪回放，再决定是否占用一条 A800 做端到端 smoke；不为重复旧结果开远程任务。
