# v07 生成图复核与 v08 修正

日期：2026-10-05  
对象：用户根据 `prompts/figure2/v07.md` 生成并直接附图的 Figure 2。

## 已通过

- 横向 event spine、五个 stage pills、下方紫色延迟带已经替代表格泳道；横竖网格问题解决。
- 输入、selector、单个 selected provider、call、terminal record、judge、release 和 future action 的模块层次清楚。
- `selected-only queue`、`τ_t=t+1+D`、`t+1 … τ_t−1` 和“this event hidden”均可读。
- 颜色和线型分工稳定，整体更接近样例库中的 framework figure；没有第二次调用或大叉号。

## 必须修的语义问题

1. 图中从 `terminal record` 直接向下连到 `selected outcome`。这绕过了 `frozen endpoint judge`，会让人误读为结果在判定前已经进入队列。
2. `terminal record` 直接弯箭头指向 `chemical correctness`，同时 `frozen endpoint judge` 变成旁路模块。正确顺序应是 `terminal record → frozen endpoint judge → chemical correctness`。
3. `system failure`、`chemical correctness` 和 selected outcome 没有显式汇合点。需要一个小的 `terminal outcome` 汇合节点：valid return 的 chemical correctness 和 runtime failure 都在终态之后形成 selected outcome，再进入延迟队列。
4. `fallback / diagnostic only` 在 Figure 2 中增加了支路，且来源不够明确。它已经由 Figure 1 承担，下一版删掉以减少文字和歧义。

## v08 重绘规则

- 保留 v07 的整体构图和风格，不重新引入泳道或网格。
- 严格禁止 `terminal record → selected outcome`、`terminal record → chemical correctness` 的直连。
- 只允许 `terminal record → judge → chemical correctness`，以及 `terminal record/runtime status → system failure`；两者汇入 `terminal outcome` 后才进入 `selected outcome`。
- Figure 2 不画 fallback ledger；该账本只在 Figure 1 出现。

新提示词：`paper/aamas2027/figures/prompts/figure2/v08.md`（纯文本版：同目录 `v08.txt`）。

## 状态

v07 保留，不能作为正式论文图。v08 是下一轮生图提示词；正式可编辑 Figure 2 仍为 `figure2/v9/figure2_v9.pdf`，等待 v08 通过语义复核后再决定是否重绘。
