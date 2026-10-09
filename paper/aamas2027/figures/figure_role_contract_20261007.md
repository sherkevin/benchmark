# Figure 1 / Figure 2 角色分工与去重复规范

日期：2026-10-07。该文档是图稿设计和审查的执行规范，不替代方法正文。

## 1. 两张图各自回答什么问题

### Figure 1：方法架构

读者看完 Figure 1 应该能回答：

- 当前选择分数由哪些模块产生？
- 冻结分数、已释放状态和选择器怎样连接？
- 一次有效结果经过什么判分和释放过程，怎样回到后续选择？

因此 Figure 1 可以展开内部结构：特征、冻结 scorer、残差/计数、合成算子、选择器和 selected-only 更新。但它不应靠重复的 `A/B/C` 文字填满模块；provider 示例可以通过图标、一个高亮行或一个 selected-tool token 表达。

### Figure 2：单次事件时间线

读者看完 Figure 2 应该能回答：

- 一个 selected call 从发起到终止经历哪些时刻？
- runtime failure 与 chemical feedback 在哪里分叉？
- 反馈为什么不能立即影响下一次动作？
- 释放后哪一个后续决策第一次能看到更新？

Figure 2 只画一个事件的时间依赖：decision → call → terminal → judge → pending → release → later decision。它不重复 Figure 1 的特征格、冻结 scorer、provider residual、计数寄存器或候选菜单的内部架构。事件中的工具身份用一个“selected call / selected result”对象表达，不在多个模块上重复写 `A/B/C`。

## 2. 去重复硬规则

1. Figure 2 不得出现 Figure 1 的 `Features`、`Frozen scorer`、`Residual`、`q=q^0+Δ`、provider counter register 或完整候选菜单。
2. Figure 2 最多保留一个 selected-action token；不在 Select、Call、Return、Feedback、Pending、Update 上分别重复 provider 字母。
3. Figure 2 的核心视觉中心是时间顺序和延迟窗口，不是工具集合或分数计算。
4. Figure 1 的候选工具只作为输入菜单和一个 selected provider 结果；不把同一 provider 身份复制到每个模块标题。
5. 文字使用自然短标签（`selected call`、`terminal record`、`pending`、`release`、`later decision`），避免单个孤立字母承担语义。
6. 两张图可以共享蓝/灰/琥珀/紫色语法，但不能共享同一套模块布局。

## 3. 评价指标

- **职责可分辨性**：遮住 Figure 1 后，Figure 2 仍能独立讲清延迟反馈；遮住 Figure 2 后，Figure 1 仍能讲清选择器结构。
- **重复率**：Figure 2 与 Figure 1 的同名模块不超过 2 个（允许 `selected call` 和 `release` 作为接口词）。
- **字符洁净度**：Figure 2 不出现连续 provider 字母串；provider 身份如非必要不写入节点文字。
- **因果完整性**：Figure 2 必须保留 terminal 后判分、selected-only pending、释放时钟和 later decision；runtime failure 不进入 chemical feedback。
- **版面**：两张图都需在 7-inch 通栏实际渲染检查；Figure 1 偏架构密度，Figure 2 偏事件路径密度。

## 4. 当前基线

用户认为 Figure 1 v07 的视觉密度和文字克制度较好。它作为 Figure 1 的视觉参考，不直接作为当前方法图源，因为其中的旧 residual / provider-bias 表达已不完全对应当前实现。Figure 2 v23–v25 的问题是重新引入了 Figure 1 的菜单、provider 身份和更新寄存器，导致两图职责重叠。后续从 v26 起按本规范重画 Figure 2。
