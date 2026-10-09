# v04 实验图表复审

日期：2026-10-08。对象：v04 `main.pdf`、`page-1.png`、`page-2.png` 和冻结源码。实际查看两页及 Scratch Table 2、Registers Table 3 参照图；沿用 v02 的 VAR/SAM 参照。未运行实验，未修改源码或其他人的文件。

## 总结

**视觉排版通过；一项自包含定义需补齐。** v04 已从台账式表格变为有主次的论文结果块。与 v02 相比，主表可快速横向比较，delay 转成图后更直观，开发诊断顶边对齐，成本/拒答不再有整列重复或 TBD。当前没有需要通过缩字解决的硬排版错误。

## 逐项结果

| 项目 | 判定 | 实际观察与必要处理 |
|---|---|---|
| 两页完整性 | PASS | 两张渲染图均完整；无第三页、页尾裁切、标签遮挡或内容叠加。v04 log 未见 Overfull/Warning/Undefined 匹配。 |
| 主表横向比较 | PASS | Natural 与 Controlled 分组清楚；受控 success、gain、CI 各自独立列；行组区分 anchors、online comparators、MIRA 和 evaluator-only reference。 |
| 主表科学强调 | PASS | 自然条件的并列值保留，正文明确 MIRA 的 0.00 pp 和跨零 CI；没有把自然 null 隐藏为受控正收益。粗体解释仍是点估计，并非显著性。 |
| 字号 | PASS | 源码表格 9 pt、行距 11 pt，表注 8.5 pt；没有 resizebox。实际两页可读，无须继续缩字。 |
| 图中对比 | PASS | 两个 panel 共享四方法图例；色彩、符号、线型一致。Natural 与 Controlled 使用不同 y 范围，并通过 panel 标题和刻度明确区分。数据只在已测 D=0/1/4 处出现点。 |
| 图例与注释 | PASS | 图例单行置于图下，未遮挡曲线；caption 明确描述性分层未展示 uncertainty。它不替代 Table 1 的主配对 CI。 |
| 并排表顶边 | PASS | Table 3/4 caption 起点与 top rule 对齐。两表采用自然宽度，已解决 v02 的错位。底部略有不同，由对应 notes 自然结束，不构成硬错误。 |
| 列与分组 | PASS | Table 4 的 multirow 直接展示 2×3 因子结构；Table 5 删除恒定调用列和未测整列；Table 6 删除重复计数，保留 coverage/accepted accuracy/utility。 |
| 科学口径 | PASS | 157 分子与 4,710 记录调用分开；722 development 独立；Table 2 保留等权 scaffold estimand 区别；Table 5 是 stream-level p95 的均值；Table 6 仍标 reused holdout secondary analysis。 |
| 效用公式的自包含解释 | **FAIL（必修）** | Table 6 写出 $U=-(W+0.25R+0.01C)/N$，但此 PDF 内没有解释 W/R/C/N。v02 原有解释在压缩时丢失。补一句变量定义，或在本稿指标定义段给出后交叉引用。 |

## 唯一必要修复

Table 6 的表注补：`W: wrong accepted returns; R: refusals; C: calls; N: records.`

这不会改变任何数值或估计。该定义补齐前，不应把当前六表块当作可独立阅读的最终正文片段；可继续用作版式预览。无需重跑实验。

## 可选的小修，不阻止版式提升

- Natural panel 把零基线加深一级，便于直接判断四条曲线的正负；不改轴范围。
- 自然宽度短表仍配全页宽 caption/notes，Table 2/5 尤其明显。可让文字块跟随表宽，或在完整论文中由正文承接。Registers Table 3 的短表也独立居中，说明不必强制撑满页宽。
- Table 3/4 的字号与行距统一，但右表下边框比左表稍早结束。无须增加填充空白强求等高；继续维持顶边对齐即可。

## 与参照的关系

Scratch 的 Table 2 主要依靠稳定数字列和方法分组形成信息密度；Registers 的 Table 3 采用自然宽度和分组线。v04 已借用这两点，没有模仿参考 PDF 的绿色链接框。VAR 的行组和 SAM 的分 panel 对比也已体现。当前仍是独立审阅稿，其页级标题和实验导航将由正式正文组织接管；不能把这两页直接等同于完整论文浮动布局已通过。

审查产物仅本文件；数据、PDF、旧版本均未改动。
