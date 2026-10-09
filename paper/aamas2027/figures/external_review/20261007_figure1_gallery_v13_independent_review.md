# Figure 1 图库风格独立审查：v12/v13 prompt 与 v06 AI 输出

- 日期：2026-10-07
- 审查身份：独立审查员；只审查图稿，不修改正文、实验、方法或 prompt。
- 审查对象：`prompts/figure1/v12.txt`、`prompts/figure1/v13.txt` 与 `generated/figure1/v06_ai.png`。
- 状态：**prompt 通过；v06 AI 图像不通过，不进入论文。**
- 纸面审查：未测。该候选已有实质错误，未为其进行论文接入与 7-inch 页面检查。
- 实际查看：三张 gallery 原图及 v06 AI 原图均通过 `view_image` 查看；v06 使用 original detail，2059×764 像素。

## 方法核对依据

当前方法冻结 scorer `q⁰(x)`。每个 provider 独立保存已释放计数 `n,s`，其残差为 `(n₀·fit_rate+s)/(n₀+n)−fit_rate`，当前分数为 `q⁰+residual`，每轮只选择一个工具。

调用终止后，对有效输出生成正确性标签；反馈在 `τₜ=t+1+D` 释放后，才更新对应 provider 的计数。Runtime failure 不生成 chemical label，不进入正确性计数。本审查不提出新方法，不修改 claim。

## 原始样例：实际看到的构图与图形语法

| 原图 | 实际观察 | prompt 中对应的具体安排 |
|---|---|---|
| TableDART，`iclr2026-1635.jpg` | 淡色纵向区域分开输入、编码、路由与输出；编码器、合并算子、路由网络和文档输出具有不同形态；箭头连接具体对象 | 方法占最大浅蓝分区；特征格、冻结权重块、计数寄存器、三格向量与 `+` 分别画出；不是把计算压成一块说明文字 |
| CRITIC，`iclr2024-17.jpg` | Output/Verify/Critiques/Correct 四个短标签围成可直接追踪的闭环；外部工具用图形列举 | 选择、执行、判分、等待、释放与后续选择形成完整反馈路径；工具用小卡片列举 |
| CollabLLM，`icml2025-0242.jpg` | 外部交互与内部学习分层；局部虚线区域包含短卡片和具体内部操作；跨层连接少而明确 | 执行与在线状态分区；反馈用记录卡，等待用堆叠队列；以有限连接表达回写 |

v12/v13 对样例的吸收已落实为区域比例、内部对象形态、合成算子和回路起止安排，超出了“polished/crisp”等形容词。v06 也保留了这些可复用的结构进步；其实际因果接线仍须重画。

## v12 prompt 中间审查

**结论：通过，可进入生成；必须修复项 0。**

冻结 scorer、逐 provider 计数、单次选择、终止后判分、selected-only queue、延迟释放、只更新已选 provider 与独立 runtime branch 均在 prompt 中写明，符合当前方法。

提出三个建议，均已纳入 v13：

1. `Δ` 明确画成 A/B/C 三分量，与 `q⁰` 按 provider 逐项合成，避免共享标量修正的歧义。
2. Selected tool、feedback token、queued event 与被更新行携带同一个 provider 标记；二元标签画类型，不填实验结果。
3. “no row or column grid lines” 限定为全画幅版面网格，保留局部 feature、weight 和 counter cells。

## v13 prompt 复核

**结论：通过。** 已读取完整 `v13.txt` 与 `v13.md`，上述三项具体修订均正确落实。Prompt 通过不构成生成图通过。

## v06 AI 图级审查

**结论：不通过。必须修复以下三项。**

1. **Provider 身份不一致。** Score vector 高亮 B，Selected tool 为 B，队列事件也是 B；Released counters 却高亮 A 的 `n,s` 行。图上没有另一条 A 事件说明。本图指定的同一反馈必须只更新 B 行。
2. **回写出现直达 Select 的分叉。** Update 后的蓝色回折线在寄存器下方分叉，一路进入 counters，另一路标着“Later decisions”直接进入 Select。该分叉跳过 counters→Residual→`+`→score vector 的实际计算；应删除直达 Select 的分叉，回路仅接入 counters，由既有计算路径产生后续动作。
3. **当前查询没有进入特征路径。** Molecule 与 Endpoint 图形都没有输入箭头，唯一跨 Inputs 分区的黑色线从 Eligible tools 出发并进入 Features。应画清 Molecule/Endpoint→Features，并将 Eligible tools 接到 Select 或明确的菜单约束入口，确保 `q⁰(x)` 的当前查询来源可追踪。

