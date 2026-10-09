# Guide v01：状态与视觉审查

日期：2026-10-08。实际查看 `guide.tex`、README、target_spec/arithmetic_audit 的状态字段，以及 `versions/v01_20261008/page-1.png` 至 `page-3.png`。只写本报告，不改正文、Guide 源码或实验。

## 结果

**PASS：可以展示为独立目标规划稿。** 本稿视觉与措辞清楚区分已有测量和假设目标，没有把 T 数字呈现为已发生的实验结果。

| 检查 | 判定 | 观察 |
|---|---|---|
| 每页状态提醒 | PASS | 三页均有斜向 `TARGET GUIDE / NOT MEASURED` 水印、planning 页眉、M/T 页脚；水印在文字背景层，不遮挡表字。 |
| 截表时 M/T 区分 | PASS | 行名或列头显式标 M/T；T 同时使用蓝色。即使裁掉页眉，目标仍有文字标记，不只依赖颜色。 |
| 目标表角色 | PASS | E1–E6 对应正式正文 Tables 3–8；E1 另有 delay 数据表对应正文 Figure 3，因此 Guide 是七张表而非新增第七组实验。 |
| 字号、列宽 | PASS | 表字 9.5 pt，自然宽度。p3 拒答表七列仍可读；无 resizebox、数字压线或换行拆坏百分比。 |
| 页面与标题 | PASS | 三页完整，页码 1/3–3/3；每个标题后均有对应内容，没有孤立标题、表格跨页被切或末页内容撞页脚。 |
| 统计身份 | PASS | 不预填目标 CI/p 值；正文反复要求真实评估。整数目标是规划情景，delay/rescue-harm 未写成预测轨迹。 |
| 数据/对照身份 | PASS | repeated calls 不称独立分子；oracle 与 retrospective fixed 区分；E2 scaffold estimand 独立；E5 为 kernel cap；E6 为 reused cohort 次级分析和 matched-count 目标。 |
| 真实正文隔离 | PASS | 源码明确独立 planning artifact；README/config 标注非 forecast、非 preregistration、科学 gate HOLD，不将 Guide 当成正式论文结果。 |

## 可选清理，不阻止展示

1. 首段 `feasible target scenario` 可改为 `arithmetically consistent target scenario`。当前数字的可行性来自整数、分母和菜单上界；没有证据保证某个训练方法一定能达到它们。
2. 当前 caption-outside-box 和 8.5 pt typewriter font 警告未造成截图中的可见错误。可在后续版本清理 LaTeX 结构与字体设置；保留 v01，不为消除警告改变数据或缩字。

本审查通过的是 Guide 的呈现与状态隔离；目标尚未变成实验结论，正式论文和完整科学 gate 均不因此升级。数值验证沿用独立算术审查，本次没有运行新实验。
