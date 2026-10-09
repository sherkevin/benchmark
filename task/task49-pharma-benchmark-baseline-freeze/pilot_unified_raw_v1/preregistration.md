# PILOT-adapted unified raw receipt v1

日期：2026-09-30  
状态：运行前预注册；CPU development baseline qualification；不提交 A800，不打开 confirmation。

## 目的

把已有的 PILOT-adapted baseline 接入当前统一 decision/terminal/feedback receipt 合同，验证它是否能在同一冻结 BBB development 输入上完成可独立重放的 delayed selected-only adaptation。该任务不是 Proposed 方法实验，也不为 PILOT 调参。

## 冻结输入与矩阵

- 输入副本：`task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json`。
- 输入 SHA-256：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 722 个 molecule、33 个 provider、Morgan-1024 raw bits、5 个 scaffold-disjoint outer folds；evaluation 顺序按输入文件固定。
- 每个 fold 使用 fit-only 数据训练 PILOT projection/provider prior；evaluation 不参与先验拟合。
- fit seeds：`20260927`、`20260928`；每个 seed 生成一条 prior，不能把两个 seed 合并拟合。
- 每个 `(fold, seed)` 使用 `pilot_frozen` 与 `pilot_online`，feedback delay `{0,1,4}`，共 `2 × 5 × 2 × 3 = 60` streams。
- 预计每条 stream 144 或 145 个 decision/terminal/feedback，完整矩阵共 8,664 triples；unknown truth 按 `stop_batch`，本冻结 cohort 中不补标签。

## PILOT-adapted 定义

- projection：`1024 → 64` 线性层，triplet margin `0.2`，projection/provider 各 100 epochs，AdamW learning rate `0.001`、weight decay `0.01`，provider L2 `0.01`。
- provider prior：fit-only provider vectors；precision 为 fit provider accuracy 的倒数并逐 provider 固定。
- decision：将 projected query 归一化；score 为 provider cosine posterior mean 加 `alpha × posterior standard deviation`，`alpha=0.5`；完整 menu、首个最大分数 tie rule。
- online：仅对已选 provider 的 `system_success ∈ {0,1}` 做 Gaussian rank-one update；failure reward 为 0；selected feature 在 decision 时复制并由 event ID 绑定。
- frozen：与 online 使用同一个 `(fold, seed)` prior、query features、流顺序和 delay，只消费/记录 delayed feedback，不更新 posterior mean/covariance；protocol version 仍按 feedback release 推进。
- feedback：terminal 后进入 queue，due tick=`decision_tick+1+delay`；只在 release tick 传给 policy；未选 provider output、truth、chemical correctness、recovered label 不进入 policy。

## Receipt 与独立核验

每条 raw event 必须记录：`run_id/method_id/episode_id`、完整 candidate menu、decision read-set、released feedback IDs、selected provider/output/status、feature hash、decision version、due/release tick、state hash before/after、update latency。terminal 只保留 selected output；feedback 只保留 policy-visible reward/status，不写 evaluator chemical label。

独立 verifier 不导入 runner 或 PILOT policy，直接用 NumPy 重建：

1. fit-only PILOT projection/provider prior 和归一化 query；
2. cosine+UCB score、first-in-menu tie rule；
3. selected-only delayed reward queue、terminal/provider/event binding；
4. Gaussian rank-one update、version、state hash、event conservation、stream summary；
5. declared commit 对应 git tree 的 source hash、input/prior/checkpoint hash。

任一输入、源码、菜单、read-set、反馈时钟、状态重放或事件守恒失败，整轮标记 `BLOCKED`，不解释效果。

## 预注册指标与停止线

主指标是每条 stream 的 selected `system_success` accuracy，以及 paired `pilot_online − pilot_frozen` difference（按 fold/seed/delay 报告 pooled 与 equal-stream mean）。附带 cumulative error、release count、update count、decision/update p50/p95 和 release-to-switch。Best Fixed 与 evaluator Oracle 只作控制/上界，不进入 online superiority 排名。

开发信号：如果 online 在至少两个 delay 的 equal-stream mean 不为正，停止把 PILOT-adapted prior + 普通 cosine/UCB/ridge update 作为当前任务的有效机制；即使通过，也只能获得统一 baseline 资格，不能写成 Proposed superiority。禁止查看新结果后调整 alpha、epochs、precision、流顺序、样本或输入版本。

## 证据边界

这是 CPU replay，无新 provider/API 调用。旧 `task49_pilot_adapted_development_20260927/run_v1/` 作为历史 receipt 保留；新 receipt 不覆盖旧结果。未形成 confirmation，也不触发 A800。完整 raw events 只保存在本地 evidence 目录，提交小型 config/summary/verification/manifest 和报告。