以下是建议，不与上述硬错误混为一类：

1. Return 的两条分支加“valid”和“failure”短边标签，明确失败不能进入 Judge/Chemical feedback。现在输出字段和状态字段已分开，但两种条件仍主要靠读者推断。
2. 去掉通宽紫色大背景和较大的“Delayed feedback (selected only)”标题，以紧凑队列、释放门和回折箭头承担闭环组织。
3. 分子改用平面线图，减少球体光泽及工具卡上的装饰性图标，使其与矩阵、寄存器和文档对象保持同一图形语法。

### 八项评分

| 项目 | 分数（0/1/2） | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 方法区最大；内部计数、残差、冻结分数与合成运算清楚呈现，evaluator 没有成为中心 |
| 模块表达 | 2 | 特征格、权重块、计数格、向量、`+`、文档和队列具有不同形态 |
| 信息密度 | 2 | 使用短标签，没有正文式长句；计算可通过对象读取 |
| 布局 | 1 | 主体较紧凑，但下方仍是大紫色分区及标题；输入查询与特征之间的空白连接未解决 |
| 色彩线条 | 1 | 蓝/灰/琥珀/紫的角色清楚；分子球体与装饰图标的质感不统一，反馈线有不必要分叉 |
| 数据路径 | 0 | 当前查询缺少入边；Update 直接进入 Select；反馈与被更新行身份不一致 |
| 方法一致性 | 0 | B 反馈更新 A 行、后续选择绕过分数重算属于实质问题 |
| 纸面可读性 | 0（未测） | 尚未实际查看本版 7-inch 论文页面，不能以放大 PNG 代替 |
| 合计 | **8/16** | 未完成纸面检查；且实质错误独立阻断通过 |

上述数据路径和方法错误不能由风格分数抵消。本版不得用作“已通过”的 Figure 1，也不得以本表证明纸面可读性。

## 可继续复用的图稿资产

保留 v13 完整 prompt 与未经手工像素修改的 v06 原始 AI 输出。下一轮可复用三分量向量、异形计算模块、记录卡、释放时钟和颜色角色；必须修正身份、输入连接与回写路径后，再对新输出逐箭头复核，并实际检查 7-inch 论文页面。

## 文件与 hash

路径均以 `paper/aamas2027/figures/` 为项目相对根；三张 gallery 原图位于 `/Users/jingwu/Documents/Codex/2026-09-27/new-chat/topconf-paper-figure-gallery/`。

| 文件 | SHA-256 |
|---|---|
| `gallery_style_study_20261007.md` | `629e1fe48c1c6e307390ec967d3c5f0d78c3c351eaee282df4b941ec84c6a837` |
| `prompts/figure1/v12.txt` | `9775441b5d89bf7994e265a88573fb3d4671ad9dea4a78b621a268762f3730c9` |
| `prompts/figure1/v13.txt` | `7f5af391c13d1127fea5c044753e48b7791890849a9468439afe23973bc28a1c` |
| `generated/figure1/v06_ai.png` | `d5610f8f23329fcd70a980a1c899cc04c0f183e02a31f83411506a8b84987915` |
| `images/iclr/final/iclr2026-1635.jpg` | `2611bc71a20cd7db05169b3343b540fca59de20e8c8574628bd12bd1e7459ec2` |
| `images/iclr/final/iclr2024-17.jpg` | `2dcc4b7d21e5924d358cdd667201f183db1e08494b5167aa8b3639e0fa3b058f` |
| `images/icml/final/icml2025-0242.jpg` | `3957d410a4a6b4ca6eb7417483a92545b2e0429f1b7283d51576838691272533` |

本审查没有执行实验、生成预测、改写 AI 像素或更改投稿声明。主 session 的交接日志由协调者汇总。
