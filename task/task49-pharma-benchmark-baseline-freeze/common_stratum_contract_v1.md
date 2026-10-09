# Common-stratum baseline 合同 v1

日期：2026-09-30  
状态：执行前合同；现有 receipt 不自动改写为本合同结果。

## 目的

现有 unified receipt 的 cohort、menu 和 feedback clock 相同，但 warm-start 和 uncertainty prior 不同。这个合同定义一条真正可比的主对照层：同一冻结表示、同一 fit-only provider prior、同一反馈预算，方法只在更新规则和探索规则上不同。

## 共同输入

- BBB development panel：输入 hash=`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`；722 个分子、33 个具体 provider。
- 表示：同一冻结 BT+CLS encoder，hidden dimension=64；所有方法使用相同的 molecule→query 向量，决策后不再重算。
- offline fit：仅在每个 outer fold 的 fit 区可见完整 provider correctness panel；validation/evaluation 区只在选定工具终止后释放 selected-only feedback。当前已有 strict-prior 收据使用 fit-only、validation-selected 的 direct neural head；不能用 Greedy 的 ridge prior 偷换 `$W_0$`。
- stream：5 个 scaffold folds × 2 个预注册 slots × delay `{0,1,4}`，每题最多一次 provider call；failure 保留在分母，unknown truth 终止该 stream。

## 共同 warm-start

在每个 `(fold, seed)` 的 fit 区，用同一个 provider-by-64 直接先验矩阵 $W_0$。当前可复用的 `$W_0$` 是 sealed structured CPU fit 的 validation-selected direct neural head；它的来源、生成器 commit、训练行和字节 hash 必须只登记一次。所有 strict-stratum 方法读取同一个 `$W_0$`，不得各自从完整 panel 重新拟合，也不得把 fit-only ridge prior 当成同一个 `$W_0$`。

初始 covariance 也统一为 `Σ0 = σ² I`，其中 `σ²`、observation noise 和 ridge precision 在 manifest 中固定；不能把不同算法的隐含单位先验混在一起。当前合同取 `σ²=0.02`、observation noise variance `=1.0`，所以 Gaussian LinUCB 的 prior precision 必须是 `λ=50.0`，不能使用 `regularization=1.0`。

这里的“共同 prior”只表示同一封存的数值初始化，不把不同损失函数强行解释成同一个概率模型。`direct_prior` 是 BCE/logit head 的权重；Gaussian baselines 若直接把它当作 Bernoulli 概率或校准后的 reward mean，必须另列为 adapted/out-of-stratum。正式结果不得把这种 adapted receipt 写成严格的 common-prior 比较。

## 输入 manifest

执行前先读取 `common_stratum_manifest_v1.md` 及其机器 manifest。该 manifest 固定 5 folds × 2 seeds 的 encoder checkpoint、direct prior、数据分区和 feedback/resource semantics；没有通过独立 verifier 的 manifest 不得进入 full receipt。

## 方法角色

| 方法 | strict common-stratum 规则 |
|---|---|
| Frozen | 只读 `$W_0$`，不消费 feedback；作为同一 warm-start 的静态控制 |
| Matched Online-SGD | 从 `$W_0$` 出发，对释放的 selected-only reward 做固定步长 rank-one/SGD 更新 |
| Greedy LinUCB | 从 `$W_0$` 出发，共同 `Σ0`，固定 alpha；只改变 UCB 选择，不改变输入/更新预算 |
| BayesianRouter adapted | 从 `$W_0$` 出发，共同 `Σ0` 和 noise；只改变 Thompson action sampling |
| MIRA | 从 `$W_0$` 出发，消费相同 selected-only terminal/release 事件；结构化 reliability state 是待检验的更新差异 |

PILOT 使用额外的 projection/provider-vector fitting，暂不放入 strict common-stratum；它保留为 adapted modern baseline，单独报告并禁止与 strict-stratum raw accuracy 混排序。

## 公平预算与记账

每个方法在每个 decision tick 允许：一次 menu scoring、一次 selected provider call、每个到期 feedback 一次状态更新。Frozen 控制可以消费 feedback 以保持协议时钟，但其模型参数必须不变。禁止读取未选 provider 输出、未释放 feedback 或 confirmation 标签；禁止用额外探测次数换取更新优势。

收据必须同时记录：decision/terminal/feedback 数、selected provider call 数、更新次数、selection/update p50/p95、峰值状态字节数和事件 hash。当前 cache replay 的 provider call 只能写为“调用次数”，没有实时价格或 token 计费时不得写成本 superiority。

## 统计与闸门

- 主要比较是每个方法相对自己 Frozen control 的 paired online−frozen；跨方法 superiority 只有在共同 `$W_0$`、`Σ0`、budget 和 runtime ledger 全部相同时才允许。
- 主报告同时给 pooled、fold-level 和 provider-family macro 结果；paired uncertainty 以 molecule/fold 聚类 bootstrap，不把 provider cell 当独立样本。
- 先跑 2-stream smoke，独立 verifier 通过且篡改测试 fail-closed 后，才运行 60-stream full receipt。
- full receipt 只补 Gate B 公平性，不改变已经完成的旧 receipt；旧结果保留为历史/异质 prior 对照。

## 失败处置

若某方法无法在共同 `$W_0$` 或共同 covariance 下表达，标记为 adapted/out-of-stratum，不删掉它，也不强行改写成 strict baseline。若共同 receipt 没有超过 Frozen 或 paired 区间跨过预登记的最小效应线，保留负结果，不通过换 split、调 alpha 或删除失败 provider 追分。
