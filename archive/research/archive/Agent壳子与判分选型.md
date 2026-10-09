> **已并入《测试 Agent 调研总报告》（本目录同名文件，以该页为准）。** 本文为选型与复核分章的原始记录，保留供检索与溯源回查；内容与结论以总报告为唯一权威。

---

# 测试 Agent 壳子与判分方式选型调研

> 目的：为材料/医药 benchmark 选定"跑分用的测试 agent"形态。调研对象 = 本项目已落盘的 35+ benchmark 中有代表性的壳子实现 + 补充的 MCP 系与 harness 系新范式。
> 状态：已复核（本地 benchmark 以 `reference/` 一手材料为准，外部以官方仓库/论文为准），逐条结论见文末「复核记录」；表格内容已按复核修正。

## 核心结论

**薄壳（MCP client + function calling）+ 传输层 JSONL tracer + 双轨判分**，与本项目现有架构（mock_server + template checkpoint）完全对齐。

## 主流 benchmark 的壳子方式

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

## 趋势一句话

**壳子在极简**（mini-SWE-agent 是标志：壳子越薄，差异越归于模型）+ **记录在标准化**（messages/JSONL/JSON-RPC 结构化事件流，文本解析式正被淘汰）+ **判分在双轨**（终态是底座，过程分是差异化加分项，且过程分都消费结构化轨迹）。

## 补充调研：用商用 CLI agent（Claude Code / Codex / Hermes）当壳子的现状

### 一、有人用，且已是 2026 年的主流对照方式

| 评测方 | 怎么用商用 CLI 当壳 | 依据 |
|---|---|---|
| **Terminal-Bench 2.0 / Harbor** | Harbor harness 内置 Claude Code、Codex CLI、Gemini CLI、OpenHands adapter；排行榜条目本身就是“**壳+模型**”成对标注（如 `Codex CLI, GPT-5.5` / `Claude Code, Claude Opus 4.5`） | tbench.ai/leaderboard；github.com/harbor-framework/harbor（“Evaluate arbitrary agents like Claude Code, OpenHands, Codex CLI”）；Terminal-Bench 论文（arXiv:2601.11868，评三个商用 CLI + 三个开源 SWE agent） |
| **Epoch AI · SWE-bench Verified** | 默认用自建极简 loop，2026-02-13 起额外“加入第三方 scaffold（Claude Code 和 Codex），经 **Inspect-SWE** 评测” | epoch.ai/benchmarks/swe-bench-verified（Methodology + Changelog） |
| **Inspect-SWE**（基础设施） | 把 `claude_code()` / `codex_cli()` / `gemini_cli()` / `kimi_code()` / `opencode()` / `mini_swe_agent()` 封成标准 Inspect agent，可直接当 solver | meridianlabs-ai.github.io/inspect_swe |
| **多 harness 对比型 benchmark** | WildClawBench（OpenClaw / Claude Code / Codex CLI / **Hermes Agent** 跑同一批 60 任务）、Claw-SWE-Bench（同任务同预算下比不同 harness） | ModelScope WildClawBench 数据集页；arXiv:2606.12344 |
| **反向对照** | mini-SWE-agent 把“beats Claude Code and Codex”当卖点——说明商用壳已是默认 baseline | github.com/swe-agent/mini-swe-agent |

结论：商用 CLI agent 当壳不仅有人用，而且已从“可选”变成“**必报对照**”；代价是排行榜必须把壳子当一级变量公开标注。壳子影响的**真实量级**（Terminal-Bench 2.0 一手数据）：同为 GPT-5.2，Codex CLI 62.9% vs Terminus 2 54.0%（相差 8.9 个点）；但论文明确给出反向结论——“**模型选择通常比 agent scaffold 更重要**”（Codex CLI 从 GPT-5-Nano 换到 GPT-5.2，解决率 +52%；Gemini-2.5-Pro 从 OpenHands 换到 Terminus 2，仅 +17%）。

### 二、源码可得性分三档

