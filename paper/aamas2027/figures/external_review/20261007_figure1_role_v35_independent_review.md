# Figure 1 角色分工独立审查：v35 prompt / v25 AI

- 日期：2026-10-07。
- 对象：`figures/prompts/figure1/v35.txt`、`figures/generated/figure1/v25_ai.png`。
- 结论：**主体架构和风格通过，但 Update→released-state 的终点仍是硬数据路径错误；当前不作为候选，10/16（纸面项未测）。**
- 硬错误：**1 个。** Update 的蓝色回写线先向左再向上，箭头头停在 released-state 卡片的底部外框；它没有进入高亮中行的 `n/s` cells。
- 纸面状态：仓库中未找到 v25 对应的 7-inch QA 页面；`build_v17/page-1.png` 属于 Figure 1 v21，不能替代 v25 的纸面检查。因此纸面可读性记为 0（未测），不将原图放大视为纸面证据。
- 审查身份：独立图稿审查员；只读审查，不修改图片、prompt、正文、实验或方法。
- 实际查看：v25 原图通过 `view_image` original 查看；原图尺寸 2057×764。

## Figure 1/2 角色分工

v25 是架构图而非 Figure 2 v26 的事件时间线。它以 Features、Frozen score、released state、residual、plus、score、Selector 和 execution branch 为主体，底部只保留 queue→Release→Update 回写；没有 `decision t`、later-decision 卡片或 Figure 2 的单事件时间脊。因此职责分辨清楚。

## 实际图像核对

### 已通过的部分

- 图中没有独立 provider 字母 `A`、`B`、`C`、provider 名称或任意 row ID。`n`、`s` 是 state register 的两列统计量。
- `molecule/endpoint → Features → Frozen score → q^0 → plus` 和 `released state → residual → plus → score` 的中心架构清楚；residual 是 state 的读出，没有另一份持久状态。
- `score → Selector → selected tool → Call → Terminal record → Frozen endpoint judge → Chemical feedback` 路径完整；valid 分支经过 Judge，runtime error 只进入灰色 Runtime ledger dead-end。
- Eligible tools tray 位于 Selector 下方，只有一条短蓝色上行边进入 Selector；没有连接 released state、residual、plus、score 或 execution 其它模块。它没有复刻 Figure 2 的时间线。
- Chemical feedback 向下进入 selected-only queue；queue→Release 是紫色虚线且箭头头指向 Release；Release→Update 是蓝色实线，方向向左且没有第二条等待边。
- 视觉风格采用白底、少量小组标题、具体网格/寄存器/向量/算子/卡片和蓝/灰/琥珀/紫色角色，密度比大面板版本更接近样例库。

### 硬错误：Update 未进入高亮 state row

v35 明确要求“released state 是唯一 persistent state store”“Update arrowhead must enter the blue middle row directly”，并禁止 U-shaped route、bottom-edge arrow 和 whitespace endpoint。实际 v25 的下方回写为：

`queue → Release → Update → [向左长线 → 向上箭头]`

蓝色箭头尖端停在 released-state register 的**底部外框**，而不是中间高亮 `n/s` 行的 cell 内。图中虽然没有第二条旁路，但合法的 `Update → highlighted released-state row` 边没有被画出来；读者无法确认更新只作用于 selected provider 的计数行。这是因果/数据路径硬错误，不能被其它美术分数抵消。

修复需要重新生成图：将 Update 放到高亮中行同高、register 的右侧（或等价邻接位置），用一条短蓝色水平箭头直接进入高亮 `n/s` row 内部；保留当前 queue→Release 紫色虚线和 Release→Update 蓝色实线，不添加 Release→register 外框/底边旁路。

## 顶会风格与纸面限制

原图的白色背景、低饱和配色、局部小标题、具体内部形态和短标签具备顶会架构图方向；中心方法区仍比 execution/queue 更重要。箭头大体短直，queue→Release 与 Release→Update 的线语义清楚。由于 Update endpoint 错误，底部闭环在视觉上仍不可靠。

当前没有 v25 的 7-inch page render。不能根据 v21 的 `build_v17/page-1.png` 给 v25 盖章；应在修复 endpoint 后重新编译单图/联合页，再检查 `q^0`、`Δ`、`n/s`、`τ_t`、valid/error、Terminal record 小字段和 queue/release 线条在纸面上的字号。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 中央 relay 的冻结打分、residual 合成、plus 和 Selector 是主要视觉主体 |
| 模块表达 | 2 | Features/weight grids、state register、residual vector、plus、Selector tray、execution cards 和 queue 形态有区分 |
| 信息密度 | 2 | 短标签和两条公式表达机制，无 provider 字母串、段落框或性能数字 |
| 布局 | 1 | 上游 hub 与 Selector 紧凑，但底部 Update→state 仍为 U 形折线，箭头落在卡片底边 |
| 色彩线条 | 2 | 紫色等待、蓝色更新、灰色冻结/诊断、琥珀判分的角色大体稳定；queue→Release 与 Release→Update 方向清楚 |
| 数据路径 | 0 | **硬错误：** Update 箭头终止于 released-state 外框底边，未进入高亮 `n/s` row；唯一合法写入链未完成 |
| 方法一致性 | 1 | 冻结 q⁰、selected-only queue、delayed release、Judge/runtime 分支正确，但 state row 写入无法由箭头证明 |
| 纸面可读性 | 0 | v25 对应的 7-inch QA 页未提供/未找到；原图不能替代纸面检查 |
| 合计 | **10/16** | **因数据路径硬错误不通过；纸面项待修复后重测** |

## 硬错误、候选结论与建议

**硬错误（1）：** Update→released-state 线未进入高亮行，停在 register 底边。当前 v25 **不推荐作为候选**，即使其它风格和架构评分较好。

**重新生成的必要条件：** 只改变回写几何：Update 与高亮 row 同高、短水平蓝箭头直接进入 `n/s` row 内部；residual 保持上方只读路径；Eligible tools 只进 Selector；不得恢复 A/B/C 或添加第二条回写边。

**纸面 QA：** 修复后生成 v25 对应的单图 7-inch page 和联合页；只有在实际页面检查主要节点/公式/箭头可读后，才重新给第 8 项评分并决定候选。

本报告基于实际 v25 原图，不以旧版本 page render 代替 v25 纸面检查，也不宣称用户已最终选定该图。

## 文件与 hash

下列路径以 `paper/aamas2027/figures/` 为根。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v35.txt` | `291257025929768c6d3c257c611fefa0efab4b61a5b2eeec0ad5ae6d759284ea` |
| `generated/figure1/v25_ai.png` | `926579b692663fd815fd7887000a0cae3627da022a3005a63573eff3341d9b62` |
| `generated/figure1/v25_manifest.json` | `929d30666ee1b44518204e7f7b454873f7472aff9bc753abe578cb6a686fdc5b` |
| `qa/v25 page` | 未找到，未计算 |

主 session 的 TODO_LOG 由协调者汇总；本独立审查仅新增本文件。
