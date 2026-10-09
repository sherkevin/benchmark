# MIRA-FamilyGuard v1 设计检查

日期：2026-10-01

## 结论

这个候选针对 full receipt 暴露的具体缺口：provider-local failure ledger 不能快速隔离共享 family。它不是把不同 provider 的 chemical correctness 混在一起，而是把 runtime availability 作为独立的 group-level hard channel；selected-only 边界仍保留。

## 需要先解决的风险

1. **group metadata 是否泄漏**：family registry 必须在运行前封存，不能由 event 后结果反推；所有方法读取同一 registry。
2. **cold start 是否改变**：group factor 必须按先验归一化为 1，candidate no-event prefix 才能和 current relative 做公平 paired。
3. **transient 误杀**：单次 failure 只更新 posterior 和 streak，不立即 gate；cooldown 和 streak 是 hard gate 的唯一触发条件。
4. **persistent 漂移**：group gate 后仍允许 cooldown 到期重新尝试，避免永久拒答；每次尝试仍受 selected-only 反馈约束。
5. **统计单位**：candidate 的 no-event 与 event 必须用同一坐标、同一 target manifest 和同一 control receipt；不能只拿 event 后 raw accuracy 比旧方法。

## 可实现性

反馈 release 时只需更新两个 group counters、一个 streak 和一个 cooldown tick，时间复杂度 O(1)。选择时对 33 个 provider 读取其 group factor，复杂度 O(33)，状态大小 O(number of groups)。状态 hash 必须包含 group counters/streak/cooldown，verifier 要逐流重算。

## 目前不能声称的内容

FamilyGuard 还没有实验结果，不能写成已优于 matched-SGD，也不能把 family-level stress 说成自然 provider drift。它只是下一轮可证伪的修复候选。

## 2026-10-01 执行后复核

上面的“尚未有实验结果”是执行前设计审查的历史状态，保留不改写。候选随后完成了两条独立回执：

- common Frozen-incumbent family-outage：300 条 stream，独立 verifier `300/300 verified`；persistent `S=23/30` 上相对 current relative 的 release-after 改善为 `+7.60/+12.24/+22.47 pp`，三个 paired bootstrap 区间均在 0 以上，transient 为 0；
- common-stratum 主分母：180 条 stream，独立 action/state/evaluator recovery 通过；FamilyGuard−Frozen 和 FamilyGuard−MIRA-relative 的 scaffold paired 区间仍跨 0。

因此设计审查中的风险判断得到如下更新：family registry、cold-start、transient gate、persistent cooldown 和状态复核均已通过回执验证；候选自身的受控压力判据通过，但普通 development superiority、自然环境非平稳、model-fresh 泛化和 confirmation 仍未证明。结果入口分别为 `family_guard_v1_report.md` 和 `family_guard_common_stratum_report.md`。
