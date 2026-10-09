# 测试 Agent 调研总报告（壳子选型 + 循环机制 + 源码核验）

> **本报告 = 测试方法的全部调研，合一份。** 覆盖五个问题：主流 benchmark 用什么壳子与判分 / 商用 CLI agent 当壳子的现状与 context 管理 / 自写 agent 的循环与预算 / 12 个仓库源码级测试方式 / 对本项目的结论。
> 证据分级：本地 benchmark 以 `reference/` 与 `datasets/` 一手材料为准；12 个仓库源码 clone 在 `.srcprobe/`（gitignore，复现命令见 §四·一）；标注 ⚠️ 的仍是二手，不可直接引用。
> 前序草稿（`archive/Agent壳子与判分选型.md`、`archive/测试Agent源码一手核验.md`）已并入本文并归档，索引以本页为准。

## 全景结论

**选壳：薄壳（MCP client + function calling）+ 传输层 JSONL tracer + 双轨判分**，与现有架构完全对齐。**机制：一个 case = 一次题面输入 + 一次交卷，中间由壳子驱动几次到几十次 LLM 调用循环**（LLM 本身一次性无状态，循环是壳子的 for 循环）。**2026 年的对照范式已变成"商用 CLI agent 当壳"（Harbor / Inspect-SWE），壳子是一级变量，同题多壳是必报对照。**

---

## 一、主流 benchmark 的壳子方式

**一句话趋势：壳子在极简 + 记录在标准化 + 判分在双轨。**

| Benchmark | 壳子形态 | 判分 | history 记录 |
|---|---|---|---|
| **SWE-bench Verified** | mini-SWE-agent（bash-only，~100行） | 纯终态（跑测试） | 线性 messages → .traj JSON |
| **tau-bench** | function calling + LLM模拟用户 | 终态（DB对比）+ pass^k | OpenAI messages 结构化 |
| **TheAgentCompany** | OpenHands CodeAct | **双轨**（checkpoint部分分 + 终态） | JSONL event stream |
| **MADE** | DSPy ReAct 编排 | 终态（发现曲线指标） | ReAct messages + 环境评估历史 |
| **AgentBoard** | 自带 ReAct 文本解析 | **双轨**（progress rate + success rate） | 文本轨迹（正被淘汰） |
| **MCP-Bench** | ReAct loop + **MCP client** | **双轨**（rule 结构理解 + o4-mini LLM judge；无终态真值） | JSON-RPC（天然结构化） |
| **MCP-Universe** | ReAct/FunctionCall + **MCP client** | 终态（execution-based：format/static/dynamic 三类 evaluator） | **Tracer 层专门设计**（Memory/File/SQLiteCollector 按 trace_id 收全量） |
| **ChemCrow** | 自带 LangChain ReAct（Thought/Action 文本格式） | 终态（LLM 评分/人工） | 文本轨迹（fragile） |
| **Biomni** | 自带 ReAct（支持挂 MCP server） | 终态（**精确匹配**，非 LLM judge） | 文本轨迹 |
| **HAL** | 标准化 harness 统一壳 | 各 benchmark 原生判分 | 统一 JSONL traces + Weave |
| **METR** | modular-public（Python+Bash） | 终态（scoring fn） | vivaria 平台统一 transcript |
| **Terminal-Bench 2.0 / Harbor** | **直接用商用 CLI agent 当壳**（Claude Code / Codex CLI / Gemini CLI / OpenHands 内置 adapter） | 终态（容器内执行校验） | Harbor harness 统一录制 |
| **Epoch AI · SWE-bench Verified** | 自建极简 loop 为主，**并列报 Claude Code / Codex 第三方 scaffold**（经 Inspect-SWE） | 终态（跑测试） | Inspect log viewer 全量 transcript |

趋势拆解：

1. **壳子在极简**：mini-SWE-agent 是标志（壳子越薄，差异越归于模型）；tau-bench 的 `ToolCallingAgent` 只有 93 行（见 §四·三）；
2. **记录在标准化**：messages / JSONL / JSON-RPC 结构化事件流是主流，文本解析式（Thought/Action 正则）正被淘汰；
3. **判分在双轨**：终态是底座，过程分是差异化加分项，且过程分都消费结构化轨迹——这正是我们的方向。

---

## 二、商用 CLI agent（Claude Code / Codex / Hermes）当壳子的现状

### 2.1 有人用，且已是 2026 年的主流对照方式

| 评测方 | 怎么用商用 CLI 当壳 | 依据 |
|---|---|---|
| **Terminal-Bench 2.0 / Harbor** | Harbor 内置 Claude Code、Codex CLI、Gemini CLI、OpenHands adapter；排行榜条目本身是"**壳+模型**"成对标注（如 `Codex CLI, GPT-5.5` / `Claude Code, Claude Opus 4.5`） | tbench.ai/leaderboard；harbor-framework/harbor；Terminal-Bench 论文 arXiv:2601.11868 |
| **Epoch AI · SWE-bench Verified** | 默认自建极简 loop，2026-02-13 起额外"加入第三方 scaffold（Claude Code 和 Codex），经 Inspect-SWE 评测" | epoch.ai/benchmarks/swe-bench-verified |
| **Inspect-SWE**（基础设施） | 把 `claude_code()` / `codex_cli()` / `gemini_cli()` / `kimi_code()` / `opencode()` / `mini_swe_agent()` 封成标准 Inspect agent | meridianlabs-ai.github.io/inspect_swe |
| **多 harness 对比型 benchmark** | WildClawBench（OpenClaw / Claude Code / Codex CLI / Hermes Agent 跑同一批 60 任务）、Claw-SWE-Bench | ModelScope；arXiv:2606.12344 ⚠️ 二手 |
| **反向对照** | mini-SWE-agent 把"beats Claude Code and Codex"当卖点——商用壳已是默认 baseline | swe-agent/mini-swe-agent |

