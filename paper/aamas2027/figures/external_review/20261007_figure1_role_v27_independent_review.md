# Figure 1 角色分工独立审查：v27 prompt / v18 AI

- 日期：2026-10-07。
- 对象：`figures/prompts/figure1/v27.txt`、`figures/generated/figure1/v18_ai.png` 与 `figures/qa/build_v14/page-1.png`。
- 结论：**方法与数据路径通过；严格 v27 空间连线仍需 1 项布局修复，14/16。**
- 硬错误：**0**。Update→released-state 的箭头头已经进入高亮中行的 cell 内，没有 Release→register 旁路或外框终点。
- 当前状态：合格候选，尚不称最终图稿；应把 Update 与高亮行对齐并改成短水平边后再定稿。
- 审查身份：独立图稿审查员；只读审查，不修改图片、prompt、正文、实验或方法。
- 实际查看：v18 原图与 build_v14/page-1 均通过 `view_image` original 查看；原图尺寸 2060×763，纸面页尺寸 1540×1540。

## 角色契约和文字清洁

Figure 1 负责内部架构：特征、冻结 scorer、released state、residual、score synthesis、Eligible tools/Selector、执行判分和 selected-only update。Figure 2 v26 负责单次事件的时间依赖；v18 没有复制 Figure 2 的单事件横向时间脊或 later-decision 卡片布局，仍以中央分数架构为视觉中心。

- 原图和 page-1 中没有独立 provider 字母 `A`、`B`、`C`，没有 provider 名称，也没有重复的单字母 token。三行状态和向量用一个中间高亮行表达 selected provider。
- 没有旧 family residual/provider bias、额外调用、trainable backbone、fallback、性能数字或虚构 output。
- 标签均为 v27 允许的短词；公式只有 `q=q^0+Δ` 和 `τ_t=t+1+D`。`n`、`s` 是 released-state 的两列统计量，不是 provider 身份。

## 实际架构与箭头检查

### 已通过的主体路径

1. `Molecule + Endpoint → Features → Frozen scorer → base score q^0` 由短箭头连接；网格/锁和三格向量表达了冻结特征与基线输出。
2. `released state (n,s) → residual Δ → plus` 清楚；高亮中行同时出现在 state register 和 residual vector。
3. `q^0` 与 `Δ` 分别进入圆形 plus，得到 `score` 三格向量，再以一条短箭头进入 Selector。
4. Eligible tools tray 紧贴 Selector 左侧，score→Eligible tools→Selector 的局部路径可读；tray 未连接 released state、residual、plus 或 score 之外的模块。没有跨面板的候选菜单长线。
5. Selector 输出单一蓝色 Selected tool，随后 `Call → Return`；valid 进入冻结 Judge，再进入琥珀 Chemical feedback `r∈{0,1}`；Return 的 error 虚线进入 Runtime ledger 并终止。
6. Chemical feedback → selected-only queue → 紫色虚线 Release (`τ_t=t+1+D`) → 短蓝边 Update。没有第二条 queue/Release 路由，也没有 runtime ledger→chemical/state 的边。

### Update 写入目标的核对

v18 已修正 v16 的关键错误：Update 发出的蓝线虽然仍先向左、再向上，但向上的箭头头**实际落在 released-state 高亮中行的右侧 `s` cell 内**，而不是 register 底边、外框或空白。高亮行的 `n` 与 `s` 两格均为蓝色，箭头尖端在行的内部，因此当前唯一外部写入链可以读作：

`selected-only queue → Release → Update → highlighted released-state row`。

没有发现 Release 直接触碰 register、Update→register 的第二条蓝线，或其它 provider 行的写入路径。箭头终点已满足数据路径的具体目标要求，所以不构成方法/因果硬错误。

### 仍需修复的严格几何问题

v27 的 FINAL GRAPH AUDIT 要求 Update 与高亮行同一垂直高度，并用**一条短水平蓝箭头**直接进入行内，同时明确禁止 U-shaped route。v18 虽然 endpoint 已进入正确 row，但 Update 在反馈带下方，连接仍是长水平段加竖直上行的折线。它降低了“Update→row”即时可读性，也没有完全执行 prompt 的空间合同；由于链条唯一且终点合法，这属于布局/线条必修，而不是科学方法硬错误。

