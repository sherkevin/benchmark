# case_sc_chain_01_open · 四步依赖链 · **open 配置**

## 〇、当前权威状态（2026-08-17，测量系统修好后的第一份可用数据）

批 A（open h1–h5 vs fixed c1–c5，各 n=30，6 模型 × 5 轮，总并发 4）：

| 验收项 | 结果 | 判定 |
|---|---|---|
| 尺子① open 均值 | **0.541** | ✓ 落 [0.50,0.60] |
| 尺子① open 最强 | **0.679**（deepseek-v3.2）| ✓ < 0.85 |
| 尺子② 差（fixed 0.614 − open 0.541）| 0.073，置换 p=0.0411、Bonf(m=2) 0.0822 | ✗ 需 ≥0.10 |
| 发现有后果 | 不可测材料发现率<0.60 均 **0.335** vs ≥0.60 均 **0.564**，置换 p=0.0096、**Bonf 0.0193** | ✓ |
| 覆盖率分布 | 0.77–1.00（中位 0.89），非恒为 1.0 | 翻页确实花预算 |
| 协议失败（计 0 分）/ api_error 剔除 | 0/30 / 0 | — |

### 本轮修掉的关键 bug（只打击 open 侧，会虚增尺子②）

`search_db` 在预算耗尽时先给 `out` 赋了"budget exhausted / submit NOW"提示，
**紧接着又被下面那个无条件的 `out = {...}` 整体覆盖**，于是模型只收到一个
"results 为空"的普通响应，永远看不到交卷指令。它一直 search 直到撞上 100 轮上限。
实测证据：`qwen-turbo` 连发 **72 次** `search_db`、覆盖率已达 **1.0**，却从未交卷。

修复前后对照（同一 fixed 批）：

| | 修复前 | 修复后 |
|---|---|---|
| open 均值 | 0.463 | **0.541** |
| qwen-turbo open | 0.112 | **0.384** |
| 尺子② 差 | 0.155（p=0.0001）| **0.073**（p=0.0411）|

即修复前那个"漂亮"的 0.155 里约一半是 bug 贡献的。旧数据已归档为
`runs_searchbug_exhaust_msg_lost`（47 个 run，**不得引用**）。

### 与 fixed 侧的唯一差别

**不给候选清单**。题面只给 1 个 seed，其余 34 个候选靠 `search_db(query, page)` 自己发现，
`PAGE_SIZE=4`、**每翻一页消耗 1 个预算单位**，与 `get_property` 共用同一份预算（45）。
其余一切相同：同一真值、同一 turn 上限（100）、同一预算数值。

**判分器是薄壳**：`judge_chain_open.py` 不含任何自己的判分逻辑，
`validate / score / collect / report` 四者全部 `import` fixed 侧 `judge_ws.py`，
并在模块加载时 `assert` 两侧 golden 真值相同（验收项⑦）。

### 复现

```bash
python src/probe_headless/chain_open_runner.py <model> <round_tag>
cat rounds.txt | xargs -P 4 -L 1 python src/probe_headless/chain_open_runner.py   # 必须 ≤4
python cases/materials/chain/open_basic_tools/case_sc_chain_01/judge_chain_open.py [runs_dir]
python src/probe_headless/stats_chain_pair.py <fixed_runs> <open_runs> <bonferroni_m>
```

每个 run 额外落盘 `discovery.json`（发现了多少候选、其中多少个不可测材料），
是"发现有后果"的轨迹证据来源。

---

## 以下为历史过程记录（结论已被上节取代）

> **旧状态（扩池 6 模型 + v2 题面）**：
> - 尺子① ✓ 两项都过：均值 0.540 ∈[0.50,0.60]、最强 0.746 <0.85
> - 尺子② ✗ 失效：差 0.081、置换 p=0.156 —— 原因是**我引入的 confound**：open 的 turn 上限 120 vs fixed 的 40，
>   交互预算未受控，deepseek 在 open 上（0.746）反高于 fixed（0.663）。该 confound 现已修正（两侧 turn 同为 100）。
> - 裸答 0.000 ✓、真值与判分口径一致 ✓
>
> 历史：3 模型池 + v1 题面时 open 均值 0.426、尺子② p=0.1722；v2（题面告知不可测数量）把 cpD 漏洞堵住、
> 尺子② 一度到 p=0.0178；扩池后尺子①达标但尺子②因 confound 失效。全部跑数分批归档。

**配置**：材料 · chain · **open** · 基本工具（ROADMAP 阶段 2）。

---

## 一、与 fixed 版的唯一差别：候选可见性

