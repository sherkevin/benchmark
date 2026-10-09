# 用户图稿复核：延迟反馈与多阶段供应商选择

日期：2026-10-05  
依据：`paper/aamas2027/figures/FIGURE_REVIEW.md` 的 8 项、每项 0–2 分规则，以及当前论文的 selected-only / delayed-release 合同。

## 总结

两张图的主线已经成形，颜色、箭头和信息边界都比普通流程图清楚，可以作为论文图的设计底稿。当前不能直接放进最终稿，原因有两个：

1. **图 2 的时钟标签有实质歧义。** 图中把 release phase 后的框写成 `next decision / decision t+1`。但合同是 `$\tau_t=t+1+D$`：当 `$D>0$` 时，决策 `$t+1$` 早于该事件释放，不能读取这条反馈。应改成“first decision at or after `$\tau_t$`”或画出 `$t+1$` 使用旧状态、`$\tau_t$` 后才使用新状态的分支。
2. **图 1 的 `ledger only` 放置位置容易误导。** 它在 selector 区，并从 `fit-only fallback` 通过虚线连过去，读者可能理解为 selector 的控制流。实际上它是 fallback 终态反馈的记账路径，应移到 termination/release 区，或明确标注为 `fallback outcome → diagnostic ledger only`。

此外，两张图都是 PNG。投稿交付还需要保留 SVG/HTML 或绘图脚本，并在论文目标栏宽下重新检查字号。当前渐变、阴影和混合字体略接近说明性信息图；内容修正后建议改成统一无衬线字体、扁平填色，减少装饰。

## 图 1：`多阶段供应商选择与延迟发布流程图 (1).png`

### 已通过的部分

- 左到右的四段结构明确：Before the call → selector → after termination → delayed release。
- `unselected / not observed`、runtime failure、frozen endpoint judge、`$\tau_t=t+1+D$` 都被画出来，核心信息边界没有被省略。
- 黑色实线和虚线分别表达 observed event 与 waiting/control flow；即使去掉颜色，主要关系仍可读。
- `family residual + provider bias` 体现了方法的具体机制，`fit-only fallback` 体现了固定参考策略。

### 必须修改

- 将 `released challenger outcome` 改为 `released selected-challenger outcome` 或 `released selected-provider outcome`，避免被理解为未选 provider 也会产生反馈。
- 把 `ledger only` 从 selector 区移到终态反馈路径；当前虚线箭头的语义不够清楚。
- 在 `released history` 到 selector 的连线上标 `$S_t$` 或 `released state`，并明确它进入 relay；当前入口箭头停在 selector 边框，方法输入关系不够显式。
- delayed release 区增加一句 `decisions continue while feedback is pending`，或画一个使用旧 `$S$` 的中间 decision，避免读者以为整个系统会等到 `$\tau_t$` 才做下一次选择。

### 评分

| 项目 | 分数 | 说明 |
|---|---:|---|
| 一句话信息 | 2/2 | 选择、终止判定和延迟释放均在图中出现 |
| 主线结构 | 2/2 | 左到右阅读顺序清楚 |
| 信息边界 | 2/2 | selected-only、runtime failure、release clock 明确 |
| 方法映射 | 1/2 | `ledger only` 的位置和 released-history 入口需修 |
| 目标版式 | 1/2 | 全宽缩放后辅助标签会偏小，PNG 不利于无损排版 |
| 视觉克制 | 1/2 | 四色和图标可控，但渐变、阴影和装饰较多 |
| 黑白/无障碍 | 2/2 | 线型、文字和框型提供了冗余编码 |
| 可编辑/可复现 | 0/2 | 当前只看到 PNG，未提供源文件或渲染脚本 |
| **合计** | **11/16** | 内容底稿可用，投稿版需修正 |

## 图 2：`延迟反馈决策流程图.png`

### 已通过的部分

- 单一纵向阅读方向非常清楚，编号 1–6 让 action、termination、judge、queue、release 的顺序易于检查。
- “action at `$t$` completes before any state update” 是很好的边界句，明确阻止当前动作偷看自己的结果。
- 未选输出被划掉，`system failure` 与 chemical correctness 分开，符合当前运行时和化学正确性分离的合同。
- `pending queue`、`correctness hidden for `$D$` ticks`、`$\tau_t=t+1+D$`、`only next decision reads it` 试图把延迟更新写成可执行时序。

### 必须修改

- 把底部的 `next decision / decision t+1` 改为 `first decision at or after release tick $\tau_t$`，或改画为：`decision t+1 uses unreleased state`；到 `$\tau_t$` 后才进入 `released state`。这是当前最重要的语义修正。
- 将 `only next decision reads it` 改为 `decisions at or after $\tau_t$ read it`，避免把一次性反馈误解为只影响一个动作。
- 第 1 步补充 `reads released state $S_t$ and eligible menu $A_t$`；现在只写 choose provider，输入边界需要靠 caption 补充。
- 统一字体。当前标题、正文和公式的字形混杂，缩小到单栏后会显得不够像论文图。

### 评分

| 项目 | 分数 | 说明 |
|---|---:|---|
| 一句话信息 | 2/2 | 延迟反馈生命周期表达完整 |
| 主线结构 | 2/2 | 纵向主线清楚 |
| 信息边界 | 1/2 | action/feedback 分界清楚，但 `t+1` 与 `$\tau_t$` 冲突 |
| 方法映射 | 1/2 | selected-only 与 judge 对齐，release 后 next-action 标签需改 |
| 目标版式 | 1/2 | 单栏缩放后辅助文字偏小，PNG 不是最终交付格式 |
| 视觉克制 | 1/2 | 结构干净，但渐变、阴影和衬线大标题偏说明图风格 |
| 黑白/无障碍 | 2/2 | 编号、边框和线型提供冗余编码 |
| 可编辑/可复现 | 0/2 | 当前只看到 PNG，未提供源文件或渲染脚本 |
| **合计** | **10/16** | 逻辑底稿可用，时钟必须先修 |

## 采用建议

- **图 1**：保留为总览图的结构，先修 `ledger only`、selected-only 标签和 pending 期间的继续决策说明；之后重绘成 SVG/PDF。
- **图 2**：保留为时序图的结构，但必须先修 `$t+1$` / `$\tau_t$` 冲突；修正前不进入论文正文。
- 两图都不需要增加更多模块。当前缺口是时序和信息边界的精确表达，不是继续堆内容。