下一版应将 Update 放到 released-state register 的同高左侧（或等价相邻位置），把当前折线替换为短、实线、蓝色水平边，箭头头仍落在高亮 `n/s` row 内；不要重新引入 Release→register 旁路。

## 图库风格与纸面观察

v18 延续 TableDART 式浅色方法分区、CRITIC 式闭环和 CollabLLM 式小型记录/计数模块。中央 MIRA-Relay 区最大；Features/weight grids、vectors、plus、tray、cards、Judge、queue 和 Release 使用不同图形语法。没有 Figure 2 的重复架构，亦没有大标题条、段落框或全画布网格。

在 build_v14/page-1 的七英寸页面上，主链、公式、Eligible tools、selected-only queue 和高亮 row 可辨认且无裁切/重叠。`n/s`、`q^0`、`Δ`、valid/error、`τ_t` 和 feedback 小字偏小，且 U 形 Update 边在缩小后不如目标水平边直观，因此纸面项记 1。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | 中央 MIRA-Relay 的冻结分数、残差合成和 Selector 是最大视觉区域 |
| 模块表达 | 2 | 特征/权重格、q⁰/Δ/score 向量、state register、plus、menu tray、cards 和 queue 语法不同 |
| 信息密度 | 2 | 无 provider 字母和重复解释句；标签短，公式数量受控 |
| 布局 | 1 | 主分区紧凑，Eligible tools 已贴 Selector；但 Update→row 仍是长折线而非同高短水平链 |
| 色彩线条 | 2 | 蓝色在线/更新、灰色冻结/记录、琥珀判分、紫色等待/释放角色稳定；箭头方向和终点均可追踪 |
| 数据路径 | 2 | queue→Release→Update→高亮 row 唯一成立，箭头头进入 row 内，无旁路、外框终点或 runtime 化学更新 |
| 方法一致性 | 2 | frozen base + released selected-only state、valid Judge、runtime dead-end 和 delayed release 均与正文一致 |
| 纸面可读性 | 1 | 已实际查看 build_v14/page-1；主结构清楚，次级字段和折线细节偏小 |
| 合计 | **14/16** | **因果/方法通过；修复严格几何后再作为最终图稿** |

## 硬错误与建议

**硬错误：0。** v18 的 Update 箭头确实进入高亮 released-state row；没有 Release→register 旁路、外框/空白终点、runtime failure 化学计数，或 Figure 2 架构重复。

**严格图稿规范的 1 项必须修复：** 将 Update 与高亮 row 放在同一水平线上，删除当前长水平加竖直的 U 形连接，改成一条短、实线、蓝色水平箭头直接进入高亮 `n/s` row 内。保留 score→Eligible tools→Selector 局部路径及当前唯一 queue→Release→Update 链。

**建议（不改变方法）：**

1. 适度增大 7-inch 页面上的 `n/s`、`q^0`、`Δ`、valid/error 和 `τ_t`；不要增加解释性句子或 provider 字母。
2. 保持 Eligible tools tray 贴 Selector 的局部布局，不要把它与 released state 或 Figure 2 的 pending timeline 相连。

本报告基于实际 v18 原图和 build_v14/page-1，不以放大 PNG 替代纸面检查，也不宣称用户已最终选定或该图已完成投稿接入。

## 文件与 hash

下列路径以 `paper/aamas2027/figures/` 为根。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure1/v27.txt` | `d23a0ae341299a29f97e33438911118e36e7de473844745a43ba4c1742672cd2` |
| `generated/figure1/v18_ai.png` | `7a059c485d62c47c8a6f58d89e4427eff434ec798dbd8a3298cf3de5f91164ea` |
| `generated/figure1/v18_manifest.json` | `f21f2a5791839ca31c108df168ac003cdf0209efd355fd52486ca77bee0d9044` |
| `qa/build_v14/page-1.png` | `4a8c044f09e79530cc19015da17fa3ba262ccfb74290ad3469ca12affac302ce` |

主 session 的 TODO_LOG 由协调者汇总；本独立审查仅新增本文件。
