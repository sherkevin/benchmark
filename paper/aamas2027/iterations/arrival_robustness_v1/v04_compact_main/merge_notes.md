# v04：合并原顺序与随机顺序主结果表

2026-10-08。只提供片段；未改 main、未编译、未编辑 TODO。目标是移除一个重复浮动体，不再大幅删正文。

## 合入方式

1. 用 `combined_table.tex` 替换 Natural and controlled outcomes 开头的原主表浮动体。它包含原 `tab:main-results`、兼容别名 `tab:mechanism-results` 和新 `tab:arrival-robustness` 三个标签，指向同一张表。
2. 删除 Arrival-order robustness 段后的独立 `table*` 环境；保留该段完整协议及其后的结果解释，不再插入第二张表。原表与 arrival 的引用都会指向合表。
3. 保留实验 map。E1 原来同时列出 main-results 和 arrival-robustness，合并后是同一个表号，应简化为 `Table~\ref{tab:main-results}, Figure~\ref{fig:delay-sensitivity}`。E2 可保留 credit 表和合表入口。
4. 原主表完整独立附表保存在 `original_main_table.tex`，保留所有原点估计、Gain 和固定轨迹 range 端点。它有独立标签 `tab:original-main-detail`，当前未被任何主稿引用或纳入 PDF。不要在正文写 “see supplement”，除非 root 后续确实发布该附表。
5. 主稿的旧 `MiraMainCaption/MiraMainBody` 若仍保留为未使用宏，不影响此片段；更整洁的合并方式是把新 caption/body/三个 labels 放回现有宏并继续复用原浮动调用。两种方式任选其一，不要把新旧浮动体同时留在正文。

## 数值与解释

- 五列：方法；Natural Original / 128-order mean；Controlled Original / 128-order mean。原九个方法的所有准确率点估计保留，新表全部十个方法、自然十行/受控六行保留。
- 原表中的收益点估计与区间没有被丢弃：全文源与结果历史不变，独立附表保留全部；合表不重复放这些可由相邻方法比较的列。没有缩字号：继续使用 `MiraTableFont` 的 9 pt 字体和 8.5 pt 表注。
- Fixed-permutation 在旧的 **main roster** 中没有一行，所以 Original 两格用横杠。旧 credit study 虽然研究过 permutation，也不能把其另一套汇总结果填入这两个格子。表注明确横杠是对应研究/条件的 roster 不包含，不是运行失败或缺失。
- Retrospective Best Fixed 保持 evaluator-only。新 order mean 排除原始顺序，且条件于固定 priors、policy seeds；不得解释为对新分子的总体均值。
- 正文必须原样保留原顺序 MIRA−Frozen：自然 `0.00 pp, [-1.24,+1.40]`；受控 `+14.18 pp, [+9.61,+19.03]`。二者仍叫固定轨迹 range。
- 正文必须保留新 primary MIRA−Online SGD 的均值、MCSE、均值区间、125/128 和胜率区间，以及自然 pooled `−0.29 pp, [-0.37,-0.20]`。新旧均值/不确定性不能混用。
- 合表直观看到自然 MIRA `89.81 → 89.52`，受控 MIRA `87.56 → 86.96`；这是不同到达顺序汇总的差异，不把它改写为同一条轨迹上的因果变化。

## 空间与检查边界

两个浮动体合为一个十行浮动体，减少一套方法重复行、表头、caption 和表注。预期比两个独立表明显省高；未编译，不能保证具体节省页数或已恢复八页。root 完整构建后再检查 table/caption、交叉引用、p8 结尾及 References 起页。

没有修改论文题目、方法、后验公式、实验矩阵或实际结果。科学 gate 与最终审计状态仍由 root 管理。