**壳子影响的真实量级（Terminal-Bench 2.0 一手数据）**：同为 GPT-5.2，Codex CLI 62.9% vs Terminus 2 54.0%（差 8.9 点）；但论文结论是"**模型选择通常比 agent scaffold 更重要**"（Codex CLI 换模型 +52%，Gemini-2.5-Pro 换壳仅 +17%）。

### 2.2 源码可得性分三档

| 壳子 | 源码 | context 管理实现位置 |
|---|---|---|
| **Codex CLI** | ✅ 开源（openai/codex，Rust） | `compact.rs`、`templates/compact/prompt.md`、`config/mod.rs`（⚠️ 阈值数字二手，见 §六） |
| **OpenCode** | ✅ 开源（sst/opencode） | `session/compaction.ts`（⚠️ 常量二手） |
| **OpenHands** | ✅ 开源（SDK） | `context/condenser/`（一手，见 §2.3） |
| **Kimi Code** | ✅ 开源（npm `@moonshot-ai/kimi-code`） | 版本可钉 npm `@<version>`；**1M context**（`KIMI_MODEL_MAX_CONTEXT_SIZE=1048576`），`KIMI_MODEL_THINKING_EFFORT=max` |
| **Hermes Agent** | ✅ 开源（NousResearch/hermes-agent） | 版本用 git `--branch` 钉（弱于 checksum）；改造讨论直接引 Codex 架构（issue #499），已接 Anthropic Context Editing API（#526） |
| **mini-SWE-agent** | ✅ 开源 | 无 compaction：全量 linear messages + step 上限 + 观测截断 |
| **Claude Code** | ❌ 闭源 | 机制公开：~95% 触发 auto-compact；API 层 context editing（tool result clearing / thinking clearing，beta header `context-management-2025-06-27`）+ memory tool（⚠️ +39%/+29% 二手偏强） |

### 2.3 context 管理的三种路线

1. **摘要式 compaction**（主流）：达阈值 → 全史 + 专用 prompt 让 LLM 写 handoff 摘要 → 重建历史。差异在三参数：阈值（Claude Code 百分比 vs Codex 绝对 token）、保留什么、摘要如何回插；
2. **裁剪式 pruning**（更轻）：OpenCode 倒扫 tool call，保护最近 40k tokens 工具输出；Anthropic 的 tool result clearing 同类（官方称"最轻触的 compaction"）；
3. **不管**：mini-SWE-agent（靠 step/token 上限）、OpenHands `NoOpCondenser`、Amp（不做自动压缩，只给 handoff/fork/thread-reference 工具）。

**OpenHands condenser 机制（一手，源码目录 README）**：定期把"前一半事件"整体替换为单个 summary event；事件日志 append-only，用 `Condensation` tombstone 事件（类 Cassandra/Kafka）记录遗忘，`View` 类负责应用压缩得到 LLM 可见视图；触发分 soft（`max_tokens`/`max_size` 超限可跳过下步再试）与 hard（上下文溢出必须压，兜底 `hard_context_reset`）。四项权衡：单次成本、缓存重建成本、早期上下文（摘要会被再摘要）、近期上下文（后一半不动）。

### 2.4 context 差异直接写在 token 账上（一手数据）

Terminal-Bench 2.0 附表：同为 **GPT-5.2**，Codex CLI 消耗 **137.5M input / 2.3M output** tokens，Terminus 2 仅 **12.4M input / 2.6M output**——输出量相当，**输入量差 11 倍**。含义：一旦引入 cost-aware 指标，**壳子而非模型会主导成本列**，成本对比必须固定壳子。

---

## 三、自写 agent 的循环与预算（一个 case 跑多少次 LLM 调用）

**结论：一个 case = 一次题面输入 + 一次交卷，中间是几次到几十次 LLM 调用的循环。绝不可能一次调用跑到底。**

### 3.1 谁发起"第二次调用"？——壳子程序，不是 LLM

**LLM 是一次性的、无状态的 HTTP 调用**，每次请求拿到响应就结束了，它不会"自己再调一次"。所谓二次调用是**壳子这个 Python 程序**在循环里**再次发 HTTP 请求**，与第一次是两次完全独立的请求。

源码证明（tau-bench）：驱动者是 `python tau_bench/run.py`。`run.py:42` 用 `agent_factory` 造出 `ToolCallingAgent` **对象**，`run.py:78` 调它的 `agent.solve(...)` **方法**——全程是普通 Python 代码在跑。循环在 `tool_calling_agent.py:39-74`：

```python
for _ in range(max_num_steps):                    # 壳子的 for 循环
    res = completion(messages=messages, ...)      # 每次循环发一次全新 HTTP 请求
    action = message_to_action(next_message)      # 模型只产出"意图"
    env_response = env.step(action)               # 壳子替它真正执行工具
    reward = env_response.reward
    messages.extend([next_message, {...}])        # 壳子把意图+结果追加进历史
    if env_response.done:                         # 终止由壳子判定
        break
```

三个关键事实：

1. **LLM 无记忆**：它不记得上一次调用，所以壳子每次请求都把**整个 `messages` 历史重新发一遍**——这是 context 随轮数线性膨胀、也是各家要管 context 的根本原因；
2. **"再来一次"不是 LLM 的决定，是壳子的循环**：LLM 每次只产出下一步的意图（调哪个工具，或"我答完了"）；壳子决定要不要执行工具、要不要循环；
3. **"结束"是一个约定信号**：模型不再返回 `tool_calls`（或返回 `respond`/`done` 动作）时，壳子就 `break`。

**三个"谁"的答案：循环的发起者是壳子程序；次数是壳子里的常量（`max_num_steps=30`）；结束是壳子检测"无 tool_calls"后主动 break。**

#### 用真实消息走一遍 tau-bench 的第 1 次与第 2 次调用

