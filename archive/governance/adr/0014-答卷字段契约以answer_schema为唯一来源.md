# ADR 0014 · 答卷字段契约以 answer_schema.json 为唯一来源；E3 须先落双解析

| 字段 | 值 |
|---|---|
| **状态** | Accepted |
| **日期** | 2026-08-06 |
| **决议人** | 监工（跨线冲突裁定，报 owner 备案） |
| **领域** | 接口层契约（答卷字段归属） |
| **关联** | 收敛 [[0011]] 的 `screened_pool`/`ranked_candidates` · 落实 [[0008]] 的全局答卷 schema · 裁定 E2（M7）与 E3（answer_schema）的字段冲突 |

## 背景：两条并行线在同一天冻结了两套字段名

2026-08-06，E2 与 E3 各自交付并冻结了涉及同一对象（agent 答卷字段）的契约，**两者不一致**：

| 来源 | 主字段 | 性质 |
|---|---|---|
| **E3** `dag_poc/answer_schema.json` | `candidates` / `ranking` / `answer` | 全局答卷 schema，声明"全部 6 个答卷侧判分符号均从这些字段派生"，并把 `screened_pool`/`ranked_candidates` 标为"**单向机械派生的旧字段，收敛后引擎不再维护双解析分支**" |
| **E2** 规范 §2.1 **M7**（MUST） | `screened_pool` / `ranked_candidates` | 规定题面 D 段必须逐字点名这两个字段，E5 将按此撰写 150 道题面 |

**第三个事实决定了裁定方向**：`src/benchmark/evaluate.py` L140–141 **当前只读旧字段**：

```
response_json.get('candidate_result', {}).get('ranked_candidates', [])
response_json.get('candidate_result', {}).get('screened_pool', [])
```

即：**新 schema 尚未落到判分代码**。E2 的 M7 与运行中的判分器是一致的，E2 不算写错；E3 的新 schema 是设计目标，但还没有任何代码读它。

**若不裁定的后果**：E5 若按 M7 写完 150 道题面，E3 收敛后题面点名的字段在契约里已不存在，150 道题面全部返工；反之若 E5 按新字段写，当前 evaluate.py 读不到，判分为 0。

## 决策

### 1. 最终来源：`dag_poc/answer_schema.json` 为唯一字段来源

理由三条：
1. 它是 ADR-0008 要求的**全局答卷 schema**（跨 template 共用，不住在单个 template 里）；
2. 它能**派生全部 6 个答卷侧判分符号**，而旧字段只覆盖候选池与合格集两项，无法表达 open 类的 `anchor_molecules`/`cited_values`/`edit_op`；
3. 架构上单一来源优于两套并存。

### 2. 但 E3 必须先落"双解析"，收敛不得早于代码

**E3 的前置义务（阻塞项）**：在 `evaluate.py` 落实新字段解析，并保留旧字段作 fallback：

- 优先读 `candidates`/`ranking`/`answer`；
- 读不到时回落到 `candidate_result.screened_pool`/`ranked_candidates`；
- 两条路径都要有单测覆盖。

**理由**：现有 37 个 case 与 63 份历史 trace 全部使用旧字段。**不做双解析就直接收敛，等于一次性废掉全部既有资产。**

### 3. 冻结与切换时序（谁在什么时点按什么写）

| 时点 | E5 题面按什么写 | 判分器状态 |
|---|---|---|
| **现在 → E3 双解析落地前** | **按新字段写**（`candidates`/`ranking`/`answer`），open 类另加三字段 | 只读旧字段——**此期间不要实跑判分**，只做题面与 DAG 设计 |
| **E3 双解析落地后** | 同上 | 新旧都能读，新 case 与旧 case 均可判 |
| **旧 case 全部迁移完毕后** | 同上 | E3 可移除 fallback，完成收敛 |

**E2 的 M7 须改为**："D 段字段名以 `dag_poc/answer_schema.json` 为唯一来源"，并列出 fixed / open 各自的完整必点字段集（含 open 类的 `candidates`/`ranking` 不得漏点——按 M4"判分器读什么就写什么"的自身口径，漏点即违规）。

