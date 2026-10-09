# MIRA relative-reliability v1

日期：2026-09-30
状态：运行前；CPU development gate only。

## 目的

验证一个比 additive correctness penalty 更干净的 selected-only 风险修正：availability ledger 保持不变；correctness ledger 用连续 Beta 后验均值构造有界相对可靠性因子，避免 `n_min` 开关和不同尺度直接相减。

这不是 context-specific provider quality 的识别方法，也不声称 anytime safety guarantee。若 provider 没有被选择，它的 selected-only 状态不会变化。

## 信息边界和状态

- 决策前只读当前分子特征、固定 provider menu、fit-only direct prior 和已释放的 selected feedback；
- runtime failure 只更新同 provider 的连续 failure streak，不进入 correctness ledger；连续两次 selected runtime failure 后 hard block；
- successful scored return 才更新 $v_t(a)$（有效返回数）和 $f_t(a)$（chemical error 数）；unknown 不计数、不清零 streak；
- terminal label/status 直到 `decision_tick + 1 + delay` 才释放，delay 固定为 `{0,1,4}`。

## 数学定义

fit-only provider score 为

$$q_0(a\mid x)=\operatorname{sigmoid}(w_a^\top x).$$

用现有风险阈值 $τ=0.20$ 作为 Beta 先验均值，用 $n_{min}=20$ 作为等效样本量：

$$e_a\sim\operatorname{Beta}(n_{min}\tau,\;n_{min}(1-	au)).$$

释放 $v_t(a)$ 个 selected scored returns、其中 $f_t(a)$ 次错误后，使用 Beta 后验均值

$$\widehat e_t(a)=\frac{n_{min}\tau+f_t(a)}{n_{min}+v_t(a)}.$$

相对可靠性因子只对高于先验风险的证据降权：

$$\rho_t(a)=\min\left\{1,\frac{1-\widehat e_t(a)}{1-\tau}\right\},
\qquad
\widetilde q_t(a\mid x)=q_0(a\mid x)\rho_t(a).$$

因此冷启动时 $v=f=0$，$\widehat e=τ$、$ρ=1$，动作与 Frozen 完全一致；例如 $τ=.2,n_{min}=20$ 时一次错误得到 $\widehat e=5/21$、$ρ=20/21\approx0.9524$，两次错误得到 $\widehat e=6/22$、$ρ=10/11\approx0.9091$。$ρ$ 是排序因子，不是校准概率，也不证明 provider 已退化。

availability hard block 单独作用：blocked provider 的决策分数为 $-\infty$；所有 provider 都 blocked 时显式退回 $q_0$。

## 对照与矩阵

复用 `run_v2_fixed` 的 inputs、fit/feature snapshots、fold/seed、episode order、evaluator、event tick 和 feedback queue；不重训、不调参、不申请 A800：

- Frozen；
- streak-only；
- relative-reliability（本候选）；
- availability-soft（上一候选）；
- full MIRA v2；
- matched Online-SGD。

5 scaffold folds × 2 seeds × 2 scenarios（no-event / controlled availability event）× 3 delays × 6 variants = 360 streams。

## 预注册指标和停止条件

每条 stream 记录 overall、pre/post、first target failure release 后 accuracy，target exposure，failure 分母，first block tick/reason，每次 decision 的 $q_0,ρ,\widetilde q$、rank change 和 release state。报告 relative-reliability 的 no-event paired delta、controlled-event suffix delta、rank-change 率和 `rho<1` 次数。

- no-event 相对 Frozen 的 $±1$pp 作为开发期 ROPE；
- controlled-event 至少在两个 delay 保留 streak-only 的主要切换收益；
- 若 relative-reliability 仍把动作大量让给无 selected evidence 的 provider，或 no-event 超出 ROPE，停止这条 correctness 变体；
- 若独立 replayer、selected-only、feedback clock、provider binding 或 state/version 守恒失败，整批为 engineering failure；
- 不根据结果修改 $τ,n_{min}$，不增加探索，不打开 confirmation。

## 证据边界

本轮只验证 fixed-pool feedback-adaptive 机制；controlled event 是预登记的 provider unavailability 压力，不等价于自然 drift。即使结果为正，也不能直接声称超过 matched Online-SGD、能估计未选 provider 当前分子质量或具备环境恢复能力。
