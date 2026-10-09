# task28 — T2 Frozen Incumbent Router Selection

> 日期：2026-09-21
> 对应：实验矩阵 v3 的 T2；TODO T-287
> 状态：✅ 完成（2026-09-21，正式 frozen incumbent 见 report §6–§7 与 t2_frozen_incumbent_spec.json）

## 目标

在不开放任何新工具的 stationary setting 下，先得到一个可冻结的 incumbent router。T3 以后只测“challenger onboarding 增量”，不再把 incumbent 自己的 routing 能力混进新工具效果。

## 关键原则

1. T2 与 T3 分工明确：T2 解决旧工具池 routing；T3 只决定 frozen incumbent vs new challenger。
2. incumbent 不能靠 T3 test outcome 选择；候选集合预先固定。
3. 先跑 method-independent / classic candidates；Ours 不参与 incumbent 选择。
4. BBB 若 contextual incumbent 不如 Best Fixed，则允许 Best Fixed 成为该 benchmark 的 incumbent；不能为了制造 onboarding headroom 故意用弱 router。

## 第一轮候选

- Best Fixed
- Thompson Sampling / Beta-TS
- LinUCB
- LinTS
- Offline supervised per-arm reward regression（只用 history outcome）

后续正式表再补 PAK-UCB / P-LinUCB(P-TS) 等 peer-reviewed strong baseline；第一轮只确定 incumbent 强度与协议可运行。
## 主指标

- stationary average reward / native quality
- regret to Full Tool Oracle
- stability across folds/seeds
- 对 Best Fixed 的增益

## Benchmark

- LLMRouterBench Performance
- ASlib SAT12-ALL
- BBB complete dense panel（scaffold split）

## 冻结规则候选

最终每个 benchmark 的 incumbent 取预声明候选中 cross-validation mean 最好的非-oracle policy；若两者差异落在噪声内，优先更简单者。正式 T3 会把 incumbent policy 本身冻结，不再在线改旧工具路由。

## 与 T3 的接口

T3 输入只暴露：
- 当前 context x
- frozen incumbent 选出的 incumbent arm 与其 reward feedback（若走 incumbent）
- challenger arm（若尝试它）

T3 的主决策是 binary: incumbent vs challenger。