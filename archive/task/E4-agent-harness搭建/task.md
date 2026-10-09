# E4 · agent harness 搭建（LangGraph 薄层 → 真实 harness）

| 字段 | 值 |
|---|---|
| **工程师** | E4（harness 线） |
| **对应 goal** | `goal.md` v3.0 §6.1（支撑步 5 的 template 测试与步 6 的 harness 维度） |
| **状态** | ✅ task-02 已收口（详见 report.md；待监工裁定 evaluate.py 判分适配缺口） |
| **认领人** | E4 harness 线实施工程师 |
| **认领日期** | 2026-08-07 |
| **目标周**（对齐 `_并行路总览.md` §8 ICLR 倒推） | **第 2–3 周** |
| **依赖** | 🟡 **骨架可先搭**；接 tools 阶段需 E3 的清单 |

## 1. 上游诉求（owner 原话）

> "这里 agent 在测试的时候就要接上我们之前做的这些 tools 了，agent 框架可以先搭个简单的，这个可以根据我们之前调研的 agent 本身框架报告来做，可以先使用薄层的 langgraph 这种来做，后续可以上 claude code，hermess 这种，逐步来做"

## 2. 与 GOAL 的对齐

- **本 task 服务于**：**步 5 的 template 迭代**（agent 跑出来的回答是"测 template 好不好"的指标）与**步 6 的 harness 维度**（三层矩阵的第二层就是 harness）。
- **硬边界关联**（§3）：**单 agent**（不搞多智能体协同）、**一次性输入**（任务信息一次给全，执行中无模拟用户对话、无长期记忆检索）。**harness 设计不得违反这两条**。

## 3. 演进路径（owner 定的顺序：先简后精）

| 阶段 | harness | 目的 |
|---|---|---|
| **一期** | **LangGraph 薄层** | 先能跑通、能接 tools、能出 trace，供 E5 迭代 template |
| **二期** | Claude Code / Hermes 等真实 harness | 供 E6 做三层矩阵的 harness 维度对比 |

**"薄层"的含义**：不做复杂 agent 逻辑，只要"接 tools + 执行任务 + 输出 trace 与答卷"，把变量留给被测模型本身。

## 4. 子目标与验收（三要素）

| # | 子目标 | 打勾通过标准 | 验证方式 | 红线 |
|---|---|---|---|---|
| 1 | 一期 LangGraph 薄层 | 能加载 case 的 query、调 E3 的 tools、跑完输出 trace + 答卷 | 端到端跑通 ≥1 个 case | 只有框架跑不通 |
| 2 | **trace 格式统一** | 输出格式与 `evaluate.py` 的输入契约一致（含逐次工具调用记录） | 判分器能直接吃 | 每个 harness 一套格式 |
| 3 | **答卷契约遵守** | 输出 `screened_pool` 与 `ranked_candidates` 两字段（ADR-0011） | 字段名与判分器一致 | 自定义字段名 |
| 4 | 硬边界合规 | 单 agent、一次性输入（无模拟用户、无跨轮记忆） | 代码审查 | 引入多轮对话或多 agent |
| 5 | 二期多 harness 接入 | 至少再接 1 个真实 harness，与一期共用同一 trace 契约 | 两个 harness 同 case 可比 | 格式不可比导致矩阵无意义 |
| 6 | 模型可插拔 | 被测模型可配置切换（供 E6 跑模型维度） | 配置切换即生效 | 模型硬编码 |

## 5. 已有资产

| 资产 | 内容 |
|---|---|
| `src/benchmark/agent_harness.py` | **已有的 harness 原型**（先读它，判断是改造还是重写） |
| `src/benchmark/evaluate.py` | 判分器——**trace 与答卷格式必须对齐它的输入契约** |
| `dag_poc/docs/INTERFACE_CONTRACT.md` | 答卷契约与符号绑定表 |
| `dag_poc/traces/` | 既有 trace 样例（63 份 response，可作格式参考；**内容已作废但格式可看**） |
| `research/` | agent 框架调研报告（owner 提到"之前调研的 agent 本身框架报告"） |

## 6. 已知坑

1. **trace 格式碎片化是已确认的问题**（PR-27）：每个商用 CLI 产出不同格式，`evaluate.py` 原本只吃自写格式，已加 adapter。**新接 harness 必须走 adapter，不要另立格式。**
2. **答卷契约已拆分**（ADR-0011）：`screened_pool`（候选池，cp2/cp3 按此评）与 `ranked_candidates`（合格集，cp4/cp5/cp6 按此评）。**混用会导致判分偏差。**
3. **旧的 63 份 response 数据已作废**（PR-26，基于修复前的 mock server），**不可作为基线复用**。

## 7. 不做的事

