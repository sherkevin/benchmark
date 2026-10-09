# Guide v07：到达顺序页视觉审查

日期：2026-10-08。按指定范围实际渲染并查看最终 PDF 的 p5（新增页）和 p6（尾部衔接）。图片保存在 `reviews/v07_rendered_views/page-5.png`、`page-6.png`。

核对身份：PDF SHA256 `90dc5fce4610a88ea93fe90bb2d6e3e1dbda2eca3057b40677a8f2ce88dfcbb6`；对应源码 SHA256 `3dcf4b5d03b61a93ab4524dbac3bfa69a2b2d6fc7b2e36ae6175f2facef5f168`。PDF 共 6 页。

**视觉 PASS，无必修项。**

| 检查 | 结果 | 观察 |
|---|---|---|
| 新页状态 | PASS | p5 的标题、caption 和首段明确新增数字为 M；页眉、背景水印、M/B/T 页脚仍完整。明确是固定 cohort 上的 order uncertainty，并非未来候选预测。 |
| 三张数据表 | PASS | mean/SD、paired mean/quantile/wins、covariance/correlation/paired SD 分表呈现，单位在列头或 caption；9.5 pt 表字可读，列名未碰撞。 |
| 公式与页尾 | PASS | covariance 公式、实例和三种统计对象解释完整；p5 末段未碰页脚，也没有另起孤立尾页。 |
| p6 尾部衔接 | PASS | 原检查矩阵、证据范围和最终版本/来源说明完整；没有水印遮字、裁切或重叠。 |

本次只审查指定两页的展示，未运行实验或改动源码/数字。可展示为独立 Guide；视觉通过不提高未来预测可信度，也不把条件预算视作新结果。
