# Figure 1 v33 独立审查（prompt v43）

日期：2026-10-08。审查对象为未经像素编辑的 `figures/generated/figure1/v33_ai.png`。依据 `figure_role_contract_20261007.md`、`gallery_style_study_20261007.md`、`iteration_20261008_architecture_rebuild.md` 和当前 released selected-only counter 规则，并对照 TableDART、CRITIC、Figure 2 v26 与本地原图样例。

## 实际查看的证据

- 原图：`paper/aamas2027/figures/generated/figure1/v33_ai.png`，2020×779；通过 `view_image` original 查看，并放大核验 state/update、plus、Selector 和 feedback interface。
- 7-inch QA：`figures/qa/build_v22/page-1.png`（Figure 1 v33）和 `figures/qa/build_v22/page-2.png`（Figure 2 v26）均通过 `view_image` original 查看。PDF 用 7 inch 图宽编译两次，无 fatal、overfull、missing-font 或 undefined-reference warning。
- 样例风格：实际对照本地 `iclr2026-1635.jpg`、`iclr2024-17.jpg`、`icml2025-0242.jpg`；F1/F2 对照沿用白底、深蓝/灰/琥珀/紫色、图标族和细边框语法，但布局职责不同。

## 结论摘要

**v33 图级通过，16/16；无因果、数据路径或方法硬错误，推荐作为 Figure 1 候选。** 这版把 Figure 1 变成计算中心的架构图：中央浅蓝区域突出 Frozen scorer、released state、Residual、plus、Scores 和 Select；右侧只保留标注 `Fig. 2` 的反馈接口，不复制 Figure 2 的时间链。7-inch 页面上主模块、公式、箭头和状态写入端口均可读。

## Exact graph 逐边核验

图中可见的语义边与 prompt v43 要求一致：

1. `Query → Frozen scorer → q^0 → plus`：Query 的箭头进入锁定 scorer，scorer 输出 q⁰ 向量，再由短边进入 plus。
2. `Released state → Residual → plus`：四个示意 provider 行的 released-state 寄存器只通过 Residual 读出，Residual 的第二条短边进入 plus。
3. `plus → Scores → Select`：plus 的输出进入四格 Scores，再进入 Select gate。
4. `Eligible tools → Select`：托盘位于 Select 下方，唯一短蓝色上行箭头进入 Select；没有边连接 state、Residual、scorer 或 feedback。
5. `Select → Selected tool → Feedback lifecycle → Released feedback → Update → highlighted state row`：选中的工具进入灰色 Feedback lifecycle 接口，再进入琥珀 Released feedback；底部单一蓝色回写边先到 Update，随后向上进入高亮 provider 行。

没有 `state → Select` shortcut、第二条反馈环、Release/queue 时间链或其它旁路。Update 箭头尖端位于高亮行内部，不停在卡片边框；其余三行保持未高亮。公式上方的短下引线是 `q=q^0+Δ` 的算子标注，不是额外数据模块或状态边；若后续要做最严格的边语法，可去掉该引线的箭头头以避免被误读为第三个输入。

## 方法与角色边界

- Frozen scorer 用锁和固定网格表达，q⁰、Residual、Scores 和 plus 使冻结分数与 released provider 证据的合成可直接读出。
- Released state 明确为四行、两列 `count/correct`；一行以浅蓝高亮，行标记使用抽象工具 glyph，没有 A/B/C 或 provider 名称。
- 反馈接口用文档、齿轮、时钟三种图标和 `Fig. 2` 表示 selected interaction 的外部生命周期；Released feedback 是唯一回写对象。Figure 1 不展开 call、terminal、judge、runtime ledger、pending 或 release 时序。
- Figure 2 v26 承担 decision→selected action→call→terminal→judge→chemical feedback→pending→release→later decision 的事件时间线。它没有 F1 的 scorer、Residual、counter register、Scores 或 Eligible tools，因此两图职责可分辨。

## 图库风格与视觉层级

中央计算区约占画面三分之二，融合算子和 released-state 寄存器是视觉中心，符合 TableDART 的淡色局部分区与具体计算对象语法。Scores/Select/Eligible tools 的局部闭合结构借鉴 CRITIC 的短反馈组织；右侧接口保持与 Figure 2 v26 相同的图标、线宽和色板，但没有照搬其长时间轴。白底、细 navy/blue 边框、灰色冻结接口、琥珀结果和紫色时钟均保持克制，没有渐变、3-D、泳道、巨型标题或装饰符号。

