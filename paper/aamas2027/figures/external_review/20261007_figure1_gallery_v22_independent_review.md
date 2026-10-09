# Figure 1 独立审查：v22 prompt / v14 AI

- 日期：2026-10-07。
- 对象：`figures/prompts/figure1/v22.txt`、`figures/generated/figure1/v14_ai.png` 与 `figures/qa/build_v11/page1.png`。
- 结论：**方法与因果路径通过；严格图稿规范仍需修正 Update→B-row 的折线，14/16。**
- 硬错误：**0**。v14 没有重新引入 Release→counters 的直连或外框旁路；唯一写入链仍可追踪。
- 当前状态：合格候选，尚不是最终图稿。必须把 Update→B-row 改为规定的短水平蓝边后，才满足 v22 的空间合同。
- 审查身份：独立图稿审查员；只读审查，不修改图片、prompt、正文、实验或方法。
- 实际查看：v14 原图与 build_v11/page1 均通过 `view_image` original 查看；原图尺寸 2060×763，纸面页尺寸 1190×1540。

## 方法与判定基准

正文方法是冻结的 provider surface `q^0_a(x)` 加各 provider 独立的已释放计数残差 `Δ_{a,t}`，形成 `q=q^0+Δ` 后选择一个工具。只有 selected provider 的有效终止反馈在 `τ=t+1+D` 释放后，才更新该 provider 的 `n,s`；runtime failure 不生成 chemical label，也不进入正确性计数。

因此 Figure 1 必须呈现唯一写入链：

`Chemical feedback B → Selected-only queue → Release → Update → Released counters B-row`。

Update→B-row 是唯一的数据写入边；不能用一条从 Release 直接落到 register 外框、空白边界或其他 provider 行的边替代它。

## 实际图像核对

### 已通过的路径

- Molecule + Endpoint → Features（`φ(x) ∈ ℝ^d`）→ Frozen scorer → 三格 `q^0`；没有把菜单接到 Features 或 scorer。
- Released counters 的 A/B/C、n/s register → 三格 Residual `Δ`，仅 B 行突出 → `+`；得到三格 score `q=q^0+Δ` 后进入 Select。
- Eligible tools 的 A/B/C 卡片均保持中性，经一条短竖直边进入 Select；没有 Update→Select shortcut。
- Select → 选中的蓝色 B → Call → Return。Return 的 valid 分支进入 Judge，Judge 才生成 Chemical feedback B 和 `r∈{0,1}`；error 分支只进入 Runtime ledger 并终止。
- Chemical feedback B → 紫色 Selected-only queue → 紫色虚线 Release，等待标记为 `τ_t=t+1+D`；反馈身份未改变。
- **Release→Update→B row：** v14 把 Release 与 Update 放到同一水平线上，并画出一条短蓝色水平边从 Release 指向 Update。Update 的另一条蓝色边经折线向上后右转，箭头头进入高亮的 B 行区域；没有新的 Release→register 直连或第二条 blue boundary edge。

### 必须修正的空间连线

v22 要求 Update 紧贴 Released counters 左侧、与 B 行垂直对齐，并由 Update 右侧以一条短水平蓝箭头直接进入 B 的 `n/s` 单元中心。实际 v14 中：

1. Update 在 register 下方，而不是 register 左侧、B 行中心高度；
2. Update→B 的边先向左，再向上，再向右进入 B 行左侧，形成 U/L 形折线；
3. 箭头头靠近 B 行的 provider-label 左侧，未落在 `n/s` 两个计数单元的中心。

这违反了 v22 的 FINAL TOPOLOGY/LAYOUT OVERRIDE，降低了 provider-local write 的即时可读性。它仍从真实 Update 节点出发并在真实 B 行内结束，未形成 Release→counters 旁路、未绕过 Update，也没有改变正文方法。因此这是**严格图稿规范的必须修复项，不是当前的科学因果硬错误**。下一版应把三个节点排成清晰的相邻链：`Release —(短蓝水平)→ Update —(短蓝水平)→ B 的 n/s cells`；保留 v14 已修好的 Release→Update 短蓝边。

