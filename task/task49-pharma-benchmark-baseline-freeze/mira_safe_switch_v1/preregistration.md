# MIRA-SafeSwitch CPU 小流预登记

日期：2026-10-02
状态：待执行；本文件先于实验。只使用已封存 BBB development cache，不读 confirmation。

## Claim

在当前 common-stratum 的 selected-only 延迟反馈合同下，incumbent-safe switching
能减少 MIRA-FamilyGuard 的 harmful switch，同时保留大部分 rescue。

## Comparator

- Frozen direct；
- Matched Online-SGD；
- 当前 MIRA-FamilyGuard；
- MIRA-SafeSwitch（本目录 proposal 中的同一公式）。

所有方法共用已封存输入、fold/seed/delay、33-provider menu、hidden-64 direct prior、
feedback release tick 和 provider cache。SafeSwitch 只读已释放 selected feedback。

## 固定配置

- base score：sealed validation-selected direct head；
- correctness prior：fit-only provider correctness，$\kappa^r_a=\max(20,V_{a,\mathrm{fit}})$；
- runtime prior：fit-only provider runtime failure，$\kappa^f_a=\max(20,V_{a,\mathrm{fit}}+F_{a,\mathrm{fit}})$；
- correction blend：$\rho=0.5$；
- incumbent margin：$\gamma=0$（不增加人工 margin，LCB 本身作为门）；
- reliability confidence：$\delta_r=0.05$；
- hazard confidence：$\delta_h=0.05$；
- minimum correctness evidence：$n_{\min}=20$；
- hazard slack：$\eta_h=0.02$；
- no discount and no extra exploration；
- delays：0, 1, 4；seed slots and five folds from the sealed manifest.

这组参数不在 evaluation/confirmation 上搜索。gamma 不做 sensitivity；如果需要其他
描述性 ablation，必须在同一 run 之后单独登记，不选最优点改写主结果。

`n_min` 的资格口径固定为
$N^{\mathrm{fit}}_a+C_a^++C_a^-$，其中 $N^{\mathrm{fit}}_a$ 是 fit-only 区域的有效返回数。
不能把它解释成“在线已选 20 次”：在 selected-only 且无探索的协议下，那会造成冷启动死锁。
fit 区统计只写入配置和先验，不在 evaluation 期间更新。

切换门固定为 challenger 的 correctness LCB $≥$ 当前 incumbent 的 posterior mean
`+ gamma`，而不是把 incumbent 再加一个 UCB。incumbent 已经是当前部署动作，双侧门在
本数据规模下会把候选全部挡住，退化成永久 fixed；单侧门只表达“替代者要有足够证据超过
当前参考”，不宣称形式化安全概率保证。

## Primary estimands

1. task success；
2. harmed/rescued 相对同一 fold fit-only Best Fixed；
3. switch-away rate；
4. runtime failure 和 valid-return wrong label；
5. selection/update p50/p95、state bytes；
6. molecule-level scaffold-cluster paired bootstrap。

## 通过和否定条件

候选只有在 harmed ≤ 0.85 × 222、rescued ≥ 0.80 × 188、runtime failure ≤ 10
且没有破坏事件恢复时，才获得“支持安全切换机制”的 development 结论。任一条件不满足，
保留负结果，不继续扩大跑批。

## 证据要求

实验前保存 config、源代码 commit、输入/cache hash；实验中逐事件写 decision/terminal/
feedback；实验后用不导入 runner 的独立 recovery 重算 action、状态、四格分解和聚合指标。
CPU 小流只用于接口和公式检查；完整 4,332-cell receipt 若要执行，需先复核小流没有
信息泄漏和计数错误。
