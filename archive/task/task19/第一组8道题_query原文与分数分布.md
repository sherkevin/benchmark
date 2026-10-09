# 第一组 8 道题：完整 query 原文 + 分数分布

> 4 个 case × 2 侧（fixed / open）= **8 道独立题**。
> census_01 / census_02 已删（零 / 浅 infer，测的是"会用工具"而非"会科学推理"，owner 2026-08-19 定）。
>
> 数据口径：只用**当前题面**的 run（`valid=1`）且判分正常（`status=ok`）。
> 过时批次（pool24 / VOID / unusable / turn40 等）已排除——混算会伪造结论，
> 此前因此把 `satisfy_02` 的 0.730 误报成 0.108。

---

## 一、8 道题总表

| # | 题 | 侧 | 测什么（infer / acquire） | tools 量 | tools 清单 | 候选可见性 | 预算 | n | 均值 | sd |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | chain_01 | fixed | #5 趋势外推 + #6 约束推理 + #8 family 归类；#29 | basic | `measure_tc` | 给全清单 | 34 | 283 | **0.796** | 0.215 |
| 2 | chain_01 | open | 同 #1 + 自主发现候选 | basic | `measure_tc` + `search_db` | 不给清单 | 45（两工具共享） | 282 | 0.597 | 0.185 |
| 3 | constraint_02 | fixed | #6 + #8 + #5；#29 | basic | `measure_tc` + `get_element_count` | 给全清单 | 35 | 93 | 0.531 | 0.356 |
| 4 | constraint_02 | open | 同 #3 + 自主发现 | basic | 同上 + `search_db` | 不给清单 | 35 | 255 | 0.210 | 0.226 |
| 5 | satisfy_02 | fixed | #6 + #8 + #5；#29 | basic | `measure_tc` | 给全清单 | 56 | **9** | 0.730 | 0.214 |
| 6 | satisfy_02 | open | 同 #5 + 自主发现 | basic | 同上 + `search_db` | 不给清单 | 56 | 221 | 0.118 | 0.152 |
| 7 | dopant_01 | fixed | #1 元素替换 + #2 比例调整 + #3 掺杂 + #5 外推；#29 | basic | `measure_tc` | 给两条边界曲线 | — | 64 | 0.778 | 0.120 |
| 8 | dopant_01 | open | 同 #7 + 自主发现 | basic | `measure_tc` + `search_db` | 不给清单 | — | **8** | 0.799 | 0.061 |

**tools 量全是 basic**：按 ADR-0026 §3，便捷工具档（some / enough）不预设，
必须从 basic 档跑出来的失败模式里导出。所以 some / enough 属第二阶段。

---

## 二、分数分布图

| 图 | 文件 | 看什么 |
|---|---|---|
| 折线图 | [`figs/eight_queries_lines.png`](figs/eight_queries_lines.png) | 每模型一条线，横轴=该模型第 N 次 run → 看**单模型跨轮稳定性** |
| 直方图 | [`figs/eight_queries_dist.png`](figs/eight_queries_dist.png) | 分数落点密度 → 看**整体分布形状**（单峰/双峰/趴底） |

图存在 `task/task19/figs/`（`src/probe_headless/logs/` 已被 gitignore，
不入库云端看不到，所以另存一份到 task 目录）。

---

## 三、完整 query 原文

### 题 1 — chain_01 · fixed

来源：`runs/*/TASK.md`（workspace 范式，agent 实际读到的原文）。

```
# Task: superconductor Tc study (four dependent steps)

You are a materials scientist with a limited measurement budget. Work **in this
workspace**: keep your running notes in `notes.md`, and write your final answer to
`answer.json`. Use the `measure_tc` tool for every Tc value you cite — never guess
a measured number.

Seed material: `O6.080Ca1Cu2Ba2Hg1`
Definition: a material is **high** if Tc >= 90 K, otherwise **low**.
Measurement budget: 34 `measure_tc` calls.

## Step A — series peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Measure the series members in `data/measurable.txt` and find the composition with
the MAXIMUM Tc. Call that maximum **T***.

## Step B — filter family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32. Among materials that contains Hg and has exactly 4 elements,
report ALL whose Tc >= threshold1. Call this set **Q1**.

## Step C — extrapolate (no measurement possible)
The materials in `data/unmeasurable.txt` have never been synthesized — there is no
physical sample to put in the instrument. `measure_tc` will tell you "no sample
exists" for them. You must **induce a rule** that separates high from low Tc using
the materials you CAN measure, then **predict** high/low for every unmeasurable
material. Warning: single-factor rules (e.g. "contains Hg" or "has 5 elements") are
wrong on several of them — the governing rule is an interaction of multiple factors.

## Step D — final pick
Among the unmeasurable materials you predicted **high** in step C, return the one
with the FEWEST elements (break ties by higher expected Tc).

## Answer format
Write `answer.json` with exactly these keys:
  peak / peak_tc / qualifying / predictions / final
```

### 题 2 — chain_01 · open

与题 1 同，差别在**不给候选清单**，且两个工具共享预算：

