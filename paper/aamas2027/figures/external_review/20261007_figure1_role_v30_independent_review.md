# Figure 1 角色分工独立审查：v30 prompt / v20 AI

- 日期：2026-10-07。
- 对象：`figures/prompts/figure1/v30.txt`、`figures/generated/figure1/v20_ai.png` 与 `figures/qa/build_v16/page-1.png`。
- 结论：**方法/因果路径通过，14/16；当前是合格的最终候选，仍有一项线条语义和一项细部质感建议。**
- 硬错误：**0**。Update 直接进入高亮 released-state 行；residual 是 state 的读出，不在写入链中；没有 Release→state 旁路。
- 状态：候选通过，但以下线条语义建议完成后再做用户最终审美选择；本报告不替用户选版。
- 审查身份：独立图稿审查员；只读审查，不修改图片、prompt、正文、实验或方法。
- 实际查看：v20 原图与 build_v16/page-1 均通过 `view_image` original 查看；原图尺寸 2057×764，纸面页尺寸 1540×1540。

## 角色契约与文字清洁

v20 仍是 Figure 1 架构图：中心展开冻结打分、released state、residual、plus、Selector 和 selected-tool 执行；底部只画架构所需的释放回写。它没有采用 Figure 2 v26 的单事件时间脊、pending-to-later-decision 卡片顺序或时间线布局，因此两图职责清楚。

- 图中没有独立 provider 字母 `A`、`B`、`C`、provider 名称或任意行 ID。中间高亮 row 和 selected-tool icon 承担 selected provider 的示意。
- 标签遵守 v30 的短词限制；唯一公式是 `q=q^0+Δ` 和 `τ_t=t+1+D`。`n`、`s` 是 released-state 的统计列，不是 provider 标识。
- 没有 family residual、provider bias、额外调用、fallback、性能数值、训练骨干或 Figure 2 的重复事件面板。

## 实际路径审查

### 主分数架构

1. `Molecule + Endpoint → Features → Frozen scorer → q^0` 的箭头均接到具体模块；Features 与 scorer 用网格/锁和 q⁰ 三格向量表达。
2. `released state (n,s) → residual Δ → plus` 清楚。state→residual 的黑色读出边位于反馈基线之上，进入 residual 后再到 plus；它没有经过 Update，也没有把 residual 放在 state 写入路径中。
3. `q^0 + Δ → score → Selector` 只有一条得分路径；Eligible tools tray 紧贴 Selector 下方/侧边，只作为 Selector 的局部输入视觉，没有连到 state、residual、plus 或 score。
4. Selector 输出一个蓝色 Selected tool，随后 `Call → Return`；valid → 冻结 Judge → Chemical feedback，error → Runtime ledger dead-end。runtime edge 不会生成化学反馈或 state 更新。

### 唯一回写链

实际图中下方链条是：

`Chemical feedback → selected-only queue → Release (τ_t=t+1+D) → Update → highlighted released-state row`。

- Chemical feedback 只有一条短紫色下行边进入 queue。
- queue、Release、Update 在同一水平基线排列，箭头均单向向左，没有第二条等待边或 Release→register 旁路。
- Update 的蓝色水平边从右侧向左进入 released-state 高亮中行的右侧 `s` cell；箭头尖端在蓝色 row 内部，不是外框、底边或空白。
- 高亮中行的 `n`/`s` 两格同时为蓝色，箭头命中该 row 后，后续 state→residual 读出会影响下一次 score；因此该图表达了 selected-only provider-local 更新，而没有把 residual 误画成写入目标。

### 与 v30 空间合同的剩余细节

语义路径已经正确，但箭头尖端落在右侧 `s` cell 内，未落在 `n/s` 两格的几何中点；这不会改变 row-level 因果含义，因为两格整行高亮，但在七英寸页面可再居中以提高“整行更新”的直观性。

queue→Release 的紫色短边在 v20 原图中看起来是连续实线，Release→Update 也保持紫色；v30 规定 queue⇢Release 使用紫色虚线表示等待，并希望在线更新边用蓝色语法。当前 `τ_t` 时钟和模块顺序仍使延迟可读，所以这是线条语义/色板一致性问题，不是数据路径或方法错误。

