# MIRA-ContextProbe CPU 小流预登记

日期：2026-10-02  
状态：代码和 verifier 已通过；先完成 coverage 诊断，再运行 CPU 小流；不进入 confirmation。

## 唯一问题

在固定 `epsilon=.20`、seed=`20261002` 的结果无关 probe schedule 下，selected-only 延迟反馈
是否终于能产生可观测的后续 action/state change；probe 成本是否抵消了 shield 的收益。

## 固定合同

- 输入、fold/seed、33-provider menu、cache、truth、特征和 direct BCE head 沿用 common-stratum；
  confirmation 关闭。
- probe schedule 是独立 PRNG：每个 decision 先采 Bernoulli(`.20`)，probe 时在除固定
  fit-only fallback 外的 provider 中均匀抽取；随机数只由 base seed、`fold/seed/delay`
  派生的 stream seed、决策顺序和候选菜单驱动。Frozen/Probe 在同一 stream 共享 seed，
  不同 stream 不重置为同一随机序列。
- shield 分支使用 ContextShield 已登记的 $λ=.01$、fit-row mean Fisher、$δ=.05$、
  $δ_h=.05$、$η_h=.02$；不在 evaluation 上调参。
- feedback 只在 `decision_tick + 1 + delay` 到期后释放，未知真值停止该 stream；runtime
  failure 的 `chemical_correct` 保持 null，`system_success=0` 才进入在线目标。

## 通过前必须完成

1. 先生成静态 coverage 诊断，保存每臂期望/实际 probe 次数；小流允许出现空臂，
   但完整 common-stratum receipt 的所有非 fallback provider 必须至少被 probe 一次；
   否则停止，不进入效果解释；
2. 独立 verifier 重算 PRNG、propensity、action、posterior、hazard、delay 和 state hash；
3. 同一 schedule 运行 no-probe、Frozen、Best Fixed、Matched Online-SGD 和 ContextProbe；
4. 结果按 probe/shield 分层，计算 fixed-reference harmed/rescued/both 和 scaffold paired CI。

## 否定条件

- 任何未选 provider 的输出或标签进入决策；
- 完整 receipt 中存在未被 probe 的非 fallback provider，或 coverage、propensity 或
  release 账本无法独立恢复；
- shield 分支没有 release 后 action/state change；
- ContextProbe 不优于同 schedule 的 no-probe ContextShield，或收益完全由 probe 直接贡献；
- 结果只有 pooled accuracy，没有成本、延迟、失败和覆盖分母。
