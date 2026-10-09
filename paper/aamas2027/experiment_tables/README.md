# 正文实验矩阵与结果表排版稿

版本：v05，2026-10-08。两页审阅稿，六张结果表和一张延迟分层数据图。
本目录的 main.pdf 是表格预览；全文唯一主版是上一层 paper/aamas2027/main.pdf。
每次迭代的源码、PDF、渲染和检查保存在 versions/，旧版不覆盖。

另有独立的[预期目标 Guide](guide/README.md)与[带水印 PDF](guide/guide.pdf)。
其中蓝色 T 是规划目标，M 是已有实测参照；Guide 不参与真实正文的数值导出或绑定。

## 六组实验与正文的对应关系

| 实验 | 要回答的问题 | 预览位置 | 全文稳定引用键 |
|---|---|---|---|
| E1 主比较与延迟 | 自然菜单是否稳定，受控变化后能否恢复，延迟是否改变收益？ | 表 1；图 1 两个 panel | tab:main-results / tab:mechanism-results，fig:delay-sensitivity |
| E2 机制消融 | 写回正确源 provider 是否是收益来源？ | 表 2，Local / Frozen / permutation 的配对差值 | tab:credit-ablation |
| E3 冷启动与选择空间 | 可选择的空间有多大，离线训练目标能否找出它？ | 表 3，静态方法与 evaluator oracle | tab:learned-prior |
| E4 更新规则诊断 | 分数校正与 residual 中心化各带来什么？ | 表 4，2×3 因子矩阵 | tab:centering-diagnostic |
| E5 效率 | 同一调用预算下，选择与更新的开销是多少？ | 表 5，selection / update 时间 | tab:cost-validity |
| E6 调用后验证 | 相同接受比例下，置信度拒答能否减少错误？ | 表 6，coverage / accepted accuracy / utility | tab:postcall，别名 tab:postcall-verification |

E1/E2/E5 是核心证据；E3/E4 是开发诊断；E6 是既有留出集的次级分析。
E2/E5 将正文已有结果段落升级为表。E6 已在当前全文有数值表，属于重排。
这是展示调整，不要求为了填表再运行六轮实验。全文表号由 LaTeX 生成，不等同于预览表号。

## 本轮实际参考了什么

详见 [原始论文排版报告](reference_style_report.md) 和 [v02 独立视觉审查](reviews/v02_visual_review_20261008.md)。

| 原始论文 | 实际查看位置 | 用在本稿的做法 |
|---|---|---|
| VAR，NeurIPS 2024 Best Paper | PDF p7，Table 1/2 | 方法族分组；宽主表与自然宽度小表各有职责 |
| Vision Transformers Need Registers，ICLR 2024 Outstanding | PDF p7/p8/p17 | 配对比较相邻；短表不强行拉宽；不同任务拆 panel |
| Never Train from Scratch，ICLR 2024 Outstanding | PDF p4/p5/p9 | 数字列统一精度与对齐；真实共同指标才设两级表头 |
| Segment Anything，ICCV 2023 正式论文 | PDF p8，Figure 7 | 子图承担不同阅读任务，共用标签与单位 |

两篇 ICLR 论文的获奖身份由官方名单核验，PDF、完整页截图、字号和行距测量均在 references/。
此前 gap report 将 DoLa 称作 Outstanding 有误，已保留原文件并作明确更正。
这些例子用于解决读者如何比较的问题，不把每个装饰细节都当成 AAMAS 的规定。

## 重排后的具体规则

- 表体统一 9/11 pt，表注 8.5/10 pt；不使用 resizebox。
- 主表用方法族横向分组和两级表头。粗体只标可部署方法的最好点估计（含并列），不代表显著性。
- 先按内容的自然宽度排表，不用 extrafill 将数值散到版心两端。
- caption 给出比较对象；共同设置在 setup，解释与完整统计在对应正文和来源报告。
- 两张开发诊断并排且顶边对齐；主结果、开发诊断、次级分析保持不同位置。
- 从成本表移出恒定的 Calls 和整列 TBD；从拒答表移出重复的 W/R 计数。完整证据仍在 v02 与来源报告。
- 延迟分层改为两个数据 panel：自然菜单和受控变化。共享图例与单位，MIRA-Relay 用蓝色，其余方法用低饱和色及不同线型。
- 数据图用 PGFPlots，由已有结果汇总生成；不是 AI 生图，不插入虚构数据或置信区间。

