# Figure 2 独立审查：v26 prompt / v25 AI / 7-inch QA

- 日期：2026-10-07。
- 对象：完整 `prompts/figure2/v26.txt`、`generated/figure2/v25_ai.png`、build_v07 的单图和联合 QA。
- 结论：**审查通过的候选，14/16；必须修复项 0。** 数据路径与方法一致性均为 2/2。
- 选用状态：**用户尚未最终选择。** 已读取生效[决议 0013](../../../../docs/决议/0013-AI图版本全部保留并展示供用户选择.md)，本报告不将审查通过或自动评分等同于最终审美选定。
- 实际查看：原始图及两张 7-inch render 均使用 `view_image` original；原图 2060×763，页面均为 1190×1540。
- 范围：只审查并新增本报告；未编辑图像、prompt、main.tex、方法或实验。旧输出及失败报告保留。

## 方法和时间依赖

当前方法的核心约束是：tick-t action 只读当时可用的 query/menu/state；当前所选有效反馈在 `τₜ=t+1+D` 释放后才改变对应 provider 的正确性计数，并可影响独立的后续 action。Runtime failure 不进入 chemical label/count。本图展示一个 B 事件，A/C 不接受该事件的更新。

## 实际图级复核

| 检查点 | v25 实际观察 | 结论 |
|---|---|---|
| 选择前输入 | Query、中性 State Sₜ、Menu Aₜ 分别进入 Select；菜单 A/B/C 的底色、边框和 glyph 均恢复中性 | 通过；前版菜单问题关闭 |
| 单次动作 | Select 后才出现蓝色 B，B 只进入一次 Call | 通过 |
| 终止后判分 | Return 的 valid 边进入灰色锁定 Judge，Judge 生成 amber B、`r∈{0,1}` feedback | 通过 |
| Failure bookkeeping | Return error→Runtime ledger 后终止；没有进入 Judge、feedback、queue 或正确性计数 | 通过 |
| Pending 与等待方向 | Feedback 向下进入 B Pending；只有一条左向紫色虚线进入 Release，没有额外箭头或时间珠 | 通过 |
| 释放时钟 | `τₜ=t+1+D` 只出现一次；D=0 注释为 next decision；其他已释放事件仍可使用 | 通过 |
| 单 provider 更新 | Release 经蓝色实线和 B/r tag 进入唯一 Update B 格；B 的 n/s 为 +1/+r，A/C 中性 | 通过；为更新操作，没有伪造结果 |
| 未来状态与 action | Update 后的 State Sτₜ 只进入独立 Later decision≥τₜ；未接回 tick-t Select | 通过 |
| 事件身份 | 输出、feedback、Pending、B/r、Update B 同一 B | 通过 |

完整链为 Query/State/Menu→Select→B→Call→Return→Judge→Feedback→Pending→Release→Update B→State Sτₜ→Later decision。等待和已释放线型清楚区分，没有 queue、Judge 或 state-update 旁路。没有当前 output、truth 或 label 被放到选择前输入。

## 实际纸面与图库风格

已查看 `qa/build_v07/page2.png` 的单独 7-inch F2，以及 `page3.png` 的 F1 v09/F2 v25 同宽组合。两图的颜色、document/gear/lock/clock/queue、计数格和箭头体系匹配；中性菜单现与 F1 一致。

F2 将真实操作分开画成 Return 文档、feedback token、Pending 卡叠、Release 时钟、单位更新寄存器与状态 glyph，保留了 TableDART 的具体模块表达、CRITIC 的清楚闭环、CollabLLM 的事件/状态小对象。三张原始 gallery 图此前均实际查看，来源/hash 见 [F1 v13 审查](20261007_figure1_gallery_v13_independent_review.md)。本图没有靠长解释文字来补错误连线。

实际纸面中主链、B 身份和 τ 公式可读，没有裁切或重叠。次级字段和单位更新小字仍小于主标签；中央上下路径之间的 gutter 仍可压窄。此处依据两张真实 7-inch render，未用放大 PNG 替代。

## 八项评分

| 项目 | 0/1/2 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | selected-event 的等待、释放与 B 更新明确；状态只用小 glyph，更新只有一个计数格 |
| 模块表达 | 2 | 文档、token、queue、clock、register 和 state 表达不同数据/运算角色 |
| 信息密度 | 2 | 短标签和 +1/+r 直接表达操作，无解释性段落 |
| 布局 | 1 | 完整 U 路径、无交叉；中央 gutter 仍偏宽 |
| 色彩线条 | 2 | 蓝/灰/琥珀/紫角色稳定，菜单中性，单向等待与已释放实线明确 |
| 数据路径 | 2 | 判分→queue→release→更新→独立 later action 完整，failure 分支独立结束 |
| 方法一致性 | 2 | selected-only、provider-local、终止后判分及 delayed release 均符合当前方法 |
| 纸面可读性 | 1 | 单图与联合 7-inch 均已检查；主链可读，Return 小字段、n/s/+r 与小注偏小 |
| 合计 | **14/16** | **审查通过的候选；用户选择待定** |

## 必须修复与建议

**必须修复：0。**

可选建议仅两项，不改变本版方法/因果通过：

1. 压窄上下路径中央 gutter，保持已有连接和独立 later-action 节点。
2. 适度提高 Return 小字段、n/s/+r 和次级注释字号，保持短标签，不新增解释段落。

7-inch QA 是单图与联合图的物理尺寸检查，本报告不宣称已经完成正式论文整稿接入、科学 gate 或投稿政策审查。

## 全部版本留存和复用

| 审查文件 | 输出 | 状态 |
|---|---|---|
| [v24 独立审查](20261007_figure2_gallery_v24_independent_review.md) | v23 AI | 不通过：等待双向/逆序 tick、菜单 B 预先异色；纸面未测 |
| [v25 独立审查](20261007_figure2_gallery_v25_independent_review.md) | v24 AI | 菜单仍异色，暂不选用；其他路径已修复，纸面已测 |
| 本文件 | v25 AI | 审查通过的候选，14/16；纸面已测，用户最终选择待定 |

旧原图、完整 prompt、QA 和独立报告均保留，不用新候选覆盖旧版本。v25 的单向等待、B 单位计数、独立 Runtime ledger、状态和 later-action 表达可复用于后续版；若用户偏好旧版风格，应依 0013 保留该风格并以新 AI 版本修正问题，再展示比较。

## 文件与 hash

下列路径以 `paper/aamas2027/figures/` 为根。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure2/v26.txt` | `f735687beae57481bbd9778025ab13cbc5a8d3f72749f77aa103a3746643fd6a` |
| `generated/figure2/v25_ai.png` | `317199c760421b540936858710ff9945a8d23a35e11100d5f4ab29cf3bb81381` |
| `qa/build_v07/ai_structures_fullwidth_20261007_v07.pdf` | `a525e1b83316d48056c163e8bf930eba0f652b214cdf5029ddaa612c46f08f29` |
| `qa/build_v07/page2.png` | `32b653489197a7fe939e411ad712700e4abf7391c436c96637044dd887ac0504` |
| `qa/build_v07/page3.png` | `4b8c147b9aec31fa1d63da7e629c37dc7bc29b369a57cd0d1ba1201041e34ee9` |

主 session 的 TODO_LOG 与全部版本对照展示由协调者汇总。本独立审查完成。
