# ContextProbe full receipt 独立方法审查

日期：2026-10-02
审查对象：`mechanism_analysis_v2.json` 与原始 `events.jsonl`  来源：独立只读审查，不修改策略和结果。

## 可确认的事实

- ContextProbe 的 30 条 stream 共 4,332 个 task；895 次 probe、3,437 次 shield action。
- probe 正确 `757/895=0.845810`，shield 正确 `3,148/3,437=0.915915`，总正确 `3,905/4,332=0.901431`。
- 相对每个 fold 的 fit-only fallback，harmed=`99`、rescued=`38`。895 次离开 fallback 的动作全部是 forced probe。
- 当前 receipt 没有保存 `shield_feasible_challengers`：`MIRAContextProbe.decision_log_payload()` 只写入 schedule 字段，因此不能从这份 receipt 判断 gate 是否打开。可确认的事实是，3,437 条 shield decision 从未真正选过非 fallback provider；“没有可行 challenger”和“可行但未超过 fallback”在现有日志中不可区分。
- 事件里的 `action_changes_after_release` 会把 probe/fallback 之间的切换也算进去；真正的 exploitation 指标应单独统计 shield-to-shield action change。当前 receipt 能确认后者为 0，不能把前者直接叫作 adaptive switch。
- 30 条 Probe stream 的全局 union 覆盖了 32/32 个非 fallback provider，但每条 stream 都有未覆盖的非 fallback provider；因此全局 coverage 不能证明每个在线状态都可学习。
- 当前 probe schedule 绕过了 ContextShield 的 hard availability、family block 和 runtime hazard。已被标记 blocked 的 provider 仍可能被强制调用，这解释了为什么 probe 分支不能被称为安全选择。

## 方法学结论

ContextProbe 的回放合同和延迟反馈边界是可信的，但当前收据不能识别三种效应：probe 直接命中、probe feedback 更新、更新后 shield exploitation。缺少同 schedule 的 `ProbeNoUpdate` 和 `NoProbeShield` 对照，也没有 molecule/scaffold paired interval；同时 decision payload 不完整，无法审计 gate 的可行集合。因此它只能作为负结果和协议诊断，不能支持动态收益或实时安全切换 claim。

## 下一候选的硬约束

1. probe 只能从当前菜单中未被 hard block、family block 和 runtime hazard 排除的 challenger 抽取；动态菜单及精确 propensity 必须写入事件。
2. 固定同一 schedule 比较 `NoProbeShield`、`ProbeNoUpdate` 和 `ProbeSharedResidual`，分别估计 `ProbeSharedResidual−ProbeNoUpdate` 与 `ProbeNoUpdate−NoProbeShield`。
3. 只有 release 后 shield action 真正改变，且该改变能由独立 verifier 重算，才算 selected feedback 进入 exploitation。
4. 结果按 molecule/scaffold cluster 做 paired bootstrap，并单独报告 harmed、rescued、switch-away、runtime failure、probe/update 成本。
5. 每条 shield decision 必须记录 `available`、`feasible_challengers`、预测分数和 gate action；缺失这些字段时，停止机制归因而不是用空列表补默认值。

## 对当前项目的影响

ContextProbe v1 保持“合同通过、候选停止”的状态。更正 coverage 文案为 32 个非 fallback provider；不重跑旧 receipt，不申请 A800。下一候选先过静态可行动率和机制 stop test，再决定是否进入完整 CPU 矩阵。
