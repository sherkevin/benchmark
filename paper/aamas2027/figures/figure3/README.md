# Figure 3 — 程序数据图

当前版本：**v02_data**；AI 数据图历史版本在 `../generated/figure3/`，不再
作为当前候选。规则见决议 0012。

| 版本 | 状态 | 变化 |
|---|---|---|
| v01_data | 保留 | 从原始数据绑定清单绘图，数值校验通过；标题靠近上边界 |
| v02_data | 当前候选 | 增大上边距，harmed 柱增加纹理；数据和比较口径不变 |

复现命令（仓库根目录，matplotlib 3.9.4 / NumPy 1.26.4）：

```bash
python3 -B paper/aamas2027/figures/figure3/v02_data/render.py
```

当前目录包含 `render.py`、`data.json`、`plotted_values.json`、`values.csv`、
`data_table.tex`、PDF/SVG/PNG 和 `manifest.json`。图宽原生 7 inches，PDF
字体嵌入，数据图没有使用 AI 底图。脚本在绘制前核查输入和三份来源的 hash，
随后核对 6 个曲线点、10 根柱、5 个散点的精确数据。

面板 (c) 用对数坐标并明确标注；不移动散点、不添加未核验的区间或趋势线。
置信区间和政策成本的边界见审查记录
`../external_review/20261007_figure3_data_v02_review.md`。
