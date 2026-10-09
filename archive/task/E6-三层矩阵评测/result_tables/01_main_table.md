# 主结果表骨架（模型 × harness × template × 题类）

> ⚠️ **占位数据，非真实结果**。本表为 task-02 交付的渲染验证骨架：所有带 † 的数值是随意填入的占位数，仅用于验证表格结构与字段口径；**正式跑测未开**（等 E5 题集、专家回执、E4 二期 harness），占位数据不得出现在任何对外文档。真实数据落盘时本文件同步改名（去占位标识），以便下游 glob 聚合可识别。

**字段口径**：与 `src/benchmark/evaluate.py` 的 `aggregate()` 输出对齐——
fixed 侧：`process` / `gate` / `task_score` / `no_path_violation` / `path_validation_mode` / `gate_failed_cps`；
open 侧增：`open_gate` / `open_score` / `grounding_mode`。
**两类分数不可加不可平均**（goal §6 步 6），fixed/open 分表呈现。

---

## A. fixed 侧主表（评过程；显著性主张挂本侧，n=120 ≥ 97 功效达标）

行 = 模型 × harness；列按 template 题类分组。显著性检验与置信区间由 `src/eval_matrix/stats.py` 的 `report_significance('fixed', ...)` 产出。

| 模型 | harness | 题类 | n | process 均值† | gate 通过率† | task_score 均值† | no_path_violation 通过率† | path_validation_mode 分布† | 终态验证通过率†(a) | 路径验证通过率†(b) |
|---|---|---|---|---|---|---|---|---|---|---|
| qwen3.7-max | thinagent | type1 约束满足 | 60 | 0.75† | 62%† | 0.68† | 95%† | strict:95% / compat:5%† | 70%† | 95%† |
| qwen3.7-max | thinagent | type2 约束最优 | 60 | 0.70† | 55%† | 0.64† | 93%† | strict:93% / compat:7%† | 63%† | 93%† |
| qwen3.7-max | cli-harness | type1 约束满足 | 60 | —† | —† | —† | —† | —† | —† | —† |
| …（M×H 全组合同构展开） | | | | | | | | | | |

> † 占位数据，非真实结果。
>
> **(a)/(b) 双验证分列**（goal §2 第 4 步）：(a) 终态验证 = 答案 ∈ 合格集 G 且各性质经接口复核通过（对应 `gate` 的 cp 阈值项）；(b) 路径验证 = trace 关键 checkpoint 顺序与 DAG 合法拓扑序比对（对应 `no_path_violation`）。`path_validation_mode` 取值对齐 evaluate.py：`strict`（严格拓扑判定）/ `compat`（旧工具集降级）/ `no_recording` / `unavailable`。
>
> **打分公式（承 ADR-0009，完整两支，不得只引通过分支）**：
>
> task_score = gate ? 0.5 × process + 0.5 : min(0.3, 0.5 × process)
>
> 其中：
> - `gate`：总闸门布尔值 = 全部 cp 得分 ≥ 各自阈值 **且** `no_path_violation` 通过（路径拓扑无违例）；
> - `process`：加权过程分 = Σ(wᵢ × sᵢ) / Σ(wᵢ)，wᵢ 为 cp 权重、sᵢ 为 cp 得分（`CP_WEIGHTS`，evaluate.py）；
> - **通过分支**（gate 成立）：0.5×process + 0.5，过程分与终态各占一半，满分 1.0；
> - **惩罚分支**（gate 不成立）：min(0.3, 0.5×process)——即使过程分很高也封顶 0.3，保证"跳步/逆序/终态不合格"不可能靠过程分刷出高分；此惩罚机制是 ADR-0009 的核心，报告时不得省略。
> - **降级分支语义**：`path_validation_mode` 非 strict（`no_recording`/`compat`/`unavailable`）时，拓扑项不卡 gate、按既有行为放行（gate_ok 不加拓扑项），此类 run 单独计数并在脚注披露，不与严格判定（strict）的通过率混合。

---

## B. open 侧主表（评终态；**只出描述性统计，不出显著性主张**——n=30 < 97，功效不足，代码级强制）

| 模型 | harness | n | grounding 条件命中率†(open_gate 率) | open_score 均值† | grounding 单独通过率† | hit(G_B) 单独命中率† | 平均 attempts† | grounding_mode† |
|---|---|---|---|---|---|---|---|---|
| qwen3.7-max | thinagent | 30 | 20%† | 0.20† | 83%† | 23%† | 4.2† | real/placeholder† |
| …（M×H 全组合同构展开） | | | | | | | | |

> † 占位数据，非真实结果。
>
> **打分公式（goal §2b 唯一口径）**：
>
> open_score = 1[hit(G_B)] × 1[grounding_pass]
>
> 其中：
> - `1[·]`：指示函数，条件成立取 1、不成立取 0；
> - `hit(G_B)`：提交分子 ∈ B 侧落盘合格集 G_B 且经 B 侧性质复核满足题面目标窗口（**不是命中 B 中任意分子**）；
> - `grounding_pass`：产物落地验证三判据全过（cited_values 与真实工具返回一致、anchor ∈ A、edit_op(anchor) 规范化后 == 提交分子）；
> - 乘法关系 = 两者缺一总分 0；预算（max_attempts ≤ 10）耗尽未命中记 0，attempts 计入指标。
>
> **称谓约定**：`open_gate` 通过率称「**grounding 条件命中率**」（命中 G_B 且 grounding 通过），不得简称"命中率"以免与 hit(G_B) 单独命中率混淆；本表按 goal §6 步 6 要求**附报 grounding 单独通过率与平均 attempts**。
>
> **统计口径**：本侧任何模型间差异只报描述性（命中率 + Wilson 95% CI），显著性主张挂 fixed 120 侧——`stats.report_significance('open', ...)` 在 n < `query_spec.json → min_publishable` 阈值时 API 级拒绝出检验结果。

---

## C. 渲染校验清单（正式填数时逐项勾）

- [ ] 所有 † 已替换为 `evaluate.py aggregate()` 真实输出；
- [ ] fixed/open 两表数字未被混合加总或平均；
- [ ] open 表未出现任何 p 值 / 显著性字样；
- [ ] `path_validation_mode` 降级档（compat/no_recording/unavailable）的 run 已单独计数并在脚注披露；
- [ ] `grounding_mode=placeholder` 的 run（G_B 占位期）已剔除或单列，不混入 real；
- [ ] 本文件头「占位数据」警示已随真实数据落盘而更新状态。