**调用 1**，壳子发：

```json
{"model":"gpt-4o","messages":[
  {"role":"system","content":"<wiki 客服政策>"},
  {"role":"user","content":"我要退机票"}],
 "tools":[{"type":"function","function":{"name":"cancel_reservation",...}}, ...]}
```

模型回（仅意图，无执行）：

```json
{"role":"assistant","content":null,
 "tool_calls":[{"id":"call_1","function":{"name":"cancel_reservation",
   "arguments":"{\"reservation_id\":\"HATHAT\"}"}}]}
```

壳子**自己**调真实数据库 API `cancel_reservation`，拿到结果 `"已退款"`。

**调用 2**，壳子把历史追加后**重新发整个 messages**：

```json
{"messages":[
  {"role":"system","content":"<wiki>"},
  {"role":"user","content":"我要退机票"},
  {"role":"assistant","tool_calls":[{"id":"call_1","function":{"name":"cancel_reservation",...}}]},
  {"role":"tool","tool_call_id":"call_1","content":"已退款"}]}
```

模型这次看到结果，回 `{"role":"assistant","content":"您的退款已办理。"}`，**没有 tool_calls** → 壳子判定 done → `break`，循环结束。**这两次 HTTP 调用之间 LLM 没有"活着"**，中间是壳子在干活。

### 3.2 为什么不能一次调用跑完

因为**工具结果必须回灌**。LLM 一次调用只能产出"我想调 `predict_admet(aa,ab,ac)`"这个意图，它拿不到结果；壳子执行完工具后，必须把返回值作为新消息追加进 messages，再发起**下一次** LLM 调用，模型才能看到 ADMET 数值并决定下一步。所以有 N 次工具调用，就至少有 N+1 次 LLM 调用。

**用我们自己的实测数据走一遍**：`dag_poc/traces/case_cns_metoprolol__qwen3.7-max.jsonl` 一个 case 记了 12 条工具调用事件（文献检索 → 靶点校验 → 候选库检索 → ADMET 预测 → 分子生成 → 再 ADMET → 出报告），对应 13 次左右 LLM 调用。这一个 case 只交一次卷（`ranked_candidates`），但内部跑了十几轮。

### 3.3 各家的循环结构与上限（源码常量，逐条一手）

| 壳子 | 循环写法 | 终止条件 | 上限默认值 |
|---|---|---|---|
| **tau-bench** | `for _ in range(max_num_steps)`（`agents/base.py:12`） | `env_response.done` 或步数用尽 | **30 步** |
| **MCP-Universe ReAct** | `for iter_num in range(max_iterations)`（`agent/react.py:145`） | 模型给终答或迭代用尽 | **5 轮**（`react.py:45`） |
| **mcp-bench** | `for round_num in range(1, max_rounds+1)`（`agent/executor.py:102`）+ 轮内重试 | 配置驱动 | 配置文件决定 |
| **mini-SWE-agent** | 每步自增 `n_calls`，超限抛 `LimitsExceeded`（`agents/default.py:132-137`） | **成本或步数任一超限** | `step_limit=0`（不限）、**`cost_limit=3.0` 美元** |
| **Biomni** | LangGraph `generate ⇄ execute` 循环 | 出现 `<solution>` 或触发递归上限 | **`recursion_limit: 500`**（`a1.py:1774`），单次代码执行另有 600 秒 timeout |
| **ChemCrow** | LangChain `RetryAgentExecutor` | `Final Answer:` 或迭代用尽 | **`max_iterations=40`** |
| **MADE** | episode 内迭代提议→评估 | **oracle 查询预算耗尽** | **`budget: 5`**（`configs/environment/convex_hull.yaml:7`） |
| **本项目** | `for step in range(max_steps)`（`agent_harness.py`） | 无 `tool_calls` 即完成 | **15 步** |

### 3.4 三条从数字里读出来的规律

1. **上限差了两个数量级（5 → 500），因为"一步"的粒度不同**：MCP-Universe 一步 = 一次 ReAct 推理（粒度粗，5 轮够）；Biomni 一步 = 执行一小段代码（粒度极细，才需要 500）。所以**跨 benchmark 比"步数"没有意义，必须先对齐一步的定义**；
2. **先进做法是用成本上限替代步数上限**：mini-SWE-agent 默认 `cost_limit=3.0` 美元、`step_limit=0`。理由：不同模型每步效率差异大，限步数会系统性偏袒"少步高质"的模型，限成本对所有模型是同一把尺；Epoch AI 同理（2M uncached + 20M cached token 预算，超了强制交卷）；
3. **预算本身就是难度旋钮**：MADE 直接把 oracle 预算 5 次写进环境配置——"用 5 次查询找到稳定材料"和"用 50 次"是两个难度的题。

### 3.5 这些论文报告"一次 query 的调用次数"吗？——只有 Terminal-Bench 2.0 认真报

**结论：绝大多数论文不报调用次数，只报结果指标（success rate / pass^k / 成本）；唯一认真把"每次 query 的模型调用次数"当一级数据报告的是 Terminal-Bench 2.0。**

| 论文 | 报什么 | 报调用次数吗 |
|---|---|---|
| **Terminal-Bench 2.0** | Figure 10 报执行时间、token、**模型调用次数**；Figure 35 报平均 turns/trial | ✅ 报，但**只对 Terminus 2**（L821："number of model (API) calls was recorded for Terminus 2 runs only"）。量级：多数 trial <25 次调用、<10M token；极端 2 小时、数百次调用、近 100M token。并用 `average episodes` 作效率指标（L2901） |
| **MCP-Universe** | Success Rate、Average Evaluator Score、**Average Steps**（排行榜页） | ⚠️ 有 "Average Steps" 列，但论文正文无统一口径调用次数表 |
| **AgentBoard** | success rate + progress rate（**progress rate 相对交互步数计算**） | ❌ 步数只用于算 progress rate |
| **MADE** | AF/EF/AUDC/mSUN + oracle 预算/episode | ❌ 报预算/episode，不报 LLM 调用次数 |
| **HAL** | **成本**（Weave 记 token + cost） | ❌ 转向报成本不报次数 |
| **tau-bench** | pass^k | ❌ 不报 |

