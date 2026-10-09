# Figure 2 图库风格迭代卡

日期：2026-10-07。上一轮为真实图稿与阅读 PDF 的进展；本轮继续当前两张 AI 结构图目标。

## 目的与边界

- 优化事件时序图，和 Figure 1 AI v09 使用同一图形语法。
- 修复 v22 的阶段错位、重复 outcome 卡和失败支路进入化学更新的歧义。
- 只修改图稿、完整 prompt、审查和必要阅读稿图源；不处理实验或方法开发。

## 必须保留的因果关系

Query/State/Menu → Select → 已选 B → Call → Return → Judge → 化学反馈 → Pending → Release → 仅 B 计数 → 新状态 → 后续决策。

运行失败由 Return status 进入独立 Runtime ledger；未知和失败不更新化学计数。
释放 tick 为 $\tau_t=t+1+D$；D=0 也不能改变 action t。待释放仅限制这个事件，不冻结来自其他事件的合法更新。

## 衡量指标

图库研究的 8 项，每项 0/1/2：视觉中心、模块表达、信息密度、布局、色彩线条、数据路径、方法一致性、纸面可读性。

- 数据路径或方法一致性有硬错误，任何总分均不能接入。
- 7-inch 宽度检查正文图，不能用高分辨率 PNG 代替。
- 短标签、无全图网格、无巨大阶段标题、无重复 outcome 行。
- 所有生成与审查版本保存。图像像素只通过 AI 重生成或 AI 编辑改变。

## 本轮复用

复用 TableDART 的局部分区、CRITIC 的闭环、CollabLLM 的事件卡片，及 Figure 1 AI v09 的色板和寄存器结构。出处与参考原图 hash 见 `style_references/20261007_gallery_v1/manifest.json`。

生成与审查结果将在后续段落追加；此文件不是结果合格证明。