| 壳子 | 源码 | context 管理实现位置 |
|---|---|---|
| **Codex CLI** | ✅ 开源（openai/codex，Rust） | `codex-rs/core/src/compact.rs`（含重试退避）、`codex-rs/core/templates/compact/prompt.md` 与 `summary_prefix.md`、`config/mod.rs`（`model_auto_compact_token_limit`）、`model_family.rs`（`effective_context_window_percent`=95%） |
| **OpenCode** | ✅ 开源（sst/opencode） | `packages/opencode/src/session/compaction.ts`（`isOverflow()`、`PRUNE_PROTECT`=40k、`PRUNE_MINIMUM`=20k）、`prompt/compaction.txt` |
| **OpenHands** | ✅ 开源（SDK） | `openhands-sdk/openhands/sdk/context/condenser/`（目录 README 为一手）：策略是**定期把“前一半事件”整体替换为单个 summary event**；因事件日志 append-only，用 `Condensation` tombstone 事件（类 Cassandra/Kafka）记录“遗忘”，`View` 类负责应用压缩得到 LLM 可见视图；触发分 soft（`max_tokens`/`max_size` 超限，可跳过下步再试）与 hard（显式请求/上下文溢出，必须压，实在不行走 `LLMSummarizingCondenser.hard_context_reset`）；官方列出四项权衡：单次成本、缓存重建成本、早期上下文（摘要会被再摘要）、近期上下文（后一半不动） |
| **Hermes Agent** | ✅ 开源（NousResearch/hermes-agent） | 它的 compaction 改造讨论直接引 Codex 架构（issue #499），并已接 Anthropic 服务端 Context Editing API（issue #526） |
| **mini-SWE-agent** | ✅ 开源 | 无 compaction：全量 linear messages + step 上限 + 观测截断（这正是它“薄”的代价与优势） |
| **Claude Code** | ❌ 闭源（仅 issue 仓库） | 机制公开：~95% 容量触发 auto-compact，`/compact` 可带自定义指令，摘要后开新 session；官方另将能力下沉为 API：**context editing**（tool result clearing / thinking clearing，beta header `context-management-2025-06-27`）+ memory tool，官方测得两者合用 +39%、仅 context editing +29% |

### 三、context 管理的三种路线（可直接对照源码）

1. **摘要式 compaction**（主流）：达阈值 → 全史 + 专用 prompt 让 LLM 写 handoff 摘要 → 重建历史。差异在三个参数：阈值（Claude Code 百分比 ~95%；Codex 给绝对 token 阈值，按模型分档）、保留什么（Codex 额外保留最近 ~20k tokens 的用户消息）、摘要如何回插（Codex 用 `summary_prefix` 明说“上一个模型的思路摘要”）；
2. **裁剪式 pruning**（更轻、信息损失可控）：OpenCode 倒扫 tool call，保护最近 40k tokens 工具输出、可剪量 >20k 才动手；Anthropic 的 tool result clearing 同类思路（官方称之为“最轻触的 compaction”）；
3. **不管**：mini-SWE-agent（靠 step/token 上限克制）、OpenHands `NoOpCondenser`、Amp（不做自动压缩，只给 handoff/fork/thread-reference 工具，靠人工纪律）。

共同代价：多次 compaction 累积信息损失（Codex 源码里就写了警告文案），且 compact 发生在任务中段时容易跑偏。

### 四、对本项目的四条启示

1. **薄壳选型不变，但 context 策略必须写进评测协议**：context 管理是壳子的一部分，不声明就不可复现。本项目默认采 **no-compaction + 显式 step/token 上限**（当前 harness 已是此行为：全量 messages、`max_steps` 默认 15），并在报告中明写；
2. **想支持商用壳跑我们的题，抱 Inspect-SWE 的 bridge 思路**：它用 `sandbox_agent_bridge()` 把壳子在沙箱内的模型 API 调用**代理回 harness**，所以闭源壳子也能拿到全量 transcript 并受 token/time 预算约束——等于在**模型 API 层**做 tracer，与我们在 **MCP 传输层**做 tracer 可叠为双层；
3. **我们的判分路线天然免疫于 compaction 失真**：compaction 丢的是 agent 自述与中间证据，而我们的 checkpoint reward 真值一律从 tool call 反查（trace 不经 compaction），故壳子无论怎么压缩，过程分仍成立——这是现有架构的一个隐性优势，值得写入方法论；
4. **面向未来的长程 case，compaction 应做成独立消融档**而不是默认开启：当题面长到必预压缩时，“压缩后能不能接住长程目标”本身就是目标维持能力的测点（与 `research/Goal-Drift.md` 同维度）。

### 五、context 策略差异直接写在 token 账上（一手数据）

Terminal-Bench 2.0 附表：同为 **GPT-5.2**，Codex CLI 消耗 **137.5M input / 2.3M output** tokens，而 Terminus 2（论文自建的中立薄壳）仅 **12.4M input / 2.6M output**——输出量相当，**输入量差 11 倍**。这就是壳子的 context 策略（历史回灌、缓存读、重试、压缩前的重复输入）造成的。对我们的含义：一旦引入 cost-aware 指标，**壳子而非模型会主导成本列**，所以成本对比必须固定壳子才有意义。

### 六、证据强度分级与自查修正（第三轮）

本节回答两个问题：为何第一轮未写商用 CLI 壳子；哪些结论是一手、哪些仍是二手。

