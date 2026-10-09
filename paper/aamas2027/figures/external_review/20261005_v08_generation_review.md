# v08 生成图复核与 v09 重构决定

日期：2026-10-05  
对象：用户根据 `prompts/figure2/v08.md` 生成并直接附图的 Figure 2。

## 已通过

- v07 的网格问题没有回来；横向主线、五个 stage pills 和紫色延迟带仍然清楚。
- `terminal record → frozen endpoint judge → chemical correctness` 已经画成连续链，judge bypass 问题解决。
- `system failure` 与 chemical correctness 分开，Figure 2 不再放 fallback 账本，内容更干净。

## 当前问题

1. 布局过于松散：上下两层距离大，左右卡片间距大，画面中间出现大片空白，整体不像紧凑的论文架构图。
2. `terminal outcome` 的竖向箭头落到 `released outcome`，跳过了 `selected outcome` 和 selected-only queue；下方的 selected outcome 变成了一个没有上游的孤立卡片。
3. stage pills 与实际模块距离较远，增加了纵向空白；底部说明文字也把反馈带进一步拉开。

## v09 设计决定

- 画布压缩为 1600×560，使用两个靠近的视觉 band，控制上下间距和外边距。
- 上方保持 `state → selector → provider → call → terminal → judge → chemical correctness` 直线；`system failure` 与 chemical correctness 汇入 terminal outcome。
- 下方必须是一条不可跳过的链：`terminal outcome → selected outcome → selected-only queue → released outcome → relay update → S_τt → first action`。
- 明确禁止 terminal outcome 直接连 released outcome，禁止任何箭头绕过 selected outcome 或 queue。
- 将“this event hidden; other released events may still be used”压缩为“event hidden until τ_t”，把其他解释交给 caption/正文。

新提示词：`paper/aamas2027/figures/prompts/figure2/v09.md`（纯文本版：同目录 `v09.txt`）。

## 状态

v08 保留，不能作为正式论文图。v09 是下一轮生图提示词；正式可编辑 Figure 2 仍为 `figure2/v9/figure2_v9.pdf`，等待 v09 通过语义和紧凑性复核后再决定是否重绘。
