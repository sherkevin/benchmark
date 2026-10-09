# task9 · 目标会议对标（ICLR/ICRA/AAMAS/WWW）+ reference 论文库 + 发现类 benchmark 构造方案

> 上游：owner 2026-07-31 指令——"看看 ICRA、ICLR、AAMAS、WWW 历年都是写的什么论文，找和我们相关的论文，放在 reference（datasets 改名为 reference）下，每个论文写一个文件夹，里面存储原始 pdf 和对该 pdf 的阅读报告，文件名前面加会议名字前缀。相关 = Long-Horizon Benchmark + 在 Bench 上验证 Agentic RL 算法。……另外现在关于探索发现方向的 benchmark 我其实没想好怎么做，现在有的只是反向构造一条 trace，这个也需要你好好想想。"

## 1. 目标

一句话：**把"投哪、按什么调性写、拿什么当对照、发现类任务怎么造"四件事一次性钉死，并把证据落成本地论文库。**

四个可勾验的产出：

1. **会议画像**：ICLR / ICRA / AAMAS / WWW 历年在「long-horizon agent benchmark」与「在 bench 上验证 agentic RL」两条线上到底收什么，收多少，什么形态。
2. **本地论文库 `reference/`**：原 `datasets/` 改名为 `reference/`；每篇相关论文一个文件夹 `<CONF>-<slug>/`，内含原始 PDF + 阅读报告。阅读报告必答三问：**① 数据量多大 ② 数据集长什么样 ③ 每个任务的"长"体现在哪**。
3. **投稿策略**：本项目「双任务 benchmark（checkpoint 型 + 发现型）+ agentic RL 优化」的组合是否合 ICLR 调性；以及 **RL 做不完时的降级方案**（只出 benchmark 是否够投）。
4. **发现类任务构造方案**：把"长上下文埋线索 → agent 大海捞针 → 推出合理医药发现结论"从一句想法变成可判分的构造法；直面当前"只有反向造一条 trace"的不足。

**判定完成的标准**：`reference/` 下每个论文文件夹都有真实 PDF（非占位）+ 三问齐全的阅读报告；`research/` 下有一份跨论文横向对比表 + 一份投稿策略与调性对标结论 + 一份发现类构造方案（含判分伪代码）。

## 2. 子任务

| # | 子任务 | 状态 | 交付物 | Todo |
|---|---|---|---|---|
| 1 | `datasets/` → `reference/`，全仓引用与规范文档同步改名 | ✅ | `reference/` + `ACTION_RULES.md` §2 + `CLAUDE.md` + `README.md` + ADR | T-24 |
| 2 | DBLP 按 (会议 × 关键词) 全量召回，只信 venue/year 元数据 | ✅ | `src/data_download/dblp_venue_scan.py` + 召回清单 | T-25 |
| 3 | 人工精筛出与本项目同赛道的论文，逐篇下载 PDF | ✅ | `reference/<CONF>-<slug>/*.pdf` | T-26 |
| 4 | 逐篇写阅读报告（三问 + 对我方的可借/可打点） | ✅ | `reference/<CONF>-<slug>/阅读报告.md` | T-27 |
| 5 | 横向对比表（数据量 / 形态 / 长程来源 / 判分粒度 / 有无 RL） | ✅ | `目标会议长程benchmark横评.md` | T-28 |
| 6 | 投稿策略与调性对标（含 RL 砍掉后的降级方案） | ✅ | `投稿策略_ICLR调性对标.md` | T-29 |
| 7 | 发现类（大海捞针型）benchmark 构造方案 | ✅ | `发现类任务构造方案.md` | T-30 |

## 3. 不做什么（边界）

- ❌ **不凭记忆断言某篇论文中了哪个会**：venue/year 一律以 DBLP / 官方 proceedings 为准，写进报告的每条元数据都要可回链（对齐 ACTION_RULES §4）。
- ❌ **不放占位 PDF**：下不到正式版就下 arXiv 版并在报告里注明"arXiv 版，会议版未开放下载"，不假装有。
- ❌ **不因为对标别人就改红线**：GOAL.md 五条红线（尤其"不出只看终答的退化版"、"不用 LLM judge 硬判"）在本 task 内不可协商。
- ❌ **不把 `reference/` 变成分析区**：跨论文的分析、结论、策略落 `research/`；`reference/<论文>/` 只放该论文的 PDF 与单篇阅读报告。

## 4. 已知风险

1. **改名波及面**：原 `datasets/` 被 7 份 research 报告、多个 case JSON、ADR-0007、ACTION_RULES 引用，漏改会让调研结论断链。
2. **ICLR/ICRA 的 PDF 可及性**：ICRA 走 IEEE Xplore（付费），大概率只能拿 arXiv 预印本；AAMAS 走 IFAAMAS 官网（开放）；WWW 走 ACM DL（部分开放）。
3. **调性判断有主观性**：结论必须建立在"该会议确实收过同类论文"的实例上，不能只凭 CFP 措辞。