三个值得写进我们论文的点：

1. **调用次数是被普遍遗漏、但能区分工作量与效率的一级指标**。Terminal-Bench 2.0 用它发现：**平均 turns/trial 与成功率基本无相关**（L139："essentially no correlation between the number of average turns per trial and model success rates"）——"调得多"不等于"调得好"。这正是我们 checkpoint 过程分要回答的：不是走了多少步，而是**每一步对不对**；
2. **必须统一口径，否则壳子间不可比**。Terminal-Bench 2.0 只对 Terminus 2 报；其附录还专门定义 **tool-call 去重规则**（L1263-L1266："distinct tool-call blocks (with block-level deduplication)... counts as at most one attempt"）。我们报"每 case 调用次数"时必须同时报：**一步的定义、是否去重、tool call 与 LLM call 是否分开**；
3. **调用次数与成本要分开报，不能互相替代**。HAL 只报成本、Terminal-Bench 只报次数，都偏。正确做法是两个都报（次数衡量过程、成本衡量经济），加上我们的过程分（衡量正确性），构成三维：效率 × 成本 × 正确性。

---

## 四、源码级测试方式（12 个仓库一手核验）

> 方法：`git clone --depth 1` 拉取 12 个仓库到 `.srcprobe/`（gitignore）。复现：`mkdir -p .srcprobe && cd .srcprobe && git clone --depth 1 https://github.com/<repo>.git`。

### 4.1 可复现性分级（拉取日期 2026-07-31）

| 仓库 | 最近提交 | 依赖锁定 | 判定 |
|---|---|---|---|
| **harbor-framework/harbor** | 2026-07-31（当天） | `uv.lock` + docker + registry | ★★★ 最可复现。40+ agent adapter + 26 个 benchmark adapter |
| **OpenHands/software-agent-sdk** | 2026-07-30 | `uv.lock` | ★★★ condenser 源码齐全 |
| **meridianlabs-ai/inspect_swe** | 2026-07-29 | `uv.lock` | ★★★ 六个 CLI agent 封装，版本+checksum 钉死 |
| **laude-institute/terminal-bench** | 2026-07-10 | `uv.lock` + docker + original-tasks | ★★★ 任务与判分同仓 |
| **princeton-pli/hal-harness** | 2026-07-01 | 无 lock | ★★ 活跃但依赖未锁 |
| **SalesforceAIResearch/MCP-Universe** | 2026-06-23 | `requirements.txt` + docker | ★★ tracer 完整 |
| **diffractivelabs/MADE** | 2026-05-08 | `uv.lock` | ★★ 材料域，Hydra 配置齐全 |
| **sierra-research/tau-bench** | 2026-03-18 | 无 lock、无 docker | ★★ 代码极简易读，但 README 自标"tasks 已过期" |
| **snap-stanford/Biomni** | 2026-01-14 | conda | ★ 环境重，判分需拉 HF 数据集 |
| **Accenture/mcp-bench** | 2025-10-07（停更 9 个月） | 无 | ✗ 判分依赖 o4-mini 硬编码 |
| **ur-whitelab/chemcrow-public** | 2024-12-19（停更 19 个月） | 无 | ✗ README 自述"不会给出与论文相同的结果" |

**重点看 harbor / inspect_swe / OpenHands SDK / terminal-bench 这四家。ChemCrow 与 mcp-bench 不必再投入。**

### 4.2 最薄的壳长什么样：tau-bench `ToolCallingAgent`（93 行，全文读过）

`tau-bench/tau_bench/agents/tool_calling_agent.py`：

- **模型层**：`from litellm import completion`（L4）——换模型不换壳；
- **主循环**（L39-L74）：`completion(messages, tools=...)` → `message_to_action()` → `env.step(action)` 拿 reward 与 observation；
- **两种动作分流**（L53-L72）：工具调用 → 追加 `role: tool`；`respond` 动作 → 追加 `role: user`（LLM 模拟用户的回话）；
- **判分**：reward 由环境每步返回（L51），壳子不参与判分；
- **成本**：`res._hidden_params["response_cost"]`（L48）累加；
- **context 管理**：无。全量 messages 一路追加，只有 `max_num_steps=30` 兜底。

对照我们的 `agent_harness.py`：结构同构，差别只有两点——我们用 `requests` 而它用 `litellm`；我们的 reward 在 trace 上离线算而它在环境里在线返回。

### 4.3 Claude Code 这类闭源 CLI 怎么被使用：三条路径，全都做了改造

#### 路径 A：Harbor —— CLI subprocess + stdin 管道（`installed/claude_code.py`，1579 行）

实际执行的命令（L1553-L1564，原文）：

```
printf "%s" "$instruction" | claude --verbose --output-format=stream-json \
  [--settings ...] [其他 flags] [--continue] --print 2>&1 | tee /logs/agent/claude-code.txt
```

**是"直接输入 query"，但外面套了七层改造**：

