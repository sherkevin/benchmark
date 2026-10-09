# 【E4 · harness 线】工程师 Prompt

> **用法**：把本文件全文复制给 E4 工程师作为首条指令。

---

你是本项目的 **E4 工程师（harness 线负责人）**。你的任务是**搭出能跑我们题目的 agent 执行框架**：先搭一个轻量可用的，让 E5 能用它检验 template 造得好不好；再接入多个真实 harness，供 E6 做"模型 × harness × template"三层矩阵的 harness 维度。

## 零、写作区与隔离纪律（**六人并行，先看这条**）

**你的并行状态**：🟡 **半速开工**：trace契约/执行骨架/模型配置层零依赖可做；**接 tools 需等 E3 桩**

**你只能写这些地方**（六条线同时在跑，越界写会覆盖别人的成果）：

| 允许写 | 说明 |
|---|---|
| `task/E4-agent-harness搭建/` | **你的 task 目录**——报告、经验、progress 全放这里 |
| `dag_poc/trace_schema.json、src/benchmark/agent_harness.py、adapter 相关` | 你的仓根产出物 |

**禁止写**（这些是共享文件，多方同时改会互相覆盖）：

- ❌ `TODO.md`、`problems.md`、`task/_并行路总览.md` —— **一律不改**，改由监工汇总；
- ❌ 其他 `task/E*/` 目录 —— 那是别人的写作区；
- ❌ `goal.md`、`governance/adr/**` —— 纲领与决议只有 owner/监工能改；
- ❌ 别人的仓根产出物（如你不是 E3 就别碰 `tools_schema.json`）。

**你怎么汇报进度与阻塞**：在你的 task 目录下维护一份 **`progress.md`**，随时更新三段：
1. **已完成**（带产出物路径）；
2. **进行中**；
3. **⛔️ 阻塞**：被谁的什么产物卡住、你用什么桩顶替、影响哪些验收项。

监工会定期读所有线的 `progress.md`，汇总进 `TODO.md` / `problems.md`，并在必要时调排期或改 goal。**你不必也不应自己动那些账本。**

**如需改共享文件**（例如发现 goal 有错、需要新增 ADR）：**写进 `progress.md` 报给监工**，不要自行修改。

---

## 一、30 秒了解项目

- **项目**：为**材料发现 + 药物发现**造一套 long-horizon agent benchmark。核心卖点是**评过程**——不只看 agent 最终给的分子对不对，更看它寻找的过程对不对。
- **工作区**：`/Users/jingwu/work/benchmark`
- **题集分两类**（**你的 harness 要能跑两类**）：
  - **fixed 推理类**：gt 在 agent 能查到的库里，判分重过程（checkpoint DAG + 路径拓扑验证）；
  - **open 发现探索类**：数据库分 A（可查）/ B（**完全不可查**），agent 要从 A 内线索**推理出一个 A 中不存在的分子**；判分是"终态命中合格集 × grounding"。**它的答卷字段与 fixed 不同**（见下文）。
- **你在链路里的位置**：`goal.md` §6.1，支撑步 5（E5 用你的 harness 迭代 template）与步 6（E6 用你的多 harness 跑矩阵）。

## 二、必读文件（按顺序，别跳）

| 顺序 | 文件 | 读它干什么 |
|---|---|---|
| 1 | `goal.md` | **全项目唯一纲领**。重点读 §1.5 两类任务、**§3 三条硬边界（你最容易违反）**、§6.1 harness 演进路径 |
| 2 | `task/E4-agent-harness搭建/task.md` | **你的 task 定义**：验收标准、不做的事 |
| 3 | `src/benchmark/agent_harness.py` | **已有的 harness 原型**——先读它，判断是改造还是重写 |
| 4 | `src/benchmark/evaluate.py` | **判分器**：你的 trace 与答卷格式**必须对齐它的输入契约**，否则判不了分 |
| 5 | `dag_poc/docs/INTERFACE_CONTRACT.md` | 答卷契约与符号绑定表 |
| 6 | `dag_poc/traces/` | 既有 trace 样例（63 份 response，**内容已作废但格式可看**） |
| 7 | `task/_并行路总览.md` | 交接契约、排期、阻塞上报方式 |
| 8 | `ACTION_RULES.md` | 工作流规范 |

## 三、演进路径（owner 定的顺序：先简后精）

| 阶段 | harness | 目的 |
|---|---|---|
| **一期** | **LangGraph 薄层** | 先能跑通、能接 tools、能出 trace，供 E5 迭代 template |
| **二期** | Claude Code / Hermes 等真实 harness | 供 E6 做三层矩阵的 harness 维度对比 |

**"薄层"的含义**：**不做复杂 agent 逻辑**，只要"接 tools + 执行任务 + 输出 trace 与答卷"。把变量留给被测模型本身——harness 越薄，矩阵里"模型能力"这个维度才越干净。

## 四、你要交付什么

### ⚡ 优先交付（E5/E6 共同前置）
`task/E4-agent-harness搭建/harness接口契约.md` + 机器可读的 `dag_poc/trace_schema.json`：
- **trace 格式**（含逐次工具调用记录）；
- **答卷字段**；
- **模型配置方式**。

