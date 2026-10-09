# ADR 0009 · checkpoint 从「布尔断言」升级为「reward 算分表达式」；模板一分为二（固定发现 / 开放发现）

| 字段 | 值 |
|---|---|
| **状态** | Accepted |
| **日期** | 2026-07-30 |
| **决议人** | jingwu |
| **领域** | benchmark 模板结构与判分口径 |
| **关联** | 补强 [[0008]]（接口层）· 承 [[0005]] GT-first · 承 [[0006]] 负例与难度 · supersede `drug_candidate_discovery_v1` |

## 背景

owner 对模板结构给出四条明确修正（2026-07-30）：

1. **`logic` 改为 `reward`**：它不是布尔断言，而是**算分的 Python 运算表达式**，求值得到该检查点的得分——可以是 0（没通过）、1（完全通过）或中间值（部分得分）；集合关系一律用 Python 写法（`in` / `not in` / `<=`）。
2. **checkpoint 模板本质是一张 DAG**：每个检查点有自己的 `pre` 与 `next`，且都**可能不止一个**（多前驱 / 多后继，不是链）。
3. **每个检查点只核查两类东西**：allowed_properties 某些 key 的范围/对账，或**过程性指标**（如"先查文献"→ 文献数量与相关性）。
4. **题目本来就有两类，因此是两类不同的模板**：一类由某个 GT 反向构造（判分参照 = 该 GT 的性质，**越接近越好**）；一类是开放性问题（无 GT，但题面一开始就给出标准，如"A 越大越好、B 越小越好"，**要把这些要求抽出来放进后续 check 模板**）。

此前的 `drug_candidate_discovery_v1` 三处不符：logic 是布尔式、依赖关系只有单向 `depends_on`、`allowed_properties` 内联的是药物性质清单（是实例而非模板），且没有开放类的位置。

## 决策

### 1. `logic` → `reward`：返回 [0,1] 的 Python 算分表达式

- 引擎 `clamp01(eval(reward))` 得到该 cp 得分并记录原值；**布尔子项显式转数值**（`int(...)`），AND 用乘法、OR 用 `max(...)`（避免 Python `and/or` 返回非数值对象）；比例分统一 `x / max(1, n)` 防除零；"越接近越好"用衰减式 `max(0.0, 1 - abs(v - gt) / span)`。
- **过程分口径随之改变**：`process = Σ(weight_i × score_i) / Σ(weight_i)`（原为二值通过率）。
- 每个 cp 新增 **`pass_threshold`**（默认 1.0；过程指标类可设 0.6–0.8），闸门 `all(score_i >= pass_threshold_i)`。这条是必需的——reward 连续化之后，"全部通过"原本没有定义。
- 打分总公式不变（承 PR-03）：`gate ? 0.5*process + 0.5 : min(0.3, 0.5*process)`。

### 2. checkpoint 模板 = 带 `pre[]` / `next[]` 的真 DAG

节点字段：`id / name / target(process|property) / pre[] / next[] / weight / pass_threshold / reads[] / reward / note`。`pre` 与 `next` 互为逆关系，引擎加载时**校验一致性**（不一致即报错），并据此做路径验证：`all(evidence_time(u) < evidence_time(v) for (u,v) in edges)`；**分叉的两支之间无边，故可任意换序**（等价路径不受罚）。

固定发现类的 DAG 已含一处 fork + 一处 join：`cp2 → {cp3a 约束性质查证, cp3b 目标量查证} → cp4`。

### 3. 模板一分为二（两个家族）

| | `fixed_discovery_v1` | `open_discovery_v1` |
|---|---|---|
| 出题方式 | 锚定真实实体作 `gt` → 反查性质 → 反向出题 | 题面直接给标准 → 抽成 `objectives` + `constraints` |
| slots 关键项 | **`gt`**（含全部性质 + provenance + `reference_literature`） | **`objectives`**（property/direction/weight）+ `constraints` + `normalizer` |
| 判优参照 | GT 的性质；命中得 1.0，未命中按目标量距离衰减给部分分 | `objective_score`（多目标加权归一）；门槛是 0/1 乘子 |
| 落点 | `dag_poc/templates/fixed_discovery/` | `dag_poc/templates/open_discovery/` |