1. **instruction 走随机环境变量再 unset**（L1547-L1556）：`harbor_claude_code_instruction_<uuid>` 传入后立刻 `unset`，再 `printf` 管道喂 stdin——避免题面出现在进程列表、也绕开 shell 转义；
2. **prompt 模板钩子**（`@with_prompt_template`，实现在 `installed/base.py:914-918`）：`render_instruction()` 默认**恒等**（无模板即原样传），配了模板路径才渲染。"是否改造 query"是可配置项，默认不改；
3. **非交互 + 结构化输出**：`--print`（一次性）+ `--output-format=stream-json`（事件流）；
4. **预算与权限全部走 CLI flag**：`--max-turns`、`--max-budget-usd`、`--effort`、`--fallback-model`、`--append-system-prompt`、`--allowedTools`、`--disallowedTools`、`--permission-mode`；
5. **换模型不换壳靠环境变量**：`ANTHROPIC_MODEL` / `ANTHROPIC_BASE_URL`，走自定义 base_url 时把 sonnet/opus/haiku/subagent 四个别名**全部指向同一模型**，防止壳子偷偷调小模型；
6. **实验卫生**：`CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1`（禁遥测）、`IS_SANDBOX=1`、`CLAUDE_CONFIG_DIR` 按 trial 隔离、可选禁 prompt caching 与 adaptive thinking；
7. **能力注入三件套**：skills 复制、memory 文件写入、**MCP server 写进 `~/.claude.json` 的 `mcpServers`**（注释说明为何不用项目级 `.mcp.json`）。

**额外一手发现**：轨迹解析器专门处理 compaction 造成的事件重放（L944/L1049："an event replayed after compaction"）——厚壳的 context 压缩会污染轨迹，必须在 harness 侧去重。成本用 `litellm.cost_per_token` 统一计价。

#### 路径 B：MCP-Universe —— 官方 Python SDK（`mcpuniverse/agent/claude_code.py`，166 行）

用 `claude_code_sdk` 的 `ClaudeSDKClient` + `ClaudeCodeOptions`（L10-L11），传 `mcp_servers=self.get_mcp_configs()`、`system_prompt=prompt`（L104-L110），解析 `ToolUseBlock` / `ToolResultBlock` / `ResultMessage` 落 tracer（L141 附近还要把 `mcp__server__tool` 形式的工具名拆开）。

**这条路径明确改造了 query**：`message = message + "\n\n" + self._get_output_format_prompt(output_format)`（L99）——追加输出格式要求，否则拿不到可判分的结构化答案。我们的 `SYSTEM_PROMPT` 强制附 JSON 是同一手法。

#### 路径 C：inspect_swe —— 沙箱内装钉版本二进制 + API 桥（`inspect_swe/_claude_code/`）

- `agentbinary.py`：`resolve_version("stable"|"latest"|semver)` → 取 `manifest.json` → **校验 `expected_checksum`** → 缓存到 `claude-<version>-<platform>`；
- `claude_code.py:14`：`sandbox_agent_bridge()`——壳子在沙箱内的模型 API 调用被代理回 Inspect，闭源壳也能拿全量 transcript 并受 token/time 限制；
- `version` 选项支持 `auto | sandbox | stable | latest | <semver>`。

**这是三条路径里唯一解决“闭源壳可复现”的**：CLI 会自动升级，不钉版本就无法复现历史结果。

#### 路径 D：Kimi Code 与 Hermes Agent（Harbor 已内置，源码全读过）

这两个壳子 Harbor 都有现成 adapter，接我们的 MCP server 都不需要改题面：

**Kimi Code**（`installed/kimi_code.py`，189 行，全文读过）：
- **query 喂法与 Claude Code 同款**（L173-L186）：随机环境变量 `harbor_kimi_code_instruction_<uuid>` + `unset` → `kimi --prompt "${var}" --output-format stream-json </dev/null 2>&1 | tee kimi-code.txt`；
- **MCP 注入**（L119-L155）：写 `$KIMI_CODE_HOME/mcp.json` 为 `{"mcpServers": {...}}`，支持 stdio / sse / url 三种 transport；
- **实验卫生**（L107-L114）：`KIMI_DISABLE_TELEMETRY=true`、`KIMI_CODE_NO_AUTO_UPDATE=true`、`KIMI_CODE_BACKGROUND_KEEP_ALIVE_ON_EXIT=true`（等 agent 后台工作完成再让 verifier 跑）；
- **版本可钉**：npm `@moonshot-ai/kimi-code@<version>`（L31 `--agent-kwarg=version=0.28.1`）；
- **长 context**：`KIMI_MODEL_MAX_CONTEXT_SIZE=1048576`（1M）、`KIMI_MODEL_THINKING_EFFORT=max`（L34-L38）。

**Hermes Agent**（`installed/hermes.py`，451 行）：
- **安装与版本**：curl 拉官方 `install.sh`，版本用 git **`--branch {version}`** 钉（L69-L77）——弱于 inspect_swe 的 checksum 做法，记为复现风险；
- **home 隔离**（L77）：`HERMES_HOME` 下 `sessions/` `skills/` `memories/` 各自目录；
- **MCP 注入**（L119-L132）：注意是 **YAML**（`yaml.dump({"mcp_servers": {...}})`），不是 JSON；
- **换模型**：必须 `provider/model_name` 格式（L340-L341），`--provider` flag + 环境变量；`--toolsets` 控制工具集（L48）。

**Kimi CLI**（`installed/kimi_cli.py`，606 行，与 Kimi Code 是两个不同壳子）有两个独有的公平性补丁值得抄：① **中和其他 `KIMI_*` 环境变量**（`_KIMI_ENV_OVERRIDES_TO_NEUTRALIZE`，L76-L80），防止宿主环境污染实验；② **API key 用占位符 `__HARBOR_KIMI_API_KEY__` 再替换**（L82），避免密钥落盘；并用 `litellm.register_model` 把 Moonshot 长 context 模型的 `max_context` 从 litellm 默认的 128K 钉正为真实 256K（L186-L189）——**context size 钉错会让模型提前截断，造成不公平**。 

> 注意区分两个维度：我们现有 traces 里的 `moonshot_kimi-k3` 是“**自建薄壳 + Kimi 模型**”，与这里的“**Kimi Code 壳 + Kimi 模型**”是两个不同实验点——前者测模型、后者测壳子，正好构成壳子消融的一列。

### 4.4 LangChain / DSPy 这类自带编排的怎么被接进 harness

