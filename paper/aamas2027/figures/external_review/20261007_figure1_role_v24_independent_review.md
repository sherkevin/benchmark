# Figure 1 角色分工独立审查：v24 prompt / v16 AI

- 日期：2026-10-07。
- 对象：`figures/prompts/figure1/v24.txt`、`figures/generated/figure1/v16_ai.png` 与 `figures/qa/build_v13/page-1.png`。
- 结论：**provider 字母清理和主体架构通过；Update→released-state-row 的终点错误使本版不通过，10/16。**
- 硬错误：**1 个数据路径硬错误。** 唯一蓝色写入线从 Update 折向上，但箭头头停在 released-state register 的外框底边，没有接入高亮行的计数单元。
- 当前状态：不接入正式论文；需修正 Update 的空间位置和箭头终点后重新审查。
- 审查身份：独立图稿审查员；只读审查，不修改图片、prompt、正文、实验或方法。
- 实际查看：v16 原图与 build_v13/page-1 均通过 `view_image` original 查看；原图尺寸 2060×763，纸面页尺寸 1540×1540。

## 角色契约与文字清洁

v16 是 Figure 1 架构图而不是 Figure 2 的单事件时间线。它可以展开 Features、Frozen scorer、released state、residual、plus、score、Selector 和 selected tool；Figure 2 才承担单次 terminal→judge→pending→release 的时间带。v16 保留这一区分：中心是分数与状态计算，底部是反馈回写闭环，没有复制 Figure 2 的横向事件脊或时间线卡片布局。

- 原图和 page-1 中没有独立 provider 字母 `A`、`B`、`C`。三行状态/向量用一个高亮中行表达 selected provider；没有把字母复制到 menu、selected tool、feedback 或 queue。
- 没有旧的 family residual/provider bias、额外 provider call、trainable backbone、性能数字或虚构标签。
- 文字集中在自然短标签：`Molecule + Endpoint`、`Features`、`Frozen scorer`、`base score`、`released state`、`residual`、`score`、`Selector`、`Selected tool`、`Call`、`Return`、`Judge`、`Chemical feedback`、`Runtime ledger`、`selected-only queue`、`Release`、`Update`；公式只有 `q=q^0+Δ` 和 `τ_t=t+1+D`。

## 实际架构和箭头检查

### 已通过的主体架构

1. `Molecule + Endpoint → Features → Frozen scorer → base score q^0` 的上游链清楚；Features 和 scorer 内部用网格/权重块表达，且 q⁰ 是三格向量。
2. `released state (n,s) → residual Δ → plus` 清楚；高亮中行同时反映在状态 register 和 residual 向量。
3. `q^0 + Δ → score → Selector → Selected tool → Call → Return` 由短箭头连接；Selector 和 Selected tool 是分开的节点。
4. Return 的 `valid` 边进入冻结 Judge，再进入琥珀 Chemical feedback `r∈{0,1}`；`error` 虚线只进入 Runtime ledger 并终止，未进入 chemical feedback。
5. Chemical feedback 通过紫色竖边进入 selected-only queue，queue 通过一次紫色虚线进入 Release（`τ_t=t+1+D`），Release 通过短蓝边进入 Update。没有 Release→register 的第二条直连，也没有 queue 绕过 Release/Update 的边。

### 数据路径硬错误：Update 没有接到高亮 released-state row

v24 的空间合同要求：`queue → Release → Update → highlighted released-state row`，并明确禁止 U-shaped detour、Update→register bottom-edge arrow，且每个箭头头必须落在具体节点/高亮行。实际 v16 中：

- Update 位于 released-state register 的右下方，不能直接水平进入 register 左侧的高亮中行；
- 蓝线先从 Update 向左，再向上折转；
- 向上的箭头头停在 released-state 卡片的**底部外框**，没有进入中间高亮行的 `n`/`s` 单元，也没有落在具体 cell 内。

因此可以确认“没有旁路”，但不能确认“Update 写入了 selected provider 的 released-state row”。这不是单纯的审美折线：箭头没有连接到合法目标状态，读者无法从图中判断该更新只改变高亮行。按图库规范“箭头必须 dock 到具体节点/行”和正文 selected-only provider-local 更新契约，该终点错误属于数据路径硬错误，不能用其他分数抵消。

### 菜单输入的次级观察

