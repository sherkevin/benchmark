# 两篇 ICLR 2024 Outstanding 论文的结果表排版样例

日期：2026-10-08。范围：只核对参考论文的获奖身份与结果表布局；没有运行实验、改动结果数字或修改主稿/预览。

## 来源与身份核验

[ICLR 官方奖项页](https://blog.iclr.cc/2024/05/06/iclr-2024-outstanding-paper-awards/)把以下两篇列在 `Award Winners` 下，且在 `Honorable mentions` 标题之前，身份是 **Outstanding Paper winner**。本次不把 DoLa 称为 Outstanding：该官方名单未列出 DoLa。

| 论文 | 官方链接的 OpenReview ID | 实际阅读的 PDF 结果表页 | 本地 PDF SHA256 |
|---|---|---|---|
| [Vision Transformers Need Registers](https://arxiv.org/pdf/2309.16588) | `2dnO3LLiJ1` | p7 Table 2；p8 Table 3；p17 Table 6 | 见 `references/source_manifest.json` |
| [Never Train from Scratch: Fair Comparison of Long-Sequence Models Requires Data-Driven Priors](https://arxiv.org/pdf/2310.02980) | `PdaPky8MUn` | p4 Table 1；p5 Table 2；p9 Table 3 | 见 `references/source_manifest.json` |

网页访问使用 agent-reach 的网页阅读路由：Jina Reader 首次请求发生 `SSL_ERROR_SYSCALL`，随后按本地网页读取回退用 curl 获取官方 HTML。原始 HTML 和去除脚本/样式后的文本均已保存。PDF 直接取自上述 arXiv 端点，以 150 dpi 渲染并实际查看六个完整页面。下载链接未固定 arXiv 版本号，复核时以保存的 PDF hash 为准。

## 表格首先解决什么问题

读者需要迅速找到比较对象，沿同一列比较同一种量，并知道哪些行共享比较条件。这里可借用的结构都对应这三个动作：短列名减少寻找成本，成对相邻行减少视线跳转，横线和两层表头显示比较条件。论文获奖身份只用于选取参考样例，不构成每个排版细节都适合 AAMAS 的依据。

## Vision Transformers Need Registers

**p7 Table 2：同一个结果问题拆为两个子表。** 左侧 (a) 是冻结特征上的线性评测，右侧 (b) 是 zero-shot 分类。两个子表底部对齐，分别有完整横线和短子标题；共同 caption 放在下方。左侧首列为模型，三个数字列的表头分为数据集名与指标名两行；`rmse ↓` 在表头交代方向。右侧只保留一个 ImageNet 数字列，没有为空间一致性添加无用列。

**p8 Table 3：方法按有/无 register 成对排列。** DeiT-III、OpenCLIP、DINOv2 各占两行，组间用细横线；没有竖线、底色或整行加粗。方法名左对齐，三列数据居中、各留一位小数。表宽约为版心的 61%，保持自然宽度；六个数据行不需要铺满整个版心。

**p17 Table 6：列太多时纵向拆块。** 14 个数据集分为两个七列块，第二块重写列名与三种 token 行名；主结果保持一位小数。标准差作为同行的小号灰色附加值，caption 说明它来自随机 patch 选择，并指出该表是 Table 1 的细化。主数字比不确定性更显眼，但 caption 和数值都保持可追溯。

| 要素 | 实际组织与测量 |
|---|---|
| 表头 | Table 2 左侧两层；Table 3 单层；Table 6 两个竖向块，各重复自己的列头 |
| 数字列 | 居中；分类/分割一位小数，Table 2 的 rmse 三位小数；精度按指标统一 |
| 行距与分组 | p7/p8 同一模型的两行文字顶部间距约 10.96 pt；跨模型组约 15.96 pt；横线占据组间空隙 |
| 字号 | 表头、方法名、主数字、caption 与正文均约 9.96 PDF pt；p7 子标题约 8.97 pt，是正文的 90% |
| 注释比例 | p7/p8 caption 各约四行，放表下方；p17 caption 四行。p17 标准差约 5.98 pt，是主数字的 60%，灰度为 0.5 |
| 线条 | 外框只有上/下横线，约 0.80 pt；内部规则约 0.50 pt；没有单元格网格 |

截图：[p7](references/registers_table2_p07.png)、[p8](references/registers_table3_p08.png)、[p17](references/registers_table6_p17.png)。

## Never Train from Scratch

**p4 Table 1：caption 先给出比较条件。** Caption 在表上方，先指出 LRA，再解释上部从零训练与下部 SPT 的关系、模型规模约束和缺失符号。表头只有一层；方法列左对齐，七列结果右对齐。八个已有方法与两个 SPT 方法之间加一条横线；同组行之间不画线。加粗只作用于各结果列的最高数字。

**p5 Table 2：长方法名与八个结果列仍保持一层表头。** `Avg.` 放最后；caption 明确平均值不含 PathX-256，并解释引用结果、重现结果的标记，以及不可计算与未报告的两个不同符号。前六个方法与两个 Pythia 行之间用横线分组。引用放进方法单元格，避免增设来源列。

**p9 Table 3：存在真实共同语义时才用两层表头。** 上层为 SC、sCIFAR、BIDMC；下层只为 SC 的 Causal/Bi. 和 BIDMC 的 HR/RR/SpO2 命名。三个组头下有各自短横线；单列 sCIFAR 不填造一个子指标。SC/sCIFAR 是 accuracy，保留两位小数；BIDMC 是 R²，保留三位小数，含义在 caption 交代。

| 要素 | 实际组织与测量 |
|---|---|
| 表头 | p4/p5 一层加粗；p9 两层，通过局部横线显示三个指标组 |
| 数字列 | p4/p5 右对齐，结果基本保留两位小数；p9 按指标分别使用两位/三位小数 |
| 行距与分组 | p4 普通行文字顶部间距约 10.23 pt，跨大组约 16.53 pt；p5 大多数相邻行约 11.20 pt，进入 Pythia 组约 14.13 pt |
| 字号 | 正文/caption 均约 9.96 pt；p4 表格约 8.86 pt（89%），p5 约 8.40 pt（84%），p9 约 9.58 pt（96%） |
| 注释比例 | caption 均在表上方；p4 三行、p5 四行、p9 三行。解释与来源标记维持正文大小，而非压为极小脚注 |
| 宽度与线条 | p4 表宽约为版心的 89%；p5 99%；p9 81%。仅横线；p5 线条也随表缩小，外横线约 0.47 pt |

截图：[p4](references/scratch_table1_p04.png)、[p5](references/scratch_table2_p05.png)、[p9](references/scratch_table3_p09.png)。

## 可迁移到当前六张实验表的五条做法

1. **用相邻行和局部横线显示直接比较。** E1 同条件下的方法相邻；E2 的方法消融与 matched control 保持清楚行组。避免每行横线，它会削弱组边界。
2. **表头至多两层，只写真实比较维度。** E1 可用自然/受控条件作为组头，下一层放准确率或延迟条件；指标方向与单位放表头。不要为了对齐给单列组添加无意义子标题。
3. **不同问题拆子表或拆表，短表保持自然宽度。** Registers p7 的线性评测/zero-shot 拆分可借给不同估计对象的表；当前 pooled 主结果、scaffold 等权消融和 E6 secondary analysis 的边界应持续可见。少列诊断表无需强行撑满版心。
4. **方法名左对齐，数字按列统一对齐与精度。** 短数字列可居中，带 CI/成本的列适合右对齐或小数点对齐；同一列共享精度。只对可比较列中预先定义的最好值加粗；相等值遵守相同规则。
5. **主数字保持易读，caption 承担边界说明。** 来源、分母、缺失状态、E6 的 secondary 身份和 E5 的 p95 聚合口径放 caption/短表注。优先缩短表头或拆块，再判断是否需要缩小字号。Never Train 的宽表确实缩到 84%–89%，但它是单栏版心，不能据此直接把 AAMAS 双栏缩到同一比例；Registers 的 60% 灰色标准差也只适合作为附加信息，关键 CI 不宜照搬这个比例。

## 复核与可复用资产

`references/source_manifest.json` 保存官方来源、下载 URL、PDF hash、页面清单与访问失败；`references/layout_measurements.json` 保存各区域字符字号、颜色、文字行坐标和横线宽度。字号来自 PDF 字符记录，行距来自字形顶部坐标差，均以 PDF pt 表示；不据此猜测作者的 TeX 宏。

可复用资产已经落盘：两篇原始 PDF、六张完整页面 PNG、官方 HTML/文本和可重跑的 `references/measure_reference_layout.py`。这些文件支持重新检查具体排版，而不需要重新检索或引用二手审美总结。
