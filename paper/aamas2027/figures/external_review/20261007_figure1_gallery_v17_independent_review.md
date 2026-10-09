# Figure 1 图库风格独立审查：v17 prompt / v09 AI / 7-inch 论文页

- 日期：2026-10-07。
- 对象：`prompts/figure1/v17.txt`、`generated/figure1/v09_ai.png` 与阅读稿 PDF 第 2 页。
- 最终结论：**通过，14/16；必须修复项 0。** 数据路径和方法一致性均为 2/2。
- 通过范围：本版结构图的机制、因果、图库风格与实际通栏阅读检查；不代表整篇论文科学 gate 或投稿政策检查。
- 审查身份：独立图稿审查员。未修改正文、实验、方法、prompt 或 AI 像素；只新增本审查文件。

## 实际查看与证据

1. 已读取完整 v17 prompt；此前读取了风格研究与 v12/v13/v15/v16 完整 prompt。
2. 通过 `view_image` original 查看 v09 原始图，尺寸 2060×763。
3. 通过 `view_image` original 查看 `qa/restore_20261007_v1/page2.png`，尺寸 1190×1540；该图是下列阅读稿第 2 页的渲染，包含通栏 Figure 1、caption 和双栏正文。
4. 已读取 `embedded_image_check.json` 与 `final_manifest.json`。嵌入核验回执记录与 v09 逐像素相同；原始图和 PDF 的文件 SHA-256 已独立读取核对，与回执一致。协调者的 pdfimages 核验为 294 ppi、约 7-inch 通栏嵌入。

阅读稿：`paper/aamas2027/build/reading_ai_20261007_v09/main.pdf`。构建回执记录总计 9 页、正文 8 页、References 从第 9 页开始；本独立图稿审查实际查看第 2 页，不将回执中的页数当作全稿科学审查。

三张 gallery 原图此前均通过 `view_image` 实际查看，其来源和 hash 完整保存在 [v13 独立审查](20261007_figure1_gallery_v13_independent_review.md)。该记录及 v15/v16 失败记录均保留，没有覆盖。

## 方法与因果复核

核对原则：某 provider 的合法已释放反馈，必须先改变其独立状态，再通过当前 residual/score 计算影响后续选择。图上箭头承担真实的计算依赖。

当前方法冻结 `q⁰(x)`。每个 provider 独立保存已释放 `n,s`，残差为 `(n₀·fit_rate+s)/(n₀+n)−fit_rate`，与其冻结分数逐项相加后选择一个工具。有效返回经过冻结 Judge 生成 selected correctness label；在 `τₜ=t+1+D` 释放后更新对应 provider。Runtime failure 不生成 chemical label，不进入正确性计数。

| 检查点 | v09 实际观察 | 结论 |
|---|---|---|
| 当前查询与冻结计算 | Molecule+Endpoint 共同进入 Features；`φ(x)∈R^d` 与正文表示符号相符；冻结 scorer 有锁，输出 A/B/C 三格 `q⁰` | 通过 |
| 独立状态与合成 | Released counters 逐 provider 分行且有 n/s；Residual 为对应的三格 Δ；q⁰ 和 Δ 进入同一 `+` 后生成 q | 通过 |
| 菜单与单次动作 | 中性 A/B/C 菜单独立向下接入 Select；q 高亮 B，只输出一个 B 工具，并进入一次 Call | 通过 |
| 有效返回判分 | valid 从 Return 折向 Judge；Judge 再向下进入 Chemical feedback B、`r∈{0,1}`；此前绕过判分器的边已删除 | 通过 |
| Runtime failure | error 分支从 Return 进入独立 Runtime ledger 并终止 | 通过 |
| 延迟反馈 | Chemical feedback 的唯一下游为 Selected-only queue；queue 到 Release 为紫色虚线，时钟为 `τₜ=t+1+D` | 通过 |
| 释放后回写与身份 | Release 到 Update、Update 到计数寄存器为蓝色实线；计数仅 B 的 n/s 单元高亮；选择、反馈和队列均携带 B | 通过 |
| 后续计算路径 | 回写仅接 counters，既有 counters→Residual→`+`→q→Select 路径完成闭环；没有直接进入 Select 的额外分叉 | 通过 |

A/B/C、空计数格和 `r∈{0,1}` 是机制示意和标签类型，没有报告实验数值。图未引入额外 provider 调用、family sharing 或可训练 backbone。

## 图库风格吸收