| | fixed 版 | **open 版（本 case）** |
|---|---|---|
| MEASURABLE 清单（29 个）| 题面列出 | **不给** |
| UNMEASURABLE 清单（10 个，step C 目标）| 题面列出 | **不给**——须靠 `get_property` 返回 "not probed yet" 自行辨识 |
| 发现方式 | 无需发现 | `search_db(query, page)`，每页最多 4 个 |
| 交互轮次 | 40 | 120 |
| 真值 / 判分 `score` | `case9.json` / `judge.py` | **同一份 / 代码级复用** |
| 判分 `validate` | 要求 predictions 覆盖全部 10 个 | **放宽为非空 dict**（题面不给清单，就不能要求提交完整清单；缺失的预测由 score 按未命中计 0）|

## 二、实测结果

### 最新：扩池 6 模型 × 预定 n=20（v2 题面）

| 模型 | open | n | 协议失败 | fixed |
|---|---|---|---|---|
| deepseek-v3.2 | **0.746** | 14 | 6 | 0.663 ⚠️open反高 |
| qwen3.7-max | 0.639 | 18 | 2 | 0.639 |
| gpt-4.1 | 0.599 | 20 | 0 | 0.674 |
| qwen3.5-plus | 0.577 | 11 | 9 | 0.639 |
| qwen3-coder-flash | 0.517 | 20 | 0 | 0.605 |
| qwen-turbo | 0.163 | 2 | 18 | 0.508 |

尺子① 均值 **0.540 ✓**、最强 **0.746 ✓**；尺子② 差 0.081、p=0.156 ✗（见顶部 confound 说明）。

### 历史：3 模型 × n=20（用于对比）

| 模型 | fixed 均值 | open 均值 | 差 | cpA | cpB | cpC | cpD |
|---|---|---|---|---|---|---|---|
| gpt-4.1 | 0.674 (n=20) | **0.714** (n=20) | **−0.040 ⚠️反转** | 1.000 | 0.847 | 0.260 | **0.750** |
| qwen3-coder-flash | 0.605 (n=18) | 0.490 (n=20) | 0.115 | 0.900 | 0.571 | 0.240 | 0.250 |
| qwen-turbo | 0.508 (n=7) | 0.064 (n=3) | 0.444 | 0.000 | 0.222 | 0.033 | 0.000 |

- 尺子① open 均值 **0.423** ✗（带 [0.50,0.60]）；最强 0.714 ✓ <0.85；turbo 协议失败 **17/20**
- 尺子② case 层面差 0.173 ✓ 但 **run 级置换 p=0.1722、Bonferroni 0.3444 ✗ 不显著**
- 硬关分档（各关只有单个边界显著）：cpB 中↔强 Bonf **0.0002**★；cpC 弱↔中 Bonf **0.0147**★；cpD 中↔强 Bonf **0.0074**★

## 三、v1 的根因：step D 的结构**奖励发现不全**（核心教训，v2 已堵住）

> **v2 修法已验证**：题面加一句“库中恰有 10 个不可测材料，少报的算错”（仍不给清单、仍属“只改题面”）：
>
> | | v1（不告知数量）| **v2（告知数量=10）** |
> |---|---|---|
> | gpt fixed vs open | 0.674 vs **0.714**（反转）| 0.674 vs 0.606（**+0.069** ✓）|
> | 尺子② 置换 p | 0.1722 ✗ | **0.0178**（Bonf 0.0356）★ |
> | cpD（漏洞指标）| **0.750**（被奖励）| **0.176**（漏洞已堵）|
> | 尺子① 均值 | 0.423 ✗ | 0.429 ✗ 仍不达 |
>
> 机制：知道要报 10 个后，model 会把未发现的也猜进去 → step D 候选池不再是“干净的 6 个” → cpD 从 0.750 降到 0.176。
> 但发现能力本身未提升（仍只找到 6/10，分布与 v1 几乎相同）——告知数量不等于给搜索线索，
> 所以尺子①仍不达标。v1 跑数归档 `runs_v1_no_count_hint/`。

### 以下为 v1 的根因分析（保留）

`step D` = "在你 step C 预测为 high 的不可测材料中，选元素数最少的"。
open 下 agent 平均只找到 **6/10** 个不可测材料（37/41 个 run 都是 6 个，无一找全）。后果是：

| 关 | 发现不全的影响 | 实测（gpt open vs fixed）|
|---|---|---|
| `cpC`（预测命中数/10）| **有害**——漏掉的算错 | 0.260（低）|
| `cpD`（终选是否命中 golden）| **有利**——候选池从 10 缩到 6，更容易选对 | **0.750 vs fixed 的 0.350** |

