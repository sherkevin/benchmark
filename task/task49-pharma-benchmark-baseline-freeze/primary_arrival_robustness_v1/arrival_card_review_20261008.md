# Primary arrival robustness：冻结卡、实现审查与结果报告一致性审查

日期：2026-10-08
审查对象：`experiment_card_draft.md`、冻结版 `experiment_card.md`、`implementation_review.md`、`report.md` 和 `resource_go.json`。
审查范围：三份生效评价标准、当前 MIRA-Relay 主线和已完成的 arrival replay。
本审查只读，不运行实验，不修改主版或上述文件。

## 结论

需要区分两个时间点：

1. **draft + implementation_review 单独看：不能冻结或执行。** 当时仍缺确认版 card、feature array 的 hash、紧凑回执合同、原序 smoke/resource GO 和独立 checker 的闭合。
2. **当前冻结版 + resource GO + report：执行条件已满足，实验已经完成。** `experiment_card.md` 在执行前冻结，`resource_go.json` 记录原序复现、独立检查和资源投影通过；完整 128-order replay 与 `full_v2` 独立核验也已在 `report.md` 中闭合。无需重跑。

这批证据只补到达顺序分布下的轨迹不确定性，不能关闭整体 Gate C，也不能把人工 controlled drift 解释成自然 lifecycle drift。`report.md` 保持这一边界，方向正确。

## 已核对且一致的部分

### 1. 估计对象与信息权限

- 157 个已查看分子、5 个冻结 prior、4 个 provider、2 个固定 policy seed、3 个 delay 和 128 个独立均匀排列在 card 与 report 中一致。
- 每个排列是一个独立统计单位；30 个 fold/seed/delay 配置先在排列内平均。分子、fold、delay 和重复 deterministic seed 没有被当成额外 Monte Carlo 样本。
- controlled 条件按新到达 tick 在 78–156（含端点）翻转 selected label；due feedback 的实际 label 保留。这与报告中“受控 drift 而非固定分子集合”的解释一致。
- card 明确禁止当前真值、未选输出和未来 feedback 进入 scorer；报告称在线 update 实际执行，没有 offline refit、provider forward、API 或 GPU。

### 2. baseline 和公平合同

- 主比较预先固定为 controlled 条件下 `MIRA-Relay − Online SGD`，且 card 明确 Online SGD 是在新排列产生前选定的 strongest strict control。报告的 `+4.7363 pp`、`125/128` 与冻结通过条件一致。
- Random、Best Fixed、Frozen、MIRA-Relative、Online SGD、Greedy LinUCB、BayesianRouter、MIRA-Relay、Retrospective Best Fixed 和 Fixed-permutation credit 的 roster 与报告表一致；Retrospective control 没有被当作可部署方法。
- card 保留了 native adapter 的限制，没有把 LinUCB/BayesianRouter 说成完整原生论文复现；这符合 B9/B10 的 scope 要求。

### 3. 统计、失败条件和负结果

- card 预登记 order-level Student-t Monte Carlo interval、正收益排列的 Clopper–Pearson 区间、order quantiles、min/max 和 histogram；report 给出了均值、MCSE、区间、125/128、负排列 69/86/90、分位数和自然条件负结果。
- card 预先区分 pooled 与 equal-scaffold estimand；report 没有用有利的 scaffold 结果替代 pooled primary，并保留了 natural pooled `−0.2870 pp`。
- `full_v1` 的 histogram 末位浮点失败没有被隐藏，`full_v2` 在明确的百分比运算合同下重新核验；report 同时保留了失败现场、原始记录和独立 verifier。
- 资源 GO 的 128-order scope、10 GiB/4 GiB/1 h 限制与实际 61,440 streams、9,646,080 decisions、约 3.145 GB、约 324 MB RSS 和 530.51 s 结果相容。

### 4. 恢复链和动态范围

- 冻结 card 的 compact event contract、orders hash、source/input hash、RNG state、pending conservation、independent checker 和 mutation tests，与 report 的 full_v2 验证描述一致。
- report 明确该结果只针对“固定已查看队列 + declared arrival law”，不是新 scaffold、model-fresh、confirmation 或部署泛化；这满足 M15、B7 和 R2 的范围要求。
- 因此该实验可以作为 Gate C4/B13 的 arrival-law 证据，但不应被写成方法新颖性或 agent 端到端成功证据。

## 需要做的文档修订（不需要重跑）

### P0：避免审计者看到互相冲突的执行状态

`implementation_review.md` 是执行前审查快照，但当前仍写着：

- 路径和 hash 记录在 `experiment_card_draft.md`；
- “No experiment was run”；
- “none has run under this card yet”。

这在其历史时间点是正确的，但与现有冻结版、`resource_go.json` 和 `report.md` 一起阅读会造成当前状态冲突。最小修订是给该文件加一段不可变的历史标注，明确：它是 **pre-freeze implementation review**，最终执行绑定的是 `experiment_card.md` SHA `9fc008e3...652f1`，后续结果与独立核验见 `report.md` 和 `full_v2`。不要直接把原审查文本改写成仿佛它在执行后完成。

### P1：补足结果报告的可审计字段

`report.md` 已给出完整流数和独立验证，但建议补三项短字段，避免审稿人必须进入 JSONL 才能确认：

1. 全部 61,440 streams completed，单 stream `n=157`、selected feedback count 为 157；failure/unknown/incomplete 的计数及分母明确为 0（若 artifact 中确实如此）；
2. `inputs/features.npy`、`orders.npy` 和 full-run manifest 的路径与 hash，尤其说明 feature array 是执行前一次生成并冻结的；
3. 结果段明确重申 Online SGD 是在排列抽样前依据历史 receipt 预选的 comparator，不是看了 128 个结果后挑出的赢家。

这些是展示层补充，不改变数值、card、metric 或实验估计量。

### P1：保留“方向检验”与“实际收益”区分

冻结 card 已写明零阈值只证明 direction，不构成非零 practical-benefit margin；report 也已写明。论文或主表继续使用本结果时，必须保留这一句，不能把 `+4.7363 pp` 的 order-law 区间改写成普遍部署收益，也不能用 125/128 代替新 cohort 或 confirmation 证据。

## 对三份评价标准的影响

- **故事线标准：局部通过。** arrival replay 提供“结果是否依赖单一记录顺序”的可反驳证据，并保留自然负结果；它不是新的 contribution 或 agent 闭环证据。H6/H8 的范围控制较好，完整 H4/H5 仍由主方法和 integration ablation 决定。
- **方法论标准：M9、M11、M14 获得有用证据。** 真实 release/update 顺序、RNG、pending queue、故障 mutation 和独立重放均有收据；它不能替代 verifier/refusal 或 full online agent 的证据。实现审查的历史状态需要上述 P0 文档标注，才不会损害 M7/M14 的可追溯性。
- **Benchmark/Baseline 标准：C4/B13 明显补强，但 Gate C 仍 HOLD。** order-level paired uncertainty、完整 roster、失败和负结果已具备；该实验不补新分子、自然生命周期、实践收益 margin 或 confirmation，因此不能单独把 Gate C 或整体科学 gate 置为通过。

## 最终建议

不再重跑 arrival 实验，也不改变 card 中的 128、roster、threshold、seed 或 metric。只做上述历史状态和 artifact 字段的文档收口，然后把 `report.md` 作为本实验唯一结果入口。后续实验复用其 order-level receipt/replay 接口，但必须回到当前主线的 verifier/拒答/agent 闭环缺口；不要把这项 robustness study 当成广义目标已经完成。