## 统计口径与保留的详细信息

- 157 是独立留出分子，124 是 scaffold clusters；每方法 30 schedules / 4,710 次记录调用不是 4,710 个独立分子。
- E1 点估计按 pooled calls；E2 按 scaffold 等权。机制的两个 replica 是确定性重复，不称随机种子稳健性。
- E1 配对区间均以 Frozen 为参照。MIRA-Relay 自然收益 0.00 pp，95% CI [-1.24,+1.40]；null 结果保留在图前正文。
- 其余自然菜单差值区间保留在 v02 表 1：Random -6.05 [-9.93,-2.26]，Best Fixed -1.66 [-3.97,+0.51]，MIRA-Relative 0.00 [0.00,0.00]，SGD -0.59 [-1.80,+0.52]，LinUCB -0.08 [-0.58,+0.41]，BayesianRouter -0.28 [-1.75,+0.97]，retrospective fixed +0.64 [-2.86,+4.07]。单位 pp。
- Retrospective Best Fixed 是 evaluator-only fixed provider；E3 的 per-molecule oracle 才使用所有工具输出。
- 延迟各点复用同一 157 分子，每方法每 D 有 1,570 calls，描述性读数没有分层 CI；折线只连接观测点。
- E3/E4 使用 722 development molecules，不与留出集拼总 N。两个训练种子分开列值；确定性方法不重复相同值。
- E5 是 30 个 stream-level p95 的均值，设备 macOS arm64，不是 pooled p95 或 A800 实测；排除 provider inference / I/O。
- Live learner memory 尚未测量，仍是待填指标。含完整历史的序列化状态不能冒充 live memory；有真实结果后再加列。
- E6 复用留出集，是 secondary analysis，五个 fit-only policies 产生每策略 785 records；random 是 evaluator-only conditional null。
- E6 原始计数仍在 v02 与 corrected report：always accept W=93/R=0，confidence refuse W=36/R=122，always refuse W=0/R=785，random W=75/R=122；各 C=N=785。
- E6 utility 是 $U=-(W+0.25R+0.01C)/N$；accepted accuracy 对 random 的区间 [+2.77,+9.29] pp，对 always accept 为 [+3.34,+9.72] pp，不可混用。

## 复用与版本

export_blocks.py 从唯一预览源码自动导出 body_tables.tex 和 body_delay_figure.tex，不手工维护第二份数字。
正式主稿使用内联的同源表体，使现有 build manifest 能绑定实际 LaTeX。E6 仍由证据绑定脚本检查。

- v01/v02：原三页稿与完整详细列，保留全部文件。
- v03：六表减法和两个 delay panel，保留两轮编译与孤立第三页，不提升。
- v04：短 caption、自然宽度居中、两页布局；独立视觉审查通过，要求补回效用变量定义。
- v05：补回 W/R/C/N，并强调自然图零基线；数值和实验范围不变。

每版的编译、PDF hash、页面图片和复核结果在 versions/*/manifest.json / qa.md。
前序参考文献补全仍是独立待办，不能把表格重排当作引用任务完成。

## 已同步到全文主版

本版六表和延迟图已合入 `paper/aamas2027/main.tex` 并重建提升唯一主版；正文结果表为 Tables 3–8，延迟图为 Figure 3。正文 8 页，References 第 9 页。E5 收紧列距以适配单栏，字体与数值不变；E6 增加每行 785 records/calls 的明确说明。最终合入和页数证据见 `reviews/main_integration_20261008.md` 与 `reviews/formal_tables_final_closure_20261008.md`。
