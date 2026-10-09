# Figure 1 v29 独立审查（prompt v39）

日期：2026-10-08。审查对象是未经像素编辑的 AI 原图 `figures/generated/figure1/v29_ai.png`，并对照 Figure 2 v26、`figure_role_contract_20261007.md`、`gallery_style_study_20261007.md` 和当前方法的 released selected-only counter 规则。

## 实际查看的证据

- 原图：`paper/aamas2027/figures/generated/figure1/v29_ai.png`，2057×764；通过 `view_image` original 查看，并放大检查 Update→state 区域。
- 纸面 QA：`figures/qa/build_v20/page-1.png`（Figure 1 单页）和 `figures/qa/build_v20/page-3.png`（Figure 1/Figure 2 联合页）均通过 `view_image` original 查看。
- `build_v20` 使用 11×8.5 inch 页面、0.35 inch 边距，图宽约 10.3 inch；它不是 7-inch 通栏 PDF。因此第 8 项只记部分通过，不能把该 build 当作最终 7-inch gate。
- 风格参考实际复看了本地 `iclr2026-1635.jpg`、`iclr2024-17.jpg`、`icml2025-0242.jpg`，并与 Figure 2 v26 对照。

## 结论摘要

**图级通过候选，15/16；无方法或因果硬错误。** v29 修复了 v25/v26 的终点问题：`Update` 与 released-state 同高，蓝色水平边穿过 register 右边框，箭头头停在高亮底行内部的无标签实心蓝色写入端口，且没有新增 state store 或旁路。原图级别可进入候选集；仍需真实 7-inch 单图/联合页复核后才可称纸面最终通过。

## 角色分工与职责边界

- Figure 1 只展开方法架构：Features、Frozen score、released state、residual、plus、score、Selector、eligible tools 和 execution branch；没有 `decision t`、`pending`、`later decision` 或 Figure 2 的时间轴布局。
- Figure 2 v26 只表达 selected event 的时间顺序与延迟窗口。两图共享 terminal/judge/feedback/release 等接口词，但没有复制 Figure 1 的特征格、残差计算、候选菜单或完整状态架构。
- 原图没有独立的 provider `A/B/C` 字母或 provider 名称。`eligible tools` 通过一条局部蓝色上行边只连接 Selector；没有连接 released state、residual、score 或 execution。

## 因果与方法检查

### 通过的主路径

1. `molecule/endpoint → Features → Frozen score → q^0`，冻结权重用锁和网格表达。
2. `released state → residual → plus`，与 `q=q^0+Δ` 合成为 score vector，再进入 Selector；residual 是 state 的读出，不是写入链上的模块。
3. `Selector → selected tool → Call → Terminal record → Frozen endpoint judge → Chemical feedback`，判分发生在终止记录之后。
4. runtime error 从 Terminal record 进入独立 Runtime ledger；没有边进入 Chemical feedback 或计数更新。
5. `Chemical feedback → selected-only queue → Release` 的紫色虚线等待边标注 `τ_t=t+1+D`；`Release → Update` 是蓝色实线；`Update → released state` 是一条短的蓝色水平实线。

### Update 终点的独立核对

在原图和 build_v20 的 Figure 1 上，Update 边从右向左穿过 released-state 卡片边框，箭头头落在高亮底行内部的蓝色圆形写入端口，端口与卡片下边界之间仍留有可见蓝色行内空间。该端口只是写入连接的视觉终点，不是额外模块。没有第二条 Update 边、Release→state 旁路或悬空箭头。因此 `queue → Release → Update → selected provider released state` 的唯一写入链在图面上成立。

## 图库风格与纸面观察

v29 延续样例库的浅色分区、具体网格/记录卡/算子/队列形态和短直连边；白底、深蓝方法结构、灰色冻结/诊断、琥珀终态、低饱和紫色等待的角色稳定。与 Figure 2 v26 的卡片、线宽、图标和纸面纹理一致，同时保持架构图与事件时间线的不同布局。没有巨大标题条、泳道、背景网格、渐变或装饰性叉号。

