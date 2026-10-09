# MIRA availability-hard / correctness-soft v1

日期：2026-09-30  
状态：运行前；CPU development mechanism gate only。

## 要回答的问题

MIRA v2 的连续运行失败隔离保留后，能否把 selected correctness 从永久 Beta veto 改成连续的排序惩罚，减少 no-event 静态损失，同时保留 controlled availability event 后的切换收益？本轮只验证这个机制，不申请 A800，不进入 confirmation，不调参。

## 信息边界

- 输入是分子特征、固定 provider menu 和 fit-only direct prior。
- 决策时只看到当前 query 的特征；不读取未选 provider 的输出、标签或未来 feedback。
- terminal 后的 selected output status/label 由 evaluator 提供；chemical correctness 在 `decision_tick + 1 + delay` 才释放。
- `runtime failure` 没有 chemical correctness，不进入 Beta ledger；`unknown` 不计数、不清零 streak。

## 方法合同

对 provider $a$，fit-only prior 分数为

$$q_0(a\mid x)=\operatorname{sigmoid}(w_a^\top x).$$

这里 $x$ 是 frozen 64 维 encoder 对当前分子的输出，$w_a$ 是同一 outer fold 的 fit-only direct-success head。它是用于排序的 [0,1] 分数，不宣称经过概率校准，也不把下面的差值解释成统计 utility。

selected feedback 维护 $v_t(a)$ 次有效 scored return 和其中 $f_t(a)$ 次 chemical error。仅当 $v_t(a)\ge n_{min}$ 时启用

$$U_t(a)=\operatorname{Beta}^{-1}_{1-\delta}\left(1+f_t(a),\;1+v_t(a)-f_t(a)\right).$$

正确性只产生 soft penalty：

$$s_t(a\mid x)=q_0(a\mid x)-\eta\,[U_t(a)-\tau]_+,$$

其中 $n_{min}=20,\delta=0.05,\tau=0.20,\eta=1.0$，且 $[z]_+=\max(z,0)$。当 $v_t(a)<n_{min}$ 时 penalty 定义为 0，避免 Beta(1,1) 冷启动上界惩罚所有 provider。

可用性通道单独处理：同一 provider 连续两次 selected runtime failure 后，设置 `blocked(a)=true`；blocked provider 的决策分数为 $-\infty$。若所有 provider 都 blocked，退回 fit-only $q_0$ 的显式 fallback。scored correctness 永远不能设置 hard block。

## 对照与实验矩阵

复用 `mira_veto_ablation_v1` 的同一 inputs、fit/feature snapshots、fold、seed、episode order、evaluator、event tick 和 delay，不重训：

- Frozen；
- streak-only（只保留连续两次 runtime failure hard block）；
- availability-soft（本方法）；
- beta-only；
- full MIRA v2；
- matched Online-SGD。

5 scaffold folds × 2 protocol slots × 2 scenarios（no-event / controlled provider availability degradation）× 3 delays `{0,1,4}` × 6 variants = 360 streams。

## 预注册指标

每条 stream 保存 overall、pre-event、post-event、first-failure-release 后 accuracy；target provider 的 pre/post/after-release exposure；首次 target block 的 decision/release tick 和 reason；每次决策的 base score、soft penalty、adjusted score、实际 rank change；每次 feedback 的 Beta state、streak 和 version。

主要比较：

1. no-event 下 availability-soft 相对 Frozen 和 streak-only 的 paired accuracy delta；
2. controlled-event 下首次 target failure release 后 suffix accuracy 与 target exposure；
3. availability-soft 与 full MIRA 的机制差异；
4. matched Online-SGD 作为同合同强在线对照，不预设本方法胜出。

`+/-1pp` 只作为开发期 ROPE，不作显著性结论。不得根据结果改动 $η,n_{min},\tau,\delta$ 或 streak；若候选为负，保留结果并定位是 penalty 无效、prior 表示受限还是 feedback credit assignment 问题。

## 失败条件与证据边界

- 任一事件守恒、selected-only、due/release、provider binding 或独立 verifier 检查失败，整批作为 engineering failure。
- 若 penalty 从未改变任何 rank，报告为“软通道未产生可观测决策作用”，不把它包装成有效 adaptation。
- 若 no-event 损失仍超过预注册 ROPE，或 controlled-event 不能保留 streak-only 的切换收益，不进入 A800。
- 本轮不声称 anytime coverage、统计安全保证、未选 provider 质量估计、自然 drift 泛化、cooldown/recovery 或超过强 baseline 的结论。
