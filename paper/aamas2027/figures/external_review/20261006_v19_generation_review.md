# Figure 2 v19 生成图复核

日期：2026-10-06  
对象：`figures/generated/figure2/v19_ai.png`  由 `prompts/figure2/v19.txt` 生成，基于 v13 视觉资产做 AI 迭代。

## 复核结论

v19 是目前 Figure 2 最合适的 AI 生成候选。它保留了 v13 的正确事件拓扑，同时把 v09 的大块空白和 v10–v18 的局部语义错误收敛到可审查的状态。建议将 v19 作为下一轮论文排版的候选图；在嵌入 AAMAS 双栏 PDF 后再做一次 100% 尺寸检查。

## 语义检查

- 顶部主链是 `S_t + A_t → selector → one selected provider → call → terminal record → frozen endpoint judge → chemical correctness`。
- runtime status 进入 `system failure`，再与 chemical correctness 一起汇入 `terminal outcome`；没有把 terminal record 直接当作化学结果。
- `terminal outcome → selected outcome → selected-only queue → released outcome → relay update → S_τt → first action` 连续可读，queue 没有被跳过。
- queue 是唯一等待模块，保留 `τ_t = t + 1 + D`、`t+1 … τ_t−1` 和 `hidden until τ_t`。
- 未选输出只在输入侧以 eye-slash 标记，没有箭头进入更新链。
- 反馈回到 future state 用局部 glyph 表示，没有长底部回路，也没有同一动作读取反馈的暗示。
- 图中没有 verifier、refusal、escalation 或第二次调用，符合当前一次调用主合同。

## 视觉检查

- 四个 phase tabs 让读者先读出 DECIDE、EXECUTE + JUDGE、DELAYED FEEDBACK、UPDATE，再沿事件带阅读。
- 蓝/灰/橙/紫四种语义色与 Figure 1 一致，图形模块多于说明文字。
- 画布约 2.86:1，模块占满横向空间，未出现 v09 的大块底部留白。
- 小字 `hidden until τ_t`、`D=0: visible before next action` 和未选输出标记，在最终单栏 PDF 中仍需按 100% 实际尺寸确认。

## 评分（内部 16 分制）

| 维度 | 得分 | 说明 |
|---|---:|---|
| 一句话信息 | 2/2 | 反馈只能在终止和延迟释放后进入下一次选择，图面可直接读出。 |
| 主线结构 | 2/2 | 主链和释放链方向明确。 |
| 信息边界 | 2/2 | selected-only、未选不可见和释放时钟均显式。 |
| 方法映射 | 2/2 | selector、terminal judge、queue、relay update、released state 与正文一致。 |
| 版式 | 1/2 | 宽幅图清楚；小字和 PNG 在最终双栏 PDF 中仍需核验。 |
| 视觉克制 | 2/2 | 只有四种语义色和必要图形，没有网格、阴影或装饰性面板。 |
| 黑白与无障碍 | 2/2 | 颜色之外有形状、箭头和图标冗余编码。 |
| 可追溯与可复核 | 2/2 | prompt、raw PNG、版本链和本复核记录均已保存。 |

**合计：15/16。** 扣分只保留给最终 PDF 尺寸检查，不是语义缺陷。

## 版本决策

- v10–v18 全部保留，不覆盖、不删除；其中部分版本专门记录了箭头方向和 junction 的失败模式。
- v19 作为当前 AI 视觉候选，原图 SHA-256=`50745a8eeb0018b81d79baff085e06a4e3e317fe346c9d6e1bc1ddc8a0029a2c`。
- 论文主文在替换 Figure 2 前，需重新编译一次并检查图注、`\\Description{}`、跨栏宽度和页数。