文字全部为 prompt 规定的短标签：`Query`、`Frozen scorer`、`Released state`、`count`、`correct`、`Residual`、`Scores`、`Select`、`Eligible tools`、`Selected tool`、`Feedback lifecycle`、`Fig. 2`、`Released feedback`、`Update` 和 `q=q^0+Δ`；未见拼写错误或额外性能/时间文字。Query 当前以文档 glyph 表达输入，未显示独立分子 glyph，这是图形细节建议，不构成方法错误。

## 7-inch 纸面检查

在 `build_v22/page-1.png` 中，central hub、四行计数格、`count/correct`、q⁰、`q=q^0+Δ`、Residual、Scores、Select、Eligible tools、Feedback lifecycle、`Fig. 2`、Released feedback、Update 和高亮 row 写入箭头均能辨认；没有裁切、重叠或缩小后方向歧义。`build_v22/page-2.png` 的 Figure 2 主链和紫色等待段也可读，证明两图在同一 7-inch 宽度下保持风格一致且职责分离。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 融合算子、Frozen scorer、released state 与 Select 位于最大的中央方法区 |
| 模块表达 | 2 | 固定网格、四行×两列寄存器、Residual/Scores 向量、plus、Select gate、工具托盘、接口卡和 Update 各有明确形状 |
| 信息密度 | 2 | 只保留规定短标签和一条公式，没有 provider 字母、时间戳、指标或重复流程卡 |
| 布局 | 2 | 查询、方法、外部反馈三层关系清楚；唯一回写边沿底部布置，无交叉和悬空终点 |
| 色彩线条 | 2 | 深蓝自适应结构、灰色冻结/接口、琥珀 released feedback、紫色时钟的角色与 F2 一致 |
| 数据路径 | 2 | 五段 exact graph 全部存在；没有 state→Select shortcut、菜单旁路或第二条反馈写入边 |
| 方法一致性 | 2 | q⁰+Δ、provider-local 两计数列、selected-only released feedback、单行 Update 和 F2 时序分工均保持 |
| 纸面可读性 | 2 | 实际 7-inch 单图和 F2 对照页均查看；主模块、公式、计数表头和箭头可读，无裁切/重叠 |
| **合计** | **16/16** | **无硬错误，推荐作为候选** |

## 候选结论与小建议

**推荐 v33 作为 Figure 1 候选。** 它已经满足本轮架构角色、exact graph、图库风格和 7-inch QA；用户最终选图与正式整稿接入仍需另行决定。

可选的视觉微调不改变评分：

1. 在 Query 卡内补一个很小的 molecule glyph，与 document glyph 并列，使输入更贴近 TableDART 的具体内容卡；不要改变 Query→Frozen scorer 边。
2. 若严格要求图面只出现语义数据边，把 q 公式到 plus 的箭头改成无箭头短引线；当前它已明显是公式标注，不影响方法路径。
3. 保持四行两列 `count/correct`、单个高亮 row 和唯一 Update 终点；不要加入 provider 字母、queue/release 时间轴或第二个反馈环。

## 文件与 hash

| 文件 | SHA-256 |
|---|---|
| `figures/prompts/figure1/v43.txt` | `7bed388792c2d204275f46799feee6cc184180e55079126ce51b504ee7e73448` |
| `figures/generated/figure1/v33_ai.png` | `b6e6b1f532a849421e202d3dc4291c30797ee38472aebf413af1e7e3500177d2` |
| `figures/generated/figure1/v33_manifest.json` | `6ef1a680924e179b4e122d91edb0a7c2a0bfe3138a946e408cf0dde1ef7639af` |
| `figures/qa/build_v22/page-1.png` | `d66d2f1b76dee7ce81740a256c933f40792a0436c36a167198b61039d4644e7b` |
| `figures/qa/build_v22/page-2.png` | `814d53e5535069bd67b49003053a4833f9d468d54b5021f6b6e7b97b357425aa` |
| `figures/qa/build_v22/ai_structures_7inch_20261008_v33.pdf` | `b8dcd23df681f39993d21e84912d8766fd46f9ff5c19eb4688cefc869319d29d` |

本报告仅新增独立审查记录，不修改图片、prompt、正文、实验或方法文件。
