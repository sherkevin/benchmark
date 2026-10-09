# Matched / BayesianRouter unified raw：独立科学审查

- 日期：2026-09-30
- 范围：`run_matched_sgd_unified_raw_v1.py`、`verify_matched_sgd_unified_raw_v1.py`、`run_bayesianrouter_unified_raw_v1.py`、`verify_bayesianrouter_unified_raw_v1.py`，以及 matched `full_v2` 收据；BayesianRouter `full_v1` 收据若仍在运行，只审查当前代码和已落盘部分。
- 结论：**matched receipt 的状态/动作重放有价值，但还不能作为完整统一公平合同的通过证据；BayesianRouter 不能沿用旧 C8 名义作为等价迁移。先修审计器和 prior lineage，再进入主表。**

## 1. 先确认新旧 Matched 是否是同一 baseline

结论不是“完全换了 encoder”。我从旧 A800 matched 收据
`experiments/logs/task49_matched_sgd_development_20260927/recovered_v2/events.jsonl`
中解码了 10 个 `fit_complete` 的 `features` 和 `prior`，再和新
`experiments/logs/task49_matched_sgd_unified_raw_20260930/full_v2/fit_snapshots/`
逐 fold/seed 对比：

- 64 维 encoder features 基本相同，只存在 CPU/GPU 浮点差；10 组最大绝对差约为 $2.4\times10^{-7}$ 到 $3.5\times10^{-7}$；
- 33×64 的 direct provider prior 明显不同。以 fold0/seed0 为例，最大绝对差为 `0.2243737`，相对 L2 差约 `0.9938`；10 组的相对 L2 差约 `0.96–1.08`。

新 runner 读取的是
`task49_composition_20260929/structured_cpu_effect_v1/full_v1/run/fit_snapshots/` 的
`direct_prior`，旧 A800 run 的 prior 来自旧 Nebula `train_bt` fit。因而准确表述应是：

> 新 receipt 复用了同构的 64 维 encoder 及同一菜单/任务，但重新采用了不同 fit prior；它是一个 **common-direct-prior matched control**，不是旧 A800 Matched-SGD receipt 的纯 raw-schema 迁移。

这会影响跨收据数值比较，也影响“Matched Online-SGD 已被公平复核”的措辞。新 receipt 的动作和更新只对新 prior 合同负责。

从已报告的 pooled 数字看，这个替换也确实改变了 baseline 难度，而不是只增加了日志字段：旧 A800 receipt 的 Frozen/Online-SGD 为 `0.8913/0.9024`（delay 0），新 common-direct-prior receipt 为 `0.901734/0.891375`。也就是说，Frozen 约高 `1.04pp`，Online-SGD 反而约低 `1.10pp`；delay 1 的差异约为 `+1.04pp/-0.89pp`，delay 4 约为 `+1.04pp/-0.06pp`。因此若把新表和旧主表并列而不标 prior lineage，会同时造成两种误读：新 Frozen 看似更强、Online-SGD 看似更弱，进而改变 MIRA 相对 baseline 的印象。旧 published baseline 不能被新 receipt 静默替换；新 receipt 必须单独标为 common-prior control，或用旧 prior 重跑一份真正的 schema migration。

## 2. 已验证的部分

- matched `full_v2` 的 config 声明了 `declared_commit`，6 个运行源文件有 hash，输入和 snapshots 有 hash；旧 `full_v1` 的绝对路径 provenance 失败被保留，未混入结果。
- matched verifier 不导入 policy，能重算 60 streams、8,664 decisions/terminals/feedback 的 action、selected cache output、reward、延迟 tick、SGD state hash。
- BayesianRouter runner 明确固定 `prior_variance=.02`、`noise_variance=1.0` 和 fold/seed/delay seed 公式；其 Gaussian action replay 公式与 kernel 结构一致。
- 两个 runner 都通过 `run_bbb_stream` 的 selected-only 调用流程；terminal output 来自 selected cache，feedback 中没有 evaluator-only correctness 字段。

BayesianRouter `full_v1/summary.json` 已写出 60 条 stream，但它的 config source hash 针对生成时的 `pharma_selection_stream.py`；当前工作区该文件已修正 `bind` 语义，hash 已不再匹配。因此在当前代码上运行 verifier 会先因 source hash mismatch 退出，不能把 `summary.json` 的 completed 当成 verifier verified。

这些证据足以说明“适配器确实能被独立重放”。它们还不足以说明每一个声明的审计字段或 baseline lineage 已被验证。

## 3. 会影响科学结论的缺陷

### P0：verifier 对 menu、read-set 和聚合指标存在 fail-open

matched verifier 只用 payload 中的 provider、score、feature hash 和 state hash 重放动作；它没有比较：

- `menu_before_choice` 是否等于冻结输入的完整 menu；
- `decision_reads` 是否恰好是允许的字段集合；
- `provider_event_ids`、`released_feedback_ids` 是否与当前事件的完整顺序一致；
- `stream_complete.summary` 的 accuracy、failure/unknown 分母、cost 是否和 raw terminal/feedback 重算结果一致。

BayesianRouter verifier 也只重算 provider action/score、feature hash 和 state hash，同样没有对 `menu_before_choice` 和 `decision_reads` 做约束，也没有验证 stream summary 的指标。

我在 matched `full_v2` 的临时复制上只做了三处修改：第一条 decision 的 `menu_before_choice` 改成 `['forged_provider']`，第一条 decision 的 `decision_reads` 改成 `['truth','full_panel']`，一个 `stream_complete` accuracy 改成 `0.123456`，同步改 summary 顶层数值；现有 verifier 仍返回 `status=verified, n_streams=60`。这证明它不是理论上的缺口，而是可复现的最小反例。

