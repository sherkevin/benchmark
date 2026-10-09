# Benchmark 与 Baseline 评价标准

- 版本：v001；状态：已归档（当前生效版本的历史副本）；对应主文档：`../03-Benchmark与Baseline.md`

## Benchmark 必须通过的检查

1. truth 有公共来源、版本、哈希、标签语义和可复核切分。
2. duplicate、冲突标签、别名、scaffold overlap 和外部数据差异有明确处理；不能静默多数票。
3. provider 菜单有版本、checkpoint、训练来源、失败语义和 family 相关性。
4. 选择前的信息、反馈释放时序、调用预算和 confirmation 隔离有逐事件收据。
5. 自建 routing 层与公共 truth 层分开命名，不能把 Task36 直接说成社区 benchmark。

## Baseline 必须通过的检查

1. 每个 baseline 完成同一任务，使用同一菜单、输入、反馈权限、延迟和预算。
2. 至少有 Random、Best Fixed、Oracle、Frozen、matched online、现代 router 和验证控制。
3. 现代方法记录原论文、源码 commit、许可证和适配差异；不能用坏默认或无法审计代码制造对比。
4. provider family 相关性、训练成员和 lineage 限制单独报告。
5. 评价同时覆盖准确率/错误、延迟、内存、调用次数和失败率；不能只挑一个 pooled 数字。
6. confirmation 方法、baseline、阈值和统计口径先冻结，结果只读一次。

## 明确不通过的结果

- 只和几十年前的经典方法比较。
- 只用一个容易超过的固定策略。
- 把完整 panel 标签给某个方法、却不给其他方法。
- 通过换测试集、预算或失败定义来赢得数字。
- 没有原始日志和恢复脚本，却声称可复现。

## 审稿式结论

审稿人应能回答“这个数字来自哪个公共真值、哪个 provider 版本、哪个信息边界、哪个 baseline”。答不出来，benchmark 和 baseline 就不够干净。

## 来源等级

- **官方硬要求**：AAMAS 要求证据支持主张，理解并恰当引用 state of the art，并考虑 reproducibility、relevance 和 impact。见 [Call for Main Track](https://warwick.ac.uk/fac/sci/dcs/aamas2027/calls/call-for-main-track/)。官方没有规定必须使用某个公共数据集或固定 baseline 数量。
- **代表性论文规律**：方法论文通常用与技术困难对齐的少量场景、canonical/modern 对照和消融，而不是无目的堆 benchmark；代表性阅读见 `research/AAMAS_2023-2026_论文调性与DynamicToolRouting选题调研.md` 与 `research/Benchmark_Baseline_收敛调研_20260920.md`。
- **项目特定门槛**：truth/provider 分层、训练成员审计、延迟反馈收据、family 相关性和一次性 confirmation 是本项目的科学防守线。 “benchmark 必须公认”是本项目加严策略，不是 AAMAS 明文条款。
