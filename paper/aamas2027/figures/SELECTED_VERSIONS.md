# Selected figure versions

2026-10-07 用户澄清后的唯一图稿候选入口。制作方式遵循
[决议 0012](../../../docs/决议/0012-按图类型区分AI绘图与数据绘图.md)。

## 当前候选

| Figure | 类型 | 路径 | 来源 |
|---|---|---|---|
| 1 | AI 架构图 | `generated/figure1/v34_ai.png`（当前候选，主版暂不变） | v34 在 v33 的完整架构重绘上只增加 Query 分子 glyph并去掉公式箭头引线；独立审查 16/16、无硬错误且 7-inch 通过；用户最终选择待完成。v33 与 v29–v32 的原图、审查和失败记录继续保留 |
| 2 | AI 流程图 | `generated/figure2/v27_ai.png`（当前候选，主版暂不变） | v27 在 v26 的单事件时间线上放大纸面次级标签，独立审查 16/16、无硬错误且 7-inch 通过；v26 与 v25 保留，等待用户最终选择 |
| 3 | 程序数据图 | `figure3/v02_data/figure3_v02.pdf` | `figure3/v02_data/render.py` + `data.json` |

Figure 3 同时输出 SVG、300 dpi PNG、精确数据 CSV 和原生 LaTeX 表格。
复核见 `external_review/20261007_figure3_data_v02_review.md`。AI Figure 3
v01–v05 和旧的三张 AI 图联合审查仅供历史比较，不再作为生效选择依据。

## 阅读稿与投稿构建引用

`main.tex` 的默认 Figure 1 已恢复为 AI v09，Figure 2 暂仍为程序 v13，尚无
Figure 3。`make draft` 生成该阅读稿。流程图 v27 是最新通过候选，但在用户选择前不自动切换正文。
完整本轮 prompt、图库分析、原图、失败迭代和审查均保留；全部版本可在
`version_gallery_20261007/index.html` 对照；独立版本通过审查并经用户选择后同步到 `figures/qa/main.*`。

`make submission` 显式定义 `MIRASubmissionFigures`，Figure 1 选择程序 v08，
Figure 2 继续程序 v13。两种输出在索引和构建中分别标明，避免再次静默
替换阅读图；AI 阅读图的制作方式不等于满足 AAMAS 官方的投稿图像条件。

用户原图 `Downloads/MIRA-Relay延迟反馈架构图 (1).png` 与
`figure1/v07/figure1_v07.png` 完全相同，hash 见 v07 manifest，没有丢失。
AI v06/v07/v08 的失败审查与 v09/v10 一起保存；程序 v08 也不覆盖。决议 0013
要求所有 AI 原图保留并展示供用户选择。
