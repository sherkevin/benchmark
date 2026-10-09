# Figure review and version selection

> 这是内部设计复核，不是外部审稿意见。每个版本保留，不覆盖旧文件。复核以 AAMAS 双栏实际尺寸和 topconf gallery 的 framework/pipeline 规律为准。

## 1. 评分规则

每项 0–2 分：0 = 未满足，1 = 部分满足，2 = 满足。满分 16。

1. **一句话信息**：只看图和 caption 能否说清本图要证明的事实。
2. **主线结构**：读者能否按单一方向读完，不需要追箭头。
3. **信息边界**：是否明确 action 前可见信息、selected-only feedback 和 release 时刻。
4. **方法映射**：图中模块是否和正文接口、变量及状态命名一致。
5. **版式**：按目标栏宽缩放后是否无重叠、无裁切、正文标签可读。
6. **视觉克制**：3–5 个主模块、少量语义色、没有装饰性噪音。
7. **黑白与无障碍**：不依赖红绿或单一颜色；AI 图片在黑白打印下仍可读，LaTeX 有 `\\Description{}`。
8. **可追溯与可复核**：保留 prompt、原始 AI 图片、生成版本、语义审查记录和可复核的 `\Description{}`；不要求人工重绘。

## 2. Figure 1 版本比较

| 版本 | 信息 | 结构 | 边界 | 方法 | 版式 | 克制 | 黑白 | 可编辑 | 合计 | 结论 |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---|
| v1 | 2 | 2 | 2 | 2 | 1 | 2 | 2 | 2 | 15/16 | 结构好，主标题和辅文在跨栏缩放后偏小 |
| v2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 16/16 | 历史版本；独立审图指出 runtime verifier、frozen judge、fallback ledger-only 和 release 语义未完全显式 |
| v3 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 16/16 | 历史版本；语义补齐但跨栏缩放后辅助文字偏小，Update panel 标题有溢出风险 |
| v4 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **15/16** | 选用；四个主区域、8pt 目标字号、显式 release 时钟和 ledger-only 分支。独立复核认为 context 区底部说明略贴近边界。 |
| v5 | 2 | 2 | 2 | 2 | 1 | 2 | 2 | 2 | **15/16** | 术语修正版；将 runtime verifier 改为 runtime ledger，并保留 released-only 语义。跨栏后辅助标签约 6.5pt，扣版式 1 分。 |
| v6 | 2 | 2 | 2 | 2 | 1 | 2 | 2 | 2 | **15/16** | 当前选定；字幕改为 immutable cold-start reference，避免把 fallback 写成安全保证；其余几何不变。最终跨栏辅助标签仍需按 100% PDF 检查。 |

独立审图对 v2 给出 14/16：版式和主线通过，但 runtime verifier、frozen endpoint judge、fallback ledger-only 与 release 后 next-visible-decision 需要显式化。v3 已补齐这些语义；未放实验数字，也没有把 probe 造成的动作变化说成效果。

## 3. Figure 2 版本比较

| 版本 | 信息 | 结构 | 边界 | 方法 | 版式 | 克制 | 黑白 | 可编辑 | 合计 | 结论 |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---|
| v1 | 2 | 2 | 2 | 2 | 1 | 2 | 2 | 2 | 15/16 | 结构清楚，但单栏目标字号偏小 |
| v2 | 2 | 1 | 2 | 2 | 0 | 2 | 2 | 2 | 13/16 | 字号放大过度，多个文字框重叠，淘汰 |
| v3 | 2 | 2 | 2 | 2 | 1 | 2 | 2 | 2 | 15/16 | 信息简化有效，但 SELECTOR 与首框相碰、边界线穿过终端框 |
| v4 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 16/16 | 几何问题已修正；保留为中间最佳版本 |
| v5 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 16/16 | 历史版本；未选输出仍可能被误读为流入 terminal，独立审图 11/16 |
| v6 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 16/16 | 历史版本；去掉误导箭头，独立审图 13/16，但 `chemical | valid` 仍可能暗示调用时已有真值 |
| v7 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 16/16 | 历史版本；语义正确但单栏辅助文字降到 4–5pt |
| v8 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **15/16** | 选用；纵向时序、正文标签约 8–9pt、独立 judge 和 release 时钟清楚。独立复核认为 family/fallback 语义部分依赖 caption，ghost 标记偏小但仍可辨认。 |
| v9 | 2 | 2 | 2 | 2 | 1 | 2 | 2 | 2 | **15/16** | 当前选定；terminal record 与 frozen judge 的直接 runner 语义已对齐。单栏正文约 7.8pt、ghost 约 5.4pt，扣版式 1 分。 |
| v10 | 2 | 2 | 2 | 2 | 1 | 2 | 2 | 2 | **15/16** | 宽幅可编辑重绘的第一版；用户 v09 几何已实现，但 ghost 与 pending markers 挤压局部层次，保留为中间版本。 |
| v11 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 0 | **14/16** | 人工 redraw 仅作内部语义参考；不能作为正式 AI 视觉资产。 |

独立审图：Figure 2 v6 为 13/16，主要问题是 `chemical | valid` 的时序暗示；v7 修正了标签语义，v8 进一步改成纵向时序以保留单栏 8pt 以上正文标签。v8 的 PDF 只有一页，未改变模板参数。

## 4. 用户新架构图复核（2026-10-05）

用户提供了两张新图，直接回应了旧图的两个问题：Figure 1 的模块边界和 Figure 2 的终端时序。两张图均按正文语义检查通过：

- `figure1/v07/figure1_v07.png`：四个区域明确分开 inputs/candidates、MIRA-Relay、tool execution/evaluation、delayed feedback/state update；fallback 只进入 diagnostic ledger，runtime failure 与 chemical correctness 分开。
- `figure2/v12/figure2_v12.png`：terminal record 先于 frozen endpoint judge，judge 与 runtime failure 汇入 terminal outcome；selected outcome 才进入 queue，release 后才更新 relay/state。

两张图都采用宽幅布局，嵌入正文后正文仍保持 8 页。图片源、哈希和语义合同分别保存在两个版本目录的 `MANIFEST.md`，Figure 2 另有 `REVIEW.md`。

## 5. 选定版本

- Figure 1：`figure1/v07/figure1_v07.png`
- Figure 2：`figure2/v12/figure2_v12.png`

这两个版本进入下一轮整篇 PDF 编译。旧版本不能删除；如果整篇排版显示文字仍不足，优先删掉次要说明或改 caption，不改 AAMAS 模板字号和页边距。

## 6. 仍需整篇复核

- 将 Figure 1/2 的选定图片嵌入 `main.tex`，重新做干净的单进程编译；不覆盖历史版本。
- 在最终 PDF 中分别检查跨栏和单栏的 100% 阅读尺寸、浮动位置、caption 和 `\\Description{}`。
- 检查 Figure 1 的 `MIRA-Relay`、family residual、provider bias、hazard ledger、runtime ledger/frozen judge 和 release clock 是否与方法段落一致；发现 AI 文字/箭头错误时回到 prompt 迭代。
- 检查 Figure 2 的 provider output、frozen endpoint judge、selected-only queue 和 fallback ledger-only 语义是否与正文一致。
- 没有真实实验结果前，不增加结果图；实验曲线和误差带待 E1–E6 收据完成后再画。
