# Figure 2 独立审查：v24 prompt / v23 AI

- 日期：2026-10-07。
- 对象：`prompts/figure2/v24.txt` 与 `generated/figure2/v23_ai.png`。
- 结论：**不通过，8/16；必须修复 2 项。**
- 实际查看：`view_image` original，2057×765；完整 prompt 和当前 `main.tex` 的 release/counter 合同已读取。
- 纸面检查：**未测**。本版未提供 7-inch QA，不继承任何历史 16/16。
- 范围：独立审查与本报告；不编辑图像、prompt、main.tex、方法或实验。

## 当前方法与参照

当前 `main.tex` 规定：tick t 的 action 先读取当时合法 state/menu/query；有效 selected feedback 在 `τₜ=t+1+D` 释放后才可进入后续状态。每个 provider 独立保存 n/s；一次有效释放只改变对应 provider 的计数。Runtime error 不进入 chemical-correctness count。D=0 的本事件可在下一次 decision 前释放，不能改变产生它的 tick-t action。

本图对照已通过的 F1 AI v09，以及此前实际查看的 TableDART、CRITIC、CollabLLM 原图。参考来源与 hash 见 [F1 v13 审查](20261007_figure1_gallery_v13_independent_review.md)，F1 最终方法和纸面审查见 [v17 报告](20261007_figure1_gallery_v17_independent_review.md)。本轮没有把参考图的评分当作 F2 的评分。

## 必须修复（2 项）

1. **等待方向和时间珠顺序矛盾。** 主事件从右侧 Pending 流向左侧 Release，但当前左侧珠是 t+1，右侧珠是 τₜ−1。沿事件流会先遇到晚 tick，再遇到早 tick；两珠间虚线还出现右向箭头，形成双向等待。必须只保留一个右→左方向。若保留时间珠，物理上应从靠 Pending 的右侧到靠 Release 的左侧依次为 t+1、…、τₜ−1，然后才在 τₜ release。
2. **选择前菜单已标选中。** 输入 Menu Aₜ 的 B 卡为蓝色，与 Select 输出 B 使用同一选中颜色。该时间图应先呈现中性候选集，再由 Select 输出蓝色 B；将菜单 A/B/C 全部恢复同等灰色。此项是图示时序与 prompt 的一致性修复，不据此断言实际代码泄漏了输出。

v24 prompt 已要求单一左向等待箭头与中性菜单，图像没有执行。Prompt 的时间珠标签只给出了列表，后续若继续采用时间珠应写明右→左的空间顺序。

## 已核对通过的路径

- Query、State Sₜ、Menu Aₜ 均接入 Select，只输出并调用 B。
- Return valid→冻结 Judge→B/r feedback→Pending，没有 terminal 直达更新的捷径。
- Return error→Runtime ledger 后结束，没有进入 Judge、Pending、Release 或 chemical counters；runtime failure 分支符合当前规则。
- Release 后为蓝色实线，依次到 Update B、State Sτₜ 和独立 Later decision≥τₜ；没有回到 tick-t Select。
- Feedback、Pending、Update 与最终 state 保持 B 身份，其他 provider 行没有更新数值；无伪造实验计数或性能结果。

方法/路径 0 分来自主等待路径的实质矛盾，不代表上述独立失败账本或 B 身份检查失败。

## 建议（2 项）

1. 初始 Sₜ 的 B 行保持中性，或明确为已存在历史状态；现图与释放后 B 更新使用完全相同的高亮，容易混同本事件何时影响状态。
2. 减小三处大计数卡与中央空白，让 selected-event timing 成为视觉中心。可用小状态符号表达 Sₜ/Sτₜ，将真正的 B 更新集中在一个计数寄存器；这是表示精简，不改变方法。

## 八项评分

| 项目 | 0/1/2 | 依据 |
|---|---:|---|
| 视觉中心 | 1 | 大输入/状态卡占较多面积，等待与释放未获得足够明确的层次 |
| 模块表达 | 2 | 文档、计数、token、队列、时钟与选择器具有不同形态，接近 F1 的图形语法 |
| 信息密度 | 2 | 主要使用短标签与数学符号，没有解释性段落 |
| 布局 | 1 | 中央 gutter 偏大，三处计数卡重复，尚不够紧凑 |
| 色彩线条 | 2 | 蓝/灰/琥珀/紫角色清楚，线条体系一致；方向错误计入数据路径 |
| 数据路径 | 0 | 等待段有相反方向箭头和逆序 tick，事件路径不成立 |
| 方法一致性 | 0 | 主等待过程的时间依赖矛盾，菜单输入已预先呈现选择 |
| 纸面可读性 | 0（未测） | 没有本版 7-inch 页面证据 |
| 合计 | **8/16** | 实质错误独立阻断通过 |

## 留存与复用

保留完整 v24 与原始 v23，失败记录独立保存。可复用与 F1 一致的颜色、document/token/queue/counter/clock 语法，正确的 Judge 入边、独立 runtime branch、B 身份和面向独立 later decision 的回写。下一轮应简化等待线的几何与中性菜单，再审查实际 AI 输出；纸面检查必须另做。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure2/v24.txt` | `d3bc8f747d5bf01e312ab4d3dfb2d68cbc4c1d44bf635ec7f1a5383ec204e7be` |
| `generated/figure2/v23_ai.png` | `2ad1d71b7091f2191baca3e24016def9dd58c174b09aa6d324855858a4866f4a` |
| 当前 `paper/aamas2027/main.tex` | `266fcdf56d52f27b3eb893c3b91c1046ef1e454152694cc91b6a62f708bf7d48` |

主 session 的 TODO_LOG 由协调者汇总；本审查仅新增本文件。
