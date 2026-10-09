# Figure 1 角色分工独立审查：v37 prompt / v27 AI

- 日期：2026-10-07。
- 对象：`figures/prompts/figure1/v37.txt`、`figures/generated/figure1/v27_ai.png`。
- 纸面证据：实际查看 `figures/qa/build_v19/page-3.png` 的上下 paired Figure 1/Figure 2 页面；该 build 是 **10.3-inch 展示页，不是 7-inch 纸面页**。
- 结论：**v25/v26 的 Update 终点问题仍未彻底修复；当前不作为候选，11/16。**
- 硬错误：**1 个数据路径终点错误。** Update 的竖直蓝箭头尖端停在高亮行的下缘/白色间隙，没有进入蓝色中行 cell 内。
- 审查身份：独立图稿审查员；只读审查，不修改图片、prompt、正文、实验或方法。
- 实际查看：v27 原图通过 `view_image` original 查看，并对 Update→state 区域做裁切；`build_v19/page-3.png` 通过 `view_image` original 查看。

## 角色分工与文字清洁

v27 仍承担 Figure 1 的方法架构职责，而不是 Figure 2 v26 的单事件时间线：它展开 Features、Frozen score、released state、residual、plus、score、Selector、Eligible tools 和 execution branch；底部只保留 queue→Release→Update 回写。没有 `decision t`、later-decision 卡片或 Figure 2 的时间轴布局。

- 原图和 paired page-3 中没有独立 provider 字母 `A`、`B`、`C`、provider 名称或任意 row ID；`n`、`s` 仅是状态统计列。
- 标签是短词，公式为 `q=q^0+Δ` 和 `τ_t=t+1+D`；没有 family residual、provider bias、额外调用、性能数字或训练模块。
- Eligible tools tray 紧邻 Selector，由一条局部蓝色上行边进入 Selector；没有任何边把它接到 state、residual、plus、score 或 execution 其它模块。

## 实际拓扑检查

### 已通过的路径

- `molecule/endpoint → Features → Frozen score → q^0 → plus` 清楚。
- `released state → residual → plus → score → Selector → selected tool` 清楚；residual 作为 state 的读出位于上方，没有插入 Update→state 写入边。
- `selected tool → Call → Terminal record → Frozen endpoint judge → Chemical feedback` 完整；runtime error 只进入 Runtime ledger dead-end。
- `Chemical feedback → selected-only queue →` 紫色虚线 `Release →` 蓝色实线 `Update` 方向和颜色正确；没有第二条等待边或 Release→state 旁路。
- 图面采用白底、input/relay/execution 小标题、具体网格/寄存器/向量/算子/卡片和深蓝/灰/琥珀/紫色角色，与 Figure 2 v26 的线图标和纸面纹理一致但布局不同。

### 硬错误：箭头仍未进入高亮 row 内

v37 prompt 明确要求把 register 改为两行，并让 Update 的竖直箭头“overlap the blue row interior”，禁止停在 card border、bottom edge 或 white row。实际 v27 原图及 page-3 裁切显示：

- 高亮蓝色行位于 register 下方；箭头从 Update 向上；
- 箭头尖端刚好停在高亮行的下缘/其下方白色间隙，没有可见部分伸入蓝色 cell 内；
- 在 10.3-inch page-3 上仍能看出尖端没有进入 row。缩到 7-inch 后，这个终点会更容易被读成“写到卡片底边”。

这不是单纯的视觉偏好。虽然没有旁路，但合法的 `Update → highlighted released-state row` 边没有连接到目标 cell，图中不能证明 released selected-only evidence 只写入该 provider 行。因此数据路径和方法一致性仍有硬错误，不能由其它风格分数抵消。

修复应把 Update 保持在 register 下方，继续使用短竖直蓝箭头，但让箭头头明确穿入高亮蓝 row 内部（留出可见重叠），而不是只碰到底边或白色间隙；不要增加第二条写入边。

## 10.3-inch 纸面检查与 7-inch 限制

我实际查看了 `build_v19/page-3.png`：上半部分 Figure 1 的主链、菜单局部边、execution 分支、queue/release 和 Update 区域均可辨认；下半部分 Figure 2 的时间线也保持独立，证明两图在 paired 页面上没有互相覆盖。主标签和公式在 10.3-inch 展示尺寸清楚，`n/s`、`τ_t`、runtime error 等次级字段偏小。

该页明确是 10.3-inch 展示页，不能宣称 v27 已通过 7-inch paper gate。修复 endpoint 后仍应另编 7-inch 单图/联合页，重新检查 row 内箭头、`q^0`、`Δ`、`n/s`、`τ_t`、valid/error 和 terminal 字段。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | relay 的冻结打分、residual 合成、plus 和 Selector 是主体，不被时间线或 queue 抢占 |
| 模块表达 | 2 | Features/weight grids、two-row state register、residual vector、plus、Selector/menu、execution cards 和 queue 形态清楚 |
| 信息密度 | 2 | 短标签和两条公式，无 provider 字母、段落框或性能数字 |
| 布局 | 1 | 主 hub 和 F1/F2 paired 排版紧凑，但 Update→state 终点仍落在 row 下缘，写入几何不清 |
| 色彩线条 | 2 | 紫虚线等待、蓝实线 Release→Update、蓝 state/update、灰记录、琥珀判分角色稳定 |
| 数据路径 | 0 | **硬错误：** Update 箭头未进入高亮 row cell；唯一写入链的合法目标终点未完成 |
| 方法一致性 | 1 | 冻结 q⁰、selected-only queue、delayed release 和 runtime 分支正确，但 provider-local row 写入无法由箭头证明 |
| 纸面可读性 | 1 | 已查看 10.3-inch paired page-3；主结构可读但次级字段偏小，且 7-inch 页面尚未测量 |
| 合计 | **11/16** | **硬数据路径错误仍在；不作为候选** |

## 硬错误、候选结论与下一步

**硬错误（1）：** Update→released-state 箭头没有进入两行 register 的高亮蓝 row 内，而是停在下缘/白色间隙。v27 **不推荐作为候选**，不能称为 v25/v26 硬问题已修复。

**重新生成条件：** 只修正箭头终点：让箭头头在高亮蓝 row 内有明确重叠；保持 Eligible tools 只进入 Selector、residual 只读 state、queue→Release 紫虚线、Release→Update 蓝实线、无 A/B/C、无 Figure 2 时间线复刻和无第二条 state 写入边。

**纸面条件：** 端点修复后另行生成 7-inch QA 页；10.3-inch page-3 只能作为当前 paired 展示的部分证据。

本报告基于实际 v27 原图和 build_v19/page-3，不以放大 PNG或10.3-inch展示页冒充7-inch通过，也不宣称用户已最终选定该图。

## 文件与 hash

下列路径以 `paper/aamas2027/figures/` 为根。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v37.txt` | `b009b7e970736fbaf8e51ab88672cdb0959fccaaa5ca70435a13f3c68e037355` |
| `generated/figure1/v27_ai.png` | `8bdee7d9d0670edd67a7663ac420b4d98dc5cbea92015ee8972c22cb62deb88b` |
| `generated/figure1/v27_manifest.json` | `5e83de81ad4c1449251df1ba967cbc97d9b93d3c58454efeaca7d0bcc3dff3d6` |
| `qa/build_v19/page-3.png` | `591f386581ffe7a8be63dc9c59e6d3ed3501f6376f93a702999c0d15eac0307c` |

主 session 的 TODO_LOG 由协调者汇总；本独立审查仅新增本文件。
