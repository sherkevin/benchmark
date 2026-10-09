# 正式正文候选 2：实验图表视觉审查

日期：2026-10-08。候选 PDF：`paper/aamas2027/build/submission_20261008_092630/main.pdf`。

实际渲染查看 p5–p8，图片保存在 `reviews/formal_candidate2_views/page-05.png` 至 `page-08.png`；另用 PDF metadata 和 p9 文本核对页数。只写本报告，没有改源码、数字或其他文件。

## 独立 gate

| Gate | 结果 | 理由 |
|---|---|---|
| 实验图表视觉与可读性 | **PASS** | 六张结果表和 delay 图均完整可读；没有硬裁切、数字重叠、图例压线或缩字塞列。 |
| 页数 | **FAIL** | 总计 10 页。p9 仍有六行 Conclusion 正文，References 尚未从 p9 开始，因此正文超过 8 页。 |
| 提升为论文主版 | **阻止** | 不能以图表视觉 PASS 代替页数 gate。压缩重复 Artifact 段后应重新构建并查看 p8/p9。 |

## p5–p8 逐项观察

| 位置 | 判定 | 观察 |
|---|---|---|
| p5，Table 2 实验地图 | PASS | 三列简短导航；E1–E6 各指向对应结果表，E1 另外指向 delay 图。列宽足够，无 v02 长句矩阵拥挤。 |
| p6，Table 3 主比较 | PASS | 自然条件一个数值列，受控条件 success/gain/CI 三列；anchors、online controls、MIRA、evaluator-only 各有稳定行组。行名、数字、CI 可直接横向读。 |
| p6，Figure 3 delay | PASS | 双 panel、共享图例、颜色/线型/marker 一致；零基线可辨。自然与受控采用各自 y 范围，标题和刻度明确；caption 保留 descriptive/no uncertainty 口径。 |
| p6，Table 4 机制 | PASS | 三列自然宽度；Local/Frozen/permutation 三个对照可见。notes 明确等权 scaffold estimand 与 Table 3 的 pooled-call point estimate 不同。 |
| p7，Table 5/6 开发诊断 | PASS | 实际位于左栏上下，E3/E4 没有强制并排。短数值表与 2×3 multirow 因子表分别清楚；seed0/1 的 `/` 说明完整。 |
| p7，Table 7 成本 | PASS | 右栏自然宽度足以容纳四列；不含恒定 calls 列或整列 TBD。表注保留 mean of 30 stream-level p95、设备、I/O/provider 排除项和内存未测。 |
| p8，Table 8 post-call | PASS | 四列紧凑；coverage、accepted accuracy、utility 区分清楚。变量 W/R/C/N、785 records/calls、evaluator conditional null 与 reused holdout 说明完整。 |
| 字号、宽度 | PASS | v05 的 9 pt 表字与 8.5 pt 注释在正式双栏中仍可读；主表自然宽度居中，单栏表无需 resizebox。 |
| 边缘与浮动 | PASS | p5–p8 图表均在版心内，页尾正文没有被表格切开。log 有 Underfull 提示，未观察到相关的实际裁切；无 Overfull 匹配。 |

## 数值与口径抽核

- Table 3 保留自然 MIRA/Frozen 89.81% 和受控 87.56% / 73.38%；受控 gain +14.18 pp、CI [+9.61,+19.03]，与 v05 相同。自然 0.00 pp、CI [-1.24,+1.40] 保留在对应正文。
- Table 4 的受控 Local–Permutation 为 +15.67 pp，CI [+10.65,+20.74]；没有因简化表宽混入 Table 3 的 estimand。
- Table 5/6 的 development cohort 仍为 722，不与 157 confirmation 组成新总 N。
- Table 7 明确 CPU policy overhead；没有改叫端到端服务延迟或 pooled p95。
- Table 8 仍为既有 157 分子的次级分析，置信拒答 84.46% coverage、94.57% accepted accuracy；random 对照 CI 在正文为 [+2.77,+9.29]，无新 holdout 声明。

当前没有需要 worker 重新设计或改数字的图表必修项。此次候选的提升阻断点是正文第 9 页溢出；最终版本应维持上述图表尺寸，并核查压缩 Artifact 后没有把 Table 8 或其定义推离对应段落。
