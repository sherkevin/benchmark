# 汇报幻灯片

## science-agent-benchmark-survey.html

面向长程任务（特别是 Science 领域）的 agent benchmark 调研，用于学术汇报 / 组会。

| 项 | 值 |
|---|---|
| 页数 | 28 |
| 主题 | `academic-paper`（Latin Modern Roman + 论文纸米白底 + 零圆角零阴影） |
| 讲稿 | 28 条，写在每页 `<div class="notes">` 里 |
| 文献 | 35 个不重复 arXiv 编号，全部可核验 |

### 怎么放

```bash
cd slides && python3 -m http.server 8899
# 浏览器打开 http://localhost:8899/science-agent-benchmark-survey.html
```

| 按键 | 作用 |
|---|---|
| `→` / `←` | 翻页 |
| **`S`** | **演讲者模式**（当前页 + 下一页预览 + 逐字稿 + 计时器，双窗口同步） |
| `T` | 切主题（可切 minimal-white / editorial-serif） |
| `F` / `O` | 全屏 / 总览 |

### 内容结构

1. **问题定位** — Agent 基准与 LLM 基准的七点差异 · 三组长程稳定性实证 · 科学域的五条特有约束
2. **现状全景** — 九个门类 · 按「判分硬度 × 判分位置」的四格切面 · 榜单生态
3. **三个核心难点** — LLM 裁判失灵（含 ChemCrow 原文证据）· 中间状态无标签 · 专家标注不可规模化
4. **八种构建范式** — 金标准来源 × 判分方式 × 硬度分级总表
5. **逐范式代表工作** — SciCode / SUPER / RE-Bench / Tartarus+TDC / MADE / DiscoveryBench+BiomniBench，含实现流程与架构
6. **结论** — 硬卡点的三条工程路线 · 四条正交指标 · 一处结构性空白

### 素材来源

内容取自本仓库 `research/` 下的调研文档（`science_agent_benchmark.md`、`长程任务Benchmark.md` 等），
数据与结论均附 arXiv 编号，未使用未经核验的信息。

### 资产

`assets/` 来自 [lewislulu/html-ppt-skill](https://github.com/lewislulu/html-ppt-skill)（7.7k star），
含 36 个主题、27 个 CSS 动画、演讲者模式运行时。纯静态，无构建步骤。