**第一轮遗漏的归因**：第一轮我把任务窄化为“对已给定的 10 条表格论断逐条验真”，只做了**正确性核查**，没做**完备性核查**（“是否遗漏整类主流范式”）。而 Terminal-Bench 2.0 / Harbor / Inspect-SWE 这条线恰好是一整类——它不在原表中，所以逐条核查永远不会发现它。教训：复核必须包含两个维度——已有论断是否为真（verify）+ 是否有整类缺失（MECE）。

**证据分级**：

| 结论 | 证据级别 | 说明 |
|---|---|---|
| Harbor 支持 Claude Code / Codex CLI / OpenHands，命令为 `harbor run --agent claude-code --model ...` | ✅ 一手 | 官方仓库 README 原文与可执行命令 |
| Terminal-Bench 2.0 评三个商用 CLI + 三个开源 agent；壳子与模型成对报；模型影响 > 壳子影响；token 差 11 倍 | ✅ 一手 | 论文 HTML 全文（§2.1/§3.2/§3.4/§4/附表）已逐段读取 |
| OpenHands condenser 的 tombstone/View/soft-hard 机制 | ✅ 一手 | 源码目录 README |
| Inspect-SWE 封装六个 agent，用 `sandbox_agent_bridge()` 代理模型 API 回 harness | ✅ 一手 | 官方文档首页 |
| Epoch AI 加入 Claude Code / Codex 第三方 scaffold | ✅ 一手 | Methodology 与 Changelog 原文 |
| Anthropic context editing（beta header、tool result clearing、+39%/+29%） | ⚠️ 二手偏强 | 官方文档与官方工程博客的搜索摘要，未逐页读原文 |
| **Codex CLI 的 `compact.rs` 路径、`model_auto_compact_token_limit` 阈值（180k/244k）、保留最近 20k 用户消息、95% 安全边际** | ⚠️ **二手，未一手验证** | 来自一份社区逆向笔记（badlogic gist）+ Medium 文章；本轮尝试抓取 `raw.githubusercontent.com/openai/codex/main/codex-rs/core/templates/compact/prompt.md` 与 `tree/main/codex-rs/core/src/codex` **均 404**，仓库目录可能已变。要引用到论文/方案里必须先 clone 仓库核实 |
| OpenCode 的 `PRUNE_PROTECT`=40k / `PRUNE_MINIMUM`=20k | ⚠️ 二手，同上一来源 | 同一份 gist，未读 `compaction.ts` 原文 |
| Claude Code ~95% 触发与 compaction prompt 原文 | ⚠️ 二手（必然） | 闭源，prompt 为社区提取，无官方可核源 |
| WildClawBench / Claw-SWE-Bench 的多 harness 对比设定 | ⚠️ 二手 | 仅搜索摘要，未读数据集页/论文正文 |

**本轮修正的一处错误**：上一轮我写“同模型换壳的分差可达十几到二十多分”，源头是一篇商业博客的“scaffold gap 超 28 点”（自报数据、无可核源）。一手数据不支持这个量级：Terminal-Bench 2.0 同模型换壳差 8.9 点，且论文结论是“模型 > 壳子”。已就地替换为一手数据。

## 对本项目最合适的组合

1. **壳**：agent 作为 MCP client 调本项目的 mock_server。**现状**：`src/benchmark/agent_harness.py` 已实现三层（LLM function calling / MCP client / Tracer），mock_server 已是 FastMCP 真 MCP server；**待办**：模型层目前是 `requests` 直调 idealab OpenAI-compatible 端点，尚未用 litellm 抽象，多厂商换型需自行适配（不影响壳子形态，仅影响换模型成本）；
2. **history**：学 MCP-Universe 的 **tracer 模式**——在传输层拦截每次 JSON-RPC 调用，落 JSONL 事件流，与模型/壳子解耦。**现状**：已落地，`dag_poc/traces/*.jsonl` 每行一条 `{trace_id, seq, timestamp, tool, arguments, result, latency_ms}`；
3. **判分**：终态对比 + 在 JSONL 事件流上跑 checkpoint reward。**现状**：`dag_poc/templates/fixed_discovery/template.json` 已定义 6 个 checkpoint 的 DAG（pre/next/weight/pass_threshold/reward 表达式），真值从 tool call 反查。

## 复核记录

总结：**8 条 benchmark 论断中 6 条属实，1 条错（Biomni 判分），1 条依据不在本地（AgentBoard 壳子）；项目自身 4 项架构前提中 3 项已就绪，1 项（litellm）是待办而非现状**。核心结论（薄壳 + JSONL tracer + 双轨判分）不变。

### 一、修正项（原论断不实）