`open_discovery` 的 `normalizer` **默认 `feasible`**（在可行集内归一，可行最优者得 1.0）。用 `universe` 归一会让金标准答卷也拿不到 1.0（例：可行最优 Tc=134 K 在含 250 K 高压陷阱项的宇宙里只得 0.54），直接卡死闸门。

### 4. `allowed_properties` 提为 `property_spec`（格式 + example）+ 领域清单

- `dag_poc/property_spec.json`：**一条性质定义的通用 key:value 格式**（alias / type / unit / tol / direction / constraint_form / source_tool / gt_key / note）+ 一个 example。
- 领域清单按该格式落在 `dag_poc/properties/*.json`（`drug_admet_v1` 7 条、`materials_supercon_v1` 4 条），template 用 `refs.properties` 引用，**模板内不再内联任何领域性质**。

## 实证（本轮实跑，非推断）

| 轨迹 | 固定发现类（gt=分子A） | 开放发现类（Tc 最大 & 必须常压） |
|---|---|---|
| 金标准 | process **1.000**，7 个 cp 全 1.0 | process **1.000**，6 个 cp 全 1.0 |
| 次优解 | 选分子B → cp6 **0.0**（B 不满足约束，接近度分支不给分） | 选材料G → cp6 **0.933**（≥0.9 阈值，算对） |
| 踩陷阱 | 选分子C（阻断 hERG）→ cp6 **0.0**，process 0.818 | 选材料F（Tc 最高但只在高压）→ cp6 **0.0**，process 0.727 |
| 归因错 | reject 全填 bbb → cp4 **0.0**，其余不受影响（各 cp 正交） | — |
| 空转 | 1 候选、0 查证 → 全 cp **0.0** | 同左 |

三条质量门（承 GOAL §4.3）均成立：金标准全过、空转不全过、陷阱在预期节点亮红。

## 后果与遗留

- ✅ "越接近越好"、"文献查了多少查得多准"、"门槛 × 达成度" 三类此前无法表达的判分，现在都是一行 reward 表达式，且全 hard（零 LLM judge）。
- ⚠️ **reward 求值必须把命名空间放在 `globals`**：`eval(expr, env)`；放 `locals` 时表达式内的推导式会 `NameError`（Python 作用域规则）。已在验证中踩到，写进引擎实现要求。
- ⚠️ **接近度分支尚未被任何 case 触发**：`case_001_cns` 的合格集只有 1 个成员，任何非 GT 选择都不满足约束 → 距离衰减无从生效。需要一个**多合格者**的 fixed case 才能验证该分支（新开 PR-21）。
- ⚠️ `cp6.pass_threshold` 是开放类"**多接近可行最优才算对**"的旋钮（本 case 0.9，故次优解 0.933 算对）；该阈值必须写进 case 并在论文里报告，不得隐式设定。
- ⚠️ 文献相关性硬判需要 GT 侧的参考文献集；`case_001_cns` 的 `reference_literature` 暂为空（不编 DOI，承 ADR-0005），此时相关性项退化为 1.0、只判检索数量。
- 🟡 `drug_candidate_discovery_v1` 及其 case 标 `superseded_by`，文件保留（历史快照不重写）。

## 关联

- 上游：[[0008]]（答卷 schema 与符号绑定；本 ADR 不改答卷 schema——过程指标从录像层抽，性质类从答卷抽，符合"先试派生"规矩）
- 新产物：`dag_poc/property_spec.json` · `dag_poc/properties/*.json` · `dag_poc/templates/{fixed,open}_discovery/*`
- 议题：PR-19（reward 与 DAG）· PR-20（两类模板家族 + property_spec）· PR-21（多合格者 case 验证接近度分支）
