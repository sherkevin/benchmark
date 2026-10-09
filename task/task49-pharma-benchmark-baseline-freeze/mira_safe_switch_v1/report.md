# MIRA-SafeSwitch CPU 探索回放报告

日期：2026-10-02  
状态：候选机制未通过；保留为探索证据，不进入主结果或 confirmation。

## 这轮到底验证了什么

这轮不是验证整篇论文，也不是验证最终 MIRA。它只检查一个窄问题：在 common BBB
development cache 上，provider-level correctness/hazard gate 能不能在 selected-only
延迟反馈下安全地离开 fit-only incumbent。每次策略只看分子 hidden-64 特征和已经释放的
selected feedback；输入、cache、fold 和 provider menu 沿用封存 common-stratum 合同。

## 版本轨迹

| 收据 | 机制版本 | 范围 | SafeSwitch | 观察 |
|---|---|---:|---:|---|
| `v2_run` | 固定 20 pseudo-count、双侧 LCB/UCB、$\gamma=.05$ | fold0/seed0/delay0/32 | 27/32 | 0 次切换；独立 verifier 通过 |
| `v3_fold0_seed0_delay0_full` | 同上 | fold0/seed0/delay0/145 | 130/145 | 0 次切换；退化成 fit incumbent |
| `v4_fold0_seed0_delay0_full` | fit count 进入 prior、$\gamma=.02$、双侧门 | fold0/seed0/delay0/145 | 130/145 | 0 次切换；仍过于保守 |
| `v5_fold0_seed0_delay0_full` | fit-count prior、单侧 LCB、$\gamma=0$ | fold0/seed0/delay0/145 | 126/145 | 1 次相对上一动作切换，最终 144 次偏离固定 incumbent |

`v2` 与 `v3` 的独立复核使用 `verify_mira_safe_switch_smoke_v1.py`；`v5` 的独立复核
通过，收据为同目录 `v5.../verification.json`，覆盖 290 个 decision/terminal/feedback
三元组。所有收据均没有读 confirmation。

## v5 的关键负结果

fold0 的 fit-only incumbent 是 `admetlab_3_0`，Best Fixed 在 evaluation 上为 130/145。
v5 只在第二个 action 离开 incumbent，随后把“当前动作”更新成 `b3clf_xgb_classic_adasyn`；
后续 143 个动作没有回到 fit incumbent。最终 SafeSwitch 为 126/145。

相对固定 incumbent 的逐格分解为：both-correct 117、both-wrong 6、rescued 9、harmed 13。
因此这轮没有显示安全切换，且没有满足“降低 harmed 并保留 rescued”的候选目标。这个数字
是一个 fold 的开发探索，不能外推到 4,332-cell 主矩阵。

需要特别区分两个日志字段：v5 的 `safe_switched=1` 是“相对上一动作”的切换数；相对
fit incumbent 的实际 switch-away 是 144。后续实现不能用前者代替标准要求的 switch-away
指标。

## 复核发现的机制问题

1. **上下文错配**：Beta 账本是 provider 的 selected-policy 边际正确率，不能说明当前
   分子上该 provider 的正确率。v5 选择了同一个 challenger 后，边际账本无法把它在新
   分子上的错误及时映射回当前动作。
2. **incumbent 路径依赖**：每次选择都把 `current_incumbent` 改成新 provider，下一步的
   安全比较不再锚定 fit-only fallback；一次早期切换会改变后面所有的比较对象。
3. **统计解释有限**：正态 LCB 和 runtime hazard 上界目前是工程门，不是已证明的覆盖
   概率或安全 regret 保证。论文不能把它写成 formal safety theorem。
4. **术语需要统一**：fit 统计必须明确区分 valid return $V^{ok}$、runtime failure $F$
   和总终止调用 $N^{term}=V^{ok}+F$；runtime Beta 的 prior strength 使用 $N^{term}$。

独立数学审查还指出，当前 Beta 方差是 provider 边际正确率 $\theta_a$ 的方差，不能直接
当成当前分子 $q_a(x_t)$ 或 corrected score 的置信区间。一个 provider 在少数分子上很差、
在其余分子上很好时，边际率可以很高，LCB 仍会通过；因此当前 gate 不能称 context-aware
confidence，也没有 formal safety/regret guarantee。`n_min=N_fit+C^++C^-` 在本菜单中
几乎总是满足，v5 第一次切换时 challenger 的 online count 仍是 0，说明这轮实际依赖
fit-only side information，而不是在线证据。

只读 analyzer
`src/probe_headless/analyze_mira_safe_switch_v1.py` 已对 v5 逐事件重算：
`fixed_switch_away=144`、`switch_without_online_evidence=1`，fixed reference 四格仍为
117 both-correct、6 both-wrong、9 rescued、13 harmed；mutable-current 四格则为
126 both-correct、19 both-wrong。该分析不重跑策略，收据为
`v5_fold0_seed0_delay0_full/posthoc_analysis_v1.json`。

## 当前处置

SafeSwitch v1/v5 保留作负对照和问题定位，不申请 A800，不打开 confirmation，不继续调
`gamma` 直到出现正数。下一候选必须同时满足：

- 用 context-conditioned posterior predictive，而不是只用 provider 边际正确率；
- 把 fit-only Best Fixed 作为显式 fallback/reference，不能把一次 challenger 选择永久
  变成新的参照；
- 对 runtime failure 与 valid-return wrong label 分开更新；
- 先有独立小流 verifier，再决定是否做 full common-stratum receipt。

已有 `mira_veto_ablation_v1` 的 controlled-event 结果还显示，早期正向信号主要来自连续
runtime-failure streak；correctness Beta veto 在 no-event 上会造成静态损失。因此下一轮若
实现 context shield，至少要保留 Frozen、availability-only streak、correctness-only 和
no-update 对照，并把 controlled outage 与自然动态分开。受控事件不能外推成自然漂移或
统计安全保证。

这一步没有降低 MIRA 的目标主张；它否定的是当前这个边际账本 + 路径依赖的候选实现。
