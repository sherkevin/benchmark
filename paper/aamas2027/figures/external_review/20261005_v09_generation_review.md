# v09 生成图复核：当前最佳视觉草图

日期：2026-10-05  
对象：用户根据 `prompts/figure2/v09.md` 生成并直接附图的 Figure 2。

## 复核结论

v09 已经达到“可作为正式重绘底稿”的标准，暂不继续增加 AI prompt 版本。它解决了 v08 的两个核心问题：

- 画布压缩为两层紧凑布局，空白明显减少；
- 反馈链实际画成 `terminal outcome → selected outcome → selected-only queue → released outcome → relay update → S_τt → first action`，没有再跳过 selected outcome 或 queue。

## 语义检查

- `terminal record → frozen endpoint judge → chemical correctness` 是连续主链；
- runtime status 的 `system failure` 分支绕过 chemical correctness，并在 `terminal outcome` 汇合；
- selected outcome 只有在终态之后出现；
- queue 用虚线表达延迟，`τ_t=t+1+D` 和 `event hidden until τ_t` 清楚；
- released state 只影响 release 之后的 future selector；
- 未选输出没有数据箭头进入主流程。

## 正式重绘时的小修

这些不是继续生成 prompt 的理由，而是 SVG/HTML 重绘时的局部规范：

1. 把底部 `future selector reads released state` 回路线减细、减淡，避免它压过主数据流；
2. 左侧 `unselected output not observed` 卡片移离回路竖线，或在最终 Figure 2 中删掉，因为 Figure 1 已经说明 selected-only 边界；
3. 如果保留 chemical correctness 标签，在 caption 或小字中注明 valid return only；不要增加长句；
4. 最终论文图仍需使用可编辑 SVG/HTML/PDF，并在单栏尺寸下复核字号。

## 状态

v09 是当前最优 AI 视觉资产，历史 v01–v08 全部保留。根据 ADR 0010，正式图继续使用 AI 生成结果；`figure2/v10/` 和 `figure2/v11/` 只作内部语义/版式审计，不替代 AI 图。正文嵌入前需要确认宽幅图片的最终排版尺寸。