#### Harbor 的 LangGraph adapter（`installed/langgraph.py`，371 行，全文读过）

壳子**完全不介入编排**，只做五件事：

1. **环境隔离安装**（L192-L235）：容器内 `curl` 装 uv → `uv python install <pin>` → 建独立 venv → 按 `langgraph.json` 安装。注释写明原因："task base images often ship an older system python (e.g. 3.9)"——不能污染任务镜像的 python；
2. **instruction 文件化**（L244-L249）：写 `instruction.txt` 上传，用 `--instruction-file` 传给 runner，不走命令行字符串；
3. **MCP 注入靠 configurable**（L253-L262）：注释原文——"Other installed agents (codex, goose, claude_code, ...) wire `self.mcp_servers` into their own config; the LangGraph runner's only channel to the graph is `configurable`"，所以塞 `configurable["mcp_servers"]`；
4. **结果回收靠 sidecar JSON**（L321-L371）：runner 写 `summary.json`，harness 读回 `answer_written` 与 `usage.{input,output,cache_read}_tokens`，且**当作不可信输入**处理（1MB 上限、类型校验、失败不致命）；
5. **trace 串联**（L26-L50、L287-L292）：转发 `LANGSMITH_*` 与 `HARBOR_LANGSMITH_PARENT`，让 graph 的 LangSmith trace 嵌套在 harness 的 `agent_start` span 下。

**instruction 到底怎么进 graph（`langgraph_runner.py`，308 行，一手）**：就是**塞一条 user message**，没有任何包装（L270、L289-L292）：

```python
instruction = Path(args.instruction_file).read_text()
result = await _ainvoke(
    resolved,
    {"messages": [{"role": "user", "content": instruction}]},
    invoke_config,
)
```

四个配套细节：

- **不注 system prompt**，不要求输出格式——那是 graph 自己的事（与 MCP-Universe 对 Claude Code 要补 output-format prompt 形成对比）；
- **graph 可以是两种形态**（L169-L177）：已编译图直接用，否则当工厂函数调一次得图；
- **答案提取**（L117-L118）：从 `result["messages"]` **倒序**找最后一条；
- **token 统计**（L134-L148）：逐条累加 `usage.input_tokens` / `output_tokens` / `input_token_details`，"best effort, always ints"。

**是不是 ReAct 完全由 graph 自己决定，harness 不规定、也看不到**。壳子只知道三件事：进去一条 user message、出来一串 messages、以及每条的 token 账。中间是 ReAct、Plan-and-Execute 还是多 agent 辩论，harness 不关心——代价是过程分无法从壳子侧算，只能靠 LangSmith trace 嵌套拿到内部细节。

#### Biomni（LangGraph 原生，但**不是标准 ReAct**）

`Biomni/biomni/agent/a1.py`：`StateGraph(AgentState)`（L1607）+ `MemorySaver`。图只有两个核心节点（L1610-L1636）：

- `generate` → `routing_function` 分三路：`execute` / 回 `generate` / `end`；
- `execute` → 固定回 `generate`；`START → generate`；
- 可选 `self_critic` 节点：critic 判"还不够"就塞一条 `HumanMessage("Wait... this is not enough to solve the task. Here are some feedbacks for improvement: ...")` 回去重跑（L1595-L1600）。

**动作不是 tool calling，是 XML 标签 + 正则解析的 code-act**：

- system prompt 要求每条回复必含 `<execute>` 或 `<solution>`，不得同时出现、不得空回复（L1142），代码块支持 Python / `#!R` / `#!BASH`（L1124-L1127）；
- 靠 `stop_sequences=["</execute>", "</solution>"]` 切出动作（L199）；
- 用 `re.search` 解析 `<think>` / `<execute>` / `<solution>`（L1423-L1425），解不到就退而匹配 markdown 代码块（L1430）；
- 工具调用是从**代码里**反解出来的：`parse_tool_calls_from_code`、`parse_tool_calls_with_modules`（L35-L36）；
- 三处脆弱性补丁直接写在码里：缺闭合标签自动补 `</execute>`（L1415-L1416）、完全无标签就塞纠错消息重生成（L1465）、对 GPT 系模型额外追加"必须用 XML 标签、不要用 markdown"的提醒（L1387）。

最后一条是关键证据：**文本解析式壳子必须为不同厂商模型打格式补丁**——这是"壳子引入模型偏差"的具体形态，也是我们坚持 function calling 而不用文本解析的理由。

#### HAL 的做法：同一 benchmark 配多档壳子

`hal-harness/agents/` 下是**一个 benchmark 一个（或多个）agent 目录**，典型如 `scicode_zero_shot_agent` / `scicode_tool_calling_agent` / `scicode_codex_agent` 三档并存，另有 `openai_codex_agent`、`swebench_example_agent`、`open_deep_research`。这是**壳子消融**的工程形态：同题分别跑 zero-shot / tool-calling / 商用 CLI，才能把"模型能力"与"壳子助力"拆开。

附带生态信号：全 HAL 仓库里提到 langchain/langgraph 的只有 `assistantbench_browser_agent` 一家——**LangChain 已不是评测壳子的主流选择**，主流是直调 function calling 或套商用 CLI。

#### MADE（DSPy ReAct，材料域最相关）

`MADE/src/made/agents/llm_react_orchestrator.py`：`OrchestratorReActSignature(dspy.Signature)`（L39）把状态**全部显式声明为输入字段**（L77-L92）：`chemical_system`、`stability_tolerance`、`max_stoichiometry`、`buffer_summary`、**`evaluation_history`**、`known_stable_materials` → `answer`。

**这条最值得我们抄**：它不指望模型自己在 context 里记住历史，而是把"已评估过什么、已知稳定相有哪些"作为结构化字段每轮重新喂入——**状态外置**，等于用编排层做了 context 管理，天然免疫 compaction 丢信息。

---

