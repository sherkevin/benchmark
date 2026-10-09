# Figure 1 角色分工独立审查：v31 prompt / v21 AI

- 日期：2026-10-07。
- 对象：`figures/prompts/figure1/v31.txt`、`figures/generated/figure1/v21_ai.png` 与 `figures/qa/build_v17/page-1.png`。
- 结论：**方法、因果路径和线条语义均通过，15/16；推荐作为最终候选。**
- 硬错误：**0**。v20 的唯一线条建议已修复：queue→Release 是紫色虚线等待，Release→Update 和 Update→state 是蓝色实线；Update 箭头头进入高亮 row 内。
- 状态：推荐进入用户最终候选选择；本报告不代替用户做审美选版。
- 审查身份：独立图稿审查员；只读审查，不修改图片、prompt、正文、实验或方法。
- 实际查看：v21 原图与 build_v17/page-1 均通过 `view_image` original 查看；原图尺寸 2058×764，纸面页尺寸 1540×1190。

## 角色契约与文字清洁

v21 仍清楚承担 Figure 1 的架构职责：冻结分数、released state、residual、plus、Selector/Eligible tools、执行判分及 selected-only 更新。它没有重复 Figure 2 v26 的单事件时间脊、pending-to-later-decision 卡片顺序或时间线布局；底部只保留架构所需的 queue→Release→Update 回写。

- 原图和 page-1 没有独立 provider 字母 `A`、`B`、`C`、provider 名称或行 ID；中间高亮 row 和 selected-tool icon 表达 selected provider。
- 文本为短标签，唯一公式是 `q=q^0+Δ` 和 `τ_t=t+1+D`。`n`/`s` 是两列计数统计量，不是 provider 身份。
- 没有 family residual、provider bias、额外 provider 调用、fallback、性能数字、训练骨干或 Figure 2 的完整事件时间线。

## 实际拓扑与线条核对

### 主分数路径

1. `Molecule + Endpoint → Features → Frozen scorer → q^0` 的箭头均接到具体模块；特征/权重格、锁和三格 q⁰ 表达冻结 base score。
2. `released state (n,s) → residual Δ → plus` 清楚。state→residual 的黑色 readout 位于反馈基线之上；residual 没有被放到 Update→state 写入边上。
3. q⁰ 和 Δ 进入唯一圆形 plus，形成 `score` 三格向量，再由一条短实线进入 Selector。
4. Eligible tools tray 贴在 Selector 旁/下方，作为 Selector 的局部中性输入；没有任何边连接它到 released state、residual、plus 或 score。Selector 只输出一个蓝色 Selected tool。
5. `Selected tool → Call → Return` 后，valid 进入冻结 Judge 再到琥珀 Chemical feedback；error 虚线只到 Runtime ledger dead-end，不生成 chemical label 或 state update。

### 唯一反馈写入链

v21 的实际下方链条为：

`Chemical feedback → selected-only queue ⇢ Release (τ_t=t+1+D) → Update → highlighted released-state row`。

- Chemical feedback 到 queue 保持一条短紫色向下箭头。
- **queue→Release：** 现在是单一紫色虚线箭头，箭头头指向 Release，明确表示隐藏等待。
- **Release→Update：** 现在是单一蓝色实线水平箭头，箭头头指向 Update，表示释放后的在线更新。
- **Update→state：** 蓝色实线水平箭头从 Update 指向左侧 released-state 的高亮中行；箭头尖端落在右侧 `s` cell 内，而不是外框、底边或空白。中行 `n`/`s` 两格整体高亮，因此 row-level provider-local 更新含义成立。
- 除上述三条边外，没有 Release→state 旁路、第二条蓝色更新边、runtime→state 边或 residual→state 回写边。

这满足 v31 的 line semantics 和 graph audit：等待是紫色虚线，已释放更新是蓝色实线，residual 只读 state 并进入 score synthesis，state 只由 Update 回写。

## 图库风格与纸面检查

v21 保留 TableDART 的浅色方法分区和内部格、CRITIC 的闭环组织、CollabLLM 的状态/事件小模块；中心 MIRA-Relay 区最大，Inputs/Execution 分列两侧。Features/weight grids、vectors、plus、Selector tray、document/lock/gear/clock/queue 各有具体图形语法。颜色与线条角色稳定：深蓝在线核心/更新，灰色冻结/记录，琥珀判分/化学反馈，低饱和紫等待/Release。

build_v17/page-1 的七英寸页面无裁切、重叠或悬空箭头，主链和反馈方向清楚。主标签、Update→row、queue→Release 虚线均可读；`n/s`、`q^0`、`Δ`、`τ_t` 和 valid/error 等次级字段偏小但仍可辨认，因此纸面项保守记 1。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 中央 MIRA-Relay 的冻结分数、残差合成和 Selector 是最大区域 |
| 模块表达 | 2 | 特征/权重格、state register、residual/score vectors、plus、tray、执行卡和 queue 语法明确不同 |
| 信息密度 | 2 | 机制由短标签和两条公式表达，无 provider 字母串、重复段落或性能数字 |
| 布局 | 2 | 三分区紧凑，state/residual 与 feedback baseline 分层，queue→Release→Update→row 没有交叉或 U 形绕路 |
| 色彩线条 | 2 | 紫色虚线表示等待，蓝色实线表示 Release→Update→state，灰/琥珀/蓝/紫角色与图库规范一致 |
| 数据路径 | 2 | 唯一 causal write path 完整，Update 终点在高亮 row 内；无旁路、外框终点或 runtime 化学更新 |
| 方法一致性 | 2 | 冻结 q⁰、provider-local released state、residual 读出、valid Judge、runtime dead-end 与 delayed selected-only release 均正确 |
| 纸面可读性 | 1 | 已实际检查 build_v17/page-1；主链可读，次级数学和状态字段略小 |
| 合计 | **15/16** | **无硬错误；推荐作为最终候选** |

## 硬错误与顶会质感建议

**硬错误：0。** 没有 A/B/C 污染、Release→state 旁路、Update→register 边界终点、residual 介入写入、runtime failure 化学计数或 Figure 2 角色重复。

**建议（不影响通过）：**

1. 若还有一次排版迭代，可把 Update 箭头尖端从 `s` cell 的右缘略移到高亮 `n/s` 行的视觉中点，使“整行更新”更一眼可见；必须保持箭头在 row 内且不触碰 register 外框。
2. 7-inch 终稿可微增 `n/s`、`q^0`、`Δ`、`τ_t` 和 valid/error 的字号；不要添加解释句、provider 字母或 Figure 2 的时间标签。

本报告基于实际 v21 原图和 build_v17/page-1，不以放大 PNG 替代纸面检查，也不宣称用户已最终选定该版本或已完成投稿接入。

## 文件与 hash

下列路径以 `paper/aamas2027/figures/` 为根。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v31.txt` | `5d868281e2c647aa1c28670f2f431c53d704d7dc1b3a43570eb3e06954490f9b` |
| `generated/figure1/v21_ai.png` | `393a8bcb08c9a59a4bc62858cb531a7978a581aab35a5cbd6b52b43468a9c0f7` |
| `generated/figure1/v21_manifest.json` | `0a8576dead46bf6eaa13a23592e4c4f02c1ec673fb084d85eacf9f26a6ed0282` |
| `qa/build_v17/page-1.png` | `de1bcd299b9cf8a5fb545f3bfd3a5257b794eda4071374d6fd9dfae3dc28531f` |

主 session 的 TODO_LOG 由协调者汇总；本独立审查仅新增本文件。
