# AI 图版本画廊

打开 `index.html` 可以逐张浏览 Figure 1 和 Figure 2 的全部 AI 原图版本。缩略图链接到原 PNG；每张卡片还链接完整 prompt、hash 和已有独立审查。`figure1_all_versions_contact_sheet.jpg`、`figure2_all_versions_contact_sheet.jpg` 是快速对照页。

- Figure 1：v01–v34，另含多次辅助输出；v25–v27 的边界写入问题均已记录为不通过，v29 用户拒绝（仍像流程图）、v31/v32 的旁路问题均保留失败记录，v33 为完整架构重绘的上一版候选；独立审查 16/16、无硬错误，7-inch 通过。v34 是在 v33 上仅增加 Query 分子 glyph、去掉公式箭头引线的微调版；独立审查 16/16、无硬错误，7-inch 通过，当前推荐候选（主版未同步）。所有旧审查与 7-inch 记录继续保留。
- Figure 2：v07–v27；v25/v26 旧候选继续保留，v27 独立审查 16/16、无硬错误，7-inch 通过，当前推荐候选。
- 当前 QA 主版：`../qa/main.pdf`，暂用 Figure 1 v09 + Figure 2 v25；只有审查通过并经用户选择才会同步。
- 没有删除或覆盖任何原图；版本 hash 在 `manifest.json` 及各自 manifest 中。旧画廊快照以 `before_v*.html/json/jpg` 保存。
- Figure 3 是程序数据图，不在这个 AI 画廊中。
