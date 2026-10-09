# task6 · 调研补充 + benchmark 解读卡 + checkpoint 设计(索引)

> task6 = 补 task4 三个缺口(材料 / 药物过程 / 2025–26 新发),据此定出**我们 checkpoint 怎么分类、怎么造 case**,并把每个 benchmark 沉淀成解读卡。

## 文档关系图(每文件唯一职责,不重复)

```
┌─────────────────────────────────────────────────────────────────┐
│  task6/  (顶层 6 份,各有唯一职责)                              │
├─────────────────────────────────────────────────────────────────┤
│  README.md            ← 本页:导航 + 关系图                     │
│  goal.md              ← 任务章程(由来/目标/方法/状态,档案)    │
│                                                                 │
│  result.md            ← 给项目组的对外成品                      │
│                         (状态 + 6 发现 + 资产 + 差异化 + 路线图)│
│         │                                                       │
│         ├──→ benchmark-cards-index.md  ────┐                    │
│         │       (符号链接 → ~/papers)      │                    │
│         │                                  ▼                    │
│         │                  ┌───────────────────────────────┐    │
│         │                  │  ~/papers/  (canonical 卡库)  │    │
│         │                  │  · 40 张 benchmark 解读卡     │    │
│         │                  │  · 每张:数据格式+样例+改造   │    │
│         │                  │  · 6 份 audit_*.md 源码亲核   │    │
│         │                  │  · 索引 Contents/Agent/       │    │
│         │                  │    AI4S-Benchmark.md          │    │
│         │                  └───────────────────────────────┘    │
│         │                                  ▲                    │
│         └──→ checkpoint_palette.md  ───────┘                    │
│                 (cp 三档分类 + 三步造 trace,方法论轴)          │
│                                                                 │
│  archive/             ← 原始调研 + 已完成 handoff 存档          │
└─────────────────────────────────────────────────────────────────┘
```

**核心规则**:
- **per-benchmark 详情(数据格式/样例/改造)= 唯一住卡里**(~/papers,canonical)。result / palette 都只**指向**卡,不重复展开。
- **两份项目文档分两轴**:`result.md`=本任务对外成品(汇报+差异化分析,按"我们怎么用"组织)· `palette`=按"cp 类型"组织的方法论。互不重叠。
- **历史合并**:原 `inventory.md` + `checkpoint_precedents.md` 已合并入 `result.md`(2026-06-10)。

## ⭐ benchmark 解读卡住文献库

- **40 张卡**(`tags: AI4S-benchmark`,医疗类加 `medical-benchmark`)+ **6 份源码级 audit**(`type: report`),全在 `~/papers/`。
- **分类总入口**:[`benchmark-cards-index.md`](benchmark-cards-index.md) —— 主分类表(每行 = 领域 / 验证什么 / 对我们 R·B·seed·baseline / 亲核 / →卡)+ dataview 自动列。⚠️ **用 Obsidian 打开**(VSCode 不渲染 wikilink/dataview)。
- 命令行捞:`python3 ~/papers/scripts/pq.py --tag AI4S-benchmark --paths`。

## 先读哪份(按角色)

| 你是谁 / 想要什么 | 看这份 |
|---|---|
| **想要某 benchmark 的解读 / 数据格式 / 改造方法** | [`benchmark-cards-index.md`](benchmark-cards-index.md) → 点进对应卡 |
| **想知道结论 + 我们怎么用 + 谁先做过 + 3 首创**(本任务对外成品) | [`result.md`](result.md) |
| **想知道 cp 分几类、怎么串 trace** | [`checkpoint_palette.md`](checkpoint_palette.md) |
| **某 benchmark 源码级亲核细节** | `~/papers/audit_*.md`(经各卡 `[[audit_*]]` 进入) |
| **task6 任务本身的目标/方法/状态** | [`goal.md`](goal.md) |
| **原始调研全文 / handoff 存档** | [`archive/`](archive/) |

## 已知待补(P0 已清空,下一步)

源码级亲核**全部完成**(6 篇 audit + 33 张深卡 ✅)。**下一步 P0 转向**:真实 LLM 实测 + 过程量↔最终答案相关性实验(论文核心实验)。详见 [`result.md`](result.md) §「我们还差什么」。
