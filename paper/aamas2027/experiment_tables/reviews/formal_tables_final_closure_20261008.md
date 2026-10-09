# 正式实验图表：候选 3 闭环

日期：2026-10-08。对象：`build/submission_20261008_093454/main.pdf`；实际查看该目录 `qa-page-8.png`、`qa-page-9.png`，并核对 PDF metadata/p8–p9 提取文本。

| 待关闭项 | 判定 | 观察 |
|---|---|---|
| 正文超过 8 页 | PASS | PDF 共 9 页；完整 Conclusion 结束于 p8 右栏，p9 从 References 开始，没有正文续行。 |
| Table 8 与定义完整 | PASS | 表仍位于 p8 顶部；四个 policy、coverage/accuracy/utility 数值完整；表注保留 W/R/C/N、五个 fitted policies、785 records and calls、一次调用、evaluator-only random。 |
| 压缩引入视觉错误 | PASS | p8 的表、公式、章节和结论没有重叠、裁切或挤出版心；p9 参考文献两栏完整。 |

候选 2 的六表与 delay 图视觉审查结论继续有效。此前页数阻断项已关闭；**实验图表呈现和 8 页正文布局可以提升**。本次没有修改源码或任何数字。
