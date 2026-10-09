# Figure 1 v34 独立审查（prompt v44）

日期：2026-10-08。审查对象为未经像素编辑的 `figures/generated/figure1/v34_ai.png`。依据 `figure_role_contract_20261007.md`、`gallery_style_study_20261007.md`，并对照 Figure 2 v26、TableDART、CRITIC 和本地样例原图。

## 实际查看的证据

- 原图：`paper/aamas2027/figures/generated/figure1/v34_ai.png`，2020×779；通过 `view_image` original 查看，另放大核验 Query、plus、state/update 和 feedback interface 区域。
- 7-inch QA：独立生成并编译 `figures/qa/build_v23/ai_structures_7inch_20261008_v34.pdf`，实际查看 `page-1.png`（F1 v34）和 `page-2.png`（F2 v26）。两次 `pdflatex` 均完成，无 fatal、overfull、missing-font 或 undefined-reference warning。
- 风格参考：实际对照本地 `iclr2026-1635.jpg`、`iclr2024-17.jpg`、`icml2025-0242.jpg`；F1/F2 在 7-inch 页面使用一致的白底、细边框、图标族和蓝/灰/琥珀/紫色角色，同时保持不同布局。

## 结论摘要

**v34 通过，16/16；无硬错误，推荐作为 Figure 1 候选。** 两项针对性修订均正确落地：Query 卡新增清晰的小分子球棍 glyph，公式下方标记改成没有箭头头的普通注释线。其余 central computation architecture、唯一回写链、短标签和 Figure 1/Figure 2 角色均保持 v33 的通过状态。

## Exact graph 逐边核验

1. `Query → Frozen scorer → q^0 → plus`：Query 卡同时包含分子 glyph 和 property document glyph；单一箭头进入带锁固定网格的 Frozen scorer，输出 q⁰ 向量，经短边进入 plus。
2. `Released state → Residual → plus`：Released state 有四个示意 provider 行和两列 `count/correct`，其中一行高亮；state 只通过 Residual 读出，Residual 的短边进入 plus。
3. `plus → Scores → Select`：plus 输出进入四格 Scores，再进入 Select gate。
4. `Eligible tools → Select`：托盘紧贴 Select 下方，单一短蓝色上行箭头进入 Select；没有接到 state、Residual、Frozen scorer 或 feedback。
5. `Select → Selected tool → Feedback lifecycle → Released feedback → Update → highlighted row`：Selected tool 进入灰色 Feedback lifecycle（含 `Fig. 2` 接口标记），再进入琥珀 Released feedback；唯一蓝色回写边进入 Update，随后向上穿入高亮 state 行。

没有 state→Select shortcut、第二条反馈环、queue/release 时间链、provider A/B/C、额外 persistent state 或其它旁路。Update 箭头头在高亮 cell 内，而不是停在 card border。`q=q^0+Δ` 下方现在是无箭头的垂直注释线，不会被读成第三个数据输入。

## 方法与角色边界

- Frozen scorer 的锁和固定网格、q⁰ 向量、Residual、Scores、plus 和 Select 直接表达“冻结分数 + released evidence → 选一个工具”。
- Released state 的两列表头和单行高亮表达 provider-local `count/correct` 写入；其它行保持不变。
- Feedback lifecycle 仅用文档、齿轮、时钟和 `Fig. 2` 表达抽象外部接口；Figure 1 不展开 call、terminal、judge、runtime ledger、pending 或 release 的事件时间线。
- Figure 2 v26 继续承担 decision→selected action→call→terminal→judge→chemical feedback→pending→release→later decision；它不复制 F1 的 scorer、Residual、counter register、Scores 或 Eligible tools。两图共享视觉语法而不共享布局。

## 图库风格与文字检查

中央浅蓝方法区约占画面三分之二，plus、Frozen scorer、Released state 和 Select 是视觉中心；这是 TableDART 的局部分区与具体计算对象语法。Residual/Scores 向量、算子、寄存器、工具托盘、反馈接口和 Update 使用不同形状，避免同质化流程框。CRITIC 的局部反馈组织和 Figure 2 v26 的图标/线宽/色板被吸收，但没有重画时间条。

