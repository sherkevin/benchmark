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

## 当前正文表号同步闭环

2026-10-08：当前主版表号变化后，再次查看 p4 并比较新旧源码。diff 仅有两行标题：成本 E5 对应 body Table 8，拒答 E6 对应 body Table 7。p4 两个标题清晰完整，未引入换行挤压或裁切。

新 PDF SHA256：`9869f8891652fe0106bc8996caafebf064720586eb28e7f01379074717f9d845`。
新源码 SHA256：`478c8708bbef4a952d4fdd5f64464b2f2c9ec94e69ab42f24ca5f4ac26f1bf81`。

用相同 pdftoppm 1600px 配置重渲染 p5/p6，与本报告初次保存的对应 PNG SHA256 完全一致，故这两页像素未变：

- p5：`6c1a70e7f2d0d0e4d9f90bc524ea3d809b003e60111f35b884e09d698850ab26`。
- p6：`e19e009de669f0474f3f2c088de389666e74110f83359389c590fa3002acf179`。

同步后的渲染留存在 `reviews/v07_master_label_sync_views/`。视觉 PASS 延续；没有重复数值审查或修改任何数字。
