# Figure 1 独立审查：v20 prompt / v12 AI

- 日期：2026-10-07。
- 对象：`prompts/figure1/v20.txt`、`generated/figure1/v12_ai.png` 与 `qa/build_v09/page1.png`。
- 结论：**不通过，10/16；必须修复 1 项。**
- 审查身份：独立图稿审查员；只读审查，不修改图片、prompt、正文、实验或方法。
- 实际查看：原图和 7-inch page1 均通过 `view_image` original 查看；原图尺寸 2060×763，page1 尺寸 1190×1540。

## 方法合同

当前 MIRA-Relay 的有效因果链为：冻结 `q^0_a(x)` 与 provider-local 已释放计数产生 `\Delta_{a,t}`，逐项合成 `q=q^0+\Delta` 并选择一个 provider；有效 selected feedback 在 `\tau_t=t+1+D` 释放后，只更新该 provider 的 `n,s`，再通过 residual 和 score 计算影响后续选择。Runtime error 不生成 chemical label，也不进入正确性计数。

v20 prompt 已明确要求 `Release → Update → B row` 的唯一链，并明确禁止 Release 直接画竖直箭头到寄存器底边。原图没有遵守这一关键约束。

## 实际路径核对

以下路径在 v12 原图中可见：

- Molecule + Endpoint → Features → Frozen scorer → `q^0` → `+`。
- Released counters 的 A/B/C、n/s 寄存器 → 三格 Residual `\Delta` → `+`；`q^0` 和 `\Delta` 合成为三格 `q`，B 高亮，再进入 Select。
- Eligible tools 的 A/B/C 卡片保持中性，通过短垂直边进入 Select；没有菜单到 Features 的错误连接。
- Select → 蓝色 B → Call → Return；valid → Judge → Chemical feedback B、`r\in\{0,1\}`；error → Runtime ledger 并结束。
- Chemical feedback → Selected-only queue → 紫色虚线 Release，反馈身份保持 B。

这些部分与当前方法一致。问题集中在 Release 之后。

## 必须修复（1 项）

1. **Release 绕过 Update，直接进入计数寄存器。** 原图同时画了两条候选写入关系：

   - Update 模块右侧有一条短水平蓝箭头，确实指向计数寄存器 B 行；
   - 但 Release 左侧又引出一条独立蓝线，沿底部向左，到寄存器下方后竖直向上，箭头停在寄存器底边/空白边界，未经过 Update，也未进入 B 行。

   因此图的实际主路径是 `Release → counters` 的旁路，Update 变成未被释放事件驱动的孤立模块；旁路线也不能证明 provider B 的 n/s 被更新。必须删除 Release→counters 的整条底部旁路，使唯一写入链为 `Release → Update → 短水平箭头 → B 行 n/s 单元`。该连接应保持 A/C 行中性，并让后续 `counters → Residual → + → q → Select` 成为唯一影响后续动作的路径。

这是方法/因果实质错误，不能用风格总分抵消；本版不能接入论文。

## 风格与构图

v12 继续吸收了已审样例的具体图形语法：TableDART 的浅色区域和特征/参数模块、CRITIC 的短反馈闭环、CollabLLM 的小记录卡与寄存器。标签重复已删除，菜单中性，q/Δ/q0 和 `q=q^0+\Delta` 清楚，执行区没有新增模块、额外 provider call 或性能结果。颜色角色和线条体系一致。

布局因旁路而变得不清楚：Release 置于底部，Update 置于计数器左侧，但 Release 的长底部蓝线让两者的真实先后关系变成歧义；这也是布局项只给 1 分的原因。

## 建议（最多 3 项）

1. 删除整条 Release→counters 旁路；保留 Update 与 B 行之间的短水平箭头，并让其终点明显落在 B 行的两个 n/s cells 共同位置。
2. 继续保持当前的标签删减；在七英寸尺寸上稍微放大 A/B/C、n/s、`q^0/q/\Delta` 和 Return 字段，不增加解释句。
3. 让 Update 与 B 行垂直对齐、让 Release→Update 的蓝线短而直接，避免再出现可被读成第二条写入路径的长底部线。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 中央 MIRA-Relay 方法区最大，冻结分数、残差合成和选择清晰 |
| 模块表达 | 2 | 特征格、scorer、计数格、向量、`+`、判分器、队列和 Release 具备不同形态 |
| 信息密度 | 2 | 旧版重复标签已删，短标签和一个公式表达机制，无性能数字 |
| 布局 | 1 | 分区紧凑，但底部长旁路线破坏 Release→Update 的空间关系 |
| 色彩线条 | 2 | 蓝/灰/琥珀/紫角色稳定，箭头样式统一；旁路造成额外线条负担 |
| 数据路径 | 0 | Release 直接绕过 Update，并停在寄存器底边，不构成唯一 B-row 写入链 |
| 方法一致性 | 0 | 图示的实际写入路径不是 selected-only 的 `Release→Update→B counters`，无法证明 B-local update |
| 纸面可读性 | 1 | 已查看 7-inch page1；主链和公式可读，无裁切/重叠，但次级行列标签和 Return 字段偏小 |
| 合计 | **10/16** | 数据路径和方法实质错误独立阻断通过 |

## 留存

保留 v12 原图、manifest、v20 prompt 和 page1 纸面证据；不覆盖 v09、v10 或其他历史审查。v12 的标签删减、q/Δ 合成、菜单中性、Judge→feedback→queue 路径可复用；下一版须只修 Release/Update/B-row 唯一链，并重新查看原图和 7-inch 页面。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v20.txt` | `8c7bf2590df3949dc5f9875ee30cc9b6f165725c5e8237bff10ecd8c74473b07` |
| `generated/figure1/v12_ai.png` | `431a45e543ba1bc4e387740f7d5c2501769bd59c64e206ea4027562c22d1b7a8` |
| `generated/figure1/v12_manifest.json` | `0177cdb5e732d46b0ae4c3bc5dc7564a90983a018956d7611c3ae5f9741a760d` |
| `qa/build_v09/page1.png` | `76acbbfae09da5e18cd101b44571de0900f91c31e50cb1827b5b24c7d7ee6b46` |

主 session 的 TODO_LOG 由协调者汇总；本独立审查仅新增本文件。
