# v02 实验表视觉审查

日期：2026-10-08。审查对象：`experiment_tables/main.pdf`、v02 三张页面图片、对应 LaTeX 和 README。只审查呈现，不改实验、数字、正文或预览。

## 结论

v02 已有正确的基本排版：booktabs、无竖线、统一字号、自然/受控条件分组。但它仍像一份完整审计清单。主要问题是**每张表承担的信息过多，主证据与诊断的权重相近，表宽没有服从内容**。继续减小字体、增加颜色或装饰，解决不了这三个问题。

按当前图像的直观阅读体验：主比较可读，第三页明显拥挤；用户的“乱”主要来自多块标题、长 caption、长 notes、重复列连续叠加。

## 实际看过的参照

| 会议版本 | 页面 | 可以借用的具体做法 |
|---|---|---|
| VAR, NeurIPS 2024 正式版本 | PDF p7，Table 1/2 | 方法族以水平分隔形成行组；主表有明确的主指标与开销指标；小规模辅助比较使用窄表，宽度服从内容；同页结果解释位于正文，表内只留下必要的符号释义。 |
| SAM, ICCV 2023 正式版本 | PDF p8，Figure 7 | 一个大结果块中的每个子图承担不同阅读动作：任务分布、跨数据集差值、人工评价、交互次数。各图的标签、色彩、单位统一，读者先识别结果，再读完整 caption。 |

本审查使用上述正式论文的图表作视觉参照，不据此新增某篇论文的获奖身份认定。参照截图保存在 `reviews/reference_views/var_p7.png` 和 `sam_p8.png`。

### 两个需要避免的错误归纳

- 好论文也有长 caption；SAM Figure 7 就有较长 caption。可以压缩的是我们重复解释设置和审计边界的文字，不能据此设定“一律两行 caption”的硬规则。
- VAR Table 1 含竖分隔线。它的清晰来自信息分组和稳定列结构；“绝不使用竖线”本身不是审美定律。我们继续使用 booktabs 足够，无需模仿其所有细节。

## 可定位的问题

| 位置 | 观察 | 修改方向 |
|---|---|---|
| p1 实验地图 | 六行都用完整问句，四个文字列都多行换行。地图几乎占半页，却没有快速导航作用。 | 在审阅说明中保留完整问题；纸面地图只显示 E 编号、短实验名、数据、结果表号。协议细节集中到一个 Setup 段落。 |
| p2 Table 1 | 一个表号包含主比较和独立 delay 子表，中间又有三行 notes。读者容易把下方表当作一张新表，但它没有子表标记。 | 主表保留自然/受控两组及主 CI。delay 若仍放正文，标明 `(b) Delay sensitivity` 并采用紧凑 4×6 小块；更推荐单独折线图或附录完整分层表。不要把分层结果伪装成独立样本。 |
| p2 Table 1 行 | 三个自然点估计均加粗；方法名、参考行、retrospective 控制混在同一视觉等级。 | 方法按 anchors / online comparators / proposed 分组；用一条规则分隔 proposed 行；evaluator-only 控制保持末组。重复并列粗体可以收敛，只在表注定义强调规则。 |
| p2 Table 1 数值列 | `tabular* + extrafill` 将表强制拉满；Natural/Controlled 组内的 accuracy 与 CI 距离大，组间界限反而不强。 | 设置固定组宽和组内紧凑间距，不用空白把每列均匀摊开；数字用统一精度与对齐方式。保留每个估计的自然/受控归属。 |
| p2 Table 2 | 只有三行消融却跨整页，caption 和 notes 共五行左右；大量横向空白被数值填充规则放大。 | 使用紧凑三列：Contrast / Natural Δ [CI] / Controlled Δ [CI]，行名用 Local–Frozen 等短写。先按自然宽度排版；若单栏容纳不下 CI，保留双栏但不强行拉满。124 个 scaffold 和等权 estimand 放统一统计说明，caption 留下相对 Table 1 的区别。 |
| p2 页尾 | “Decision changes accompanying ...” 大标题和长计数段落打断两张主表的阅读。 | 移回正文的结果解释；审阅稿用一条短结论，不用第三个独立的章节级视觉入口。完整 rescued/harmed 计数继续保存。 |
| p3 Table 3/4 | 左 caption 四行、右三行，导致 table top rule 不在同一高度；左 notes 五行、右六行，使块底部也不齐。 | 相同高度的 caption 区；对齐 top rule。更重要的是移出长注，避免为了对齐再插入空白。若压缩后仍不同高，不强制并排。 |
| p3 Table 3 | 三个确定性方法复制两个相同 seed 数字；训练种子和 evaluator oracle 共同占据两列。 | 训练行可用 mean/range 或 seed0/1 的一列并排简写；确定性行只显示一个值并注明 deterministic。不同 estimand 不变。oracle 留单独分隔行。 |
| p3 Table 4 | Raw/Corrected 各重复三次，因子关系难以直接扫到。 | 用组标题或 multirow 显示两个 score scale，每组下三种 update。把内部定义移入方法正文，表只保留读数。 |
| p3 Table 5 | Live state 整列为 TBD；Calls/input 全是 1；两列既不形成比较也占宽度。Controlled 只给 update，表头不对称。 | 统一调用预算写 caption 一句。全 TBD 的内存单独列为待填测量说明，在得到值之前不占正式结果列。成本表只留 Method / Selection / Update-natural / Update-controlled，或者一张窄表加说明。 |
| p3 Table 5 注 | 270/150 流、receipt、控制范围、未测内存、排除项同时写在一段 notes。 | timing 定义保留一条：mean of stream-level p95、设备、排除 provider I/O。receipt 完整验证放 artifact 段或附录。 |
| p3 Table 6 | 7 列里 Wrong、Refused 与 Coverage、accepted accuracy 描述相同信息；Calls 又恒为 1。两个长 notes 位于页底，形成密集黑块。 | 缩为 Policy / Coverage / Accepted accuracy / Utility；完整 counts 放附录。保留 matched-coverage random 的 evaluator-only 标记。效用公式在指标定义段出现一次。 |
| p3 全页 | 两张诊断、成本、拒答四个表块排在同页；每块都有 caption+notes，视觉焦点分散。 | 页级编排按用途：主比较一页；机制和成本一页；开发诊断与次级扩展一页。数据表无需 AI 生图。 |

