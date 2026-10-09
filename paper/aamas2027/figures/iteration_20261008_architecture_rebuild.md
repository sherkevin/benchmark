# Figure 1 重构：架构与事件时序分工

## 目的

用户仍认为最新 Figure 1 没有达到样例库质感，且要求两图分工明确。此次仅修改 AI 图稿、提示词、审查与版本索引，不处理实验。

## 问题与保留

- v29 的图级因果审查通过，但两条长链仍像流程图；图级通过不能代替用户的视觉选择。
- v30 改为中央融合区，选择器文字渲染有误，保留原始输出。
- v31 修正文字，但独立审查发现状态直达 Selector 的旁路与未连接的 eligible menu；10/16，不通过。
- v32 局部改图保留了旁路，并把菜单误接状态；预检拒绝，保留。

## v33 的图稿切口

依据决议 0015，Figure 1 的中心是分数合成及选中 provider 的状态归因。Figure 2 承担单事件时序。Figure 1 把外部调用、终态判分、等待和释放合为标注 `Fig. 2` 的反馈生命周期接口；接口只有已选且有效的已释放化学结果可以回写。这是表达粒度调整，不更改算法或反馈权限。

参考 TableDART 的淡色局部分区和具体计算对象，CRITIC 的闭环及短标签；色板、图标和字体与 Figure 2 保持一致。参考原图与 hash 见 `style_references/20261007_gallery_v1/manifest.json`。

## 可检查的目标

1. 中央方法占画布约三分之二；融合算子和 released-state 寄存器是视觉中心。
2. 寄存器四个示意 provider 行、每行两个计数列，单行高亮，不显示 A/B/C。
3. 菜单只接 Select；状态只经 Residual→plus→Scores 到 Select，不能直连。
4. Figure 1 不出现完整 call/terminal/judge/queue/release 时间链；Figure 2 不出现 scorer/residual/counter/menu。
5. 实际 7-inch 页面核验文字、箭头和颜色；按现有八项 0/1/2 评分，数据路径与方法一致性的硬错误不能用总分抵消。
6. prompt、原始 PNG、生成请求、hash、失败及独立审查全部留存。未选定的版本不覆盖 `qa/main.pdf`。

## 结果

- v33 已生成并保存为 `generated/figure1/v33_ai.png`，prompt 为 `prompts/figure1/v43.txt`。
- v33 的预检图上只出现规定的计算和反馈接口路径；仍需独立审查与 7-inch 纸面 QA，不能视为已选定。
- v29、v30、v31、v32 均保留原 PNG、prompt、manifest、generation request 和失败审查，不覆盖任何历史版本。

## v34 微调（prompt v44）

v33 的独立审查已通过，因此本轮不重画方法结构，只做两个可逆的 AI 微调：在 Query 卡中加入小型分子 glyph，使输入同时显出分子与性质文档；把 `q = q⁰ + Δ` 下方的短线改成无箭头注释线，避免被误读成第三条数据边。v34 保持 v33 的全部模块、短标签、颜色、布局和 exact graph，Figure 2 仍只承担 selected event 的延迟时间线。原图、prompt、manifest、generation request 已保存；v34 尚未替代 v33，等待独立审查。

- 原图：`generated/figure1/v34_ai.png`
- prompt：`prompts/figure1/v44.txt`
- SHA-256：`e37f8dd8c0648f4b442157d54095a8c4de134705c2457470b3ab1b52fed1589f`
- 参考版：v33，SHA-256=`b6e6b1f532a849421e202d3dc4291c30797ee38472aebf413af1e7e3500177d2`
- 审查预登记：`external_review/20261008_figure1_v44_independent_review.md`

## v34 独立审查结果

独立审查报告 `external_review/20261008_figure1_v44_independent_review.md` 给 v34 **16/16，无硬错误，推荐候选**。新增分子 glyph 没有挤压 Query；无箭头公式引线没有形成第三条数据边；v33 的 exact graph、唯一回写链和 Figure 1/Figure 2 职责边界全部保持。v34 已成为画廊中的推荐当前候选，但用户尚未做最终选择，`figures/qa/main.pdf` 不变。

7-inch QA 实际查看 `qa/build_v23/page-1.png/page-2.png`；当前 PDF SHA=`b3a84b838b93dfa587780fefa2a56994b8b45161b2e2ea8c7c063168b7d1ed57`，F1 页面 SHA=`25598f4e2470dbd9a4b4ff517d9a98914c2caa7b89f0e220ee18e42669c0ad15`，F2 页面 SHA=`814d53e5535069bd67b49003053a4833f9d468d54b5021f6b6e7b97b357425aa`。审查报告中另一份 PDF hash 是独立编译时的元数据版本，作者侧校正已追加在报告末尾。

## Figure 2 v27 独立审查结果

`external_review/20261008_figure2_v28_independent_review.md` 实际查看 v27 原图及与 Figure 1 v34 配对的 7-inch QA，给出 **16/16、无硬错误、推荐候选**。放大后的次级标签和图例可读；主链、runtime ledger dead-end、唯一紫色等待边、release→released state→later decision 均保持；没有引入 Figure 1 的 scorer、Residual、counter register、Scores 或菜单。v27 已成为推荐当前候选，v26 保留为上一版。
