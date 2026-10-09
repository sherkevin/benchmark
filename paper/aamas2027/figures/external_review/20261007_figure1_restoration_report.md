# Figure 1 恢复与图库风格迭代

日期：2026-10-07。只处理图稿、版本与编译，不跑实验、不改方法或效果数字。

## 目的与衡量方式

恢复用户阅读 PDF 中的 AI Figure 1，并使图形本身表达模块、内部计算、数据路径和在线更新机制。
衡量方式：实际图库像素对照；完整连边和 provider 身份审查；7-inch 纸面检查；PDF 内嵌图与原始 AI PNG 逐像素一致；正文不超过 8 页。

## 原图没有丢失

用户文件 `Downloads/MIRA-Relay延迟反馈架构图 (1).png` 与仓库
`figure1/v07/figure1_v07.png` 均为 2120×742 RGB，SHA-256：
`f29cb6865b85f601e1969e8e145b2ca9052fbdcf20cf6f15191a6f8df8520f4c`。
正文后来引用了程序版 `figure1/v08/figure1_v08.png`，并且 AI 候选没有接回默认阅读稿。
原图含旧 family residual/provider bias 标签，本轮保存原图并按当前 provider-local counter 方法重新生成。

## 实际吸收的样例规律

- TableDART：主方法是视觉中心，淡色区域分组，内部模块和合并算子具体可见。
- CRITIC：用连接闭环表达迭代，不另堆一排解释性 outcome 方框。
- CollabLLM：用记录卡和局部状态表达交互与学习。
- GenAL：局部记忆与跨层反馈的表达作为补充参考。

完整逐图拆解、适用范围和检查规格见 `../gallery_style_study_20261007.md`。
四张参考原图、作者链接与 hash 在 `../style_references/20261007_gallery_v1/`。
没有把这些样例称作同一种万能风格，也没有复制其研究内容。

## 每轮结果

| AI 图 | 完整 prompt | 独立审查 | 处理 |
|---|---|---|---|
| v06 | v13 | 8/16；选 B 却更新 A、绕过残差的 Select 支路、输入接错 | 保存，不接入正文 |
| v07 | v15 | 9/16；身份修正，但旁路和输入问题保留 | 保存，不接入正文 |
| v08 | v16 | 9/16；主区正确，Return 绕过 Judge，特征类型符号错误 | 保存，不接入正文 |
| v09 | v17 | 14/16；0 必须修复；实际纸面检查通过 | 当前阅读稿 Figure 1 |

v12 为最初风格方案，v14 为根审查修复稿；两者作为 prompt-only 中间版本保留。
四次生成均调用真实内置 `image_gen.imagegen`；生成请求、原始输出、hash 和独立失败意见分别保存。
没有手工重画、覆盖旧版或修改 AI 图像像素。PDF 渲染与提取仅用于验证。

最终独立评分：视觉中心 2、模块表达 2、信息密度 1、布局 2、色彩线条 2、
数据路径 2、方法一致性 2、纸面可读性 1。评分记录为
`20261007_figure1_gallery_v17_independent_review.md`。
剩余两项：Frozen scorer/输入说明有轻微重复；output/status、索引和反馈类型可进一步放大。
本轮没有因此声称三份完整科研验收标准已经通过，也不声称全部图稿已最终完成。

## 正文与编译证据

- `main.tex` 默认 Figure 1：`figures/generated/figure1/v09_ai.png`。
- 当前阅读稿：`../../build/reading_ai_20261007_v09/main.pdf`；已同步到 `../../build/main.pdf`。
- PDF：9 页总计；正文 8 页，References 从第 9 页开始；Figure 1 在第 2 页。
- PDF SHA-256：`8b078e87e659d7921618f1c9cae680891ed0c7f646b31408ec5425bed5427cdd`。
- 英文源码 SHA-256：`063a29377ff1c5c7c096b9dc344ca5cfbe6a4bdd2280c544a48b666f6090121e`。
- AI PNG SHA-256：`c793d1d37c98133513d662317a7c9770fd5788d3816d4993be335d819c2ae97c`。
- BibTeX 和三遍 LaTeX 完成，无 Overfull、Undefined、Citation、Missing $、Fatal、Emergency。
- PDF 提取出 2060×763、294 ppi 的 Figure 1，RGB 像素与原始 AI 图完全一致。
- 第 2、8、9 页已查看；源码在构建前后相同。

前后源码、旧 `build/main.pdf`、渲染、提取图和最终 manifest 在 `../qa/restore_20261007_v1/`。
编译日志、第一次页码检测失败及修正记录在 `../../build/reading_ai_20261007_v09/`。
页码检测最初误要求 References 独占大写整行；实际为 title-case 且右列同一行已有引文，修正检测后确认第 9 页。没有为此更改 PDF 或重新安排正文。

## 阅读与投稿的图源

本轮重新核对 [AAMAS 2027 官方说明](https://warwick.ac.uk/fac/sci/dcs/aamas2027/guidelines-and-policies/instructions/)。
默认阅读稿用 AI v09；`make submission` 显式定义 `MIRASubmissionFigures` 后选用程序 Figure 1 v08。
两种构建源清楚标明，避免再次把用户的阅读图静默换掉。投稿图源分支本轮没有重新执行完整科学 preflight，不把当前 AI PDF 当作符合会议图像政策的投稿版本。

## 下一步

1. 在不改拓扑的前提下，用新完整 AI prompt 精简重复标签、提升次级字号，保留 v09。
2. Figure 2 已保存同风格完整 prompt `../prompts/figure2/v23.txt`，尚未生成；程序 v13 和 AI v22 都保留。
3. 数据图继续绑定真实结果用程序绘制；不扩大到实验或方法修改。