## 五、对本项目的结论

### 5.1 最合适的组合

1. **壳**：agent 作为 MCP client 调本项目的 mock_server。**现状**：`src/benchmark/agent_harness.py` 已实现三层（LLM function calling / MCP client / Tracer），mock_server 已是 FastMCP 真 MCP server；**待办**：模型层目前是 `requests` 直调 idealab，尚未用 litellm 抽象；
2. **history**：学 MCP-Universe 的 **tracer 模式**——在传输层拦截每次 JSON-RPC 调用，落 JSONL 事件流。**现状**：已落地，`dag_poc/traces/*.jsonl` 每行一条 `{trace_id, seq, timestamp, tool, arguments, result, latency_ms}`；
3. **判分**：终态对比 + 在 JSONL 事件流上跑 checkpoint reward。**现状**：`dag_poc/templates/fixed_discovery/template.json` 已定义 6 个 checkpoint 的 DAG，真值从 tool call 反查。

### 5.2 架构就绪度（推荐方案可行性校验）

| 前提 | 状态 | 证据 |
|---|---|---|
| mock_server 是真 MCP server | ✅ 已就绪 | `dag_poc/mcp_mock_server/server.py`：`FastMCP("lingshu-mcp-tools", stateless_http=True)` |
| 传输层 JSONL tracer | ✅ 已就绪 | `agent_harness.py` `Tracer`；`dag_poc/traces/*.jsonl` 每行一个 tool call 事件 |
| template checkpoint reward | ✅ 已就绪 | `templates/fixed_discovery/template.json`：6 个 checkpoint + helper_functions + scoring |
| 模型层 litellm 抽象 | ❌ 待办 | `agent_harness.py` 直调 idealab，无 litellm |

### 5.3 可执行清单（按优先级）

1. **壳子直接用 Harbor，不要自己再造**：40+ adapter 都支持 `self.mcp_servers` 注入，mock_server 挂进去即可，零成本获得“同题多壳对照”。**壳子矩阵建议：自建薄壳（主轴）+ Claude Code / Codex CLI / Kimi Code / Hermes Agent（商用厚壳对照）**——Kimi Code 与 Hermes 已验证可经 Harbor 直接接 mock_server（见 §四·三路径 D），注意 Kimi 现有 traces 是“薄壳+Kimi 模型”，与“Kimi Code 壳”是两个维度，勿混；
2. **我们的 MCP 注入方式已被验证可行**：Claude Code 走 `~/.claude.json`、LangGraph 走 `configurable["mcp_servers"]`、MCP-Universe 走 SDK 的 `mcp_servers=`；
3. **必须钉壳子版本并记录**：照 inspect_swe 的 `agentbinary.py`（版本 + checksum），否则闭源 CLI 自动升级会让历史分数不可复现；
4. **题面要显式要求结构化输出**：MCP-Universe 追加 output-format prompt、我们的 `SYSTEM_PROMPT` 附 JSON，是同一必要改造，不算"作弊"；
5. **状态外置优于压缩**：MADE 的 `evaluation_history` 输入字段模式，把"已查过哪些候选、已排除哪些"做成结构化上下文回灌，既控 context 又不丢证据；
6. **实验卫生清单直接照抄 Harbor**：禁遥测、session 目录隔离、模型别名全部对齐同一模型、按需禁 prompt caching；
7. **预算改"成本/token + 步数"双上限**：主约束用成本（跨模型公平），步数只作防死循环兜底；并把预算档位当难度维度（宽/紧两档）；
8. **发布路径：把我们的 benchmark 写成 Harbor task 格式**。`harbor/adapters/` 已有 `scienceagentbench/` 与 `medagentbench/` 适配层——科学与医疗 benchmark 正在向这个统一 harness 汇聚。我们同时提供原生薄壳与 Harbor adapter，就能接入它的 40+ 壳子矩阵做对照实验。

### 5.4 商用 CLI 证据链标准（2026-08-01 补充）

**核心结论：没有任何论文要求商用 CLI 提供原始 request/response JSON——stream-json 事件流就是行业标准。**

| 论文/项目 | 统计什么 | 有没有原始 request/response | 有没有 token 明细 |
|---|---|---|---|
| Terminal-Bench 2.0 | 执行时间、token 总量、模型调用次数 | ❌ 没有（只对自家 Terminus 2 报，L821 原文） | ✅ 有：GPT-5.2 用 Codex CLI = 137.5M input / 2.3M output |
| Harbor | tool 事件（stream-json）+ 成本 | ❌ 没有（CLI 内部调 API） | ✅ 有：`litellm.cost_per_token` 统一计价 |
| Inspect-SWE | 全量 transcript | ⚠️ 唯一能拿到的——靠 `sandbox_agent_bridge()` 代理 API 层 | ✅ 有（因为代理了 API 层） |
| MCP-Universe | ToolUseBlock/ToolResultBlock/ResultMessage | ❌ 没有（只解析结构化事件） | ❌ 没有 |
| HAL | 成本（Weave 记 token + cost） | ❌ 没有 | ✅ 有成本但无次数 |

**对我们的意义**：我们的证据链已经超过了绝大多数论文。QoderCLI 的 stream-json 数据（tool_use 完整事件 + tool_result 完整返回 + thinking 推理过程 + num_turns + cost）已经超过 Terminal-Bench 2.0 对商用 CLI 的报告水平。原始 request/response 只有 Inspect-SWE 的 bridge 方案能拿到，那是基础设施级改造，不是论文必须。我们自己的 agent_harness 已有 `record_llm` 捕获完整 request/response——这就是我们的 "Inspect-SWE bridge" 等价物，作为主证据链；QoderCLI/Claude Code 的 stream-json 作为补充证据链。

**要补的不是"更多数据"，而是把现有数据按论文口径整理好**。Terminal-Bench 2.0 的教训是：报数必须同时报口径（一步的定义、是否去重、tool call 与 LLM call 是否分开）。我们已在实验矩阵 §二里钉死口径。

