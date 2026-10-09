# v05 生成图复核

日期：2026-10-05  
对象：用户根据 Figure 1/2 prompt v05 生成的两张图。

## 总体判断

这轮已经从“文字流程图”转成了真正的模块图和泳道图。风格上开始接近 TopConf gallery：白底、扁平模块、短标签、有限配色、箭头表达数据流，巨大叉号也已去掉。

Figure 2 基本达到可继续精修的状态；Figure 1 的几何结构很好，但还有一个会误导审稿人的选择顺序问题。两张图都需要保留源 SVG/HTML，PNG 只作为布局草图。

## Figure 1

### 已通过

- 输入、selector、tool execution/evaluation、delayed feedback 四个模块组清楚。
- family residual + provider bias、shared family ×N、provider bias chips 已把核心创新画出来。
- selected-only outcome、`$\tau_t=t+1+D$`、released state 和回流箭头可读。
- 没有继续使用大叉号，候选 provider 用灰化卡片和闭眼图标表示不可见，视觉上明显改善。

### 必须修

1. `Inputs and Candidates` 区已经出现一个蓝色 `selected provider`，随后 MIRA-Relay 又输出一个 `selected provider`。这会让读者以为 provider 在进入 selector 前已经被选过。
2. 输入区到 fit-only reference 的箭头会把候选输入误读成直接喂给 frozen reference。候选菜单应统一进入 MIRA-Relay，唯一的 selected provider 只能出现在 selector 之后。
3. runtime failure 目前又连到了 `diagnostic ledger only`。应拆开：runtime failure → runtime/hazard ledger；fallback outcome → diagnostic ledger only。两条灰色记账路径不能混成一条。

### 评分

内容与视觉：13/16。扣分来自选择顺序和记账语义；可编辑源文件尚未提供，交付层面仍不能验收。

## Figure 2

### 已通过

- 三条 lane 和五个时间列清晰，符合当前图组计划。
- 灰色 state rail 明确表示 pending 期间决策继续使用旧状态。
- selected-only queue、release `$\tau_t$`、`first action ≥ τ_t` 与 `$D=0$` 语义对齐。
- runtime status、chemical correctness、system failure 已分开；没有第二次调用和即时反馈环。
- 视觉上已经接近可以直接重绘的论文图。

### 必须修

- `fallback (e.g., failure)` 把 fallback 和 runtime failure 混为一谈，改为单独的 `fallback outcome`。
- `chemical correctness` 建议补成 `chemical correctness (valid return)`，保留失败时 chemical label 未定义的语义。

### 评分

内容与视觉：14/16。扣分只来自 fallback/failure 标签；可编辑源文件仍需补齐。

## v06 修订

- `prompts/figure1/v06.md/.txt`：删除输入区的预选 provider，规定唯一 selected card 只能在 MIRA-Relay 之后出现；拆开 runtime ledger 与 fallback diagnostic ledger。
- `prompts/figure2/v06.md/.txt`：删除 `fallback (e.g., failure)`，加入 `fallback outcome` 和 `chemical correctness (valid return)`。
- v05 保留，不覆盖；本轮没有修改论文正文和实验结果。

