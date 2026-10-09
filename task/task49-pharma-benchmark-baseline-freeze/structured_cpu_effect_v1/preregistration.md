# Structured JEV CPU pre-screen v1

日期：2026-09-29  
状态：实现与契约测试阶段；未打开 confirmation，未提交 A800 效果任务。

## 目标

在同一个 BBB development 输入、scaffold-disjoint fold、冻结表示和
selected-only 延迟反馈合同下，区分普通的 selected correctness 更新和候选
`Truth-Recovered Structured Kernel` 的结构性收益。该轮是 CPU 预筛，不能把
smoke 或接口通过写成效果结果。

## 输入与表示

- 输入固定为 `fingerprint_diagnostic_v1/inputs.json`，722 分子、33 provider、5 个
  outer scaffold fold；输入 SHA 必须写入 `config.json`。
- 每个 fold/seed 读取
  `recovered_v3/checkpoints/fold{fold}_seed{seed}_bt_cls.pt`，只取其
  `encoder.0.weight/bias`，固定 `1024 -> 64 -> ReLU`。每个 checkpoint 的 SHA、
  encoder 形状和 fit snapshot SHA 写入 `fit_complete` 事件。
- evaluation 的完整 provider panel 只存在于 `FrozenPanelEvaluator` evaluator
  facade 中。policy 的输入是冻结的 64 维 hidden；调用后只绑定 selected provider
  的 `status/label`，未选输出、truth 和完整 panel 不进入 policy。

## Fit-only warm start

每个 fold/seed 仅用 fit 区拟合 q、h 和 direct head，validation 只用于选择
validation-loss 最小的 checkpoint；不使用 evaluation，固定 AdamW、full batch、
200 epochs、learning rate `0.001`、weight decay `0.01`、无 bias。固定随机种子、
初始化标准差和 AdamW 的 `betas=(0.9,0.999), eps=1e-8` 必须记录。

- q head：`BCE(truth)`，输入为 64 维 hidden。
- h head：每个 provider 一个输入 `[hidden, truth]` 的 correctness head；fit/validation
  中只保留该 provider `status=ok` 且有 binary output label 的 cells。失败没有可恢复
  的 output，不作为 h 的 fit 标签。
- direct head：每个 provider 一个输入 hidden 的 `BCE(system_success)`；这是 A1 的
  matched direct baseline，fit 中失败按 `system_success=0`。

所有 q/h/direct best epoch、训练/validation 曲线、状态快照和 hash 都必须留存。
不做探索率、学习率、lambda 或表示选择搜索；`exploration_epsilon=0`，冷启动来自
上述 fit-only warm start。

## 在线方法

主配置为 6 个动态 stream × 5 folds × 2 seeds × delays `{0,1,4}` = 180 条：

| ID | 方法 | 释放时的更新 |
|---|---|---|
| A0 | `frozen_structured` | q/h 冻结，只消费反馈 |
| A1 | `matched_online_sgd` | selected `system_success` 的 direct provider head；成功用 correctness，失败用 0；unknown 不更新 |
| A2 | `truth_only` | 用 selected output 与 correctness 恢复 `y`，只更新 q，h 冻结 |
| A3 | `correctness_only` | 只更新 selected provider 的 h，q 冻结 |
| A5 | `no_integration` | q/h 都更新，但选择时使用 `h_a(v, 1[q(v)\ge0.5])`，不做概率积分 |
| Proposed | `structured` | q/h 都更新，选择分数为 `q h_a(v,1)+(1-q)h_a(v,0)` |

另记录 fit-only `Best Fixed` provider 作为同 fold/seed reference，不把它计入
180 条 adaptive stream。其 provider 只由 fit 区 `system_success` 均值决定。

决策 tick 为 $t$，delay 为 $d$ 时反馈在 $t+1+d$ release；释放前不得更新。
成功且 reference 可评分才产生 correctness；失败和 unknown 都守恒消费，但结构化
方法不恢复标签。A1 的失败是明确定义的 `system_success=0`，因此它会更新 direct
head。每个 event exactly once；terminal 日志不得含 `truth`、`chemical_correct` 或
`recovered_label`。

## 日志与独立复核

- `events.jsonl`：逐 decision、terminal、feedback、stream_complete 记录。
- `config.json`：输入 hash、checkpoint hash、源文件 hash、方法/seed/delay、优化器、
  信息边界和硬件信息。
- `fit_snapshots/`：每个 fold/seed 的 q/h/direct 状态和 manifest。
- `verify_structured_cpu_effect_v1.py` 不导入 policy 实现，独立检查事件守恒、provider
  绑定、delay、terminal 泄漏、非 scored 更新和版本转移。

## 停止条件

任一事件读到未选 provider 或完整 truth/panel、失败被结构化方法写入恢复标签、
checkpoint/snapshot hash 不一致、或独立复核失败，则该轮作废。CPU 预筛未通过之前
不开启 confirmation，也不提交新的 A800 效果任务。
