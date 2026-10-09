# AAMAS 论文图表版式审计

> 状态：内部排版审计，2026-10-03。目标是先把版式和信息层级固定，再填入真实图和结果；占位框不代表已经完成实验。

## 1. 参考材料与观察

使用本地 `topconf-paper-figure-gallery` 的说明、元数据和示例图进行审计：

`/Users/jingwu/Documents/Codex/2026-09-27/new-chat/topconf-paper-figure-gallery`

画廊包含约 3,516 个收录图，其中 pattern 统计约为：teaser 1,599、architecture 523、conceptual 444、pipeline 400、framework 288、taxonomy 249。architecture/pipeline 图的横向中位宽高比约 2.2–2.3。

反复出现、适合本论文的结构规律：

- 从左到右讲清输入、决策模块、外部执行、反馈和下一次决策；主模块通常控制在 3–5 个。
- 用有限的有界 panel 分组；箭头表示数据或控制关系，回环箭头明确表示反馈。
- 颜色少而稳定，并且用文字、线型和位置重复表达语义，不能只靠颜色。
- 图中保留关键概念和接口，不把一整段论文压成小字；公式放正文，图负责让数据流和时序一眼可见。
- 不使用 UI 截图、无边界的文字墙或无法解释的装饰元素。

## 2. 论文中预留的两幅核心图

### Figure 1：方法总览（`figure*`）

目标高度约 2.5–2.8 英寸，跨双栏。建议最终从左到右排列：

`molecule + provider menu + released history` → `frozen direct selector` → `family/provider relay` → `one selected provider` → `verification and outcome` → `delay queue` → `released update` → `next decision`。

建议颜色：输入和固定模块用中性灰/蓝，在线 relay 用主蓝，provider 执行用浅灰，延迟反馈用琥珀，验证/风险账本用紫色。每个 panel 必须写清它看到什么、何时看到，尤其标出“未选 provider 的输出不可见”。正式图不把所有公式塞进去，只保留变量名和一个短的反馈箭头。

### Figure 2：延迟反馈生命周期（单栏）

目标高度约 1.8–2.2 英寸，使用三个横向泳道：

1. selector：decision tick $t$，选中 provider；
2. provider/verifier：call、terminal event、runtime/chemical 分层；
3. updater：等待 $D$，在 $τ_t$ release，更新 state，进入下一次 action。

用实线表示已发生的调用，用虚线或浅色表示等待；显式写出“selected-only”“not visible yet”“release at $τ_t$”。这幅图的任务是消除读者对在线信息边界和延迟更新的疑问。

当前 `main.tex` 选用 Figure 1 v4 和 Figure 2 v8 的正式矢量图，均有 caption、label 和 `\\Description{}`。后续替换版本时必须重新检查整篇分页。

## 3. 表格版式

| 表格 | 位置和作用 | 设计要求 |
|---|---|---|
| Baseline contract | 单栏 | 每行只回答一个比较问题；角色、信息权限和是否可部署分开写 |
| Experiment matrix | 双栏或跨栏 | 用 E1–E6 对齐研究问题、数据层、反馈延迟、主要指标和否证条件 |
| Main results | 跨栏 | 最多约 7 列；主指标、95% 区间、harmed/rescued、成本分开，不在单元格里塞段落 |
| Mechanism/ablation | 单栏 | 对比 probe、shield、fallback 和 delay，避免只给 pooled accuracy |

表题在表上，图题在图下。长标签优先换行或使用短名称并在 caption 解释，不能通过缩小字号制造可读性问题。

## 4. 页级版式预算

这是内容预算，不是硬性页数承诺：

- p1：标题、摘要、问题和贡献切口；
- p2：Introduction 收束、Figure 1、任务定义；
- p3：Figure 2、方法总览和信息边界；
- p4：更新公式、复杂度、可证伪条件；
- p5：benchmark、baseline、实验矩阵；
- p6：主结果和机制表；
- p7：错误/成本、相关工作、局限；
- p8：结论（若结果和图表需要）。

参考文献另计。当前图表集成版 pre-results PDF 为 7 页；正式结果表尚未填入，不应通过无关扩写继续增加页数。

## 5. 当前检查结论

- 已解决：并发构建造成的临时页/XRef 问题；最新隔离构建 `build/verify_figures_v7_20261003/main.pdf` 和 `build/main.pdf` 均为有效 7 页 PDF。
- 已完成：Figure 1 v4 和 Figure 2 v8 的正式矢量图、版式位置、caption、Description 和目标尺寸检查。
- 未完成：最终结果表、黑白/色盲复核、真实结果图和最终投稿分页。
- 禁止：把 `TBD` 当作结果，或在没有真实收据时填入数字。

下一次替换图表后必须重新跑干净构建、检查页数、渲染全部页面，并记录图表源文件和 hash。
