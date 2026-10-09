# 消融表骨架（harness 对比）

> ⚠️ **占位数据，非真实结果**。所有带 † 的数值为随意填入的占位数，仅验证表结构；**正式跑测未开**，占位数据不得出现在任何对外文档。真实数据落盘时本文件同步改名（去占位标识），以便下游 glob 聚合可识别。

**消融设定**：同一模型、同一题集、同一种子策略（`matrix_design.md` §4），仅切换 harness（H1 薄壳 AgentHarness vs H2 真实 harness），度量 harness 本身对得分的影响。**H2 端到端未就绪前（E4 二期），本表不适用**——单 harness 时此表冻结为占位，不得以 H1 自比充当消融。

## A. 同模型跨 harness 对比（fixed 侧）

| 模型 | 题类 | 指标 | H1 thinagent† | H2 cli-harness† | Δ(H2−H1)† | 备注 |
|---|---|---|---|---|---|---|
| qwen3.7-max | type1 | task_score 均值 | 0.68† | 0.61† | −0.07† | † 占位 |
| qwen3.7-max | type1 | gate 通过率 | 62%† | 55%† | −7pp† | † 占位 |
| qwen3.7-max | type1 | process 均值 | 0.75† | 0.72† | −0.03† | † 占位 |
| qwen3.7-max | type2 | task_score 均值 | 0.64† | 0.58† | −0.06† | † 占位 |
| …（每模型同构展开） | | | | | | |

> † 占位数据，非真实结果。
> **可比性前提**（E4 task.md 红线：两 harness 必须共用同一 trace 契约，否则矩阵无意义）：
> - 两 harness 的 trace 均经 `evaluate.py` 同一判分链（H2 走 `cli_trace_adapter.py` / `qodercli_trace_adapter.py` 转换后进同一入口）；
> - `path_validation_mode` 分布须两 harness 同档对比（若 H2 大量落入 compat/no_recording 降级，拓扑项不可比，该指标行标注"不可比"而非硬出数字）；
> - 答卷字段按 ADR-0014：`answer_schema.json` 新字段优先 + 旧字段 fallback 双解析，两 harness 同口径。

## B. 同模型跨 harness 对比（open 侧，描述性）

| 模型 | 指标 | H1 thinagent† | H2 cli-harness† | Δ† | 备注 |
|---|---|---|---|---|---|
| qwen3.7-max | grounding 条件命中率 | 20%† | 17%† | −3pp† | † 占位；只报描述性 |
| qwen3.7-max | grounding 单独通过率 | 83%† | 80%† | −3pp† | † 占位 |
| qwen3.7-max | 平均 attempts | 4.2† | 5.1† | +0.9† | † 占位 |

> † 占位数据，非真实结果。open 侧 n=30 < 97（`query_spec.json → min_publishable`），**Δ 列只作描述性呈现，不做显著性主张**（代码级强制：`stats.report_significance('open', ...)` 拒绝出检验）。

## C. 渲染校验清单

- [ ] E4 二期 harness 端到端就绪（同 case 双 harness 可比）后再填数；
- [ ] 两 harness trace 均通过同一 `evaluate.py` 判分链的验证记录已附；
- [ ] 单 harness 场景下本表保持冻结，未被拿 H1 自比冒充消融；
- [ ] 文件头「占位数据」警示已随真实数据更新。