## 图库风格与纸面检查

v20 延续 TableDART 的浅色分区和具体内部格、CRITIC 的闭环组织、CollabLLM 的事件/状态小模块。中央 MIRA-Relay 区最大，Inputs/Execution 退居两侧；Feature/weight grids、vector cells、plus、Selector tray、document/lock/gear/clock/queue 各有不同图形语法。没有全画布网格、段落框、渐变、阴影、巨大说明条或性能数字。

build_v16/page-1 的实际七英寸页面无裁切或重叠，主分数链、state/residual 读出、Update row 和执行分支均可追踪。主标签清楚，`n/s`、`q^0`、`Δ`、`τ_t` 与 valid/error 等次级字段在页面上偏小但仍可辨认；该项保守记 1，建议终稿再放大少量次级文字。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 中央 MIRA-Relay 的冻结分数、residual 合成和 Selector 占最大视觉区域 |
| 模块表达 | 2 | 特征/权重格、state register、残差/分数向量、plus、Selector tray、执行卡和 queue 形态有区分 |
| 信息密度 | 2 | 短标签和两条公式表达完整机制；无 provider 字母、重复段落或性能数字 |
| 布局 | 2 | 三分区紧凑；state/residual 位于方法区，queue→Release→Update 与 row 同基线，主箭头无交叉 |
| 色彩线条 | 1 | 主色角色正确，但等待边显示为连续紫线，Release→Update 未采用蓝色在线更新边，线语义仍可统一 |
| 数据路径 | 2 | Update 尖端进入高亮 row 内；residual 只读 state；queue→Release→Update→state 唯一且无旁路/边界终点 |
| 方法一致性 | 2 | 冻结 q⁰、released selected-only state、valid Judge、runtime dead-end 和 delayed release 均与正文一致 |
| 纸面可读性 | 1 | 已实际检查 build_v16/page-1；主链清楚，次级数学/状态字段略小 |
| 合计 | **14/16** | **无硬错误；线条语义和次级字号优化后具备顶会图稿质感** |

## 硬错误与顶会质感建议

**硬错误：0。** 没有 A/B/C 污染、Release→state 旁路、Update→register 外框终点、residual 介入写入、runtime failure 化学计数或 Figure 2 结构重复。

**建议 1（线条语义，优先）：** 将 queue⇢Release 改为一条明确的紫色虚线，把 Release→Update 改为实线蓝色；保持箭头方向和 `τ_t=t+1+D` 位置不变。这样“等待”与“释放后在线更新”与图库色板/线条语法一致，且不增加文字。

**建议 2（箭头几何）：** 将 Update→state 箭头尖端从右侧 `s` cell 边缘略移到高亮 `n/s` 行的内部中点，继续保持短水平、单向和唯一；不要让它接触 register 外框，也不要把 residual 放回 baseline。

**建议 3（纸面）：** 终稿可小幅增大 `n/s`、`q^0`、`Δ`、valid/error 与 `τ_t`；保持当前少字、白色 gutter、局部 Eligible tools tray，不增加 Figure 2 的时间线标签。

本报告基于实际 v20 原图和 build_v16/page-1，不以放大 PNG 替代纸面检查，也不宣称用户已最终选定该版本或已完成投稿接入。

## 文件与 hash

下列路径以 `paper/aamas2027/figures/` 为根。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v30.txt` | `81b6b1d7696991f0f9d113f27bae0d16929244e9508c2fc504970e39c0d5fb38` |
| `generated/figure1/v20_ai.png` | `7a874d95e51d5f5740bcbf4724d07417f4d65131c3f7de0f04fe0eb427f92fa5` |
| `generated/figure1/v20_manifest.json` | `e9dce577784968815be4671669cd1dd784b9ae1593e792c9ad0ac200775ac7a6` |
| `qa/build_v16/page-1.png` | `494a8cc04dd75cf233fc891bfbf344bfffc916e895912ed704d433bc524514a2` |

主 session 的 TODO_LOG 由协调者汇总；本独立审查仅新增本文件。
