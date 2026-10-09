# Figure 1 v31 独立审查（prompt v41）

日期：2026-10-08。审查对象为未经像素编辑的 `figures/generated/figure1/v31_ai.png`。依据 `figure_role_contract_20261007.md`、`gallery_style_study_20261007.md`、当前 released selected-only counter 规则，并与 Figure 2 v26 及本地 ICLR/ICML 图库原图对照。

## 实际查看与纸面状态

- 原图 `paper/aamas2027/figures/generated/figure1/v31_ai.png`，2057×764；使用 `view_image` original 查看，并裁切检查 central hub、`next selection` 回线、eligible-tools 托盘和 Update 终点。
- 目前没有 v31 对应的 7-inch QA 页面；原图尺寸和历史 v29 页面不能替代本版本的 7-inch 检查。因此纸面可读性记为 0（未测）。

## 总结结论

**central architecture hub 已明显改善，但 v31 仍不通过，10/16，有一个硬数据路径错误和一个必须修复的菜单连线问题。** 浅蓝 relay 区占最大面积，固定分数、released evidence、residual、plus、score 和 `select one` 形成了可辨认的方法中心；橙色 execution 区与 Figure 2 v26 共享图标和色板而不复制时间轴。`select one` 拼写正确，未见 A/B/C、渐变、3-D 或水印。

然而，高亮 released-evidence 行的写入端口同时发出一条标注 `next selection` 的蓝色曲线，直接向上进入 Selector。这条边绕过 `residual → plus → score`，在图上形成第二条选择输入，不能解释为单纯的时间提示。左侧 eligible-tools 托盘也没有任何边进入 Selector。两点都使图面无法证明选择只由 `q=q^0+Δ` 和 eligible tools 驱动。

## 通过的结构与风格

- `Features → fixed score → q^0` 和 `released evidence → residual → plus` 的两个输入确实进入中央算子；`q=q^0+Δ`、score vector、`select one` 和 selected tool 的主评分链清楚。
- selected tool→Call→Terminal record→frozen judge→chemical feedback 方向正确；runtime error 只到 Runtime ledger，不进入 chemical feedback。
- chemical feedback→selected-only queue→Release（紫色虚线，`τ_t=t+1+D`）→Update（蓝色实线）→高亮 row 的回写端点方向正确。Update 的箭头头穿入高亮行内部，问题不在该写入终点本身。
- 浅蓝 relay / 橙色 execution 分区吸收了 TableDART 的淡色模块分区；具体网格、寄存器、算子、文档卡、锁/雪花、队列和时钟保留了图库的形状语法。与 Figure 2 v26 共享白底、线宽、图标族和蓝/灰/琥珀/紫色角色。
- Figure 1 没有 Figure 2 的 `decision t`、`pending` 堆叠、`later decision` 或完成/等待图例；Figure 2 也没有 Features、residual、`q=q^0+Δ`、counter register 或候选菜单，因此职责边界总体保持。

## 必须修复的因果与输入问题

### 1. 删除 `next selection` 直连回线（硬错误）

蓝色回线从 released-evidence 高亮行的写入端口出发，沿 central hub 底部右行并上行进入 `select one`。该边把状态直接接到 Selector，绕过上方唯一合法路径 `released state → residual → plus → score → Selector`。虽然它可能想表达“更新后进入下一次选择”，但没有经过残差和分数合成，读者会得到错误的数据依赖。

应删除整条回线及其 `next selection` 标签；保留上方 state→residual→plus→score→Selector 连接。`Update→highlighted row` 只负责写入 state，不再从同一端口向 Selector 发出第二条边。

### 2. 把 eligible tools 接到 Selector

eligible-tools 托盘现在孤立在左侧 input 面板内。它既没有短边进入 Selector，也没有被中央 hub 内的选择器局部包含；图中无法读出候选集合如何约束 `select one`。

建议将托盘移到 `select one` 下方或左下方，在 central hub 内加一条短蓝色上行边，只连接 Selector。不要把托盘连到 state、residual、score 或 execution，也不要让该边与 Update 回写边重叠。

## 文字与寄存器清洁度

- `select one` 已正确，当前未见拼写错误。
- `fixed score` 可改为小标签 `q^0`/`frozen scorer`，与正文和公式统一；`released evidence` 可改为 `released state`，避免读者把 evidence 当成另一种存储。
- `next selection` 是造成错误直连的冗余文字，应随回线删除。
- released-evidence 寄存器是四列两行且没有 `n/s` 表头。它可以作为抽象状态格保留，但在最终稿中应使用两列 `n,s` 或补极小表头，避免被读成四个独立计数维度；不增加 provider A/B/C。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | central relay architecture 是最大分区，plus/score/Selector 成为主体 |
| 模块表达 | 2 | 特征格、固定权重格、released register、residual vector、算子、选择器、执行卡和队列形状可区分 |
| 信息密度 | 2 | 文字总体短，公式只有 `q=q^0+Δ` 与 `τ_t=t+1+D`；需删除误导性的 `next selection` |
| 布局 | 1 | 三分区层级改善，但孤立菜单和 state→Selector 回线造成重复边与局部拥挤 |
| 色彩线条 | 2 | 蓝色方法/更新、灰色冻结/账本、琥珀执行反馈、紫色等待角色稳定；主要箭头方向清楚 |
| 数据路径 | 0 | **硬错误：** `next selection` 回线从 state 直达 Selector，绕过 residual/plus/score；eligible-tools 也未接入 Selector |
| 方法一致性 | 1 | q⁰、Δ、冻结 judge、selected-only queue 和 runtime 分叉正确，但第二条选择输入破坏唯一评分依赖 |
| 纸面可读性 | 0 | v31 对应 7-inch QA 尚未生成；不能用原图或 v29/v30 页面代替 |
| **合计** | **10/16** | **有硬数据路径错误，不作为候选** |

## 候选结论与可执行修改

**v31 不推荐作为候选。** central hub 的布局方向值得保留，但必须先删除 `next selection` state→Selector 直连，并把 eligible-tools 托盘通过一条短局部边接入 Selector。随后重新生成原图并进行独立 7-inch 单图/联合页检查；只有在唯一路径成为 `state→residual→plus→score→Selector`、菜单输入可见且没有新增旁路后，才重新评估候选资格。

## 文件与 hash

| 文件 | SHA-256 |
|---|---|
| `figures/prompts/figure1/v41.txt` | `e6f3f2743aeac83564ae07c0a3cba9c264e92684f24e3186e80af9ed5e47c0e2` |
| `figures/generated/figure1/v31_ai.png` | `5756876ef99fdf1aeba8732d81f50f22164889a7a0c4082cf6bcd0996f2fdfc6` |
| `figures/generated/figure1/v31_manifest.json` | `f3842891da85dae6284851dfdb3efddbbb1815df8d2c6909018fa86689fb5ea1` |

本报告仅新增独立审查记录，不修改图片、prompt、正文、实验或方法文件。