左侧 `Eligible tools` 图标组保持中性且无 provider 字母，但图中没有一条明确的 Eligible tools→Selector 箭头；它看起来像输入侧的候选集合而不是已接入的 mask。v24 把该组定义为 Inputs 区的 input-only 视觉，因此我不把它升级为本轮硬错误；若投稿版需要直接宣示 eligible-menu 约束，下一版可增加一条不穿越主图的短中性边，并保持不出现 A/B/C。

## 图库风格与纸面观察

v16 采用浅蓝 Inputs/MIRA-Relay、浅黄 Execution 和淡紫等待区；网格、向量、算子、锁/齿轮/文档/时钟/事件卡保持 TableDART、CRITIC、CollabLLM 的具体图形语法。中心方法区最大，文本短，没有段落框或全画布网格。纸面 build_v13/page-1 中主模块和公式可读，无裁切/重叠；`n/s`、`q^0`、`Δ`、valid/error 和 `τ_t` 等次级字段偏小，且错误的 Update 终点在纸面上更容易被误读成写入寄存器外框。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 最大区域是 MIRA-Relay 的冻结分数、残差合成和 Selector，执行区没有抢中心 |
| 模块表达 | 2 | Features/weight grids、q⁰/Δ/score vectors、released-state register、plus、cards、Judge 和 queue 均有不同形态 |
| 信息密度 | 2 | 无 provider 字母串、无重复架构标题和性能数字；节点均为短标签 |
| 布局 | 1 | 三分区紧凑，但底部 Update 与 register 不在同一行，U 形回写边破坏了直接写入读法 |
| 色彩线条 | 1 | 蓝/灰/琥珀/紫角色总体正确，但关键 Update 箭头以折线落在 register 外框，未建立具体 row 的线语义 |
| 数据路径 | 0 | **硬错误：** Update→released-state 线终止于 register 外框底边，而非高亮行 `n/s` cells；唯一合法写入链未完成 |
| 方法一致性 | 1 | selected-only queue、释放时钟、valid/error 分支和 provider-local 高亮意图存在，但实际箭头未证明只更新该行 |
| 纸面可读性 | 1 | 已查看 7-inch page-1；主模块可读，次级字段偏小，错误终点在缩小后更难辨认 |
| 合计 | **10/16** | **因数据路径硬错误不通过；总分不能抵消该错误** |

## 硬错误与必须修改

**硬错误（1）：** Update→released-state 的蓝色箭头只落在 register 外框底边，没有进入高亮行。该图不能作为正式方法图使用，尽管其它链路和文字清洁度通过。

**必须修改：** 将 Update 放在 released-state register 的左侧并与高亮中行垂直对齐（或采用等价的相邻几何），删除当前向左再上行的 U 形线；画一条短、实线、蓝色水平箭头，箭头头明确进入高亮行的 `n/s` 单元中心。保持唯一链 `selected-only queue → Release(τ_t) → Update → highlighted row`，不要新增 Release→register 外框/底边旁路或第二条更新边。

**建议：**

1. 保持当前零 A/B/C 的文字策略和一个高亮行；修好终点后可略增大 7-inch 页面中的 `n/s`、`q^0`、`Δ`、valid/error 和 `τ_t` 字号，不增加解释句。
2. 若需强调候选 menu 约束，增加一条不穿越架构的中性 Eligible tools→Selector 边；不要把 menu 图标复制到 Figure 2。

本报告基于实际 v16 原图和 build_v13/page-1，不以放大 PNG 替代纸面检查，也不宣称用户已最终选定或该图已满足投稿接入条件。

## 文件与 hash

下列路径以 `paper/aamas2027/figures/` 为根。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v24.txt` | `0ac6c859f73285a743ee90375e298316a682bc8da609c7b575e1d06a588e1262` |
| `generated/figure1/v16_ai.png` | `1bcaede0754ea5d9d853feda866096b75352ade34a541857c26bd66b7aa99e35` |
| `generated/figure1/v16_manifest.json` | `8866e78b9105ff91debd4f4e853013ea73308e4230c8c050f28fa4d6be126f1a` |
| `qa/build_v13/page-1.png` | `1b0edfce9f5b2d0f7f5ab6033f292982c899e3e65f1126e07565b6b525d7bdea` |

主 session 的 TODO_LOG 由协调者汇总；本独立审查仅新增本文件。
