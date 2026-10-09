# Static router role sufficiency audit

日期：2026-09-30
状态：development baseline 资格审查；不改变 confirmation。

## 问题

Gate B 需要一个能回答“收益是否只来自静态全 panel 训练/表示”的强静态对照。此前把 GraphRouter 列成待运行项，但它的原始接口需要 provider description embedding、图 edge 语义和 `torch_geometric`；当前 BBB routing layer 没有冻结这些输入。直接删掉这些输入后重写，不能再称为 GraphRouter。

## 已有等价静态对照

`fingerprint_diagnostic_v1` 已在完全相同的 722 分子、33 provider、五个 scaffold-disjoint outer folds 上训练并独立恢复：

- **MLP BCE**：Morgan-1024 分子特征，33 个具体 provider 输出，fit 区 full-panel `system_success`，64 hidden units、200 epoch AdamW，validation 只选 epoch；evaluation 不更新。
- **BT + CLS**：同一输入和 split 的 provider pair objective 加 correctness auxiliary loss；仍是 fit-only static router。
- 两个确定性 seed 均有 raw training log、checkpoint、独立 NumPy score recovery、逐分子结果和失败/Oracle/Best Fixed 对照。

结果：MLP BCE seed 0/1 为 `0.8961/0.8906`，BT+CLS 为 `0.8878/0.8947`；Best Fixed 为 `0.9155`，Oracle 为 `0.9972`。它们没有超过固定 incumbent，但已经能作为静态 full-panel router 的强负对照，防止把 online gain 误解为“只要训练一个静态 head 就有”。

## GraphRouter 的边界

GraphRouter 仍保留论文和源码审计，来源是 ICLR 2025 论文与 MIT 许可 commit `0edfb8758d45acd93c8029354867ffb49bb8a97f`。当前不把它硬改成 BBB baseline，原因是：

1. provider description/version embedding 尚未有唯一、可追溯的公共输入；
2. BBB 的单性质 provider edge 语义不能直接替代其原始 multi-task/new-LLM edge；
3. 本机缺少 `torch_geometric`，自行重写会增加实现偏差；
4. GraphRouter 是 offline/static/few-shot 对照，不能回答 selected-only delayed adaptation。

## 结论和动作

对于“静态训练/表示能否解释收益”的角色，MLP BCE/BT+CLS 已提供同任务、同菜单、同 split、可独立恢复的 task-native static router；PILOT-adapted、BayesianRouter-adapted 和 Greedy LinUCB 提供现代在线近邻。GraphRouter 不作为可运行主表行，保留为 audit-only，并在论文中说明输入和语义不匹配的原因。

这不是把 baseline 要求降低：主表仍须保留 Random、Best Fixed、Frozen、matched Online/Replay、至少一个现代 online nearest、一个静态 full-panel router 和 Oracle；每一类都必须有同合同 raw run/recovery。当前 Gate B 仍 HOLD，剩余硬缺口是统一 raw-run contract、provider lineage/model-fresh 资格和 Laya 的正式范围裁定。

证据入口：

- `task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/report.md`
- `experiments/logs/task49_fingerprint_diagnostic_20260926/recovered_v4/summary.json`
- `task/task49-pharma-benchmark-baseline-freeze/graphrouter_static_adapter_v1/blocking_report.md`
