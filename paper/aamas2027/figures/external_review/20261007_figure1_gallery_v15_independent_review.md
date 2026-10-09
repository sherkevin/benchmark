# Figure 1 图库风格独立审查：v15 prompt / v07 AI

- 日期：2026-10-07。
- 对象：`prompts/figure1/v15.txt` 与 `generated/figure1/v07_ai.png`。
- 结论：**prompt 通过；生成图不通过，不进入论文。**
- 证据：已读取完整 v15；使用 `view_image` original 实际查看 2060×763 的 v07 原图。三张 gallery 原图已在前轮实际查看，来源与 hash 保存在 [v13 独立审查](20261007_figure1_gallery_v13_independent_review.md)。
- 范围：图稿审查；没有修改正文、实验、方法、prompt 或 AI 像素。v13/v06 失败记录保持不变。

## 方法与 prompt

当前机制为冻结 `q⁰(x)`，每个 provider 独立保存已释放的 `n,s`，以 `(n₀·fit_rate+s)/(n₀+n)−fit_rate` 生成该 provider 残差；逐项加到冻结分数后选一个工具。有效的所选反馈在 `τₜ=t+1+D` 释放后只更新对应 provider。Runtime failure 不产生 chemical label，不进入正确性计数。

v15 将前轮问题明确写成输入、provider 身份和允许的回写连接：查询进入 Features；菜单约束 Select；示意事件 B 只更新 B 行；Update 只有一条入 counters 的边。其机制和修复意图均正确，prompt 通过。生成图仍未完整执行这些要求。

## 必须修复（3 项）

1. **Update 仍直达 Select。** 在 counters 下方，蓝色 Update→counters 的竖线上仍有黑色分叉，横跨主体底部后直接向上进入 Select。其文字“Later decisions (via q=q⁰+Δ)”不能代替 counters→Residual→`+`→q→Select 的计算路径。删除该分叉，仅保留实际计算路径。
2. **菜单仍被接到 Features。** Eligible tools 的连线向上并入 Molecule→Features 的查询入线，图中没有独立菜单约束 Select 的边。应使菜单与 Select 相连，避免把候选工具当作当前分子特征。
3. **Endpoint 仍没有输入边。** Endpoint 文档右侧没有线通向 Features 的查询入线。Molecule 的连接已出现，但性质输入仍孤立；应将 Endpoint 的入边实际接到该查询路径。

这三项涉及图上可见的数据来源与计算连接，属于阻断项。不能用总分通过，也不能靠增添解释性标签掩盖错误连接。

## 已修复与风格建议

B 已一致出现在选择、高亮 score、Chemical feedback、queue 与计数 B 行；该问题已关闭。Return 的 valid/error 标签明确了有效输出判分和失败账本；release 后使用蓝色实线。分子成为平面线图，大型紫色反馈标题被去掉，等待区域缩为局部浅紫容器。

样例的图形语法仍有可用进步：TableDART 的浅色分区与内部合成运算、CRITIC 的短标签闭环、CollabLLM 的事件卡与学习/执行分区均可辨认。建议仅一项：继续统一 provider 卡的小型 outline glyph；当前 B 的填充计算器与 C 的绿色放大镜仍混用图标样式。

## 八项评分

| 项目 | 0/1/2 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 中央方法最大，scorer/state/合成/选择可直接看见 |
| 模块表达 | 2 | 矩阵、寄存器、三格向量、`+`、记录卡和队列具备不同形态 |
| 信息密度 | 2 | 仍以短标签为主，没有大段解释文字 |
| 布局 | 2 | 反馈标题去掉、等待容器局部化；整体紧凑 |
| 色彩线条 | 1 | 角色配色成立；provider 图标样式仍混用，额外反馈分叉增加线条负担 |
| 数据路径 | 0 | 菜单与查询混接、Endpoint 孤立、回写直接进入 Select |
| 方法一致性 | 0 | 仍绕过 residual-score 重算，输入对象关系不符合当前机制 |
| 纸面可读性 | 0（未测） | 未实际查看 v07 的 7-inch 论文页面 |
| 合计 | **9/16** | 数据路径/方法实质错误独立阻断接入 |

此版已有硬错误，因此不为其制作论文页面来补风格分。最终候选仍需独立查看 7-inch 纸面证据，不能继承本表的可读性结论。

## 留存与复用

保留完整 v15 和原始 v07；后续新图应另存，不覆盖此次失败。可复用已修复的 B 身份、valid/error 分支、平面分子与局部等待区，以及矩阵/计数/向量/算子这套图形语法。下一轮优先从空间位置与限定连边消除旁路和输入混接，再复核纸面。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v15.txt` | `06524fb51af909c70d883f4cecc051e45981e7ee4dd0e1a45bc688207d5f8fcc` |
| `generated/figure1/v07_ai.png` | `7f4dfb48eae4fbc7e29f73bba72ea90904c847428f259190af0fd029cb3953dc` |

主 session 的 TODO_LOG 由协调者汇总；本审查仅新增本文件。