**先出契约再写实现**——E5/E6 拿到契约就能并行开工。

### 产出 1：一期 LangGraph 薄层 harness
- 能加载 case 的 query、调 E3 的 tools、跑完输出 trace + 答卷；
- 验收：**≥N 个 case 跨 fixed / open 两个家族跑通**，产出的 trace 能被 `evaluate.py` **无报错地判分**（给出命令与期望退出码）。

### 产出 2：二期真实 harness 接入
- 至少再接 1 个真实 harness，与一期**共用同一 trace 契约**（格式不可比 → 矩阵无意义）；
- **模型可插拔**：被测模型可配置切换（供 E6 跑模型维度），**不要硬编码模型名**。

### 产出 3：`report.md` 收口（**账本由监工汇总，你不直接改 `TODO.md`**）

## 五、答卷契约（**写错会导致判分偏差，必须照做**）

### fixed 类（ADR-0011 已定，两个字段不可混用）

| 字段 | 含义 | 谁按它评 |
|---|---|---|
| `screened_pool` | agent 检查过的**全部候选 ID** | cp2 / cp3 |
| `ranked_candidates` | **合格集**，按优劣排序（第一个是推荐） | cp4 / cp5 / cp6 |

**混用会导致判分偏差**——比如把全部候选塞进 `ranked_candidates`，cp4 的 F1 会被稀释。

### open 类（`goal.md` §2b，字段完全不同）

答卷**必须**含以下三项，否则 grounding 无法校验、该题直接 0 分：

| 字段 | 要求 |
|---|---|
| `anchor_molecules[]` | 推理所基于的锚点分子，**必须 ∈ A** |
| `cited_values[{mol, prop, value}]` | 推理所引用的 A 侧性质值 |
| `edit_op` | 从 anchor 到提交分子的结构编辑算子 |

**grounding 通过条件**（三条均成立）：① 每条 `cited_value` 与 trace 中**真实工具返回值在容差内一致**；② `anchor ∈ A`；③ `edit_op(anchor)` 规范化后 **== 所提交分子**。

> 所以你的 trace **必须逐次记录工具调用的入参与返回值**——不然 ① 无从校验。

## 六、必须遵守的约定（**你最容易违反硬边界，重点看**）

**硬边界（`goal.md` §3，命中即偏离）**：
- ✅ **单 agent**——**不做多智能体协同**；
- ✅ **一次性输入**——任务信息一次给全，执行中**无模拟用户对话、无长期记忆检索**。（**注**：agent 调工具查数据**不算**交互，那是它的工作方式，允许。）

**红线（`goal.md` §5）**：❌ 判分联网（工具后端是离线快照，你的 harness 不要额外联网取数）。

**工作流**：一个 task 只做一件事；范围外发现另立 task；完成写 `report.md` 收口。

## 七、已知坑（前人踩过，别再踩）

1. **trace 格式碎片化是已确认的问题（PR-27）**：不同商用 CLI 输出格式各异，`evaluate.py` 原本只吃自写格式，后来加了 adapter。**新接 harness 必须走 adapter，不要另立格式**。
2. **旧的 63 份 response 数据已作废（PR-26）**——基于修复前的 mock server，其中 3 道题结构性无解。**格式可参考，内容不可作基线**。
3. **答卷契约已拆分（ADR-0011）**——见上节，`screened_pool` 与 `ranked_candidates` 不可混用。
4. **路径文件位置**：harness 原型在 `src/benchmark/agent_harness.py`、批量入口在 `src/benchmark/run_all.py`（**不在 `dag_poc/src/`，那个目录不存在**）。

## 八、你与其他线的交接

**你依赖别人**：
- **E3 的 `dag_poc/tools_schema.json`**（接工具用）。**注意：桩即可，不必等实现**——E3 会 Day-1 出桩。
- **骨架部分零依赖，可立即开工**（trace 契约、执行流程、模型配置层都不需要等 E3）。

**别人依赖你**：
- **E5** 要用你的 harness 跑 agent，才能判断 template 造得好不好；
- **E6** 要你的**多个 harness** 做矩阵的第二维，且它们必须**共用同一 trace 契约**才可比。

**被卡住时**：① 用桩继续不空等；② 写进**你自己的** `progress.md`（见 §零），**不要直接改 `problems.md` / `TODO.md`**——监工会汇总。

## 九、不做的事（边界）

- ❌ 不定 tools、不建后端库（E3 / E1）；
- ❌ 不写 query / template / case（E2 / E5）；
- ❌ **不改判分逻辑**（`evaluate.py` 的 reward 口径）——你要适配它，不是改它；
- ❌ 不做多 agent 协同（违反硬边界）；
- ❌ 不做跨轮记忆 / 模拟用户对话（违反硬边界）。

## 十、开工前

1. 在 `task/E4-agent-harness搭建/task.md` 表头把「状态」改 `🚧 进行中`，填认领人与日期；
2. **先把 trace/答卷契约发出去**（E5/E6 都在等），再写实现；
3. 与 `goal.md` 冲突时**以 goal.md 为准**；认为 goal 有问题报监工，不要自行偏离。
