# MIRA-ContextProbe 候选方法卡

日期：2026-10-02  
状态：候选已实现并通过 CPU contract smoke；尚无效果结论，不进入 confirmation。

## 为什么要换候选

ContextShield 的局部 Bernoulli MAP 更新可以实时算，但在固定 fit reference 和 95% logit
门下，fold0/seed0 的 `gate-open=0/145`。没有选中的 provider 没有当前分子反馈，单靠
fit covariance 不能识别它是否值得切换。继续调门槛会把静态停止测试变成事后搜索。

## 协议

每轮仍先释放到期的 selected feedback。决策时把动作分成两个互斥分支：

1. 以预先固定的概率 $ε=.20$ 进入 probe 分支；probe provider 从除 fallback 外的菜单按
   stream-specific PRNG 均匀抽取，propensity 记录为 `ε/(K-1)`。stream seed 由
   `SeedSequence([base_seed, fold, seed, delay])` 从 base seed `20261002` 派生；同一
   stream 的 Frozen/Probe 对照共享它，不同 stream 不重置为同一随机序列。
2. 其余 `1-ε` 进入 shield 分支，使用 ContextShield 的不可变 fit reference、logit-space
   lower gate 和独立 runtime hazard ledger；没有可行 challenger 时调用 fallback。

probe 分支只由 `(stream_seed, decision_tick, candidate_menu)` 决定，不读取分数、标签、运行
状态或未来结果。probe 反馈仍按 `decision_tick + 1 + delay` 延迟释放；未释放反馈不能影响动作。

这不是把 exploration 叫成创新。它是为 selected-only 反馈补上可审查的 coverage 条件，
使我们可以测“实时反馈是否改变后续动作”，并把 probe 造成的错误和调用成本单独报告。

## 必须记录

每条 decision 增加 `is_probe`、`probe_seed`、`probe_index`、`propensity`、`gate_action`、
`released_feedback_ids` 和 `decision_reads`。主结果分成 probe 与 shield 两部分，同时报告：

- 每个 provider 的 selected 次数、释放反馈次数和有效返回次数；
- probe 比例、propensity、delay 下真正影响后续动作的 release 数；
- fixed-reference harmed/rescued/both、task success、runtime failure、unknown/refusal；
- selection/update p50/p95、状态字节和 probe 调用成本；
- molecule/scaffold 成对区间，不能只报 pooled accuracy。

## 近邻和对照

必须有 no-probe ContextShield、同一 probe schedule 的 Frozen/Best Fixed、Matched Online-SGD、
FamilyGuard/SafeSwitch，以及 ContextShield 的 no-hazard/hazard-only、no-gate 和 frozen-state
消融。所有方法共享 probe schedule 或明确分层，不能用 probe 预算提高某一方法的比较权限。

## 停止条件

- PRNG 结果不能由独立 verifier 从 stream seed、tick、菜单重算；
- 完整 common-stratum receipt 中仍有未被 probe 的非 fallback provider，或反馈 release 后
  state/action 没变化；小流中的空臂只作为 coverage 分母报告，不单独作停止结论；
- 主要收益只来自 probe 直接选到赢家，shield 分支没有作用；
- probe 成本造成的 harmed 不单列，或把 forced probe 的结果写成纯 exploitation 效果；
- 未选 provider 标签、整行工具输出或 evaluation truth 进入 probe/action/update。

这些条件触发时保留负结果，不调 $ε$、seed、门槛或 probe 顺序追分。