```
You have two tools:
- `measure_tc(composition)` — measure Tc of a synthesized sample (costs 1 budget unit)
- `search_db(query, page)` — search the materials database (costs 1 budget unit per page)

**Both tools share the same budget of 45 units total.**

## How to discover materials
You do NOT have a list of all materials. Use `search_db` to find them:
- `search_db("Hg", page=1)` finds materials containing "Hg"
- `search_db("Cu", page=2)` gets the second page of Cu-containing materials
- Each page shows at most 4 results and tells you if more pages exist

Plan your searches carefully — every page costs 1 budget unit.
```

其余四步（A/B/C/D）与 fixed 侧逐字相同。

### 题 3 — constraint_02 · fixed

来源：`ws_runner_constraint02.py` 的 `build_task_md()`（旧 runs 是自写 harness 范式、未存 TASK.md）。

```
# Task: find the highest-Tc superconductor under an element-count constraint

You are a materials scientist working in THIS directory. You have a list of
68 candidate superconductors in `data/candidates.txt`.

## The constraint (hard)

The final answer MUST contain **exactly 4 distinct chemical elements**.
A candidate with any other element count is disqualified no matter how high its Tc.

## Tools

- `get_element_count(composition)` — how many distinct elements a candidate has.
  Free (does not use measurement budget).
- `measure_tc(composition)` — measure a candidate's Tc (K) on the instrument.
  You have a budget of **35 measurements** for 68 candidates, so choose
  wisely. Some candidates **cannot be probed** by this instrument (it returns
  probe_unavailable) — for those you must **infer** Tc from measurable ones in
  the same chemical family.

## Goal

Report the 4-element candidate with the highest Tc — **including candidates
that cannot be measured directly**. If the true optimum is a composition the
instrument cannot probe, you must recognize that from the pattern of the ones
you can measure, and still report it.

## Deliverables in this workspace

1. `notes.md` — your reasoning: which candidates are legal (4-element), what
   you measured, the family trend you inferred, and why you believe the final
   answer is optimal even if you could not measure it directly.
2. `answer.json` — valid JSON, exactly:
   final / legal_set / legal_count / legal_mean_tc /
   measured_max_id / measured_max_tc / reasoning

Write `answer.json` with the Write tool. Every Tc you cite from a measurement
must come from a real `measure_tc` call; inferred values must be justified in notes.md.
```

**这道题的杀手机制**：真最优是 4 元素 Hg-1201（98.0K），**它在不可测集里**。
只靠测量永远拿不到答案——必须从可测的汞系（121–127K，但那些是 5 元素、违约束）
归纳出"Hg-Ba-Cu-O 体系 Tc 高"，再外推到不可测的 4 元素成员。
而全场最高 127K 是 **5 元素陷阱**，只盯 Tc 不看约束的模型会抓它。

### 题 4 — constraint_02 · open

同题 3，不给 `data/candidates.txt`，加 `search_db` 自主发现候选。

### 题 5 — satisfy_02 · fixed

来源：README 的题面定义 + `answer_schema.json`（该题 runner 待写，旧 runs 未存 TASK.md）。

> 报出**完整**满足双约束（恰好 4 元素 **且** Tc ∈ [90,100] K）的候选集合。

任务结构：

```
候选池 65 个，测量预算 56 次。

约束（两条都要满足）：
  1. 恰好 4 种元素
  2. Tc ∈ [90, 100] K

要交的产物（answer.json）：
  satisfying_set          — 完整的满足集（不是任选一个）
  ec_legal_set            — 所有 4 元素候选（33 个）
  excluded_with_tc        — 实测后因 Tc 越界而排除的项（26 个）
  measured_max_legal_id   — 实际测过的最高 Tc 合法项
  measured_max_legal_tc   — 它的实测 Tc
  satisfying_mean_tc      — 满足集的平均 Tc（含须外推的不可测者）
```

**杀手机制**：9 个满足者**全部落在不可测集**（Tc 被工具拒答）。
可测的 4 元素候选（La 系）Tc 仅 22–39K、全部越界，但**必须实测才知道要排除**。
所以流程是：先实测排除 La 系 → 再从可测的 5 元素 Hg-1223 系（121–127K）
归纳"Hg-Ba-Cu-O 体系 Tc 高" → 才敢把不可测的 Hg/Tl-1201 判为满足。

**为何要求交完整集合而非任选一个**：v1（`satisfy_01`）只要交任一满足者，
裸答基线最高 0.673、判别性硬关 5/5 全过 —— 满足集 9 个、蒙中率 100%，
判为"可背"已退役。交完整集合才把"可背"压掉。

### 题 6 — satisfy_02 · open

同题 5，不给清单，加 `search_db` 自主发现。

### 题 7 — dopant_01 · fixed

来源：`runs/*/TASK.md`。

