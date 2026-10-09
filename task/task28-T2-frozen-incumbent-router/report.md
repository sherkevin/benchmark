# task28 report — T2 Frozen Incumbent Router Selection

> 日期：2026-09-21
> 状态：第一轮 classic stationary baseline 已完成；strong published baselines 尚未补齐。

## 1. 第一轮结果

| Benchmark | Best Fixed | LinUCB | LinTS | Offline Ridge | 当前最好 |
|---|---:|---:|---:|---:|---|
| LLMRouterBench | 0.6737 | 0.7012 | 0.6873 | **0.7035** | Offline Ridge |
| ASlib SAT12-ALL utility | 0.7433 | **0.8494** | 0.7994 | 0.8348 | LinUCB（online） |
| BBB dense accuracy | **0.9310** | 0.9178 | 0.9072 | 0.9187 | Best Fixed |

这里的 Offline Ridge 与 LinUCB/LinTS 都使用与 G3 一致的 train-only 32 维 context representation；不是 T1 为资格审查使用的更高维/更充分 supervised model。

## 2. 一个关键区分：T3 的 incumbent 必须是 frozen policy

T3 要隔离的是“认识新 challenger”这一件事。因此 incumbent router 在 challenger 到来后不能同时继续学习，否则 onboarding manager 的变化和 incumbent 自己的变化仍会混在一起。

所以第一轮表中：
- LinUCB/LinTS 是 stationary reference baseline；
- 真正 eligible 的 frozen incumbent 候选应是 Best Fixed 或离线训练后冻结的 contextual policy。

这意味着不能机械地把 ASlib 的在线 LinUCB 0.8494 直接拿去当 frozen incumbent；需要冻结一个 history-only policy。

## 3. 当前 provisional frozen incumbents

- LLMRouterBench：offline contextual Ridge；
- ASlib：offline contextual Ridge；
- BBB：Best Fixed。

理由不是为了制造 onboarding headroom，而是 BBB 的多个 contextual selector 已在 scaffold split 下稳定输给 Best Fixed；LLM/ASlib 则明确存在可利用的 context signal。
## 4. 下一步补强

正式冻结前再做两件事：
1. 用 T1 的 full-feature supervised policy 重算 frozen-incumbent test performance，避免 32 维压缩不必要地削弱 incumbent；
2. 补 peer-reviewed strong baseline（PAK-UCB / P-LinUCB 或 P-TS）作为 stationary reference，但它们是否作为 frozen incumbent 取决于能否以 history-only 方式冻结。

formal T3 的 matched incumbent-only 就是这些 frozen policy 本身，不再让旧工具池在线漂移。

## 5. 第一轮产物

- `src/probe_headless/t2_stationary_core.py`
- `src/probe_headless/t2_llmrouterbench_stationary.py`
- `src/probe_headless/t2_aslib_stationary.py`
- `src/probe_headless/t2_bbb_stationary.py`
- `data/tool_results/divergence/t2_llmrouterbench_stationary.json`
- `data/tool_results/divergence/t2_aslib_stationary.json`
- `data/tool_results/divergence/t2_bbb_stationary.json`
## 6. 第二轮 full-feature frozen incumbent — 正式冻结

第一轮 32 维 context 只用于快速协议冒烟；正式 incumbent 已按 T1 中验证过的 full-feature supervised policy 重算，并为每个 held-out challenger 明确形成“incumbent-only frozen policy”。

| Benchmark | Frozen incumbent class | Stationary quality | Full Oracle | Held-out challenger opportunity | Challenger positive-context rate |
|---|---|---:|---:|---:|---:|
| LLMRouterBench | prompt TF-IDF 60k + per-arm Ridge(α=20) | **0.71550 macro** | 0.91853 | **+0.05715 macro** | 5.58% |
| ASlib SAT12-ALL | full 113 features + per-arm ExtraTrees | **0.92483 utility / PAR10 902.01** | 0.97988 utility | **+0.00932 utility** | 11.33% |
| BBB dense | train-derived Best Fixed | **0.93098 accuracy** | 0.97853 | **+0.03921 accuracy** | 3.92% |