- ❌ 不定 tools（E3）、不建后端库（E1）；
- ❌ 不写 query/template/case（E2/E5）；
- ❌ 不改判分逻辑（`evaluate.py` 的 reward 口径）；
- ❌ 不做多 agent 协同（违反硬边界）；
- ❌ 不做跨轮记忆/模拟用户对话（违反硬边界）。

## 8. 交付物

1. 一期 LangGraph 薄层 harness（可跑通、接 tools、出合规 trace）；
2. `harness接口契约.md`——trace 格式、答卷字段、模型配置方式（**交给 E5/E6 的书面契约**）；
3. 二期至少一个真实 harness 接入；
4. `report.md` 收口。

## 9. 任务状态追加（不覆盖表头）

| 任务 | 状态 | 收口文档 |
|---|---|---|
| task-02（多模型接入 + trace 定稿 + 新契约端到端冒烟） | ✅ 已收口（见表头；report.md 六条标准全勾） | `report.md` |
| task-03（trace 落盘防护与证据固化，task_id=7） | ✅ 已收口（QA 独立核验六项全过；状态行滞后已于 task-04 修复）：落盘迁至 `traces_new/`（TRACE_OUT_DIR/--trace-dir 可配）、dag_poc/traces 只读零写入（md5 证据）、E3 交接包 e3_handover/（3 组 fixed + README + run_eval.py + MD5SUMS；PR-34 通过标准已实跑达成，见 README §2） | `report-task-03.md`（含还原事件完整时间线） |
| task-04（视图接线 PR-40 harness 一半 + task-03 状态行修复，task_id=10） | ✅ 已收口：case_id 从 case JSON 读入并传入 gateway 会话（现行通道 DAGPOC_CASE_ID 校验 + 会话头 X-Dagpoc-Case-Id 预留）；open 缺 case_id/通道不一致报错不静默（实测 8/8 断言过）；**合跑已由 task-05 完成（2026-08-08，13/13 PASS）**；answer_schema 0.3.0 枚举引导已更新 | `report-task-04.md`（§四已回填终验收结果） |
| task-05（H2 真实 harness 接入与合跑终验收，task_id=17） | ✅ 已收口：H2 端到端跑通（claude 2.1.212，fixed+open 各 1 道 4/4 PASS，必经 adapter 硬边界，对账各 10/10；qodercli 被内容网关拦如实报）；PR-40 合跑终验收 13/13 PASS（B1 零泄漏/B2 gt 不可达）；交卷断链双层根因修复复跑 4/4 PASS；耗时知会 E6（open×H2 1313s 超规划值 2.2 倍） | `report-task-05.md` |
| task-06（堵 H2 内置工具泄漏，task_id=19） | ✅ 已收口：重清点实质泄漏 0（50 条=46 拒绝信封+4 CLI 内部 todo，无一触 dag_poc/data/联网，订正 task-05 误判）；三层封堵（disallowed-tools 补 Task*/MCP 资源工具 + settings permissions.deny + PreToolUse hook 白名单）各层冒烟实测生效；同题重跑 4/4 PASS（MCP 资源/Task* 归零，残留 6 条被拒尝试逐条举证无害）；知会 E6 前置 8 解除；§7 token/日志汇总脚本交付 | `report-task-06.md` |
| task-07（清 adapter 日志冗余 + R7 嵌套深度断言，task_id=23） | ✅ 已收口：定性跟进 owner 纠正（三层壳非 bug，CLI 2 层合规，第 3 层是后处理包壳块加的，存量仅 2/278 条）；adapter 记 trace 拆壳为与 H1 同构单层（调用链路零改动）；raw 原样性取证（CLI stdout 直写、session_key 原样、md5 留档）；R7 嵌套深度 >1 报错落盘可复跑（旧 R7 timestamp 让位改 R9，R4 同步改拆壳语义），存量包裹产物如实 FAIL 报出 2 条不掩盖；同题实跑 5/5 PASS exit 0（422s，99 条全单层，新 raw 对账 11/11）；存量判分投影 --reprocess 重生成前后 response 逐字相同口径可比 | `report-task-07.md` |
| task-08（H2 llm_call 证据流，task_id=25） | ✅ 已收口：判断能拿到逐次 usage（盘上实证：默认流 assistant usage 恒 0 占位仅 result 给会话总数，加 --include-partial-messages 后每回合 message_delta 带真实 usage）；补流落地只碰记录不碰行为（adapter 产 llm_call 与 H1 同构，降级字段 null 注明原因登记 trace_schema）；同题重跑 14/14 对账全过，两路交叉校验四项一致；存量 H2 产物不可追溯计 null 不回填（PR-55 红线不踩）；知会 E6 口径变更 | `report-task-08.md` |

