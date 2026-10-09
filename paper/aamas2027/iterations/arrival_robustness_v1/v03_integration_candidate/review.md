# v03 全文合入候选：独立写作审查

2026-10-08。只读对照正文与结果来源；唯一写入文件是本报告。未编译、未修改源码或主版，不宣称实测页数，不代替独立实验核验。

## 结论

**合入没有损坏方法公式或数值；三处重要口径歧义已由 root 修复。** 当前没有发现由此次合入引入的 P0 数值错误。可在下面的小幅文字修正后进入布局检查；实验独立核验的完成状态仍由 auditor/root 决定，本审查不提升科学 gate。

审查版本：

- 合入前 SHA：`831149e1f05b17a6943dafa8d309ddb98c4643eef5083a1a82e820fc54f47a72`。
- 初审候选 SHA：`30134187e3303dfd3d094d42c16c4989a2afa0b9fd50e3858a50ff12a07ea826`。
- 三处修复后候选 SHA：`771c1357c0191952718893c8eeb86d5553ddab0c6c60f81ad90f8349aa7062e7`，与更新后的 `integration_plan.json` 一致。
- 新结果来源 SHA：`dbaf3e64f9f3695b0d0a20ade3124b25b1732e24cacad46e76e8ec0e12044946`。

## 已检查通过的内容

| 检查项 | 结果 |
|---|---|
| 方法公式 | 前后全部 8 个 `$$...$$` 块逐字一致。冻结 sigmoid 得分、收缩率、残差、argmax、延迟和逐源计数更新没有被压缩改坏。 |
| 后验/计数含义 | 分子与分母保留 `n0 * fit_rate + successes` / `n0 + count`；冷启动残差为零，错答只增加计数，执行失败不进入 chemical count。与当前主策略源码的 sigmoid + residual 形式一致。没有添加后验覆盖率保证。 |
| 旧结果数字 | main、credit、learned-prior、centering、cost 五个结果宏的全部数字 token 与合入前一致。旧区间端点未被新实验覆盖。 |
| 新表 | 与自动生成 v02 的全部行逐行一致：自然 10 行、受控 6 行；其余横杠严格对应预冻结 roster，不是删掉负结果。 |
| 不利结果 | 表中保留自然 pooled MIRA−Frozen `−0.29`，正文保留 `[-0.37,-0.20]`；没有用 equal-scaffold Local−Frozen 的正数替代。Fixed-permutation 的受控负收益也保留。 |
| 主比较 | MIRA−Online SGD 的均值、MCSE、均值区间、125/128 和精确概率区间均与来源一致。受控 Local−Permutation 明确使用不同的 equal-scaffold 估计对象。 |
| 旧/新不确定性 | main/credit 已改为 fixed-trajectory ranges；新 arrival 是条件于固定 cohort/priors/seeds 的 order-law Monte Carlo。post-call 的独立静态条件口径没有被错误地全局改名。 |
| 状态边界 | Validity 的 270/150 通过数明确限定 original-order streams，没有拿旧通过数冒充新 arrival 审计通过。 |
| map 与字体 | 保留实验 map，E1/E2 已增加新证据入口；新表沿用 `MiraTableFont = 9/11 pt` 和 8.5 pt 表注。 |

## 初审发现及已修复状态

1. **新旧 order/condition 混用：已修复。** 摘要的 `Natural-order replays` 原来可能被读成原始到达顺序，实际应是随机顺序下的自然结果条件。现已明确 `Random-order replays with natural outcomes`；贡献段也已改。
2. **摘要 credit-permutation 结论范围过宽：已修复。** 现加 `Under this controlled drift`，不再把受控效应讲成自然条件的归因增益。
3. **`identical observations` 与自适应曝光冲突：已修复。** 各策略会产生自己的 selected-feedback history。现明确 same information/release contract，且允许不同 selected outcomes。

## 还建议执行的具体小修

### A. 摘要补齐区间类型，并合并为八句

当前摘要为九句；“Monte Carlo mean interval”未写近似和 95% 水平。改数字附近的短语即可：