TableDART 的浅色区域和具体内部计算已落实为特征/权重/计数/向量/合并算子，核心方法是最大分区。CRITIC 的稀疏闭环组织被落实为可逐段追踪的 feedback→queue→release→update→score 路径。CollabLLM 的记录卡和行动/学习分层被落实为 Return、Chemical feedback、queue 与独立状态寄存器。风格借鉴由对象形态和连接关系承担。

v09 的反馈对象位于白色画布上，紫色只用于局部等待与释放；菜单紧贴 Select，减少横跨方法区的长线。没有沿用前轮整条紫色标题带，或通过长解释标签来掩盖旁路。

## 八项最终评分

| 项目 | 0/1/2 | 实际依据 |
|---|---:|---|
| 视觉中心 | 2 | 中央方法为最大分区，分数合成和选择为主体；判分区保持次级 |
| 模块表达 | 2 | 特征格、固定权重、计数格、向量、`+`、文档、队列和释放门具有不同形态 |
| 信息密度 | 1 | 大多数为短标签；Frozen scorer 仍重复，输入底部 Molecule+Endpoint / x+endpoint 两行存在轻微冗余 |
| 布局 | 2 | 查询、菜单、计算、执行与回写连接分布清楚；反馈对象紧凑，主路径没有交叉 |
| 色彩线条 | 2 | 蓝色在线状态、灰色冻结/外部、琥珀反馈、紫色等待角色稳定；等待与已释放线型区分明确 |
| 数据路径 | 2 | 当前输入、选择、返回、判分、队列、释放与计数回写均能追踪；失败分支独立结束 |
| 方法一致性 | 2 | 保持冻结 prior、provider-local residual、单次调用和 selected-only delayed update |
| 纸面可读性 | 1 | 已看真实 7-inch 论文页；主链和 τ 公式可辨，无裁切或重叠；Return 小字段、向量/计数索引和反馈标签类型偏小 |
| 合计 | **14/16** | **通过；方法和数据路径无实质错误** |

纸面分数来自第 2 页的实际通栏检查，没有用放大原图或单独大字号图稿替代。次级文字偏小属于可读性改进项，本版主要机制和标签仍可读取。

## 必须修复与建议

**必须修复：0。**

建议仅两项，可用于后续 AI 迭代，不改变本版方法通过结论：

1. 精简重复 Frozen scorer 标题及输入底部重复说明。
2. 在保持通栏画幅和紧凑构图的同时，提高 Return 小字段、A/B/C/n/s 索引、反馈类型等次级标签字号；无需增加解释句。

## 迭代记录与可复用资产

| 审查 | 输出 | 结论 |
|---|---|---|
| [v13 独立审查](20261007_figure1_gallery_v13_independent_review.md)，含 v12 prompt 中间意见 | v06 AI | 不通过：B 反馈更新 A 行、Update 直达 Select、查询来源缺失 |
| [v15 独立审查](20261007_figure1_gallery_v15_independent_review.md) | v07 AI | 不通过：Select 旁路、菜单进入 Features、Endpoint 未接线 |
| [v16 独立审查](20261007_figure1_gallery_v16_independent_review.md) | v08 AI | 不通过：valid 绕过 Judge、Features 类型符号错误 |
| 本文件 | v09 AI + 阅读稿第 2 页 | 通过：14/16；0 必须修复 |

后续可直接复用本轮的 gallery 风格拆解、完整 v17、未经像素修改的 v09、provider 向量/计数/合成图形语法、就近菜单约束、valid/error 分支和完整延迟闭环。原始图、三版失败记录及纸面证据均已保存在本地；新图应继续另存版本并保持这些因果约束。

## 文件与 hash

下列路径除 PDF 外均以 `paper/aamas2027/figures/` 为根。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v17.txt` | `8776b10b1600509eec4e4a9fcd133660bc75f76eb4b94511b67a1ffb11f814d5` |
| `generated/figure1/v09_ai.png` | `c793d1d37c98133513d662317a7c9770fd5788d3816d4993be335d819c2ae97c` |
| `paper/aamas2027/build/reading_ai_20261007_v09/main.pdf` | `8b078e87e659d7921618f1c9cae680891ed0c7f646b31408ec5425bed5427cdd` |
| `qa/restore_20261007_v1/page2.png` | `9c9fcc499f0087433f353e826b98bf38e5fed763d586437f39fd73b0ccd739bd` |

嵌入核验回执：`qa/restore_20261007_v1/embedded_image_check.json`，状态 `identical_pixels`，RGB pixel SHA-256 为 `d34359c09ad9bded5c2a47f497349713639a64f931983f249ae6a1bdd7545210`。构建信息：同目录 `final_manifest.json`。

主 session 的 TODO_LOG 由协调者汇总。本独立审查工作已完成。
