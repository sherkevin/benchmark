# 0011 — AAMAS 投稿图使用程序绘制版本

- 状态：待双方确认
- 日期：2026-10-06
- 关联：0007、0010；AAMAS 2027 Submission Instructions

## 背景

项目曾将 AI 生成图作为正式视觉候选，并保留了完整的 prompt、版本和审查记录。AAMAS 2027 投稿说明对生成式 AI 图像有更窄的使用条件：论文主题本身需要是生成式 AI，且图像要作为研究输出的定性证据。当前论文主题是延迟反馈下的科学工具路由，不满足这一条件。

## 候选决议

投稿构建使用可编辑的人工或程序绘制图。当前替代图为 `paper/aamas2027/figures/figure2/v13/figure2_v13.png`，其源文件是同目录的 SVG/HTML 和渲染脚本；Figure 1 继续使用 `figure1/v08`。AI 生成图只保留在内部审查档案，不进入投稿 PDF 或补充材料。

## 理由

这样同时满足方法语义一致性、可复核性和会议图像政策。v13 已通过 selected call、terminal record、frozen judge、selected-only queue、delayed release 与 relay update 的逐项审查，并且不含旧版 family residual/provider bias 标签。

## 后果

本候选决议若经双方确认，将替代 0010 中“AI 生成图作为正式视觉资产”的投稿适用部分；0010 的历史生成记录仍保留。未确认前，论文构建仍按会议官方规则采用程序绘制图。
