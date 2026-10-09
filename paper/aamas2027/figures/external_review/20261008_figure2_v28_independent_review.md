# Figure 2 v27 独立审查（prompt v28）

日期：2026-10-08。审查对象为未经像素编辑的 `figures/generated/figure2/v27_ai.png`。依据 `figure_role_contract_20261007.md`、`gallery_style_study_20261007.md`，并与 Figure 1 v34、TableDART、CRITIC 及本地样例原图对照。

## 实际查看的证据

- 原图：`paper/aamas2027/figures/generated/figure2/v27_ai.png`，2100×749；使用 `view_image` original 查看。
- paired QA：`figures/qa/build_v24/page-1.png`（Figure 1 v34）和 `figures/qa/build_v24/page-2.png`（Figure 2 v27）均使用 `view_image` original 查看。页面图宽为 7 inch；PDF 两次编译完成且日志无 fatal、overfull、missing-font 或 undefined-reference warning。
- 风格对照：Figure 1 v34 的 central computation architecture 与 v27 的事件时间线共享白底、深蓝/灰/琥珀/紫色、细边框和图标族；F2 没有复制 F1 的 scorer、Residual、counter register 或菜单。

## 结论摘要

**v27 通过，16/16；无硬错误，推荐作为 Figure 2 候选。** 本版仅放大了指定的 runtime error/runtime ledger、feedback hidden until release、visible only at or after `τ_t` 和图例文字；事件拓扑、卡片、颜色和位置保持不变。7-inch paired QA 中主路径、等待边、失败分叉、释放后状态和两行辅助说明均可读。

## 时间因果链逐边核验

1. 顶部完成路径为 `decision t → selected action → call → terminal record → judge → chemical feedback`。`valid return` 标签位于 terminal record→judge 边上，判分发生在终止记录之后。
2. `runtime error` 从 terminal record 向下进入独立 `runtime ledger`，箭头在 ledger 处终止；没有边进入 judge、chemical feedback 或 pending。因此 runtime failure 不生成 chemical label，也不进入 selected-only feedback。
3. chemical feedback 通过唯一的紫色竖直边进入 `pending`，pending 明确标注 `selected-only`。
4. pending 到 `release` 只有一条紫色虚线等待边，方向从 pending 指向 release，边上标注 `τ_t=t+1+D`；没有第二条等待边、时间珠、横向泳道或旁路。
5. `release → released state → first later decision` 是蓝色实线左向链；`visible only at or after τ_t` 位于 released state 下方，清楚表达更新在释放后才可见。

## Figure 1 去重复与风格一致性

- F2 只画一个 selected event 的时间依赖：decision、selected action、call、terminal、judge、chemical feedback、pending、release、released state、first later decision。
- F2 没有 Figure 1 v34 的 `Frozen scorer`、`q^0`、`q=q^0+Δ`、Residual、Scores、Eligible tools 或 provider counter register；released state 仅以通用柱形状态表达时间结果。
- F1 v34 的 `Feedback lifecycle`/`Fig. 2` 接口与 F2 的完整时间线形成清楚的抽象—展开关系，没有重复内部架构。两图共用 document、gear、lock、clock、flask、queue 图标和深蓝/灰/琥珀/紫色语法，但布局分别是架构中心与事件时间条。
- 没有 provider A/B/C、provider 名称、性能数字、features、menu 或额外学习模块。所有辅助文字仍是短标签，未新增解释段落。

## 文字与纸面检查

v27 中 `runtime error`、`runtime ledger`、`feedback hidden until release`、`visible only at or after τ_t` 以及 legend 两行已明显放大。7-inch `page-2.png` 上这些文字与 `τ_t=t+1+D`、`selected-only`、`valid return` 和主卡片标签均可辨认；没有裁切、重叠或箭头方向歧义。`page-1.png` 的 F1 v34 也保持可读，证明两图在同一宽度下风格和密度相容。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 单事件的从 decision 到 release/later decision 时间路径占据主体，runtime 分叉和等待窗口有清楚层级 |
| 模块表达 | 2 | 决策/动作/调用用卡片和齿轮，terminal 用文档卡，judge 用锁，pending 用堆叠卡，release 用时钟，later state 用柱形图 |
| 信息密度 | 2 | 仅保留必要事件和少量辅助文字；没有 F1 架构重复、provider 字母或性能文字 |
| 布局 | 2 | 上方完成链、中央失败分叉、下方延迟回路形成单一时间条；没有交叉边或多余泳道 |
| 色彩线条 | 2 | 蓝色完成/可见路径、灰色记录/ledger、琥珀 judge/chemical result、紫色 pending/release 与 F1 v34 一致 |
| 数据路径 | 2 | terminal 后 judge、runtime error 终止、chemical feedback→pending、单一紫色等待边、release→state→later decision 全部正确 |
| 方法一致性 | 2 | runtime failure 不更新 chemical counter；selected-only pending 与释放门槛保留；没有架构模块或额外反馈来源 |
| 纸面可读性 | 2 | 实际 7-inch paired page-2 查看；放大的辅助标签、公式、图例、主链和箭头均可读，无裁切/重叠 |
| **合计** | **16/16** | **无硬错误，推荐作为候选** |

## 候选结论

**推荐 Figure 2 v27 作为事件时间线候选。** 它完成了目标微调，runtime failure 仍在 ledger 终止，紫色等待边保持唯一，release 后状态和 later decision 的方向正确，并与 Figure 1 v34 保持共享风格而不重复内部架构。用户最终选图和正式整稿接入仍需另行决定。

## 文件与 hash

| 文件 | SHA-256 |
|---|---|
| `figures/prompts/figure2/v28.txt` | `3a4efbc2720e67c384abad66aa0c671a7de37737260e757767cc3a8b37d6de99` |
| `figures/generated/figure2/v27_ai.png` | `10642a1fb7e8f4df1b0bdb1caee6666c6566e0d2303cafd8c64fc2c0c10dbbb2` |
| `figures/generated/figure2/v27_manifest.json` | `ca5b3a491c1c1c5c9909f7aecc709b603ace5c051935d9147f6c7cd24e85b72d` |
| `figures/qa/build_v24/page-1.png` | `25598f4e2470dbd9a4b4ff517d9a98914c2caa7b89f0e220ee18e42669c0ad15` |
| `figures/qa/build_v24/page-2.png` | `ab03df4ac747ba1b9568bbe2c13b279319976f145e4911654fe75c4f8044ddb8` |
| `figures/qa/build_v24/ai_structures_7inch_20261008_v34_v27.pdf` | `e668d5aa47c49fa65f1aacb940de6dedc2cfde66e43f5b87028def5e5f39b8e9` |

本报告仅新增独立审查记录，不修改图片、prompt、正文、实验或方法文件。
