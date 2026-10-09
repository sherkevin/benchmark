# Figure 1 独立审查：v21 prompt / v13 AI

- 日期：2026-10-07。
- 对象：完整 `prompts/figure1/v21.txt`、`generated/figure1/v13_ai.png` 与 `qa/build_v10/page1.png`。
- 结论：**因果/方法审查通过；严格图稿规范仍需 1 项视觉修复，13/16。**
- 硬错误：**0**。旧版 Release→counters 底边旁路已删除，唯一写入链成立。
- 当前状态：作为合格候选保留；在把 Release→Update 改为短蓝色水平边前，不称为最终图稿。
- 审查身份：独立图稿审查员；只读审查，不修改图片、prompt、正文、实验或方法。
- 实际查看：v13 原图和 page1 均通过 `view_image` original 查看；原图尺寸 2060×763，纸面页尺寸 1190×1540。

## 方法与检查标准

当前方法先用冻结 `q^0_a(x)` 和各 provider 的已释放计数生成 provider-local 残差 `\Delta_{a,t}`，逐项合成 `q=q^0+\Delta` 后选一个工具。有效 selected feedback 在 `\tau_t=t+1+D` 释放后，才更新对应 provider 的 `n,s`；runtime error 不生成 chemical label、不进入正确性计数。

因此，本图的唯一合法写入链是：

`Chemical feedback B → Selected-only queue → Release → Update → B-row counters`。

更新后的 counters 再经 `Residual → + → q → Select` 影响后续动作。任何 Release 直接进入 register 外框或其他 provider 行都属于实质因果错误。

## 实际原图路径核对

### 已通过的关键路径

- Molecule + Endpoint → Features → frozen scorer → 三格 `q^0` → `+`。
- Released counters 的 A/B/C、n/s register → 三格 Residual `\Delta`（B 高亮）→ `+`；`q=q^0+\Delta` 后输出三格 q，B 高亮并进入 Select。
- Eligible tools 的 A/B/C 卡片均为中性，经单一垂直边进入 Select；没有菜单到 Features/scorer 的连接。
- Select → 蓝色 B → Call → Return；Return 的 valid 边折向 Judge，Judge 向下生成 Chemical feedback B、`r\in\{0,1\}`。
- Return 的 error 边进入独立 Runtime ledger 并终止，没有进入 Judge、queue 或 chemical counters。
- Chemical feedback → Selected-only queue → 紫色虚线 Release，反馈身份保持 B。
- **Release→Update→B row：** v13 的 Update 模块右侧有一条短水平蓝箭头，箭头头实际进入 Released counters 的 B 行 n/s cells；没有再发现 v12 的 Release→counters 底部直连线，也没有箭头停在寄存器外框或空白边界。

### 需要视觉修复但不是硬因果错误

v13 的 Release→Update 边是**长的实线紫色折线**：从 Release 向左延伸，再向上以紫色箭头进入 Update 的底部。它仍然连接到真实的 Update 节点，并且唯一后继是 Update→B-row，因此没有形成第二条写入路径，也没有改变方法语义。

但 v21 prompt 的空间合同要求 Release 与 Update 相邻，使用**短、蓝色、水平**箭头从 Release 进入 Update 左侧；图中当前紫色长折线和底部竖直进入不符合该要求，颜色也让“等待/释放”和“已释放后更新”的视觉语义不够清楚。它属于布局与色彩线条问题，不是 Release 绕过 Update 的方法错误。本版保留为合格候选，建议在下一版中修正后再作为最终候选。

## 图库风格观察

v13 延续了已审 Figure 1 体系：TableDART 式浅色方法分区和具体内部格子，CRITIC 式短反馈闭环，CollabLLM 式事件卡和计数小模块。标签重复已删除；Frozen scorer、Molecule + Endpoint、`q^0/q/\Delta`、菜单和判分路径清楚，没有 family residual、额外调用、可训练 backbone 或虚构性能数字。整体仍然是机制图，未退化为形容词堆叠或大段说明框。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | MIRA-Relay 中央方法区最大，冻结分数、残差合成和选择为主体 |
| 模块表达 | 2 | 特征/权重格、计数寄存器、向量、`+`、Judge、记录卡、队列和 Release 形态各异 |
| 信息密度 | 2 | 短标签和一个公式表达机制，没有重复标题、解释段或性能数据 |
| 布局 | 1 | 唯一链可追踪，但 Release→Update 长折线把更新模块拉到寄存器左侧之外，未形成相邻下方三节点 |
| 色彩线条 | 1 | 蓝/灰/琥珀/紫角色大体稳定；Release→Update 仍是紫色实线而非规定的蓝色水平后释放边 |
| 数据路径 | 2 | 无 Release→counters 旁路；queue→Release→Update→B-row 唯一写入链成立，箭头均接到真实节点 |
| 方法一致性 | 2 | selected-only、provider-local B 更新、终止后判分和 delayed release 均正确，无科学机制新增或绕过 |
| 纸面可读性 | 1 | 已查看 build_v10 page1；主链、公式和 B identity 可读，无裁切/重叠；A/B/C、n/s、向量索引和 Return 小字段仍偏小 |
| 合计 | **13/16** | **因果通过；视觉空间/线色修复后才是严格规范候选** |

纸面分数基于实际 7-inch page1，不用放大原图替代。该页面确实显示 v13 图稿，但本报告不据此宣称已经接入正式论文或完成科学投稿 gate。

## 必须修复与建议

**硬错误：0。**

**严格图稿规范的 1 项视觉修复：** 将 Release 与 Update 放成相邻模块，删除当前长紫色底部折线，改为一条短、蓝色、水平箭头从 Release 右/左对应出口直接进入 Update 左侧；保留 Update→B-row 的短水平蓝箭头，且不要重新引入任何 Release→register 外框/空白边界旁路。

建议最多 2 项：

1. 保持当前标签删减，同时略微增大七英寸页面中的 A/B/C、n/s、`q^0/q/\Delta` 和 Return 字段；不增加解释句。
2. 修复 Release/Update 几何后维持现有白色窄 gutter 和中性菜单，不再添加 Later decision 或第二条反馈箭头。

## 文件与 hash

下列路径以 `paper/aamas2027/figures/` 为根。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v21.txt` | `ee9491c0cb6f14798401d03e713f6014572d9b4c330e5af9dc8e8aaf56743e98` |
| `generated/figure1/v13_ai.png` | `8f49d0e08c09c5450966e332d918827dfa22d523f20a6758f29aa6b8128b643e` |
| `generated/figure1/v13_manifest.json` | `aa5e190fc9fb123fbe6876fdb3eec166914df34273ef21f95706f4d2012be717` |
| `qa/build_v10/page1.png` | `fe5bac3de29e06eea4f7f49d02547b45fefd844a183c69997f053c58f07420e3` |

主 session 的 TODO_LOG 由协调者汇总；本独立审查仅新增本文件。
