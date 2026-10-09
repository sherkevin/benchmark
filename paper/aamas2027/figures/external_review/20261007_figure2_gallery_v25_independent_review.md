# Figure 2 独立审查：v25 prompt / v24 AI / 7-inch QA

- 日期：2026-10-07。
- 对象：完整 `prompts/figure2/v25.txt`、`generated/figure2/v24_ai.png` 与 build_v06 的单图/联合 QA。
- 结论：**不选用，13/16；菜单中性角色仍有 1 项必须修复。除菜单外没有发现其他必须修复项。**
- 实际查看：原始 AI 图 2060×763、`page2.png` 单图 7-inch 和 `page3.png` F1/F2 同宽联合页均通过 `view_image` original 查看。两张页面尺寸均为 1190×1540。
- 纸面状态：已实际检查 7-inch QA；该证据是单图/联合审查 PDF，不宣称已经接入正式整稿。
- 范围：独立审查及本报告；未编辑图像、prompt、main.tex、方法或实验。

## 唯一必须修复项

**Menu Aₜ 的 B 仍为蓝色。** A/C 的卡片和 glyph 是中性颜色，B 使用与 Select 输出 B 相同的蓝色。v25 明确要求候选集 A/B/C 同等中性，并且选择身份只在 Select 后产生；F1 v09 也已采用中性菜单。应将这一输入 B 的边框、底色和 glyph 恢复与 A/C 一致，保持输出 B、Update B 和 feedback B 的身份颜色。

这属于图示角色与时间组织的必须修复。本报告没有据此断言实际实现读取了未来输出。等待、释放和正确性计数的实际连线均已复核通过。

## 除菜单外的机制复核

| 检查 | v24 实际观察 | 结果 |
|---|---|---|
| 合法输入与单次动作 | Query、State Sₜ、Menu Aₜ 接入 Select，只输出并调用 B；没有输入当前 output/truth/label | 通过；菜单配色另述 |
| 终止后判分 | Return 的 valid 边进入锁定 Judge，只有 Judge 生成 B、r∈{0,1} feedback | 通过 |
| Runtime failure | Return error 分支进入独立 Runtime ledger 并结束 | 通过；未进入 Judge、queue 或 chemical counters |
| 单一等待方向 | Feedback 向下进入 Pending；queue 只有一条左向紫色虚线，唯一箭头进入 Release | 通过；双向线已删除 |
| 时序公式 | 虚线上方为 τₜ=t+1+D，没有时间珠；下方说明其他已释放事件仍可使用 | 通过；逆序珠问题已关闭 |
| 释放后单位更新 | Release 后实线进入唯一 Update B 寄存器；B 的 n/s 格分别是 +1、+r，A/C 中性且不变 | 通过；是更新运算，不是伪造实验计数 |
| 更新身份 | B/r tag、feedback、queue、Update B 保持同一 B | 通过 |
| 后续 action | 更新后 Sτₜ 只进入独立 Later decision≥τₜ；没有进入 tick-t Select 的回路 | 通过 |

初始 Sₜ 和最终 Sτₜ 已改为小型状态 glyph，只有 Update B 保留计数格。这修复了前版三处大计数卡的重复。+1/+r 与当前 `main.tex` 的 selected released update 相符：有效错误增加 n，正确事件增加 n 与 s；failure 没有 chemical count。

## 实际纸面和同风格检查

已查看 QA PDF 第 2 页的单独 F2 和第 3 页的 F1 v09/F2 v24 同宽组合。两图的蓝/灰/琥珀/紫、document/gear/lock/clock/queue 和 counter glyph 属于同一体系。F2 已采用紧凑状态符号和一个更新寄存器，与 TableDART 的具体模块、CRITIC 的闭合路径、CollabLLM 的小事件/状态对象相符。

实际 7-inch 页面里主标签、τ 公式、方向与 B 更新可读，没有裁切或重叠。Return 的 output/status、n/s/+r 和次级小注比主标签小，保留纸面 1 分；不能把原图放大可读代替该判断。中央两条路线之间仍有较宽空白，可进一步压窄。

## 八项评分

| 项目 | 0/1/2 | 依据 |
|---|---:|---|
| 视觉中心 | 2 | selected-event 的 Pending/Release/Update 成为清楚的下半路径，状态表重复已消除 |
| 模块表达 | 2 | Return、feedback、queue、release、counter 和 state 分别表达内容与作用 |
| 信息密度 | 2 | 短标签为主，+1/+r 直接表达单位更新，没有解释性段落 |
| 布局 | 1 | 路径完整、节点无交叉；中央 gutter 仍宽于紧凑目标 |
| 色彩线条 | 2 | 同 F1 的颜色和线条体系；等待单一虚线、释放后蓝色实线明确 |
| 数据路径 | 2 | Judge/queue/release/counter/later-action 顺序正确，failure 独立结束 |
| 方法一致性 | 1 | 更新与反馈规则符合方法；菜单 B 的输入角色仍需修复 |
| 纸面可读性 | 1 | 已看实际 7-inch 单图和联合页；主链可读，次级字偏小 |
| 合计 | **13/16** | **菜单必须修复仍阻断本版选用** |

## 建议（2 项）

1. 保持已修好的连接和单位更新，压窄上下路径中央 gutter，避免再加入时间珠或额外分叉。
2. 适度提高 Return 小字段、n/s/+r、次级注释字号；无需增加解释句或新模块。

## 留存与复用

v24 的失败记录已独立保存：[v24 prompt / v23 AI 审查](20261007_figure2_gallery_v24_independent_review.md)。本版修复了双向等待、逆序时间珠、重复计数格与初始状态高亮；其正确连线与更新表示可复用。下一轮只修中性菜单时，也需实际检查整个新 AI 输出，防止旧路径回归。原始输出和本轮纸面 QA 保留不覆盖。

| 文件 | SHA-256 |
|---|---|
| `prompts/figure2/v25.txt` | `ab92b1422b027667f4acbb0b4b06d6eeb8b606e3318db10860c5c38bcdd16a9d` |
| `generated/figure2/v24_ai.png` | `973b4e2accb040a7a45d722d1065a0208c5c7396a170e8c63a70383064be6277` |
| `qa/build_v06/ai_structures_fullwidth_20261007_v06.pdf` | `7290a203a5286fc13b402a8b3a68dbd3585a372618d36fbcd0f7e376d45ce8e5` |
| `qa/build_v06/page2.png` | `fadae64660875f734c79d5085177750bb2a42181759c37b000e7e85f0e93de14` |
| `qa/build_v06/page3.png` | `5605517658781df36a84581bbdc3472d5a83a1ea785d9f87aa48263a42bee7dd` |

上述路径以 `paper/aamas2027/figures/` 为根。主 session 的 TODO_LOG 由协调者汇总；本审查仅新增本文件。