**修复优先级：P0。**

1. 固定并核验 `menu_before_choice == inputs.providers`（若允许子菜单，则核验其来源、顺序和 provider 集合）；
2. 固定允许的 `decision_reads`，拒绝 `truth`、`full_panel`、未选输出和未来 feedback；
3. 由 verifier 从 terminal/feedback 重新计算每条 stream 的 accuracy、valid/failure/unknown 分母、版本数和成本，再与 `stream_complete` 及最终 summary 逐字段比较；
4. 对 `event_id` 格式、decision tick 连续性、terminal/feedback 各一对一绑定、feedback release tick 非递减和同 tick ordinal 顺序加断言；
5. 加入 menu/read-set/metric/feedback-order 的变异测试，变异必须退出码 1。

### P0：统一 raw receipt 的 `bind` 信息边界需要单独修正

当前公共 `TerminalFeedbackQueue.complete()` 对暴露 `bind` 的策略传入
`record['recovered_label']`。这不是 selected provider 的原始输出 label，而是使用 evaluator correctness 恢复出的标签；若一个 policy 的 `bind` 保存该值，就能在 terminal、feedback release 前看到 evaluator 派生真值。当前 Matched 和 BayesianRouter 没有 `bind`，所以这次两个 receipt 的 action 未被该字段污染；但该 queue 正被 MIRA/Structured 策略共用，统一 schema 不能因此把它视为安全。

**修复优先级：P0。** `bind` 只能接收 selected output 的原始 label；recovered label 只能在 release 时、并且只能传给明确声明需要它的 learner。增加错误 label、延迟 0/1/4 和未选 provider 注入测试。

### P1：BayesianRouter 的 prior lineage 与旧 C8 不一致

`run_bayesianrouter_unified_raw_v1.py` 的 `SNAPSHOT_ROOT` 默认指向 matched `full_v2/fit_snapshots`，并读取其中的 `direct_prior`。它没有读取旧 BayesianRouter A800 run 中的原生 BT `score.weight` prior。虽然 feature encoder 可同构，但 provider prior 已换源；因此 `METHOD_IDS['bayesianrouter_online']='C8'` 容易让主表读者误以为这是旧 C8 的统一日志迁移。

**修复优先级：P0。** 二选一并写入配置与报告：

- 用旧 C8 的 BT checkpoint/prior 重新生成 native-C8 unified receipt；或
- 将当前版本命名为 `bayesianrouter_common_direct_prior`，单独作为 matched-prior control，不把它排入旧 C8 的数值行。

两者不能在同一行合并。当前 `full_v1` 即使 verifier 通过，也只能支持后一个解释。

### P1：BayesianRouter source manifest 少了实际输入依赖

BayesianRouter runner 从 `run_structured_cpu_effect_v1.py` 导入 `DEFAULT_INPUTS` 和 `load_inputs`，但 config 的 `source_sha256` 没有包含该文件；其 verifier 也依赖当前工作区版本的 `load_inputs`。这让输入解析、fold 字段和标签校验的来源没有被 receipt 完整锁定。

**修复优先级：P1。** 把 `run_structured_cpu_effect_v1.py`、verifier 自身及用于生成 snapshot 的固定脚本纳入 source manifest，并在独立 provenance checker 中检查 commit + source hash，而不是只检查 events/config。

### P1：当前 verifier 只验证 snapshot bytes，不验证 checkpoint/source snapshot lineage

manifest 中记录了 checkpoint 和 source snapshot 的路径/hash，但 matched verifier 实际只检查生成后的 `fit_snapshots/*.npz` hash；没有重新核对 checkpoint/source snapshot 的 hash。这不影响“给定 sealed snapshot 的动作重放”，但不足以支持“该 snapshot 来自声明的 checkpoint/encoder/prior 训练合同”。

**修复优先级：P1。** 在不重算科学结果的情况下补 provenance-only checker：核对 checkpoint/source snapshot 的存在、hash、shape、prior/feature 生成关系，并将结果作为 receipt 的必要字段。

## 4. 关于反馈顺序

当前 verifier 对每个 feedback 的 `release_tick == decision_tick + 1 + delay` 有检查，也通过 version/state hash 追踪更新；但它没有单独检查 raw event 中 feedback 的全局 release tick 非递减或同 tick ordinal 顺序。正常 runner 产生的 stream 是有序的，现有收据没有发现乱序；问题在于一个改写者若同步重写受影响的 state/version payload，审计器没有“原始队列顺序”的独立锚点。

最低修复应是：逐 stream 检查 feedback 的 `(release_tick, queue_ordinal)` 单调，且在每个 decision 前，所有 `due_tick <= decision_tick` 的事件都已释放；把 queue ordinal 写入 raw receipt 并纳入 verifier。不要只依赖 state hash，因为 hash 证明状态内容，不证明事件到达顺序。

## 5. 当前可用范围与放行建议

- Matched `full_v2` 可以保留为“common-direct-prior 下的统一 raw/replay 工程证据”；其 60-stream numeric result 不能与旧 A800 Matched-SGD 数字直接混为同一 prior 的重复实验。
- BayesianRouter `full_v1` 在没有 native prior 说明和上述 verifier 修复前，不进入主 baseline ranking；可作为待修的 common-prior receipt。
- 在 P0 缺陷修复并通过最小变异测试前，Gate B 的 baseline fidelity/raw schema 仍 HOLD；不申请或解释 A800 full matrix 结果为最终公平比较。
- 这次审查不否定 MIRA 目标，也不把组合路线降级；它只阻止一个能返回 `verified` 的弱审计器被当作科学证据闭合。
