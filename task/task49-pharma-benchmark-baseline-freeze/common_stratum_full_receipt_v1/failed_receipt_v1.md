# Full receipt v1：工程失败记录

状态：`superseded`，不得用于方法效果、显著性或论文表格。

2026-09-30 首次全量回执实际完成了 150 条 stream、64,980 条事件，使用了真实 BBB cache 和 selected-only delayed-feedback 协议。运行没有在执行层崩溃，但回执在合同复核时被判为无效：`greedy_online` 使用了 `regularization=1.0`，于是其初始 covariance 是 `I`；合同要求的 Gaussian 初始 covariance 是 `Σ0=0.02I`，对应 precision `50.0`。

这不是结果上的负例，而是实验配置错误。原始 `config.json`、`events.jsonl`、`summary.json` 和远程/本地执行日志全部保留，不能覆盖或删除。后续回执必须使用修正后的 precision，并由独立验收器核对 manifest、源码树、事件身份、反馈时钟、cache/truth 绑定和方法状态。

另外，`direct_prior` 来自 BCE/logit head。Gaussian LinUCB/Thompson sampling 直接把它作为 reward-scale prior 的语义尚未成立，因此在提供独立 reward-scale prior 前，二者只能标为 adapted/out-of-stratum comparator，不能写成严格 common-prior 结论。