```latex
(approximate 95\% Monte Carlo mean interval [+4.32,+5.16])
```

可把第七、八句合为一句，既保留正负结果，也减少行文切碎：

```latex
Under this controlled drift, matched credit permutation supports source-aligned
updates; with natural outcomes in the same random orders, MIRA-Relay trails
Frozen by 0.29 points on average.
```

如果要在摘要本身进一步锁定不确定性范围，将第六句开头替换为：

```latex
Across 128 complete random-order replays of the previously inspected
157-molecule cohort, with priors and policy seeds fixed, ...
```

这几处替换不改变效果主张，也不把新 study 称为 fresh confirmation。

### B. 让 “95% range” 的定义独立完整

现在名称已正确，建议明确它是中央分位区间，避免被理解成重复结果的最小/最大值。在 Evaluation protocol 中，用下面两句替换现有关于旧 ranges 的两句；表注无需再堆同样细节：

```latex
Original-order ranges are the central 95\% of 10,000 scaffold resamples of
fixed recorded trajectories, retaining each scaffold's repeated configurations;
they do not establish new-trajectory coverage. The arrival study instead reruns
each policy on every sampled order.
```

main 是 pooled-call weighting，credit 是 equal-scaffold weighting，现有各表注已区分，必须保留。此替换不适用于 post-call 段。

### C. 把验收口吻换成统计解释

`The prespecified mean-and-majority directional criteria pass ...` 像内部验收记录。建议只替换这一句，不增加一个解释段：

```latex
The intervals support both a positive mean gain and a greater-than-half
probability of improvement under this arrival law.
```

精确的联合判断、阈值、逐流验证仍保存在实验卡和审计中。论文此处已有均值/胜率区间，读者可以直接核对。

### D. 压缩后的状态句加一个必要条件；贡献句去掉未展示的术语

`The 2K counters, menu masks and pending queue are sufficient to replay ... from its released event prefix` 容易把“已释放前缀决定计数”与“未释放队列可由前缀恢复”混在一起。替换为：

```latex
Given the fixed heads and current input and menu, the released event prefix
determines the residual scores; the pending queue determines future releases.
```

此外，贡献段沿用了 `Its score-margin condition ...`，正文没有显式写出该条件。这不是此次压缩造成的公式丢失。最省空间的准确替换是：

```latex
Its zero-residual initialization and source-local updates make the effect of
released evidence on later choices explicit.
```

无需为这一术语新加一个定理或实验。

## Conclusion 与工程日志比例

新版 Conclusion 已把原始顺序的相等结论换成随机顺序的受控增益与自然负差，方向正确。建议把 `Across randomized arrival orders` 改为 `Averaged over randomized arrival orders on this fixed cohort`，避免暗示每个顺序都赢；把 `slightly reducing` 改为 `reducing`，让量级由实测数字说明。

压缩后的 Validity/Artifact 比旧版更像论文：仍交代独立重建覆盖什么，没有逐字段复述方法。最后一句 `The contrast makes future progress measurable ...` 较空，可用下面一句收束，不再讨论工程验收：

```latex
The measured benefit is recovery from the declared correctness shift;
stationary performance remains a separate test.
```

## 9 pt 与八页的布局判断

新表是双栏宽度、一个方法列加四个短数字列、十个方法行；按当前 9 pt 字号，从结构上适合排入双栏浮动表。没有新增九列宽表或极小字号，表体宽度风险较低。

**主要风险是高度和 float 排队，不是字号。** 当前连续有原 main table、delay figure、credit table、新 arrival table，且 map 的 E1/E2 证据列增加了引用，可能多换行。必须实编译后检查浮动顺序、p8 结尾和 References 起页。本次没有排版或 PDF 测量，不能据此称已满足八页。优先用已经建议的重复正文压缩和浮动位置解决，保留 map、所有 comparator 行和自然负结果。

本报告只说明文字/公式/表格来源的一致性；全实验独立审计和投稿科学验收均不由这份写作审查代替。
