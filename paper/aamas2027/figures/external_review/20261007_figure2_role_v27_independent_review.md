# Figure 2 角色分工独立审查：v27 prompt / v26 AI

- 日期：2026-10-07。
- 对象：`figures/prompts/figure2/v27.txt`、`figures/generated/figure2/v26_ai.png` 与 `figures/qa/build_v12/page2.png`。
- 结论：**角色分工与因果路径通过，15/16；无必须修复的科学或结构错误。**
- 状态：合格候选；用户尚未在候选版本中做最终审美选择，本报告不把“通过”写成用户已最终选定。
- 审查身份：独立图稿审查员；只读审查，不修改图片、prompt、正文、实验或方法。
- 实际查看：v26 原图与 build_v12/page2 均通过 `view_image` original 查看；原图尺寸 2098×749，纸面页尺寸 1190×1540。

## 角色契约核对

Figure 2 的职责是回答一次 selected call 的时间依赖：`decision t → selected action → call → terminal record → judge → chemical feedback → pending → release → released state → first later decision`。Figure 1 承担特征、冻结分数、provider-local residual/counters、菜单和选择器的内部架构；Figure 2 不应复制这些结构。

v26 满足去重复硬规则：

- 图中没有字面 provider 标签 `A`、`B`、`C`，也没有单独的大写 provider token。
- 没有 `Features`、`Frozen scorer`、`Residual`、`q=q^0+Δ`、`n/s` counter register、Eligible tools/menu 或完整架构面板。
- `judge` 与 `release` 是角色契约要求的接口词，不构成 Figure 1 的内部架构复制；没有把 provider 身份复制到 action、terminal、feedback、pending 或 state。
- 视觉中心是上方事件脊与下方紫色等待窗口，而不是工具集合或分数计算；遮住 Figure 1 后，时间线仍能独立说明 delayed feedback。

## 实际路径检查

### 通过的主路径

1. `decision t → selected action → call → terminal record` 由短的深蓝实线水平箭头连接。
2. terminal record 的 `valid return` 只进入灰色的 `judge / frozen endpoint`，再进入琥珀色 `chemical feedback`，并带有 `r∈{0,1}`。
3. chemical feedback 通过单一下行紫色边进入 `pending / selected-only` 卡片；卡片不写 provider 身份。
4. pending 通过一条单向紫色虚线向左进入 `release` 时钟，释放时刻只标一次 `τ_t=t+1+D`，并写明 feedback hidden until release。
5. release 后是单向蓝色边到 `released state`，再由一条蓝色边到 `first later decision`；“visible only at or after `τ_t`”与这段顺序一致。

### 运行失败分支

terminal record 下方的 `runtime error` 分支进入灰色 `runtime ledger` 后终止，没有箭头通向 chemical feedback、pending 或 released state。因此 runtime failure 不会被画成 chemical correctness，也不更新后续状态。

### 时间方向与可读性

上方事件脊从左向右表达调用至终止；下方释放段从右向左用箭头明确表达 pending 先等待、release 后才可见，再进入左侧 later decision。箭头头均落在具体卡片，不存在双向等待箭头、重复时间珠、泳道横线或 terminal→state/judge→state 旁路。方向虽在 pending 后折回左侧，但虚线与实线图例、单向箭头和 `τ_t` 标签使顺序可读。

## 图库风格与纸面观察

v26 采用了与 Figure 1 相容而不重复的语法：蓝色表示已完成/可见的决策状态，灰色表示记录和冻结判分，琥珀表示 chemical feedback，低饱和紫表示 pending/release；文档卡、锁、齿轮、时钟和堆叠事件卡各自表达不同状态。没有大标题条、段落框、渐变、阴影、装饰网格或性能数字。

在实际 build_v12/page2 的七英寸页面上，主事件卡、pending/release/state 和主箭头可读且无裁切或重叠。`runtime status`、`feedback hidden until release`、`visible only at or after τ_t` 以及右下角两行图例明显更小，因此纸面项保守记 1；这不影响主因果路径，但最终排版可略增大这些次级标签。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 单事件主脊和紫色等待/释放窗口是核心，未被工具菜单或架构计算抢占 |
| 模块表达 | 2 | action/call、terminal document、judge lock、feedback flask、pending stack、release clock 和 released state 各有具体形态 |
| 信息密度 | 2 | 标签短且无重复架构词，满足时间线功能；没有解释段或性能数值 |
| 布局 | 2 | 上方主脊、下方释放段和 runtime dead-end 分层清楚；无交叉、重叠或多余路径 |
| 色彩线条 | 2 | 蓝/灰/琥珀/紫角色稳定，实线与紫色虚线图例对应完成/等待，箭头统一且方向明确 |
| 数据路径 | 2 | terminal→judge→chemical feedback→pending→release→released state→later decision 完整；runtime ledger 不进入化学链 |
| 方法一致性 | 2 | 反馈严格在终止后判分，selected-only pending，释放时钟后才可见；无 provider 复用、同动作反馈或 runtime 化学计数 |
| 纸面可读性 | 1 | 已实际检查七英寸 page2；主标签清楚，但次级状态字段、等待说明和图例偏小 |
| 合计 | **15/16** | **角色与因果通过；只建议做次级字号微调** |

## 硬错误与建议

**硬错误：0。** 未发现 Figure 1 架构重复、provider 字母污染、等待方向错误、terminal→state/judge→state 旁路，或 runtime failure 进入 chemical feedback 的方法错误。

**建议（不改变方法或角色）：**

1. 如果最终纸面仍使用当前缩放，可将 `runtime status`、`feedback hidden until release`、`visible only at or after τ_t` 和图例文字略放大；不增加新解释句。
2. 保持当前单一紫色虚线和单一 `τ_t=t+1+D`，不要为强调时间再加时间珠、第二条等待边或重复的 later-state 卡。

本报告基于实际 v26 原图和 build_v12/page2，不以放大 PNG 替代纸面检查，也不宣称用户已最终选定该版本或已完成投稿接入。

## 文件与 hash

下列路径以 `paper/aamas2027/figures/` 为根。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure2/v27.txt` | `b2d72731d552a25b5eb8df9223393c61526609d818269716922d6008fe0da89e` |
| `generated/figure2/v26_ai.png` | `417b12d5b441f727ac2040fc49be711fabec3288cfbb0cfe123d59204dab8d64` |
| `generated/figure2/v26_manifest.json` | `d5ca6075d56dfe331df3cf76fbcc5a748ed8849649ce683a80dde1059c23f8d4` |
| `qa/build_v12/page2.png` | `b34a55cc992a981bd042c7ce83419f8b97bc27887af15bb8830ff766bff38457` |

主 session 的 TODO_LOG 由协调者汇总；本独立审查仅新增本文件。