两者方向相反、互相抵消，所以总分反而升。证据链完整：

- golden `stepD_final` = `O4.180Cu1Ba2Hg1`（Hg 系 4 元素）——**搜 'Hg' 即可发现**；
- 41 个有效 run 中 **38 个（93%）都发现了 golden**；
- 于是"漏掉的 4 个"从不影响 cpD，只减少干扰项 → 发现不全被奖励。

这也解释了 goal② 的「发现有后果」检验为何在本 case 不成立：
发现不全 n=41 均分 **0.566** vs 发现全 n=2 均分 0.528，置换 **p=0.86**（无差异）。
（对照：satisfy_02_open p<0.0001、constraint_02_open p=0.0008，两者都成立。）

## 四、替代设计（未实施，供后续）

要让 chain 类任务的 open 版成立，必须让"漏发现"在下游关上受罚。三条候选方案：

1. **终选关改为"全库最优"判据**：`step D` 判"你选的是否等于**全部** 10 个不可测材料中的真最优"（现口径已是如此，问题在 golden 太易发现）→ 应把 golden 放在**难发现**的材料上（例如只出现在冷门元素组合里），使漏发现直接导致选错。**注意**：这需要改 golden，即改真值，不属于"只改题面"，必须作为新 case 立项。
2. **增设"发现完整性"关**：新增 `cpE = |found ∩ held_out| / 10` 并计入总分。这会改判分口径，破坏与 fixed 版的可比性，因此只能在 fixed 版就一并引入。
3. **题面明确给出不可测材料的数量**（如"库中有 10 个不可测材料"）但不给清单：这仍属"只改题面"，且能让"少报"变成明显失分（模型知道自己漏了）。**这是成本最低、最符合 goal① 的方案，建议下一步先试它。**

## 五、尺子①不达标的精确根因（已定量）

**不是题难，是弱档模型选错。** 证据两条：

### 1) 剔除 turbo 后均值落带

| 模型池 | open 均值 | 尺子①[0.50,0.60] |
|---|---|---|
| 全池（turbo/flash/gpt）| 0.426 | ✗ |
| **剔除 turbo（flash/gpt）** | **0.558** | **✓ 落带** |

说明题目难度本身合适，turbo 一个模型把池均值拖出了带。

### 2) turbo 的 20 轮失败构成（逐条查 ledger）

| 失败模式 | 轮数 | 性质 |
|---|---|---|
| **从未调用 submit_answer**（ledger 里 0 条）| **12** | 能力问题——做不完就放弃 |
| API 层失败（连 ledger 都未生成）| 6 | 环境问题 |
| 正常完成 | 2 | — |

已排除判分 bug：逐条比对 ledger 确认那 12 轮确实没有 submit_answer 记录（不是读取格式不匹配）；
并已把该失败模式的报错从误导性的 `missing_peak` 改为 `never_submitted`。

### 下一步可执行修法

用一个**能完成 open chain 的弱档模型**替掉 turbo（turbo 在本配置下 90% 交不了卷，不具区分价值）。
代价：chain fixed 版现有模型中没有介于 turbo 与 flash 之间且有 n=20 数据的模型，引入新模型需重跑
fixed 版（而 fixed 版 chain_01 是已定稿的第一组标杆，改它会动已发布结论）——故此修法需单独立项。

---

## 六、其余验收项

| 项 | 证据 | 判定 |
|---|---|---|
| 答案不可背（goal⑤）| 裸答 k=5 **全部 0.000** | ✓ |
| 真值与判分口径一致（goal⑦）| GT/case9 复制自 fixed 版；`score`/`get_submission` 代码级 import；仅 `validate` 按题面差异放宽并已在代码注释说明 | ✓ |
| 预定轮次 | 开跑前定死 r1–r20 | ✓ |
| 尺子① / 尺子② / 发现有后果 | 见 §二、§三 | ✗ |

## 七、复现

```bash
python src/probe_headless/sweep_chain_open.py
python cases/materials/chain/open_basic_tools/case_sc_chain_01/judge_chain_open.py
```

## 八、文件清单

- `artifacts/case9_open.json` — harness 真值（复制自 `src/probe_headless/case9.json`）
- `artifacts/ground_truth.json` — judge 真值（复制自 fixed 版）
- `judge_chain_open.py` — 判分薄壳（复用 `score`，放宽 `validate`）
- `runs/` — 60 个 workspace，含 `discovery.json`（记录发现覆盖率与找到的不可测材料）
- `src/probe_headless/chain_open_runner.py` — open harness
- `src/probe_headless/sweep_chain_open.py` — 预定轮次批跑
