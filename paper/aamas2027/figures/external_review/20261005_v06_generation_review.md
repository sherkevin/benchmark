# v06 生成图复核与 v07 重构决定

日期：2026-10-05  
对象：用户根据 `prompts/figure2/v06.md` 生成并直接附图的 Figure 2。

## 直接复核

这张图的模块文字基本可读，但整体被画成了“时间表格”：三条横向泳道、五条竖向虚线和顶部列标题共同形成了大量格线。它削弱了模块之间的数据流，也让图更像审计表而不是方法架构图。

存在三处时序误导：

1. `selected outcome` 出现在 `selected provider` 下方并先于 call、terminal record 和 endpoint judge，像是调用前就进入 pending queue。
2. `frozen endpoint judge` 和 `chemical correctness` 被放在 pending/release 列，像是结果要等延迟结束才被判定；实际语义是动作终止后立即形成终态记录，只有已选结果对后续状态的可见性被延迟。
3. `S_t` 被画成跨越整个 pending 区间的固定状态轨道，容易让人误读为延迟期间没有其他已释放事件；这里应表达“本事件隐藏”，而不是保证所有状态都不变。

另外，fallback/diagnostic 分支放在 provider/call 之前会混淆它和运行失败、终态记录的关系。Figure 1 已经承担诊断账本的细节，Figure 2 应把视觉重点收回到 action → terminal → delayed release。

## v07 设计决定

保留 Figure 2 的问题和科学语义，但不再使用规则泳道和表格网格：

- 用一条横向 event spine 串起 `decision t → selected call → terminal → pending D → release / next action`；阶段只用小型 stage pills 和留白表达。
- 用下方一条紫色 delayed-feedback ribbon 表示 selected-only 结果从终态之后进入队列，经 `τ_t=t+1+D` 后释放，再进入 relay update。
- 用少量灰色时间点表示 `t+1 … τ_t−1` 期间本事件不可见，旁注允许其他已释放事件继续被使用。
- runtime failure、chemical correctness 和 fallback diagnostic 只保留局部短分支，不与 pending queue 或 relay update 相连。
- 不再使用大叉号；未选输出只用一个很小的灰色 closed-eye card 表示“not observed”。

## 样例库依据

v07 的构图参考了本地 gallery 的实际像素：

- `images/icml/final/icml2025-0242.jpg`（CollabLLM）：横向主脊、局部虚线胶囊和稀疏回折箭头；
- `images/iclr/final/iclr2026-1635.jpg`（TableDART）：淡色局部分区与留白代替贯穿网格；
- `images/iclr/final/iclr2024-17.jpg`（CRITIC）：主路径旁的局部诊断容器；
- `images/aaai/final/aaai2025-32038.jpg`（GenAL）：少量跨层反馈箭头而不是全图分隔线。

新提示词：`paper/aamas2027/figures/prompts/figure2/v07.md`（纯文本版：同目录 `v07.txt`）。

## 状态

v06 保留，不覆盖。v07 是下一轮生图提示词；尚未把任何新的 PNG 当作论文资产。Figure 2 的可编辑正式源文件仍为 `figure2/v9/figure2_v9.pdf`，等 v07 生成图通过语义和可读性复核后再决定是否重绘。