```
# Task: predict the Tc curve of a not-yet-synthesized material family

You are a materials scientist working in THIS directory. You study
`RE(1-x) Pr(x) Ba2 Cu3 O7` cuprates: substituting Pr for the rare earth (RE)
suppresses the critical temperature Tc. How fast Tc falls with Pr content x
depends on which RE is used.

## What you already have

- `data/synthesized.txt` — **19 compositions you have already synthesized**
  (two RE families across a range of Pr). Each has a physical sample, so you can
  measure its real Tc (K) by calling `measure_tc` on it.
- `data/to_predict.txt` — **11 compositions of a THIRD RE family that has NOT
  been synthesized yet.** There is no sample, so `measure_tc` cannot return a value
  for them. You must **predict** each one's Tc.

## Goal

For **every** composition in `data/to_predict.txt`, predict its Tc (K). Measure as
many of the synthesized compositions as you need to pin down how Tc depends on Pr
content and on the RE, then reason out where the un-synthesized family must fall.

## Deliverables in this workspace

1. `notes.md` — your running reasoning: what you measured, the pattern you inferred
   (how Tc falls with Pr, how the RE families compare), and how you extrapolated to
   the un-synthesized family.
2. `answer.json` — valid JSON, exactly:
   predictions（每个待预测组成 → 预测 Tc，数字）/ reasoning

`predictions` MUST contain an entry for **every** composition in
`data/to_predict.txt` (a number in K; use 0 for a composition you believe is no
longer superconducting). Write `answer.json` with the Write tool. Every Tc you cite
from a measurement must come from a real `measure_tc` call.
```

**两条边界曲线的真实物理**（真值 `golden.law` 原文）：

> RE(1-x)Pr(x)Ba2Cu3O7: Tc falls monotonically as Pr content x rises; the suppression
> rate depends on RE. Er suppresses slowest, Nd fastest, so every RE curve is bracketed
> by the Er (upper) and Nd (lower) curves. Gd lies strictly between them, so its whole
> curve can be interpolated from Er and Nd.

**为何这道题最纯**：目标（整条 Gd 系 11 个点）**根本没合成**，
`measure_tc` 如实返回 `sample_available: false` —— 科学上唯一站得住的
"测不到"理由是没有实物。没有任何绕过推理的捷径。

**为何覆盖最宽**：这道题有**两个变化轴**，各对应一个 action ——
RE 元素轴（Er/Gd/Nd 同族替换 = #1）× Pr 比例轴（13 个比例点 = #2）。
效应量级实在：同一 Pr=0.30 处，Er 是 61.1K、Nd 是 5.6K，差 55.5K。

### 题 8 — dopant_01 · open

同题 7，不给清单，两条边界曲线也要自己用 `search_db` 发现。

---

## 四、折线图暴露的三个问题（直方图看不出）

| 题 | 折线特征 | 判断 |
|---|---|---|
| #3 constraint_02·fixed | 单模型的线在 **0 与 1 之间反复跳** | 轮间波动极大（批间极差 0.721），稳定性不合格 |
| #4 constraint_02·open | 开局高，**逐轮 collapse 到 0.1–0.25** | 后期批次系统性变差，需查是否题面/池子变过 |
| #7 dopant_01·fixed | 所有线**挤在 0.75–0.85 窄带** | 稳，但模型间几乎不分高下（区分度不足） |

对照 #1 chain_01·fixed：多数模型稳在 0.8–1.0，只有 `DeepSeek-V3-671B` 频繁掉 0 ——
**强模型稳、弱模型断链**，这才是想要的形状。

---

## 五、数据缺口（必须补跑才能下结论）

| 题 | n | 问题 |
|---|---|---|
| #5 satisfy_02·fixed | **9** | 样本严重不足，0.730 这个均值不可靠 |
| #8 dopant_01·open | **8** | 单批次，连不成趋势；尺子② 目前反向（−0.020）但不能定论 |
| #3 constraint_02·fixed | 93 | 够量但批间极差 0.721，需第二独立批次确认 |

跑批已移交云端（本机 claude 被内网诊断器 SIGKILL）。
协同口径见 `task/task20-跨机协同/`。

---

## 六、action 覆盖

| action | 覆盖 | 由谁测 |
|---|---|---|
| #1 元素替换 | ✅ | dopant_01（RE 轴 Er/Gd/Nd 同族） |
| #2 比例调整 | ✅ | dopant_01（Pr 轴 13 个点） |
| #3 掺杂 | ✅ | dopant_01 |
| #5 同族 Tc 趋势外推 | ✅ | 四道全部 |
| #6 element_count 约束推理 | ✅ | chain_01 / constraint_02 / satisfy_02 |
| #8 family 归类判断 | ✅ | chain_01 / constraint_02 / satisfy_02 |
| #4 晶系 / 层型变 | ❌ | 缺（`Tl-Ba2` 有 4 种层型但 n=3 仅 1 个样本，单点预测抛硬币） |
| #7 制备条件-超导关联 | ❌ | 缺（真值无压力字段；且真实数据 H3S/LaH10/YH9 恰是最出名的，直接问就是记忆题） |

**8 个覆盖 6 个**，缺 #4、#7，均卡在后端真值库缺数据（需扩库，属 `goal.md` 步 1 + 步 4）。
