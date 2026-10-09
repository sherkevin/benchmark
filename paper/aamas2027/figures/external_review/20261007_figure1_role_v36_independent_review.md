# Figure 1 角色分工独立审查：v36 prompt / v26 AI

- 日期：2026-10-07。
- 对象：`figures/prompts/figure1/v36.txt`、`figures/generated/figure1/v26_ai.png`。
- 结论：**v25 的唯一硬错误尚未修复；Update 箭头仍未进入高亮 row，当前不作为候选，10/16（纸面项未测）。**
- 硬错误：**1 个数据路径终点错误。** Update→state 蓝线的箭头尖端停在高亮中行下方的白色间隙/行边缘，没有落入中行 cell 内。
- 纸面状态：未找到 v26 对应的 7-inch QA 页；旧版本 page render 不替代本版，纸面可读性记为 0（未测）。
- 审查身份：独立图稿审查员；只读审查，不修改图片、prompt、正文、实验或方法。
- 实际查看：v26 原图通过 `view_image` original 查看；原图尺寸 2057×764；另对 Update→state 区域做原图裁切核对。

## 角色分工与无重复

v26 仍是 Figure 1 架构图，不是 Figure 2 v26 的事件时间线。它展开 Features、Frozen score、released state、residual、plus、score、Selector 和 execution branch；底部只保留 queue→Release→Update 回写，没有 `decision t`、later-decision 卡片或 Figure 2 的时间脊。Eligible tools tray 局部接入 Selector，不复制 Figure 2 的 pending/state 时间布局。

图中没有 provider 字母 `A`、`B`、`C`、provider 名称或行 ID。`n`、`s` 是 state register 的统计列。标签短，只有 `q=q^0+Δ` 与 `τ_t=t+1+D` 两条公式；没有 family residual、provider bias、额外调用、性能数字或训练模块。

## 实际路径核对

### 通过的路径

- `molecule/endpoint → Features → Frozen score → q^0 → plus` 清楚。
- `released state → residual → plus → score → Selector` 清楚；residual 是 state 的只读读出，不是另一个 state store，也没有介入 Update→state 写入边。
- `score → Selector` 有一条短边；Eligible tools tray 由一条局部蓝色上行边进入 Selector，未连接 released state、residual、plus、score 或 execution。
- `Selector → selected tool → Call → Terminal record → Frozen endpoint judge → Chemical feedback` 完整；runtime error 只到灰色 Runtime ledger dead-end。
- `Chemical feedback → selected-only queue →` 紫色虚线 `Release →` 蓝色实线 `Update` 的方向和颜色均清楚；没有第二条 queue/Release 路径或 Release→state 旁路。

### v25 硬错误的复核：仍未进入 row

v36 prompt 要求箭头“terminate with a clearly visible arrowhead INSIDE the highlighted middle-row cells”，并禁止 card border/bottom-edge endpoint。v26 的实际裁切显示：

- state register 的蓝色中行下缘约在原图 y≈477；
- Update 的向上箭头尖端约在 y≈480，处于中行下方的白色间隙，而不是蓝色中行内部；
- 线随后继续向下连接到 Update，因而读者看到的是一条长水平加竖直回折，终点仍没有落在合法 `n/s` cell 内。

这不是单纯的版式偏好。当前图虽没有旁路，但唯一合法的 `Update → highlighted released-state row` 边没有连接到目标 row，不能证明 released selected-only evidence 只写入该 provider 的 `n,s` 计数。因此数据路径和方法一致性仍有硬错误，不能由样例风格或其它得分抵消。

修复方式仍是：把 Update 与高亮中行同高并紧邻 register 右侧（或等价位置），用一条短蓝色水平箭头，令箭头尖端明显落在中行 `n/s` cells 的内部；不要再用向上折线、card bottom edge 或白色间隙作为 endpoint。

## 样例库风格和纸面限制

原图的白色背景、input/relay/execution 小标题、网格/寄存器/向量/plus/卡片、深蓝/灰/琥珀/紫色角色与 Figure 2 v26 和本地样例库一致；整体是架构 hub，不是流程海报。queue→Release 虚线与 Release→Update 蓝实线的颜色语义已正确。

仓库中没有 v26 对应的 `build_v18` 或其它 7-inch page；不能把 v21 的 `build_v17/page-1.png` 当作 v26 纸面证据。修复 endpoint 后需重新编译单图/联合页，再检查 `q^0`、`Δ`、`n/s`、`τ_t`、valid/error、Terminal record 和 queue/release 在纸面上的字号及箭头清晰度。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | relay 的冻结分数、残差合成、plus 和 Selector 是主要视觉中心 |
| 模块表达 | 2 | Features/weight grids、state register、residual vector、plus、Selector/menu、execution cards 和 queue 形态有区分 |
| 信息密度 | 2 | 短标签和两条公式，未出现 provider 字母、段落说明或性能数字 |
| 布局 | 1 | 中心 hub 紧凑，但 Update→state 仍是长水平加竖直折线，且尖端落在 row 下方间隙 |
| 色彩线条 | 2 | 紫虚线等待、蓝实线 Release→Update、蓝 state/update、灰记录、琥珀判分角色稳定 |
| 数据路径 | 0 | **硬错误：** Update 箭头尖端未进入高亮中行 cell；唯一写入链的目标终点不合法 |
| 方法一致性 | 1 | 冻结 q⁰、selected-only queue、delayed release 和 runtime 分支正确，但 provider-local row 写入未被箭头证明 |
| 纸面可读性 | 0 | v26 7-inch QA 页未找到；原图不能替代实际页面检查 |
| 合计 | **10/16** | **硬数据路径错误仍在；当前不作为候选** |

## 硬错误、候选结论与下一步

**硬错误（1）：** Update→released-state 箭头终止在高亮 row 下方的白色间隙/边缘，不在中行 `n/s` cells 内。v26 **不推荐作为候选**，也不能称为 v25 修复完成。

修复后应保持：Eligible tools 只进入 Selector；residual 只读 state；queue→Release 紫色虚线；Release→Update 蓝色实线；Update→中行 cell 的短蓝水平边；无 A/B/C、无 Figure 2 时间线复刻、无第二条 state 写入边。

修复后重新生成 v26+并提供对应的单图七英寸 page，再补做第 8 项纸面评分和最终候选判断。

本报告基于实际 v26 原图及放大区域核对，不以旧版本 page render 代替 v26 纸面检查，也不宣称用户已最终选定该图。

## 文件与 hash

下列路径以 `paper/aamas2027/figures/` 为根。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v36.txt` | `7e457576dc201769e4f8bc3c435d0b88ce7cf81a03cf7bd0e6a0adc7bba5f00e` |
| `generated/figure1/v26_ai.png` | `d06837499d31f0bb96c64e3ebc96571b1ae203dd717246a06bde7dd5c8c2a492` |
| `generated/figure1/v26_manifest.json` | `6d6fa950d9f238aefeee91d75b5daf09adb73c57bbfc8813fabc1925be7bda75` |
| `qa/v26 page` | 未找到，未计算 |

主 session 的 TODO_LOG 由协调者汇总；本独立审查仅新增本文件。
