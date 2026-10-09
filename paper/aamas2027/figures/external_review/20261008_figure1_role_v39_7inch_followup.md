# Figure 1 v29：7-inch QA follow-up to v39 independent review

日期：2026-10-08。本文是对 `20261007_figure1_role_v39_independent_review.md` 的纸面补充；原图、prompt、拓扑判断和硬错误结论不变。

## 新增纸面证据

- `paper/aamas2027/figures/qa/build_v21/page-1.png`：Figure 1 v29，实际嵌入宽度 7 inch。
- `paper/aamas2027/figures/qa/build_v21/page-2.png`：Figure 2 v26，实际嵌入宽度 7 inch，用于职责和风格对照。
- `paper/aamas2027/figures/qa/build_v21/ai_structures_7inch_20261008.pdf`：两页 PDF，页尺寸 7.5×3.35 inch，编译日志无 warning。

两页均通过 `view_image` original 查看。Figure 1 在 7-inch 页面上仍能读出输入—relay—execution 分区、`q=q^0+Δ`、Selector、selected tool、Terminal record、Frozen endpoint judge、Chemical feedback、runtime ledger、`τ_t=t+1+D` 和 queue→Release→Update 回路。水平 Update 箭头仍清楚穿入高亮 released-state 行并落在内部蓝色端口；没有出现边框终点、旁路或缩小后方向歧义。Figure 2 的时间顺序、紫色等待段和 later released state 也保持独立。

主标签、主箭头和两条公式在 7-inch 页面可辨。`Terminal record` 内的 `output/runtime status`、`runtime error`、计数格的隐含列含义以及部分图标细节偏小，但没有裁切或重叠；这些次级字段仍可作为局部可读信息。因此纸面项保持部分通过，而非未测。

## 更新后的评分与结论

| 项目 | 分数 | 7-inch follow-up |
|---|---:|---|
| 视觉中心 | 2 | relay 核心和回写链仍是视觉中心 |
| 模块表达 | 2 | 网格、计数格、向量、算子、卡片和队列在缩小后仍可区分 |
| 信息密度 | 2 | 没有新增 provider 字母、解释段落或多余模块 |
| 布局 | 2 | 主路径和水平 Update 写入边无交叉、无悬空终点 |
| 色彩线条 | 2 | 蓝色更新、紫色等待、灰色冻结/账本、琥珀反馈在两页一致 |
| 数据路径 | 2 | selected valid feedback 才进入唯一 queue→Release→Update→state 链；runtime error 不进 chemical feedback |
| 方法一致性 | 2 | `q^0+Δ`、selected-only queue、`τ_t=t+1+D` 和冻结判分语义保持 |
| 纸面可读性 | **1** | **真实 7-inch 已测；主结构和公式可读，次级 output/status、runtime error 与计数格细节偏小** |
| **合计** | **15/16** | **无硬错误** |

**候选结论不变：** v29 可作为 Figure 1 候选，且现在完成了 7-inch 纸面检查；没有发现新的因果或方法硬错误。由于次级字段仍偏小，建议最终整稿接入前保持短标签和单一写入端口，不再增加解释文字或第二条回写边。

## QA 文件 hash

| 文件 | SHA-256 |
|---|---|
| `figures/qa/build_v21/page-1.png` | `bf08cd90e0b98739615135a518059d34c45c92c7172f4c4e0b3ea7ec139c1ace` |
| `figures/qa/build_v21/page-2.png` | `814d53e5535069bd67b49003053a4833f9d468d54b5021f6b6e7b97b357425aa` |
| `figures/qa/build_v21/ai_structures_7inch_20261008.pdf` | `059907f167ba3907872ba3fa1b03f0772f70af87b50f67103c3c7e4cda6debb8` |

