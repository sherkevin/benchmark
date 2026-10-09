# 顶会论文实验章节结构调研（ICLR / NeurIPS / ICCV）

调研日期：2026-10-06。只使用会议官方奖项/论文页面、官方开放获取版本或作者在 arXiv 的论文 PDF。PDF 页码均为 PDF 页码；arXiv 版本与会议版本的页码可能因排版略有差异。

## 1. 一手来源与证据

| 来源 URL | 论文（准确标题） | 年份 / 身份 | 来源明确展示的结构或做法 | 证据定位 | 置信度 |
|---|---|---|---|---|---|
| https://blog.iclr.cc/2024/05/06/iclr-2024-outstanding-paper-awards/ | Never Train from Scratch: Fair Comparison of Long-Sequence Models Requires Data-Driven Priors | ICLR 2024 Outstanding Paper（官方奖项页列为 winner） | 奖项委员会称其“exceptionally well executed”，强调简单性和系统性；论文正文将 setup 压缩为第 2 节，先用 Table 1/2 给出横向主结果，再按设计问题拆分 3.1–3.7；超参数和额外结果放 Appendix。 | 官方奖项页对该文的评语；论文 PDF `https://arxiv.org/pdf/2310.02980`，第 3–5、7–9 页 | 高 |
| https://arxiv.org/pdf/2310.02980 | Never Train from Scratch: Fair Comparison of Long-Sequence Models Requires Data-Driven Priors | 2024（ICLR 2024） | `2 Experimental Setup` 只定义 LRA、任务和 SPT；`3 Results` 开头直接列 3.1–3.7 的问题分解。Table 1/2 是“方法 × 任务”的紧凑矩阵；每个结果段落遵循设置→表/图→主要差异→解释。 | 第 3 页（Setup 与矩阵说明）；第 4 页 Table 1、3.1；第 5 页 Table 2、3.2；第 6–9 页 3.3–3.7；第 9 页 Table 3 | 高 |
| https://blog.iclr.cc/2024/05/06/iclr-2024-outstanding-paper-awards/ | Vision Transformers Need Registers | ICLR 2024 Outstanding Paper（官方奖项页列为 winner） | 奖项委员会称其“identifying an issue, understanding why it is happening, and then providing a solution”。正文不放行政式实验清单，而按“训练算法/数据→下游评价→对象发现→定性分析”组织；主表只保留模型、任务和指标。 | 官方奖项页评语；论文 PDF `https://arxiv.org/pdf/2309.16588`，第 6 页 §3.1–3.2，第 7 页 Table 2，第 8 页 Table 3 | 高 |
| https://arxiv.org/pdf/2309.16588 | Vision Transformers Need Registers | 2024（ICLR 2024） | §3.1 用三段定义 DeiT-III/OpenCLIP/DINOv2 与训练数据，§3.2 立刻给出下游评价。Table 2 将 ImageNet/ADE20k/NYUd/零样本结果合在一个小表，Table 3 将 LOST 三个数据集合在一个小表；文字只解释趋势和异常。 | 第 6 页 §3.1–3.2；第 7 页 Table 2 与 register 数量图；第 8 页 Table 3 及一段结果解释 | 高 |
| https://media.neurips.cc/Conferences/NeurIPS2024/NeurIPS2024_Best_Paper_Awards.pdf | Stochastic Taylor Derivative Estimator: Efficient amortization for arbitrary differential operators | NeurIPS 2024 Best Main Track Paper（官方奖项公告） | 官方公告明确把“experimental validation and insights”作为获奖理由之一。正文以一个实验总述段承接方法，直接说明主结果留在正文、完整细节放附录；速度/显存只用两张窄表呈现，再用一段解释基线和尺度效应。 | 官方奖项公告第 2 页附近 STDE 条目；论文 PDF `https://arxiv.org/pdf/2412.00088` 第 8–10 页 | 高 |
| https://arxiv.org/pdf/2412.00088 | Stochastic Taylor Derivative Estimator: Efficient amortization for arbitrary differential operators | 2024（NeurIPS 2024） | §5 开头仅用一段交代 PDE 类型、对比对象和“主结果在正文/细节在 Appendix H/I”。§5.2 用一个问题驱动的 ablation；Table 1/2 只呈现 Speed/Memory × 输入维度 × 方法。结果段落报告相对基线的数量级差异，再解释为什么小维度和大维度结论不同。 | 第 8 页 §5、§5.1；第 9 页 §5.2、Table 1/2；第 10 页 STDE 总结 | 高 |
| https://openaccess.thecvf.com/content/ICCV2023/papers/Al_Kader_Hammoud_Rapid_Adaptation_in_Online_Continual_Learning_Are_We_Evaluating_It_ICCV_2023_paper.pdf | Rapid Adaptation in Online Continual Learning: Are We Evaluating It Right? | ICCV 2023 主会开放获取论文（未在本报告中宣称为 Best Paper） | 先用 Table 1 对比 benchmark 的属性，再把正文实验集中在两个设置（slow/fast stream）。§4.1 一次性给 dataset、metric、model/optimizer、方法；Figure 6 承载主结果。每个结果段落只回答一个比较问题，并给出结论。 | 第 2 页 Table 1；第 6 页 Figure 5 与 §3.5；第 7 页 §4.1；第 8 页 Figure 6 与 fast-stream 结论 | 高 |
| https://openaccess.thecvf.com/ICCV2023 | ICCV 2023 Open Access Repository | 2023（CVF 官方论文库） | 该页面声明论文是与接收版本一致的开放获取版本；上行 ICCV 论文 PDF 来自该官方库。 | Repository 首页说明段 | 高 |