### 5.5 对本项目的四条方法论启示

1. **薄壳选型不变，但 context 策略必须写进评测协议**：context 管理是壳子的一部分，不声明就不可复现。本项目默认采 **no-compaction + 显式 step/token 上限**，并在报告中明写；
2. **想支持商用壳跑我们的题，抱 Inspect-SWE 的 bridge 思路**：`sandbox_agent_bridge()` 把壳子在沙箱内的模型 API 调用**代理回 harness**，等于在**模型 API 层**做 tracer，与我们在 **MCP 传输层**做 tracer 可叠为双层；
3. **我们的判分路线天然免疫于 compaction 失真**：compaction 丢的是 agent 自述与中间证据，而我们的 checkpoint reward 真值一律从 tool call 反查（trace 不经 compaction），壳子无论怎么压缩，过程分仍成立——这是现有架构的隐性优势，值得写入方法论；
4. **面向未来的长程 case，compaction 应做成独立消融档**而不是默认开启："压缩后能不能接住长程目标"本身就是目标维持能力的测点（与 `research/Goal-Drift.md` 同维度）。

---

## 六、证据强度分级与自查修正

**为什么第一轮漏写商用 CLI 壳子**：第一轮把任务窄化为"对已给定的 10 条表格论断逐条验真"，只做了**正确性核查**，没做**完备性核查**（"是否遗漏整类主流范式"）。Terminal-Bench 2.0 / Harbor / Inspect-SWE 是一整类，它不在原表中，所以逐条核查永远不会发现它。教训：复核必须包含两个维度——已有论断是否为真（verify）+ 是否有整类缺失（MECE）。

**证据分级**：

| 结论 | 级别 | 说明 |
|---|---|---|
| Harbor 支持 Claude Code / Codex CLI / OpenHands，命令 `harbor run --agent claude-code --model ...` | ✅ 一手 | 官方仓库 README 原文与可执行命令 |
| Terminal-Bench 2.0 评三个商用 CLI + 三个开源 agent；壳子与模型成对报；模型影响 > 壳子影响；token 差 11 倍；调用次数只对 Terminus 2 报 | ✅ 一手 | 论文 HTML 全文逐段读取 |
| OpenHands condenser 的 tombstone/View/soft-hard 机制 | ✅ 一手 | 源码目录 README |
| Inspect-SWE 封装六个 agent，`sandbox_agent_bridge()` 代理模型 API 回 harness | ✅ 一手 | 官方文档首页 + 源码 `_claude_code/agentbinary.py` |
| Epoch AI 加入 Claude Code / Codex 第三方 scaffold | ✅ 一手 | Methodology 与 Changelog 原文 |
| tau-bench 93 行壳子、循环上限 30、litellm 模型层 | ✅ 一手 | `tool_calling_agent.py` 全文 |
| Harbor 接 Claude Code 七层改造、接 LangGraph 只塞一条 user message | ✅ 一手 | `installed/claude_code.py`、`langgraph.py`、`langgraph_runner.py` 全文 |
| Biomni 非标准 ReAct、XML 标签 + 三处模型格式补丁 | ✅ 一手 | `a1.py` 关键行 |
| MCP-Universe SDK 接 Claude Code + 追加 output-format prompt | ✅ 一手 | `mcpuniverse/agent/claude_code.py` |
| Anthropic context editing（beta header、+39%/+29%） | ⚠️ 二手偏强 | 官方文档与工程博客的搜索摘要，未逐页读原文 |
| **Codex CLI 的 `compact.rs` 路径与 token 阈值（180k/244k）、保留最近 20k 用户消息、95% 安全边际** | ⚠️ **二手，未一手验证** | 社区逆向笔记 + Medium；`raw.githubusercontent.com/openai/codex/main/codex-rs/core/templates/compact/prompt.md` 与 `tree/main/codex-rs/core/src/codex` 均 404，仓库目录可能已变。**要引用必须先 clone `openai/codex` 本体核实** |
| OpenCode 的 `PRUNE_PROTECT`=40k / `PRUNE_MINIMUM`=20k | ⚠️ 二手 | 同一份 gist，未读 `compaction.ts` 原文 |
| Claude Code ~95% 触发与 compaction prompt 原文 | ⚠️ 二手（必然） | 闭源，prompt 为社区提取，无官方可核源 |
| WildClawBench / Claw-SWE-Bench 的多 harness 对比设定 | ⚠️ 二手 | 仅搜索摘要，未读数据集页/论文正文 |
| AgentBoard 壳子 = 自带 ReAct 文本解析 | ⚠️ 外部依据 | 本地只采了数据层，壳子实现在上游 hkust-nlp/agentboard，未落盘 |

**本轮修正的两处错误**：

1. 上一轮写"同模型换壳的分差可达十几到二十多分"，源头是商业博客的"scaffold gap 超 28 点"（自报数据、无可核源）。一手数据不支持：Terminal-Bench 2.0 同模型换壳差 8.9 点，且论文结论是"模型 > 壳子"。已替换为一手数据；
2. 第一轮写"Biomni 判分 = LLM judge"错误，实际是**精确匹配**（`eval_biomni_eval1.py`：基因名/变异号/选项字母大小写无关相等，罕病比 OMIM_ID，无任何 LLM 参与）。

## 七、遗留与风险

- **Codex CLI 的 context 参数仍未一手核实**：未拉 `openai/codex` 本体（Rust，仓库大）。要引用前需单独 clone 核实；
- `.srcprobe/` 不入库（约 612MB），行号对应各仓库 2026-07-31 的 `--depth 1` HEAD，仓库更新后行号可能漂移，故关键处均同时给出符号名与原文引用；
- `.srcprobe/` 已在 `.gitignore`，不占仓库体积。
