# Guide v06 分布版：视觉审查

日期：2026-10-08。审查对象是稳定后的 `versions/v06_20261008/guide_final.pdf` 与该版本源码，非尚未提升的 root `guide.tex`。实际渲染并查看全部五页，保留图片于 `reviews/v06_rendered_views/page-1.png` 至 `page-5.png`。

**视觉 PASS。** 这表示本版可以作为内部设计 Guide 阅读；不表示未来性能分布可信、目标已可实现或科学 gate 已通过。

| 检查 | 判定 | 实际观察 |
|---|---|---|
| 状态区分 | PASS | 每页均有 CONDITIONAL TARGETS ARE NOT RESULTS 页眉、NOT MEASURED 水印及 M/B/T 页脚；混合表里 B 或 T 与蓝色同时保留。 |
| 五页完整性 | PASS | 五页完整，页号一致；p3 的长反馈覆盖说明、p4 joint success criterion、p5 最终 evidence/version 段均在版心内，未拆成孤立尾页。 |
| 字号和线条 | PASS | 表格为 9.5 pt / 11.5 pt 行距，booktabs 和自然宽度可读；无数字压线、列名重叠、表格裁切。 |
| 重表理解 | PASS | p1 accuracy/SD/errors 按两条件分组；paired errors 的 rescue/harm/both-wrong 明确；四个重复的 Bits/Dev/Fresh 三列组可识别。 |
| 条件预算理解 | PASS | p2 情景、参数、M/B 分层、pooled/scaffold 口径分别呈现；页面文字解释预算为 fractional weights，未画成预测轨迹或未来 CI。 |
| 先验/更新分开 | PASS | p3 measured negative attempts 和 measured factorial 独立于两行 T requirement；目标未混进实测排名。 |
| 成本/coverage | PASS | p4 CPU kernel 的 M/T cap 区分，逐 fit 接受量和 B swap 预算分表；没有用目标隐含替换实际分母。 |
| 最终证据段 | PASS | p5 typewriter 文件名和来源说明完整、可读，距页脚有明显留白。报告的两处 Underfull 未产生可见溢出或遮挡。 |

可选清理：p5 的文字检查矩阵使用两端对齐，个别长句字间空隙较大；可改为 raggedright。它不影响含义或当前阅读，不需为此延迟内部 Guide 展示。

本次只写审查与保留渲染图片，没有运行实验、采样、模型、API 或改动正文。未来轨迹、自然漂移、方法可达性和预测区间仍由独立科学审查判断；broader HOLD 不能由本视觉 PASS 推翻。