## 2. 来源明确展示的共同模式

1. **正文没有“项目管理式矩阵”。** 论文通常不把 `status / hash / out of scope / verification` 放进主文表格。主文表格是方法×任务、方法×指标或 benchmark 属性×方法的数值/勾选矩阵。可复现细节在附录、代码或补充材料。
2. **实验设置只回答复现所需的最小问题。** 数据/任务、比较方法、指标、训练/推理条件通常在一个短 subsection 或一段话中交代；随机种子、超参数、硬件和完整运行记录移到 appendix。
3. **矩阵按科学问题拆，而不是按实现清单拆。** Never Train from Scratch 的 `3.1–3.7`、STDE 的 `5.1–5.2`、ICCV OCL 的 slow/fast stream，都是“一个问题对应一个结果块”。
4. **新增结果的段落结构很稳定：** (a) 说明比较设置或研究问题；(b) 指向一张表/图；(c) 报告最重要的绝对值或差值；(d) 解释机制、边界或异常；(e) 必要时说明下一节为什么存在。
5. **主文只保留能改变结论的消融。** STDE 只在正文保留速度/显存以及性能来源的关键消融；VIT Registers 只保留三个训练来源、下游任务和对象发现。额外数据集、超参数、完整 per-task 表在附录。
6. **一张表承担一个阅读动作。** Table 1/2（Never Train）、Table 1/2（STDE）、Table 2/3（Registers）都能在几秒内看懂：行是方法，列是任务/指标，caption 解释符号和比较协议，表格单元不承载长句。

## 3. 对当前论文的直接推论（这是调研后的设计建议，不是来源原话）

当前 `main.tex` 的 `Pre-registered matrix` 更像审计/项目台账：五列中包含长句、`VERIFIED/OUT OF SCOPE`、证据边界和历史控制关系。这样的信息对复现很有价值，但放在正文会打断“问题→设置→结果”的阅读路径。建议：

- **正文保留一张四行以内的 compact settings table**，列为 `Study / Data and stream / Compared methods / Metric`。示例行：`Natural / BBB, 157 scaffold-disjoint molecules, 4 fresh providers, D∈{0,1,4} / Frozen, Online-SGD, LinUCB, BayesianRouter, MIRA / selected correctness, paired scaffold CI`；`Drift / same menu, fit-best selected label flip after midpoint / same methods / recovery correctness and rescued/harmed`。
- **把当前完整预注册矩阵移到 appendix/supplement**，保留 E1–E6、状态、hash、验证脚本和 out-of-scope 边界。正文只用一句话说明：“完整预注册清单和证据索引见 Appendix X”。
- **Baseline 表压成“比较角色→方法”两列**。正文只保留 Random、Retrospective Best Fixed（明确 evaluator-only）、Frozen、Online-SGD、Greedy LinUCB、BayesianRouter、MIRA；PILOT/GraphRouter/Replay 等状态与适配原因移到 appendix 的 baseline contract。
- **正文结果只保留两张主表/一张成本表**：自然 fresh confirmation、controlled drift stress、runtime/update cost。不要再放没有结果的 VOI 表；未来扩展用一行 scope note 和附录表格即可。
- **每个结果块改成五句以内**：`Question → Setting → Table/Figure → headline number → interpretation/boundary`。例如：“在自然 fresh menu 上，MIRA-Context 与 Frozen 均为 89.81%，配对 scaffold CI 为 …；这说明当前 panel 的静态头部已经足够强，不能把动态动作变化写成自然收益。受控 midpoint flip 下，MIRA-Context 比 Frozen 高 …，说明 released selected-only residual 能在声明的 drift 下恢复；matched controls 的结果见 Table …。”
- **正文不写哈希、逐事件数量、验证脚本路径和完整失败分类。** 这些属于 artifact/reproducibility appendix；正文仅保留样本数、split、delay、seed、主要指标和统计区间。
- **避免“把 8 页填满”式文字。** 顶会正文通常用更紧凑的主表、图和短结果段落提高信息密度；节省出的空间应给方法直觉、假设和两张主结果表，而不是把台账文字留在正文。

## 4. 证据与推论的边界

- “这些论文如何排版和组织实验”是对上述 PDF 的直接观察。
- “当前矩阵应移附录、正文只留 compact settings table”是基于这些观察对本项目的推论；不是任何会议的硬性格式规定。
- ICCV 示例在本报告中只用于实验组织结构，不作为 ICCV Best Paper 的身份样本；ICLR/NeurIPS 的 award 身份由对应官方奖项页/公告确认。
