# Figure 1 独立审查：v18 prompt / v10 AI

- 日期：2026-10-07。
- 对象：完整 `prompts/figure1/v18.txt`、`generated/figure1/v10_ai.png` 与 build_v08 阅读页。
- 结论：**不通过，11/16；必须修复 1 项。**
- 审查身份：独立图稿审查员；只读审查，不修改图像、prompt、正文、实验或方法。
- 实际查看：v10 原图通过 `view_image` original 查看，尺寸 2060×763。`build_v08` 没有保存 `page1.png`；因此从 `ai_structures_fullwidth_20261007_v08.pdf` 第 1 页渲染到临时 PNG 后再通过 `view_image` original 检查，作为本版七英寸纸面证据。`page2.png`、`page3.png` 是该 build 中的其他页，不冒充 Figure 1 page1。

## 当前方法核对

当前 MIRA-Relay 使用冻结 `q^0_a(x)`，每个具体 provider 有独立已释放计数 `n_{a,t},s_{a,t}`，残差为

\[
\Delta_{a,t}=\frac{n_0\bar r_a+s_{a,t}}{n_0+n_{a,t}}-\bar r_a,
\qquad q_{a,t}(x)=q^0_a(x)+\Delta_{a,t}.
\]

每轮只选择一个 provider。有效 selected outcome 在 `\tau_t=t+1+D` 释放后，才更新对应 provider 的计数；runtime error 不生成 chemical label，也不进入正确性计数。图中的 B 事件因此必须落到 Released counters 的 B 行，随后才经 `Residual → + → q → Select` 影响后续动作。

## Prompt 与原图对照

v18 prompt 对旧版的修订方向正确：删除 Frozen scorer 和输入的重复标签，要求增大次级字号，并明确“Update sends a solid blue edge into B's Released counters row”。v10 实际实现了大部分文字删减和颜色/标签约束，但没有完成这条方法性连接。

### 已实际追踪并通过的路径

- Molecule + Endpoint → Features → Frozen scorer → 三格 `q^0` → `+`。
- Released counters 的 A/B/C 行和 n/s 列 → 三格 `\Delta`（B 行高亮）→ `+`；`q=q^0+\Delta` 后生成三格 `q`，B 高亮并进入 Select。
- Eligible tools 的 A/B/C 卡片均为中性，菜单通过短垂直边进入 Select；没有菜单到 Features/scorer 的错误边。
- Select 输出一个蓝色 B → Call → Return；Return 的 valid 边折向 Judge，Judge 向下生成 Chemical feedback B、`r\in\{0,1\}`。
- Return 的 error 边进入独立 Runtime ledger 并结束，没有进入 Judge、queue 或 chemical counter。
- Chemical feedback → Selected-only queue → 紫色虚线 Release（`\tau_t=t+1+D`）→ 蓝色 Update；队列和 provider B 身份保持一致。

### 必须修复（1 项）

1. **Update 没有接到 B 行。** Update 下方的蓝色边向左后再向上，箭头终止在 Released counters 寄存器的底边，位于 C 行以下的空白边界；它没有落在 B 行的 n/s 单元或 B 行中心。这样图无法证明释放的 B 反馈只更新 B provider，也违反 v18 的“every arrow docks on its real node / B's row”要求。应把终点直接放到 B 行（最好接到 B 行两个 n/s cells 的共同位置），保持 A/C 行中性，并继续让计数经 `Residual → + → q → Select` 影响后续动作。

这是因果和 provider-local 更新的实质问题，不能用视觉总分抵消。本版不接入论文。

## 风格与构图观察

TableDART 的浅蓝中心分区、具体的 feature/weight/register/vector/plus 内部形态已吸收；CRITIC 的短标签闭环体现为 feedback→queue→release→update；CollabLLM 的记录卡和状态寄存器语法也可辨认。v10 没有新增 family residual、训练 backbone、额外调用或性能结果，整体不再是说明文字堆叠。

v18 的标签删减已改善：Frozen scorer 只有一处文字，输入只有一行 “Molecule + Endpoint”。方法区占主要宽度，Execution 没有夺走视觉中心。蓝/灰/琥珀/紫的角色稳定，箭头体系统一，菜单保持中性。

## 建议（不超过 3 项）

1. 将 Update→counter 的终点精确画在 B 行中心或 B 行 n/s 单元之间，避免再次停在寄存器外框。
2. 继续保持寄存器宽度和布局，在不增加说明句的情况下略微放大 A/B/C、n/s、`q^0/q/\Delta` 以及 Return 小字段。
3. 若需进一步减密度，可缩短 “Eligible tools” 小标题或把它作为菜单标签处理；不要删掉 provider 行、计数列或 `+` 算子。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | MIRA-Relay 中央浅蓝方法区最大，冻结分数、残差合成和选择为主体 |
| 模块表达 | 2 | 特征格、冻结 scorer、计数格、残差/分数向量、`+`、文档、判分、队列和释放门形态不同 |
| 信息密度 | 2 | 重复标签已删，使用短标签和一个公式，没有解释性段落或性能数据 |
| 布局 | 2 | 2.7:1 通栏结构紧凑，输入/方法/执行/反馈分区均衡，无大标题带或网格 |
| 色彩线条 | 2 | 在线蓝、冻结灰、判分琥珀、等待紫角色明确，箭头线型统一 |
| 数据路径 | 0 | Update 箭头停在寄存器底边，未进入 B 行；provider-local 的写入终点不成立 |
| 方法一致性 | 0 | 图没有证明 B 反馈只更新 B 计数，违反 selected-only provider-local update 合同 |
| 纸面可读性 | 1 | 已检查 PDF 第 1 页的七英寸渲染；主链与公式可读，无裁切/重叠，但 A/B/C、n/s、向量索引和 Return 次级字段仍偏小 |
| 合计 | **11/16** | 数据路径与方法实质错误独立阻断通过 |

纸面分数基于从 `build_v08/ai_structures_fullwidth_20261007_v08.pdf` 第 1 页渲染的实际页面，不用放大原图替代；仓库当前没有同目录 `page1.png`，已如实记录。

## 留存与后续

保留 v10 原图和 v18 prompt；不覆盖已通过的 F1 v09/v17 审查，也不覆盖 v10 之前版本的失败记录。可复用本版的标签删减、菜单中性、q/Δ 合成和 Judge→feedback→queue 路径。下一版只需修正 B 行落点后重新做原图和纸面检查；修正 prompt 本身不能替代图级核验。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v18.txt` | `564dbb5cc94745b2875dc188e30eb5dd5c41552723fb99813c3041b7dfaff4cf` |
| `generated/figure1/v10_ai.png` | `d579f1f1e3bd0021ae8058447fe0e96fc453ca9afb3390f94e91e9a934e37f67` |
| `generated/figure1/v10_manifest.json` | `aac9f9779a79b910ccd4e73cf4fce52b704140aaf4938a07f69d01875c8ff4da` |
| `qa/build_v08/ai_structures_fullwidth_20261007_v08.pdf` | `7f867121f790f01b44a24b1d81ce111764646c70ec9081790c5e4637e2f5127d` |
| `qa/build_v08/page2.png` | `32b653489197a7fe939e411ad712700e4abf7391c436c96637044dd887ac0504` |
| `qa/build_v08/page3.png` | `3a17f7f7a8111daf3ceeacf04d11f5cb3fbe087c82315cc8bec6d4ace7b03576` |

主 session 的 TODO_LOG 由协调者汇总。本独立审查仅新增本文件。