这里的 held-out opportunity 不是 full-tool Oracle headroom，而是：先把 candidate challenger 从 incumbent action set 中移除，冻结 incumbent policy，然后比较 `max(r_inc,r_new)-r_inc`。因此它直接回答 formal T3 是否还有足够 challenger signal。

三套环境都仍有非零、可测的 challenger opportunity；同时 BBB 的 harmful-if-always-new 平均 6.99%，说明“直接全量接入新工具”也确实有风险。

ASlib 同时比较了 full-feature Ridge 与 ExtraTrees：Ridge utility 0.87610 / PAR10 1486.79；ExtraTrees utility **0.92483** / PAR10 **902.01**，因此按预声明“cross-validation mean 最好且 history-only 可冻结”规则选择 ExtraTrees，不为了制造 onboarding headroom 故意保留较弱 Ridge。

LLMRouterBench 的 full TF-IDF Ridge 0.71550 也高于第一轮 32D Ridge 0.70351 和 stationary LinUCB 0.70125；BBB contextual candidates 已在 T1 多次输给 Best Fixed，所以冻结 Best Fixed。

正式 authority：`data/tool_results/divergence/t2_frozen_incumbent_spec.json`。

## 7. T2 判定

**T2 完成。** 后续 T3 不再让 incumbent pool 自己在线漂移；每个 held-out challenger 都只面对一个 history-only 训练后冻结的 incumbent policy。强 published online methods（PAK-UCB / P-LinUCB/P-TS / FC-Entry 等）转为 T3 onboarding comparator，不再参与 incumbent 选择，避免把“选 incumbent”与“认识 challenger”重新混在一起。

新增产物：
- `src/probe_headless/t2_llmrouterbench_frozen_full.py`
- `src/probe_headless/t2_aslib_frozen_full.py`
- `src/probe_headless/t2_bbb_frozen_final.py`
- `data/tool_results/divergence/t2_llmrouterbench_frozen_full.json`
- `data/tool_results/divergence/t2_aslib_frozen_full.json`
- `data/tool_results/divergence/t2_bbb_frozen_final.json`
- `data/tool_results/divergence/t2_frozen_incumbent_spec.json`
## 6. Frozen incumbent artifacts 已生成：T3 接口现在可直接使用

为了避免 T3 一边认识 challenger、一边让 incumbent 自己继续漂移，本轮又生成了真正 history-only frozen policy；每个 held-out challenger 都从 incumbent 候选中排除后再冻结。

| Benchmark | Frozen incumbent | Mean incumbent reward | Challenger Oracle 增量机会 | Challenger 更好 context 比例 |
|---|---|---:|---:|---:|
| LLMRouterBench | prompt TF-IDF + per-arm Ridge(alpha=20) | **0.70426** | **+0.05580** | **5.58%** |
| ASlib SAT12-ALL | full features + per-arm Ridge(alpha=10) on log PAR10 | **0.87395 utility**（PAR10≈1512.6） | **+0.02153 utility** | **17.41%** |
| BBB dense | train-fold Best Fixed excluding challenger | **0.92768** | **+0.03921** | **3.92%** |

这里的“Challenger Oracle 增量机会”严格定义为 `mean(max(r_inc,r_new)-r_inc)`，已经不包含旧工具池内部 routing 的 Oracle gap。

一个重要结果是：换成更强、更真实的 frozen incumbent 后，new-tool opportunity 仍然存在，但确实变得更稀疏。这正是 selective onboarding 要解决的 setting，而不是人为使用弱 incumbent 制造空间。

产物：
- `src/probe_headless/t2_llmrouterbench_frozen_incumbent.py`
- `src/probe_headless/t2_aslib_frozen_incumbent.py`
- `src/probe_headless/t2_bbb_frozen_incumbent.py`
- `data/tool_results/divergence/t2_*_frozen_incumbent.json`

因此 formal T3 的环境接口已经冻结：每一轮只需给 onboarding manager 当前 context；environment 内部已有 frozen incumbent reward trace 与 challenger reward trace。