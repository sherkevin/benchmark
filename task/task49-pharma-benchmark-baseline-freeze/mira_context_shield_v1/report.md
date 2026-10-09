# MIRA-ContextShield 诊断报告

日期：2026-10-02  
状态：**候选停止，不进入 full receipt 或 confirmation**。

## 预登记版本

本候选使用 common BBB development cache、fold0/seed0、33-provider 完整菜单和
selected-only delayed feedback。fit curvature 固定为

$$
0.01 I + \frac{1}{N_{fit}}\sum_i p_i(1-p_i)\phi_i\phi_i^\top,
$$

在线只更新释放过的 selected `system_success`；valid wrong 和 runtime failure 都是
`y=0`，但 failure 的 `chemical_correct` 保持 null。gate 是 logit-space 单侧门，reference
是不可变的 fit-only Best Fixed，runtime failure 使用独立 Beta hazard ledger。

## 先做的静态停止测试

在任何 online replay 之前，只用 fit snapshot 和 evaluation molecule features 计算 gate 是否
有机会打开；不读取 evaluation label，也不执行 policy。机器收据：

`experiments/logs/task49_mira_context_shield_gate_diagnostic_20261002/v1.json`

| context gate 的 $δ$ | 至少有一个 challenger 的 molecule 数 | 打开的 challenger cell 数 |
|---:|---:|---:|
| 0（无 logit 半径） | 119/145 | 377 |
| 0.05（预登记） | 0/145 | 0 |
| 0.25 | 0/145 | 0 |
| 0.40 | 0/145 | 0 |

预登记版本在静态阶段已经没有可行动的 challenger。它会一直调用 fallback，selected
feedback 虽然能更新 fallback 的 state，但不能产生 provider switch；继续调 $λ$ 或 $δ$
会把这个停止测试变成事后搜索，因此不做。

## 已完成的实现性 smoke

- `v1/`：在 state hash 接口处失败，保留失败现场。
- `v2/`：旧 Gaussian-surrogate 版本，2 streams/192 events，已被数学修订覆盖。
- `v3/`：Bernoulli local-MAP 版本、旧 delta gate，2 streams/192 events；Frozen `28/32`，
  ContextShield `27/32`，gate-open=0。
- `v4/`：fit mean Fisher 版本，2 streams/192 events；独立恢复通过，仍 gate-open=0。

当前候选代码已进一步切换为 logit-space gate，并补了独立恢复器和 Bernoulli MAP 数值测试；
上述 smoke 只能作为历史实现收据，不能合并为方法效果。它们都没有读取 confirmation。

## 结论和下一步

ContextShield 的局部 MAP 更新数学上可实现，且 selected feedback 的信息边界可以复核；失败
在 gate 可识别性和候选覆盖，不在二分求根或 rank-one 更新。没有探索时，未选 provider 的
当前质量不可由 selected-only history 识别。

下一候选必须先写一张新实验卡，固定一个与结果无关的 exploration/audit schedule（例如
固定比例的 challenger probe），并同时报告 probe 成本、coverage、harmed/rescued 和
release 后真正影响后续 action 的次数。它不能把现有 0/145 gate-open 的结果改写成正结果。