## 下一版的具体重排方案

保持 E1–E6 与结果标签的对应关系；改变的是展示的组织方式。

1. **地图页：** 四列短地图 `Study / Scope / Main metric / Result`。全局统计口径在统一说明中出现一次。完整实验矩阵可留 README/附录。
2. **主结果页：** Table 1 是唯一宽表，方法按角色分三组。主 CI 保留；delay 分层放清楚标记的小 panel 或独立数据图。caption 指明 157 分子与 controlled label-flip，重复的 seeds/delays/调用总数移 setup。
3. **机制与效率：** Table 2 三列紧凑，Table 5 四列紧凑。先试并列窄块；若 CI 使 Table 2 单栏过宽，就分成上下自然宽度块，分别回答来源归因和实际更新开销。timing 口径必须保留；不能把均值 p95 改叫 pooled p95。
4. **开发诊断：** Table 3/4 只保留数值与因子行组，不再重复完整定义。它们仍是 development，不与 holdout 主表合并成一个排名。
5. **次级拒答：** Table 6 四列。CI 与效用解释位于一段对应结果正文，不再堆两个表下注。157 分子 reused-cohort 身份必须保留。

若实际正文页预算只够三张表，可把 E3/E4 全表放附录并在正文保留关键结果句；E 编号仍对应同源表，不删除实验或改变科学结论。这是排版建议，需由管理者决定正文篇幅。

## 审查下一版的可执行条件

- 每个表块能在约 5 秒内识别行是谁、列是什么、主比较是什么。
- 方法名、指标名和 Natural/Controlled 的命名全稿一致。
- 单栏并排表的顶边对齐；不靠缩到 7 pt 以下或 resizebox 塞内容。
- 无整列重复常数，未完成的整列占位只留审阅说明，不伪装成正式结果。
- 每张表只有一种主估计口径；配对 CI、scaffold 等权、pooled-call、accepted-only 各自可辨。
- 主图表先呈现结果，再由正文说明协议；审计轨迹保存于版本目录与附录，不丢失。
- PDF 实际渲染检查列宽、caption 换行和页尾拥挤；只有编译无错不足以通过视觉验收。

未运行新实验，未修改 `main.tex`、`main.pdf` 或表格数字。可复用产物是定位清单、重排方案和两张参照截图。
