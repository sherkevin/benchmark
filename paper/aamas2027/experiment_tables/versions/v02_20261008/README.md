# 正文实验矩阵与结果表排版稿

版本：v02，2026-10-08。状态：供用户审阅的排版方案。

这里维护这一份预览的 `main.tex` / `main.pdf`，迭代快照放在 `versions/`。
论文主版仍是上一层的 `paper/aamas2027/main.pdf`。本预览没有替换论文主版，
也没有改动生效方法、benchmark 或验收标准。

## 需要的六组实验

| 实验 | 要回答的问题 | 预览结果表 | 对应当前正文 |
|---|---|---|---|
| E1 主比较与延迟 | 静态环境是否稳定，受控变化后是否能恢复，延迟是否改变收益？ | 表 1，方法 × 两种环境；附 delay 分层 | 合并 `tab:main-results` / `tab:mechanism-results`，保留两个引用键 |
| E2 机制消融 | 正确写回源 provider 是否是收益来源？ | 表 2，Local / Frozen / cyclic 的配对差值 | 将 Mechanism attribution 段落升级为 `tab:credit-ablation` |
| E3 冷启动与选择空间 | 工具有没有可选择的空间，训练目标能否找出它？ | 表 3，固定/静态/两种目标与 evaluator oracle | `tab:learned-prior` + headroom 段落 |
| E4 更新规则诊断 | 分数校正和 residual 的中心化分别带来什么？ | 表 4，2 × 3 因子矩阵 | `tab:centering-diagnostic` |
| E5 效率 | 同一调用预算下，选择和更新有多快，状态有多大？ | 表 5，方法 × 时间/状态/调用量 | 将 Decision changes and costs 中的成本升级为 `tab:cost-validity` |
| E6 调用后验证 | 相同接受比例下，置信度拒答能否减少错误？ | 表 6，policy × coverage/accuracy/utility | 将 Post-call verification extension 升级为 `tab:postcall-verification` |

这是**六张结果表的候选正文结构**，不是要求为了填表新增六轮实验。
E1/E2/E5 是核心证据；E3/E4 是开发诊断；E6 是已有留出集的次级分析。
六行矩阵分别引用六张表，没有“列了实验但结果无处可找”的孤立行。
实际并入正文后，表号由 LaTeX 自动生成；对应关系用稳定的 label 保持。

## 版式与取舍

- 主比较将自然环境和受控漂移并列，避免读者翻两张窄表比数值；delay 分层放在同一结果块。
- 机制表单独列出，避免把 scaffold 等权估计和 pooled-call 估计混成一张排名。
- 两张静态诊断用单栏并排。主比较、机制、成本、拒答用双栏宽表。
- 结果表字号 8.5 pt，单栏约 3.34 inch、双栏约 7 inch，使用 booktabs；没有 `resizebox` 或竖线。
- Changes / rescued / harmed 留在主结果解释段；不继续给主表堆列。
- 表格里的粗体只表示该设置下可部署方法的最高点估计（含并列），不表示显著性。
- `TBD` 是待填测量；横杠是未运行/不可定义。当前只给 live-state 留占位，不编数值。

预览共三页：第 1 页实验地图，第 2 页核心结果，第 3 页诊断、成本与扩展。
它展示拟并入正文的实际表宽；不是正式 AAMAS 全文，也不据此宣布全文页数合规。
最终合入需重新编译完整正文并核对实验节页数与浮动位置。

## 数据口径

- 157 是独立留出分子；30 是每方法的 fold/seed/delay 设置；4,710 是每方法记录调用数，不能相加成独立样本。
- 开发诊断是 722 分子，不与 157 留出分子合并为一个总 N。
- 机制 v2 的两个 replica 是确定性重复，不称作随机种子稳健性。
- 成本来自已有 summary 的 stream-level p95，表中对 30 个 p95 取均值；不把它称作 pooled p95。
- 序列化状态包含审计历史，不能直接充当 live learner memory；此列因此先用 TBD。
- 调用后分析复用已有 157 分子，不是新的一次未触碰确认。same-coverage random 单独放在 evaluator-only 行。

## 来源与可复用文件

`versions/v02_20261008/manifest.json` 记录本轮正文快照、结果来源和文件 hashes。
`body_tables.tex` 从本预览的 RESULT BLOCK 自动导出；不是第二份独立维护的表。
将来合入正文可使用这份同源 LaTeX，避免预览与论文数字、列名发生分叉。

参考依据仍是 `research/experiment_section_style_20261006/topconf_experiment_literature.md`：
主表采用方法 × 设置/指标，机制表围绕单一干预，成本独立比较，正文只解释改变结论的数值。
获奖论文的任务数量不能直接作为本项目样本量要求；本项目的规模由独立分子、配对方差与泛化范围共同约束。

本轮先完成可看的表格稿。前序参考文献补全仍在待办中，不能把这份预览的完成当作引用任务已完成。

## 版本留痕

- v01：首版三页预览，保留源码、PDF 和页面图片。
- v02：表格数据与结构不变；收紧 caption 的作用域、关闭非浮动预览的 hypcap，并在导出的每张表内固定行距。