## 图库风格观察

v14 保留了 TableDART 式浅色方法分区、CRITIC 式闭环和 CollabLLM 式事件卡/计数小模块。Frozen scorer、residual vector、plus、counter register、Judge、Return、queue 与 Release 使用了不同图形语法；没有新增模型模块、额外 provider 调用、family sharing、性能数字或解释性段落。主体仍由 MIRA-Relay 中央区承担，紫色只标等待/释放，蓝色标在线核心和更新。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 中央 MIRA-Relay 方法区最大，冻结分数、残差和选择是视觉主体 |
| 模块表达 | 2 | 特征/权重格、provider 计数格、窄向量、`+`、Judge、Return、事件卡和队列均有具体形态 |
| 信息密度 | 2 | 标签精简，只有 `q=q^0+Δ` 和释放时钟，无重复说明框或性能数据 |
| 布局 | 1 | Release→Update 已同线且短，但 Update→B 仍绕到左侧后上行，未形成与 B 行对齐的三节点水平链 |
| 色彩线条 | 2 | 蓝色在线/更新、灰色冻结/外部、琥珀判分、紫色等待/释放的角色稳定；主要箭头均有方向 |
| 数据路径 | 2 | queue→Release→Update→B-row 唯一写入链成立；箭头终点在 B 行而非外框/空白，未出现旁路 |
| 方法一致性 | 2 | selected-only、provider-local B 更新、终止后 Judge 和 delayed release 均正确；runtime failure 不进化学计数 |
| 纸面可读性 | 1 | 已查看 build_v11 的 7-inch page1；主链、公式和 B identity 可读且无裁切/重叠，但 A/B/C、n/s、Return 小字段和折线路径仍偏小 |
| 合计 | **14/16** | **方法/因果通过；完成 Update→B 的短水平几何修复后再作为最终候选** |

## 硬错误与建议

**硬因果/方法错误：0。** 当前折线不构成第二条写入路径，因为它从 Update 出发、箭头头在 B 行内，并且没有 Release 直接到 register 的边。

**必须修复（严格图稿规范 1 项）：** 把 Released counters 放在 Update 右侧，使 Update 与 B 行垂直对齐；删除 Update 向左再上行的折线，改成一条短、实线、蓝色水平箭头，箭头头落在 B 行 `n/s` 单元中心。不要添加任何 Release→register 外框/底边/垂直边，也不要改变现有 queue→Release 虚线或 Release→Update 短蓝边。

**建议（不改变方法）：**

1. 继续保持中性 A/B/C menu、单一 q 合成公式和窄白 gutter；不要用额外文字解释折线路径。
2. 7-inch 页面中可略放大 B-row 的 `n/s` 标记、Return 的 output/status 与 valid/error 标签；不增加新模块或新箭头。

本报告基于实际 v14 原图和 build_v11/page1；不把放大 PNG 的可读性替代纸面检查，也不宣称用户已最终选定或已接入正式投稿。

## 文件与 hash

下列路径以 `paper/aamas2027/figures/` 为根。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v22.txt` | `dd3a1a7b885662971c0ff5a2b3e00e94a8a497c7cd386a5e439ab6c567f0be4f` |
| `generated/figure1/v14_ai.png` | `aa0a4fa3f84e58393da9baf438fd0d40d7599461ce293103102855dc6d3ec320` |
| `generated/figure1/v14_manifest.json` | `a21e7a6f0158f2e691be630180abde42057d09e69b6652fdac87a44d757eebe5` |
| `qa/build_v11/page1.png` | `06f58c132029e95e94994daf065b20ca386b943156e4f4ab1a2e474d5708bd63` |

主 session 的 TODO_LOG 由协调者汇总；本独立审查仅新增本文件。