### 4. 此类冲突的通用规则（防再犯）

**同一对象的契约只能有一个所有者。** 答卷字段的所有者是 **E3**（接口层）；E2/E5 只能引用，不得另定。

**冻结前置检查**：任何线在冻结一份契约前，须由监工核对是否与其他线同期交付的契约冲突。**本次冲突正是因为监工只核了各线内部自洽、未做跨线核对而漏掉。**

## 影响

| 对象 | 动作 |
|---|---|
| **E2** | 改 M7 与 §5.3，改为引用 `answer_schema.json`；补 open 类漏点的 `candidates`/`ranking`（列入第三轮补派 T-73） |
| **E3** | **新增阻塞项**：`evaluate.py` 落双解析 + 单测；收敛不得早于 E5 迁移完成 |
| **E5** | 题面按新字段写；**在 E3 双解析落地前不实跑判分**，只做题面与 DAG |
| **E6** | 跑测管道须兼容双字段，直到收敛完成 |
| ADR-0011 | 不废止，但其 `screened_pool`/`ranked_candidates` 降为**派生字段**，不再是题面点名对象 |

## 备案

本裁定由监工做出以解除并行阻塞，**报 owner 备案**；若 owner 认为应以旧字段为准（保既有资产、推迟架构收敛），则本 ADR 需改为 Superseded 并反向调整 E3 的 schema。

---

## 修订 R1（2026-08-06，owner 决议）：fallback 不移除，改为独立 legacy_adapter

**原 §2/§3 的"旧 case 全部迁移完毕后 E3 可移除 fallback"作废**，理由是独立审查指出原方案有两处推理错误：

| 原理由 | 实测纠正 |
|---|---|
| "只有 4 个 case 含旧字段，迁移成本低" | 含旧字段的确 4 个，**但缺新字段的是 37/37**（`query_lint` M7 41 条 = 37 缺新字段 + 4 含旧字段字样；M4 亦 37/37 全败）。33 个 `fixed_discovery` case 的题面是中文口语，**连字段名概念都没有** → 迁移不是改字段名，是题面全文重写 |
| "50 份含旧字段的 trace 本来就要因判据修复而重跑" | **混淆了「重跑」与「重判」**。判分器是纯函数、trace 是原始录像，判据改了应当**重判（re-score）**而不必重跑 agent；而重判恰恰要求能解析历史 response。移除 fallback 会让 58 份含旧字段的 `_response.json` **永久不可重判**，把"本来要付的成本"变成"永久丢失的资产" |

**另两处破坏**：① `tests/test_answer_dual_parse.py` 有 6 个测试专测旧路径（本 ADR §2 自己要求的"两条路径都要有单测"），删分支等于放弃对"模型自由输出旧字段"的容错——**agent 输出不受我们控制，它吐旧字段时记 0 分不是能力差异**；② 改写 `_response.json` 里的字段名等于**篡改模型实际输出的记录**，审稿人角度是伪造实验记录。

### 修订后的做法（架构收敛与可复现兼得）

| # | 规定 |
|---|---|
| 1 | **`evaluate.py` 主路径只读新契约**（`candidates`/`ranking`/`answer`）——架构收敛，双解析分支从判分器消失 |
| 2 | 旧数据经**独立的 `legacy_adapter`** 预处理成新形状再喂判分器；**复用 `gateway.py` 已有的 `_bridge_legacy_payload`**（`test_equivalent_data_same_symbols` 已覆盖），不新写一套 |
| 3 | 每份 response 落 **`schema_version`**，adapter 按版本分派，**不再靠"字段存在性"猜** |
| 4 | 旧路径单测**保留**，改为测 adapter |
| 5 | **不得改写历史 `_response.json`**；迁移只发生在 case 与题面侧 |

**净效果**：单一字段来源（收益全拿）+ 历史可重判可复现（代价归零）。这才是"消除代价"，而不是把代价重命名为"本来就要付的"。
