# TopConf figure gallery round-2 audit — architecture redesign

> Historical audit. Its earlier “final must be editable” recommendation is superseded by [ADR 0010](../../../docs/决议/0010-AI生成图作为正式视觉资产.md); the gallery observations remain valid, while the production rule now keeps the AI image as the formal visual asset.

日期：2026-10-05  
目的：根据用户对 v04 草图的反馈，重新检查本地 TopConf Figure Gallery 中的 framework / pipeline / architecture 图，提炼可迁移到本项目的视觉结构。

## 实际查看的样例

画廊路径：`/Users/jingwu/Documents/Codex/2026-09-27/new-chat/topconf-paper-figure-gallery`。

本轮实际查看的图稿：

- `iclr2024-17` — CRITIC：输入、黑箱模型、输出/批评/验证闭环，外部工具单独成组。
- `iclr2024-0206` — CRAFT：问题/旧方案与新方案并列，工具集、检索和生成模块用少量框与箭头表达。
- `neurips2023-0525` — GPT4Tools：输入内容、工具口袋、模型和用户交互，锁图标表达冻结组件。
- `aaai2025-32038` — GenAL：全局学习层与局部执行层分区，反馈箭头连接两个层级。
- `icml2025-0242` — CollabLLM：横向 pipeline，用户、模拟、奖励计算、策略更新各为短标签模块。
- `acl2025-2025.acl-long.973` — DARS：输入、阶段模块、反馈与评估分区，重复模块用相同几何表示。
- `iclr2026-1635` — TableDART：输入、编码、gating 和多条推理路径，分区颜色服务于结构而不是装饰。
- `iclr2025-0100` — AgentOccam：agent / observation / environment 三块边界清楚，状态接口用短标签和箭头表达。
- `icml2025-1369` — ShieldAgent：多级模块和反馈环，但每个框只承担一个动作或状态。

## 从样例中抽出的硬规则

1. **图先说结构，正文再说解释。** 框内只放模块名、变量名和极短的状态词；完整句子放 caption 或正文。
2. **每个框只承担一个动作或状态。** 例如 `Selector`、`Tool call`、`Frozen judge`、`Pending queue`、`Relay update`，不把多个定义塞进一个文本框。
3. **创新点用几何关系表达。** 本项目的 family residual + provider bias 应画成共享 family 连接加 provider-specific 小偏置，而不是用一段文字解释“共享但保留差异”。
4. **冻结与可更新组件要一眼分开。** 灰色锁定的 fit-only reference 与蓝色 MIRA-Relay 并列，蓝色更新箭头不能回到灰色参考。
5. **候选 provider 用卡片表示。** 只画 2–3 张抽象卡片；选中的卡片高亮，未选卡片灰化，不画大叉、不画未选输出值。
6. **反馈是一条独立的可见路径。** 选中结果 → terminal record / frozen judge → pending queue → released state → later action，不能让 correctness 直接回到当前 action。
7. **颜色只表达角色。** 深蓝=方法和状态，灰=冻结/旧状态，琥珀=终态/judge，紫=pending/delay；不按 lane 随意换颜色。
8. **一张图只保留一个主阅读方向。** Figure 1 用左到右的系统架构；Figure 2 用左到右的时间泳道。不要再用竖向编号清单替代架构结构。
9. **少量图标可以帮助识别模块，不能成为装饰。** 只保留 molecule、provider card、lock、judge document、queue/clock、state node 等功能图标。
10. **历史建议：最终必须可编辑。** This production recommendation is superseded by ADR 0010; now preserve the raw AI image and correct content through prompt iteration.

## 对当前两张图的直接改法

- Figure 1：改成“输入状态 → 双分支 selector → 一个 selected provider → 一次 tool call → terminal/judge → delayed relay update”的模块图，并用 family bracket + provider bias chips 显示核心机制。
- Figure 2：改成三条水平 lane、五个时间列；用灰色 state rail 表示 pending 期间决策继续，用 queue/release 列表示 selected-only delayed update。
- 删除大段说明、编号步骤、巨大叉号和不必要的图标；保留 selected-only、runtime failure / chemical correctness 分离、`$\tau_t=t+1+D$` 和 fallback ledger-only 四个关键合同。

## 本轮结果

- 新提示词：`prompts/figure1/v05.md/.txt`、`prompts/figure2/v05.md/.txt`。
- v04 保留为历史版本，未覆盖。
- 新提示词还没有生成新的图片；生成后的每一版仍需按 `FIGURE_REVIEW.md` 复核，并最终重绘为可编辑矢量图。