短标签均正确：`Query`、`Frozen scorer`、`Released state`、`count`、`correct`、`Residual`、`Scores`、`Select`、`Eligible tools`、`Selected tool`、`Feedback lifecycle`、`Fig. 2`、`Released feedback`、`Update` 和 `q=q^0+Δ`。数学向量标签 `q^0` 也保留且清晰。未见拼写错误、解释句、时间戳、性能值或多余箭头。

## 7-inch 纸面检查

`build_v23/page-1.png` 中分子 glyph 与 document glyph、`q^0`、无箭头公式注释线、`count/correct`、Residual、Scores、Select、Eligible tools、反馈接口、Update 及高亮行写入端口均可辨认；主边没有裁切、重叠或缩小后的方向歧义。`build_v23/page-2.png` 的 Figure 2 主链与等待段同样可读，二者的职责和视觉语法保持一致。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 中央融合区和 released-state 寄存器占最大视觉权重 |
| 模块表达 | 2 | Query glyph、固定网格、两列寄存器、Residual/Scores 向量、plus、Select、托盘、接口卡和 Update 形状各异且语义明确 |
| 信息密度 | 2 | 标签短且完整，只有规定公式和数学向量符号，没有重复流程或指标文字 |
| 布局 | 2 | Query—方法—反馈接口三层关系清楚；回写边唯一、直接、无交叉；公式标记不再形成额外箭头 |
| 色彩线条 | 2 | 深蓝自适应结构、灰色冻结/接口、琥珀 released feedback、紫色时钟与 F2 统一且克制 |
| 数据路径 | 2 | 指定五段 exact graph 全部成立；没有 state→Select shortcut、菜单旁路或第二反馈边 |
| 方法一致性 | 2 | q⁰+Δ、provider-local 两计数列、selected-only released feedback、单个高亮 row 更新和 F2 时序分工均正确 |
| 纸面可读性 | 2 | 实际 7-inch F1/F2 页面均查看；主模块、短标签、公式、计数表头和写入箭头可读，无裁切/重叠 |
| **合计** | **16/16** | **无硬错误，推荐作为候选** |

## 候选结论

**推荐 v34 作为 Figure 1 候选。** 本版已满足 exact graph、方法角色、图库风格和 7-inch QA。保持当前唯一 feedback route 和 scoring route；不要再加入 provider 字母、queue/release 时间链、state→Select 边或第二个反馈环。用户最终选图和正式整稿接入仍需另行决定。

## 文件与 hash

| 文件 | SHA-256 |
|---|---|
| `figures/prompts/figure1/v44.txt` | `1a084d983aef1e899a3dde11105d8168b77b0429960b1c5b6f93940f373142cb` |
| `figures/generated/figure1/v34_ai.png` | `e37f8dd8c0648f4b442157d54095a8c4de134705c2457470b3ab1b52fed1589f` |
| `figures/generated/figure1/v34_manifest.json` | `428ea727d791c9bddd3544f029bee2aac983ef9287a5c555b7983e58e3345108` |
| `figures/qa/build_v23/page-1.png` | `25598f4e2470dbd9a4b4ff517d9a98914c2caa7b89f0e220ee18e42669c0ad15` |
| `figures/qa/build_v23/page-2.png` | `814d53e5535069bd67b49003053a4833f9d468d54b5021f6b6e7b97b357425aa` |
| `figures/qa/build_v23/ai_structures_7inch_20261008_v34.pdf` | `e4e0ce6b307628e137bc7a012f36b018912c57263c87500484a6869fff6f45e7` |

本报告仅新增独立审查记录和对应 QA 构建，不修改原图、prompt、正文、实验或方法文件。

## 作者侧证据校正（2026-10-08）

独立审查正文保留不变。审查完成后在当前工作树重新计算 QA PDF，当前不可变文件的 SHA-256 为 `b3a84b838b93dfa587780fefa2a56994b8b45161b2e2ea8c7c063168b7d1ed57`；两页 PNG hash 与正文表格一致：Figure 1 `25598f4e2470dbd9a4b4ff517d9a98914c2caa7b89f0e220ee18e42669c0ad15`，Figure 2 `814d53e5535069bd67b49003053a4833f9d468d54b5021f6b6e7b97b357425aa`。独立评分、图像结论和原始审查意见不变；差异来自审查者与作者分别编译 PDF 时的 TeX 元数据时间戳。