在 10.3-inch `page-1`/`page-3` 上，主链、Update 内部端口、`q=q^0+Δ`、`τ_t=t+1+D`、runtime ledger 和菜单均可辨认；`output/runtime status`、计数格含义和小公式相对偏小。由于尚未有 7-inch 页面，不能确认缩小后这些次级字段仍满足投稿纸面标准。

## 八项评分（0/1/2）

| 项目 | 分数 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | relay 的冻结分数、残差合成与 Selector 构成主体，execution 和延迟回路服务于主方法 |
| 模块表达 | 2 | 特征格、锁定权重格、两行 released-state 计数格、残差向量、plus、score、菜单和记录卡各有不同图形语法 |
| 信息密度 | 2 | 短标签和两条公式，未加入 provider 字母、解释段落、性能数字或额外调用 |
| 布局 | 2 | 输入—relay—execution 的分区清楚；下方回写链短、同高、无交叉，Update 终点明确进入目标行 |
| 色彩线条 | 2 | 蓝色核心与实线更新、紫色虚线等待、灰色冻结/运行账本、琥珀反馈角色一致，箭头方向明确 |
| 数据路径 | 2 | selected valid feedback 才进入 queue→Release→Update；runtime failure 只到 ledger；唯一写入边进入高亮 row 内部端口 |
| 方法一致性 | 2 | `q^0+Δ`、selected-only queue、`τ_t=t+1+D`、冻结 judge 和 provider-local released-state 语义均保持；无 family sharing 或 chemical-label 旁路 |
| 纸面可读性 | 1 | 已看 build_v20 的 10.3-inch 单图和联合页，主结构可读但次级字偏小；对应 7-inch QA 尚未提供 |
| **合计** | **15/16** | **无硬错误；第 8 项等待真实 7-inch gate** |

## 硬错误、候选结论与建议

**硬错误：无。** v29 的水平 Update→released-state 边已实质进入高亮行；蓝色圆点是行内写入端口，不应当被当作新状态模块。

**候选结论：推荐作为 Figure 1 图级候选。** 它可以进入候选展示和后续整稿比较，但当前不能宣称已经通过 7-inch 纸面 gate，也不能替代用户对最终图稿的选择。

建议按以下顺序收尾：

1. 另编真实 7-inch 单图和 Figure 1/Figure 2 联合页，复核 Update 端口、`q^0`、`Δ`、`τ_t`、计数格、`output/runtime status` 与 runtime error 在缩小后的可读性。
2. 如果正文需要让计数含义更自解释，可在不引入 provider 字母、不增加模块的前提下给两列计数格加极小的 `n`/`s` 表头；这是可选的洁净度改进，不是当前方法错误。
3. 保持当前单一写入边和端口位置；不要增加第二条 Release→state 边或把端口改成独立节点。

## 文件与 hash

| 文件 | SHA-256 |
|---|---|
| `figures/prompts/figure1/v39.txt` | `f63332ed43152bbfd757d57863279953ae5176a55e6c0355749bdb2143224c2a` |
| `figures/generated/figure1/v29_ai.png` | `f912a527f36f706860dd647ebce4b39cfb56590434ebcf36601a4abfe084dbc0` |
| `figures/generated/figure1/v29_manifest.json` | `d59612e67d03924f6b7f79de7cc1d32bb8f480d1aa82a6e3b0704dabcc427db9` |
| `figures/qa/build_v20/page-1.png` | `b77acc9fe7044292f57c538a7c1ae91c79434c86cd617416ea1c3724419025b5` |
| `figures/qa/build_v20/page-3.png` | `6c18edf5a7245a6b4d5c8f8b7dcfbba8318938fd6dc18c25e53c47182fd84d59` |
| `figures/qa/build_v20/ai_structures_fullwidth_20261008_v20.pdf` | `40d8bb56333f658caf79e5fca90ef56f927aef857c5ec56f2e202921e25539ab` |

本报告仅新增独立审查记录，不修改图片、prompt、正文、实验或方法文件。