| # | 原论断 | 复核结果 | 依据 |
|---|---|---|---|
| 1 | Biomni 判分 = 终态 LLM judge | ❌ 错。实际是**精确匹配**：基因名/变异号/选项字母大小写无关相等，罕病比 OMIM_ID，patient_gene 比集合交集非空，无任何 LLM 参与 | `reference/Biomni/eval_biomni_eval1.py` L74-L155 `_compute_reward` |
| 2 | 本项目壳子"模型用 litellm 抽象" | ⚠️ 写成现状不实。`agent_harness.py` 直接 `requests.post` 调 idealab，全文无 litellm；已改标为待办 | `src/benchmark/agent_harness.py` L36-L74 |
| 3 | ChemCrow 与 Biomni 归并为一行 | ⚠️ 两者判分不同（前者 LLM/人工，后者精确匹配），且 Biomni 已支持挂 MCP server；已拆为两行 | `reference/ChemCrow/agent_chemcrow.py`、`reference/Biomni/README.md` L259-L269 |
| 4 | MCP-Bench "双轨" 等同于"过程分+终态分" | ⚠️ 语义不同。它的两轨是 **rule-based schema understanding + LLM-as-judge（o4-mini，硬编码）**，四个维度均无 ground-truth 终态；已在表中注明 | github.com/Accenture/mcp-bench README（Overall Score 定义行、runner.py L429-436 judge 硬编码） |

### 二、属实项（逐条实证）

| # | 论断 | 依据 |
|---|---|---|
| 5 | SWE-bench Verified：mini-SWE-agent bash-only 约 100 行；纯终态跑 FAIL_TO_PASS/PASS_TO_PASS；轨迹 `.traj` JSON | 本地 `reference/SWE-bench_Verified/README.md` L47/L81-L83；外部 swe-agent 官网 trajectories 文档、mini-swe-agent 仓库（“does not have any tools other than bash”） |
| 6 | tau-bench：tool-calling 壳 + LLM 模拟用户（llm/react/verify/reflection 四种用户策略）；Pass^1..4 | `reference/tau-bench/REPO_README.md` L15-L36、L62-L120（DB 终态对比为论文/上游代码依据，本地 README 未直述） |
| 7 | TheAgentCompany：OpenHands 壳；result-based（primary）+ subcheckpoints（secondary）双轨；trajectory 用于给部分分 | `REPO_README.md` L108-L124、L188-L193；`docs_EVALUATION.md` L120-L126（注：官方对 trajectory **格式无强制要求**，“JSONL event stream” 是 OpenHands 实现层的产物） |
| 8 | MADE：DSPy/LLM ReAct 编排（`llm_react_orchestrator.py` 56KB）；判分 = AF/EF/AUDC/mSUN 发现效率；轨迹 `trajectories/episode_*.json` | `reference/MADE/README_采样说明.md` L16-L30；`README_REPO.md` L56-L103（注：轨迹是逐 episode 的 JSON，不是 JSONL） |
| 9 | MCP-Universe：MCP client + 专设 Tracer 层，`mcpuniverse.tracer.collectors` 提供 Memory/File/SQLiteCollector，按 `trace_id` 取全量记录；execution-based evaluator（format/static/dynamic） | mcp-universe.github.io/usage.html（Foundation Layer 含 Tracers；`trace_collector.get(trace_id)` 示例）；arXiv:2508.14704 |
| 10 | HAL：统一 harness 壳，各 benchmark 原生判分，全量 trace 由 W&B Weave 记录并开放下载 | hal.cs.princeton.edu（“full traces ... every single model call as logged by W&B Weave”）；princeton-pli/hal-harness |
| 11 | METR/RE-Bench：符 METR Task Standard，配 modular-public 壳子 + Vivaria 平台；终态 scoring function | `reference/RE-Bench/REPO_README.md` L10-L16 |

### 三、依据不足项

| # | 论断 | 说明 |
|---|---|---|
| 12 | AgentBoard 壳子 = 自带 ReAct 文本解析 | 本地只采了**数据**（`DATASET_README.md` + case jsonl），可确认 progress rate（subgoal/match 两种）+ success rate 双轨与 subgoals 字段；壳子实现在上游 hkust-nlp/agentboard 仓库，未落盘。该条保留，但标为外部依据 |

### 四、本项目架构就绪度（推荐方案可行性校验）

| 前提 | 状态 | 证据 |
|---|---|---|
| mock_server 是真 MCP server | ✅ 已就绪 | `dag_poc/mcp_mock_server/server.py` L29/L167：`FastMCP("lingshu-mcp-tools", stateless_http=True)` |
| 传输层 JSONL tracer | ✅ 已就绪 | `agent_harness.py` L126-L177 `Tracer`；`dag_poc/traces/*.jsonl` 每行一个 tool call 事件 |
| template checkpoint reward | ✅ 已就绪 | `templates/fixed_discovery/template.json` L115-L219：6 个 checkpoint + helper_functions + scoring |
| 模型层 litellm 抽象 | ❌ 待办 | `agent_harness.py` 直调 idealab，无 litellm；不阻碍当前跑分，作为工程项录入 TODO |
