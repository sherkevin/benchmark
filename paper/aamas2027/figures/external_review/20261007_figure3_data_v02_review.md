# Figure 3 程序数据图 v02 审查

- 类型：折线图 + 分组条形图 + 成本散点图；matplotlib 绘制
- 候选：`figure3/v02_data/figure3_v02.pdf`（7×2.8 inches）
- 数据：`generated/figure3/data_binding_v1.json`
- PDF SHA-256：`a072f7f81115fad46d1bc7cc4c14cea0d44f52151fb1aad563c8aac96238b5fc`
- PNG SHA-256：`743af9509086527f057150a5da03bc27543d6b66e6dc82dcf3b299f5a0c4c013`

## 检查结果

| 检查 | 证据 | 结果 |
|---|---|---|
| 输入来源 | 三份 source hash 和 binding hash 与当前结果文件一致 | 通过 |
| 曲线坐标 | Matplotlib line artist 的 6 个 x/y 与输入逐项相等 | 通过 |
| 条形长度 | 10 根 bar artist 的宽度与 rescued/harmed 计数逐项相等 | 通过 |
| 散点坐标 | 5 个 scatter artist 的原始 offset 与成本数据逐项相等 | 通过 |
| 口径/单位 | natural 与 controlled 区分；paired count；µs/KiB；log 坐标明示 | 通过 |
| 矢量/字体 | 单页 PDF；TrueType 字体全部嵌入；另有 SVG | 通过 |
| 视觉缩放 | PDF 在原生 7-inch 图宽渲染后检查，标题、方法名、柱标、单位可读 | 通过 |

v01 的标题贴近上边界，保留为旧版。v02 增大上边距，并用纹理区分 harmed
柱，字体/单位不裁切。成本上两种接近的 baseline 保持真实坐标，通过方法
标记和引导线定位；没有用 jitter 人为改变数据位置。

## 结论边界

此次只是重绘已有结果，不执行新实验。Panel (a) 为按延迟配对的均值；
绑定清单没有逐延迟已核验的 scaffold interval，因此不添加误差线。
Panel (b) 为同收据 Frozen action 下的 rescued/harmed 计数；Panel (c)
为 stream p95 的均值与最大 serialized state 的均值，不是 pooled p95、
provider inference 或端到端 latency。

程序版是当前数据图候选。先前 AI v04/v05 的 16/16 是视觉自检，不能证明
生成坐标精确；这些评分不再用作当前数据图验收依据。当前重绘的合格也
不代表正文、投稿政策和整稿页数已经验收。
