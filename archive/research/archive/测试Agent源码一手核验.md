> **已并入《测试 Agent 调研总报告》（本目录同名文件，以该页为准）。** 本文为源码核验分章的原始记录，保留供检索与行号回查；内容与结论以总报告为唯一权威。

---

# 测试 Agent 源码一手核验（12 个 benchmark 仓库）

> 方法：`git clone --depth 1` 拉取 12 个 benchmark/harness 仓库到 `.srcprobe/`（已 gitignore，不入库），逐个读实现代码。本文所有论断标注**文件路径 + 行号**，均为本轮亲自读到的源码，非搜索摘要。
> 复现拉取：`mkdir -p .srcprobe && cd .srcprobe && git clone --depth 1 https://github.com/<repo>.git`（仓库清单见第一节）。
> 回答四个问题：谁可复现 / Claude Code 这类怎么被使用 / LangChain·DSPy 这类怎么编排 / 最薄的壳长什么样。

## 一、可复现性分级（拉取日期 2026-07-31）

判据：最近提交时间、依赖锁定、容器化、一键入口、判分脚本是否随仓库发布。

| 仓库 | 最近提交 | 依赖锁定 | 容器 | 判定 |
|---|---|---|---|---|
| **harbor-framework/harbor** | 2026-07-31（当天） | `uv.lock` | `docker/` + `registry.json` | ★★★ 最可复现。40+ agent adapter + 26 个 benchmark adapter，一条命令跑 |
| **OpenHands/software-agent-sdk** | 2026-07-30 | `uv.lock` | 有 | ★★★ condenser 源码齐全，接口稳定 |
| **meridianlabs-ai/inspect_swe** | 2026-07-29 | `uv.lock` | 依赖 Inspect sandbox | ★★★ 六个 CLI agent 封装，**版本+checksum 钉死**（见第三节） |
| **laude-institute/terminal-bench** | 2026-07-10 | `uv.lock` | `docker/` + `original-tasks/` | ★★★ 任务与判分同仓 |
| **princeton-pli/hal-harness** | 2026-07-01 | 无 lock | 无 | ★★ 活跃但依赖未锁 |
| **SalesforceAIResearch/MCP-Universe** | 2026-06-23 | `requirements.txt` | `docker/` | ★★ 有 Makefile/pytest，tracer 完整 |
| **diffractivelabs/MADE** | 2026-05-08 | `uv.lock` | 无 | ★★ 材料域，Hydra 配置齐全 |
| **sierra-research/tau-bench** | 2026-03-18 | 无 lock、无 docker | 无 | ★★ 代码极简易读，但 README 自标"tasks 已过期，请用 τ³-bench" |
| **snap-stanford/Biomni** | 2026-01-14 | `biomni_env/`（conda） | 无 | ★ 环境重（conda 全量生信栈），判分需拉 HF 数据集 |
| **Accenture/mcp-bench** | 2025-10-07（停更 9 个月） | 无 | 无 | ✗ 判分依赖 o4-mini 硬编码，外部 MCP server 会漂移 |
| **ur-whitelab/chemcrow-public** | 2024-12-19（停更 19 个月） | 无 | 无 | ✗ README 自述"不含论文全部工具，**不会给出与论文相同结果**" |

**结论：重点看 harbor / inspect_swe / OpenHands SDK / terminal-bench 这四家，它们是 2026 年事实标准。ChemCrow 与 mcp-bench 不必再投入。**

## 二、最薄的壳长什么样：tau-bench `ToolCallingAgent`（93 行，全文读过）

`tau-bench/tau_bench/agents/tool_calling_agent.py`：

- **模型层**：`from litellm import completion`（L4）——换模型不换壳，靠 `custom_llm_provider` 参数；
- **主循环**（L39-L74）：`for _ in range(max_num_steps)` → `completion(messages, tools=self.tools_info)` → `message_to_action()` 把 `tool_calls[0]` 转成 `Action` → `env.step(action)` 拿 `reward` 与 `observation`；
- **两种动作分流**（L53-L72）：工具调用 → 追加 `role: tool` 消息；`respond` 动作 → 追加 `role: user`（即 LLM 模拟用户的回话）；
- **判分**：`reward` 直接由环境每步返回（L51），壳子不参与判分；
- **成本**：`res._hidden_params["response_cost"]`（L48）累加；
- **context 管理**：无。全量 messages 一路追加，只有 `max_num_steps=30` 兜底。

对照我们的 `src/benchmark/agent_harness.py`：结构同构（LLM → action → env/MCP → 追加 messages → 上限兜底），差别只有两点——我们用 `requests` 而它用 `litellm`；我们的 reward 在 trace 上离线算而它在环境里在线返回。

## 三、Claude Code 这类闭源 CLI 怎么被使用：三条路径，全都做了改造

### 路径 A：Harbor —— CLI subprocess + stdin 管道（`harbor/src/harbor/agents/installed/claude_code.py`，1579 行）

实际执行的命令（L1553-L1564，原文）：

```
printf "%s" "$instruction" | claude --verbose --output-format=stream-json \
  [--settings ...] [其他 flags] [--continue] --print 2>&1 | tee /logs/agent/claude-code.txt
```

**是"直接输入 query"，但外面套了七层改造**：

1. **instruction 走随机环境变量再 unset**（L1547-L1556）：`harbor_claude_code_instruction_<uuid>` 传入后立刻 `unset`，再 `printf` 管道喂 stdin——避免题面出现在进程列表、也绕开 shell 转义；
2. **prompt 模板钩子**（`@with_prompt_template`，L1381；实现在 `installed/base.py:914-918`）：`render_instruction()` 默认**恒等**（无模板即原样传），配了 `_prompt_template_path` 才渲染。所以"是否改造 query"是可配置项，默认不改；
3. **非交互 + 结构化输出**：`--print`（一次性）+ `--output-format=stream-json`（事件流），全量落 `/logs/agent/claude-code.txt`；
4. **预算与权限全部走 CLI flag**（L46-L96）：`--max-turns`、`--max-budget-usd`、`--effort`、`--fallback-model`、`--append-system-prompt`、`--allowedTools`、`--disallowedTools`、`--permission-mode`；
5. **换模型不换壳靠环境变量**（L1464-L1487）：`ANTHROPIC_MODEL` / `ANTHROPIC_BASE_URL`，走自定义 base_url 时把 sonnet/opus/haiku/subagent 四个别名**全部指向同一模型**，防止壳子偷偷调小模型；
6. **实验卫生**（L1490-L1502）：`CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1`（禁遥测）、`IS_SANDBOX=1`（容器内允许 bypassPermissions）、`CLAUDE_CONFIG_DIR` 指向本次 trial 目录（session 隔离）、可选 `DISABLE_PROMPT_CACHING`、`CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING`；
7. **能力注入三件套**（L1302-L1355）：skills 复制、memory 文件写入、**MCP server 写进 `~/.claude.json` 的 `mcpServers`**——注释明确说选 `~/.claude.json` 而非项目级 `.mcp.json`，因为后者需显式启用。

**额外一手发现**：轨迹解析器专门处理 compaction 造成的事件重放（L944、L1049 注释："an event replayed after compaction"），说明厚壳的 context 压缩会污染轨迹、必须在 harness 侧去重。成本用 `litellm.cost_per_token` 统一计价（L695）。

### 路径 B：MCP-Universe —— 官方 Python SDK（`mcpuniverse/agent/claude_code.py`，166 行）

用 `claude_code_sdk` 的 `ClaudeSDKClient` + `ClaudeCodeOptions`（L10-L11），传 `mcp_servers=self.get_mcp_configs()`、`system_prompt=prompt`（L104-L110），再解析 `ToolUseBlock` / `ToolResultBlock` / `ResultMessage` 落 tracer（L141 附近还要把 `mcp__server__tool` 形式的工具名拆开）。

**这条路径明确改造了 query**：`message = message + "\n\n" + self._get_output_format_prompt(output_format)`（L99）——追加输出格式要求，否则拿不到可判分的结构化答案。我们的 `SYSTEM_PROMPT` 强制附 JSON 是同一手法。

### 路径 C：inspect_swe —— 沙箱内装钉版本二进制 + API 桥（`inspect_swe/src/inspect_swe/_claude_code/`）

- `agentbinary.py`：`resolve_version("stable"|"latest"|semver)` → 取 `manifest.json` → **校验 `expected_checksum`** → 缓存到 `claude-<version>-<platform>`（L16-L35、L56-L84）；
- `claude_code.py:14`：`sandbox_agent_bridge()`——壳子在沙箱内的模型 API 调用被代理回 Inspect，于是闭源壳也能拿全量 transcript 并受 token/time 限制；
- `version` 选项支持 `auto | sandbox | stable | latest | <semver>`（L131）。

**这是三条路径里唯一解决"闭源壳可复现"的**：CLI 会自动升级，不钉版本就无法复现历史结果。

## 四、LangChain / DSPy 这类自带编排的怎么被接进 harness

### Harbor 的 LangGraph adapter（`installed/langgraph.py`，371 行，全文读过）

壳子**完全不介入编排**，只做五件事：

1. **环境隔离安装**（L192-L235）：容器内 `curl` 装 uv → `uv python install <pin>` → 建独立 venv → 按 `langgraph.json` 的 `source`/`dependencies` 安装。注释写明原因："task base images often ship an older system python (e.g. 3.9)"——不能污染任务镜像的 python；
2. **instruction 文件化**（L244-L249）：写 `instruction.txt` 上传，用 `--instruction-file` 传给 runner，不走命令行字符串；
3. **MCP 注入靠 configurable**（L253-L262）：注释原文——"Other installed agents (codex, goose, claude_code, ...) wire `self.mcp_servers` into their own config; the LangGraph runner's only channel to the graph is `configurable`"，所以塞 `configurable["mcp_servers"]`；
4. **结果回收靠 sidecar JSON**（L321-L371）：runner 写 `summary.json`，harness 读回 `answer_written` 与 `usage.{input,output,cache_read}_tokens`，且**当作不可信输入**处理（1MB 上限、类型校验、失败不致命）；
5. **trace 串联**（L26-L50、L287-L292）：转发 `LANGSMITH_*` 与 `HARBOR_LANGSMITH_PARENT`，让 graph 的 LangSmith trace 嵌套在 harness 的 `agent_start` span 下。

#### instruction 到底怎么进 graph（`langgraph_runner.py`，308 行，一手）

就是**塞一条 user message**，没有任何包装（L270、L289-L292）：

```python
instruction = Path(args.instruction_file).read_text()
...
result = await _ainvoke(
    resolved,
    {"messages": [{"role": "user", "content": instruction}]},
    invoke_config,
)
```

四个配套细节：

- **不注 system prompt**，不要求输出格式——因为那是 graph 自己的事（与 MCP-Universe 对 Claude Code 要补 output-format prompt 形成对比）；
- **graph 可以是两种形态**（L169-L177）：已编译图（有 `invoke`/`ainvoke` 属性）直接用，否则当工厂函数调一次得图；
- **答案提取**（L117-L118）：从 `result["messages"]` **倒序**找最后一条作为答案；
- **token 统计**（L134-L148）：逐条 message 累加 `usage.input_tokens` / `output_tokens` / `input_token_details`（含 cache read），“best effort, always ints”。

**是不是 ReAct 完全由 graph 自己决定，harness 不规定、也看不到**。壳子只知道三件事：进去一条 user message、出来一串 messages、以及每条的 token 账。中间是 ReAct、Plan-and-Execute 还是多 agent 辩论，harness 不关心——代价是过程分无法从壳子侧算，只能靠 LangSmith 的 trace 嵌套拿到内部细节。

### Biomni（LangGraph 原生，但**不是标准 ReAct**）

`Biomni/biomni/agent/a1.py`：`StateGraph(AgentState)`（L1607）+ `MemorySaver`（L14）。它的图只有两个核心节点（L1610-L1636）：

- `generate` → `routing_function` 分三路：`execute` / 回 `generate` / `end`；
- `execute` → 固定回 `generate`；`START → generate`；
- 可选 `self_critic` 节点：critic 判“还不够”就塞一条 `HumanMessage("Wait... this is not enough to solve the task. Here are some feedbacks for improvement: ...")` 回去重跑（L1595-L1600）。

**动作不是 tool calling，是 XML 标签 + 正则解析的 code-act**：

- system prompt 要求每条回复必含 `<execute>` 或 `<solution>`，且不得同时出现、不得空回复（L1142），代码块支持 Python / `#!R` / `#!BASH`（L1124-L1127）；
- 靠 `stop_sequences=["</execute>", "</solution>"]` 切出动作（L199）；
- 用 `re.search` 解析 `<think>` / `<execute>` / `<solution>`（L1423-L1425），解不到就退而匹配 markdown 代码块（L1430）；
- 工具调用是从**代码里**反解出来的：`parse_tool_calls_from_code`、`parse_tool_calls_with_modules`（L35-L36）；
- 三处脆弱性补丁直接写在码里：缺闭合标签就自动补 `</execute>`（L1415-L1416）、完全无标签就塞纠错消息重生成（L1465）、对 GPT 系模型额外追加“必须用 XML 标签、不要用 markdown”的提醒（L1387）。

最后一条是关键证据：**文本解析式壳子必须为不同厂商模型打格式补丁**，这就是“壳子引入模型偏差”的具体形态，也是我们坚持 function calling 而不用文本解析的理由。

### HAL 的做法：同一 benchmark 配多档壳子

`hal-harness/agents/` 下是**一个 benchmark 一个（或多个）agent 目录**，典型如 `scicode_zero_shot_agent` / `scicode_tool_calling_agent` / `scicode_codex_agent` 三档并存，另有 `openai_codex_agent`、`swebench_example_agent`、`open_deep_research` 等。这是**壳子消融**的工程形态：同题分别跑 zero-shot / tool-calling / 商用 CLI，才能把“模型能力”与“壳子助力”拆开。

附带一条生态信号：全 HAL 仓库里提到 langchain/langgraph 的只有 `assistantbench_browser_agent` 一家——**LangChain 已不是评测壳子的主流选择**，主流是直调 function calling 或套商用 CLI。

### MADE（DSPy ReAct，材料域最相关）

`MADE/src/made/agents/llm_react_orchestrator.py`：`OrchestratorReActSignature(dspy.Signature)`（L39）把状态**全部显式声明为输入字段**（L77-L92）：`chemical_system`、`stability_tolerance`、`max_stoichiometry`、`buffer_summary`、**`evaluation_history`**、`known_stable_materials` → `answer`。

**这条最值得我们抄**：它不指望模型自己在 context 里记住历史，而是把"已评估过什么、已知稳定相有哪些"作为结构化字段每轮重新喂入——**状态外置**，等于用编排层做了 context 管理，天然免疫 compaction 丢信息。

## 五、一个 case 跑多少次 LLM 调用？（自写 agent 的循环与预算，一手）

**先给结论：一个 case = 一次题面输入 + 一次交卷，但中间是几次到几十次 LLM 调用的循环。绝不可能一次调用跑到底。**

### 〇、谁发起"第二次调用"？——壳子程序，不是 LLM

这是最容易误解的点。**LLM 本身是一次性的、无状态的 HTTP 调用**，每次请求发出去拿到响应就结束了，它不会"自己再调一次"。所谓"二次调用"是**壳子这个 Python 程序**在循环里**再次发 HTTP 请求**，与第一次是两次完全独立的请求。

源码证明（tau-bench）：驱动者是 `python tau_bench/run.py` 这个进程。`run.py:42` 用 `agent_factory` 造出 `ToolCallingAgent` **对象**，`run.py:78` 调它的 `agent.solve(...)` **方法**——全程是普通 Python 代码在跑。循环在 `tool_calling_agent.py:39-74`：

```python
for _ in range(max_num_steps):                    # 壳子的 for 循环
    res = completion(messages=messages, ...)      # 每次循环发一次全新 HTTP 请求
    next_message = res.choices[0].message.model_dump()
    action = message_to_action(next_message)      # 模型只产出"意图"
    env_response = env.step(action)               # 壳子替它真正执行工具
    reward = env_response.reward
    messages.extend([next_message, {...}])        # 壳子把意图+结果追加进历史
    if env_response.done:                         # 终止由壳子判定
        break
```

三个关键事实：

1. **LLM 无记忆**：它不记得上一次调用。所以壳子每次请求都把**整个 `messages` 历史重新发一遍**——这就是 context 随轮数线性膨胀、也是各家要管 context 的根本原因；
2. **"再来一次"不是 LLM 的决定，是壳子的循环**：LLM 每次只负责产出`下一步`的意图（调哪个工具，或者"我答完了"）；壳子代码决定要不要执行工具、要不要循环；
3. **"结束"也是一个约定信号**：模型不再返回 `tool_calls`（或返回 `respond`/`done` 动作）时，壳子就 `break` 停止调用。模型本身从不知晓循环的存在。

所以问题的三个"谁"答案是：**循环的发起者是壳子程序；次数是壳子里的常量（`max_num_steps=30`）；结束是壳子检测"无 tool_calls"后主动 break。**

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
模型这次看到结果，回 `{"role":"assistant","content":"您的退款已办理。"}`，**没有 tool_calls** → 壳子判定 done → `break`，循环结束。

这**两次 HTTP 调用之间 LLM 没有"活着"**，中间是壳子在干活。

### 为什么不能一次调用跑完

因为**工具结果必须回灌**。LLM 一次调用只能产出"我想调 `predict_admet(aa,ab,ac)`"这个意图，它拿不到结果；壳子执行完工具后，必须把返回值作为新消息追加进 messages，再发起**下一次** LLM 调用，模型才能看到 ADMET 数值并决定下一步。所以有 N 次工具调用，就至少有 N+1 次 LLM 调用。

用我们自己的实测数据走一遍：`dag_poc/traces/case_cns_metoprolol__qwen3.7-max.jsonl` 一个 case 记了 12 条工具调用事件（seq 0-11：文献检索 → 靶点校验 → 候选库检索 → ADMET 预测 → 分子生成 → 再 ADMET → 出报告），对应 13 次左右 LLM 调用。这**一个** case 只交一次卷（`ranked_candidates`），但内部跑了十几轮。

### 各家的循环结构与上限（源码常量，逐条一手）

| 壳子 | 循环写法 | 终止条件 | 上限默认值 |
|---|---|---|---|
| **tau-bench** | `for _ in range(max_num_steps)`（`agents/base.py:12`、`tool_calling_agent.py:39`） | `env_response.done` 或步数用尽 | **30 步** |
| **MCP-Universe ReAct** | `for iter_num in range(self._config.max_iterations)`（`agent/react.py:145`） | 模型给出终答或迭代用尽 | **5 轮**（`react.py:45`）——轮数本身当难度参数 |
| **mcp-bench** | `for round_num in range(1, max_rounds+1)`（`agent/executor.py:102`），轮内还有 `for attempt in range(max_retries)` 重试（L673） | 配置驱动 | 配置文件决定 |
| **mini-SWE-agent** | 每步自增 `n_calls`，超限抛 `LimitsExceeded`（`agents/default.py:132-137`） | **成本或步数任一超限** | `step_limit=0`（不限）、**`cost_limit=3.0` 美元**（L26-L28） |
| **Biomni** | LangGraph `generate ⇄ execute` 循环 | 出现 `<solution>` 标签或触发递归上限 | **`recursion_limit: 500`**（`a1.py:1774`），单次代码执行另有 600 秒 timeout（L222） |
| **ChemCrow** | LangChain `RetryAgentExecutor` | `Final Answer:` 或迭代用尽 | **`max_iterations=40`** |
| **MADE** | episode 内迭代提议→评估 | **oracle 查询预算耗尽** | **`budget: 5`**（`configs/environment/convex_hull.yaml:7`） |
| **本项目** | `for step in range(max_steps)`（`agent_harness.py`） | 无 `tool_calls` 即视为完成 | **15 步** |

### 三条从数字里读出来的规律

1. **上限差了两个数量级（5 → 500），因为"一步"的粒度不同**：MCP-Universe 一步 = 一次 ReAct 推理（粒度粗，5 轮够）；Biomni 一步 = 执行一小段代码（粒度极细，才需要 500）。所以**跨 benchmark 比"步数"没有意义，必须先对齐一步的定义**；
2. **先进做法是用成本上限替代步数上限**：mini-SWE-agent 默认 `cost_limit=3.0` 美元、`step_limit=0`（即不限步数只限钱）。理由是不同模型每步效率差异大，限步数会系统性偏袒"少步高质"的模型，而限成本对所有模型是同一把尺；Epoch AI 同理（2M uncached + 20M cached token 预算，超了强制交卷）；
3. **预算本身就是难度旋钮**：MADE 直接把 oracle 预算 5 次写进环境配置——"用 5 次查询找到稳定材料"和"用 50 次"是两个难度的题。

### 对本项目的三条动作

1. **`max_steps=15` 偏紧，需要改**：我们一步 = 一次工具调用（细粒度），实测 CNS case 已用掉 12 步，长程 case 必然撞顶。参照 tau-bench 的 30 起步；
2. **改成"成本/token 预算 + 步数"双上限**：主约束用成本（跨模型公平），步数只作防死循环的兜底；两者都写进结果元数据，否则跨模型结果不可比；
3. **把预算档位当难度维度**：同一 case 分"宽预算/紧预算"两档跑，紧预算下的过程分才真正区分"会不会规划"——这正是 MADE 用 oracle 预算做的事，也是我们 checkpoint DAG 能提供的差异化。

### 四、这些论文报告"一次 query 的调用次数"吗？——只有 Terminal-Bench 2.0 认真报

**结论：绝大多数论文不报调用次数，只报结果指标（success rate / pass^k / 成本）；唯一认真把"每次 query 的模型调用次数"当一级数据报告的是 Terminal-Bench 2.0。**

| 论文 | 报什么 | 报调用次数吗 |
|---|---|---|
| **Terminal-Bench 2.0** | Figure 10 报 agent 执行时间、token、**模型调用次数**；Figure 35 报平均 turns/trial | ✅ 报，但**只对 Terminus 2**（论文 L821："number of model (API) calls was recorded for Terminus 2 runs only"）。量级：多数 trial <25 次调用、<10M token；极端 2 小时、数百次调用、近 100M token（L139/L825）。并用 `average episodes` 作效率指标（L2901） |
| **MCP-Universe** | Success Rate、Average Evaluator Score、**Average Steps**（排行榜页） | ⚠️ 有 "Average Steps" 列，但论文正文未见统一口径的调用次数表，"一步" 是 ReAct 轮数 |
| **AgentBoard** | success rate + progress rate（**progress rate 相对交互步数计算**） | ❌ 步数只用于算 progress rate，不把调用次数当独立指标报 |
| **MADE** | AF/EF/AUDC/mSUN + oracle 预算/episode | ❌ 报的是"查询预算/episode"，不直接报 LLM 调用次数 |
| **HAL** | **成本**（Weave 记 token usage + cost tracking） | ❌ 明确转向报成本不报次数（README："focus on accuracy while ignoring costs"是其要解决的问题） |
| **tau-bench** | pass^k | ❌ 不报调用次数 |

#### 三个值得写进我们论文的点

1. **调用次数是个被普遍遗漏、但能区分工作量与效率的一级指标**。Terminal-Bench 2.0 用它发现一个重要事实：**平均 turns/trial 与成功率基本无相关**（L139："essentially no correlation between the number of average turns per trial and model success rates"）——即"调得多"不等于"调得好"。这正是我们 checkpoint 过程分要回答的：不是走了多少步，而是**每一步对不对**；
2. **必须统一口径，否则壳子间不可比**。Terminal-Bench 2.0 只对 Terminus 2 报调用次数（其他壳子拿不到统一数字）；其附录还专门定义了 **tool-call 去重规则**（L1263-L1266："distinct tool-call blocks (with block-level deduplication) ... Each tool-call block counts as at most one attempt"）。我们报"每 case 调用次数"时必须同时报：**一步的定义、是否去重、tool call 与 LLM call 是否分开**；
3. **调用次数与成本要分开报，不能互相替代**。HAL 只报成本、Terminal-Bench 只报次数，都偏。正确做法是两个都报（次数衡量过程、成本衡量经济），加上我们的过程分（衡量正确性），构成三维：效率 × 成本 × 正确性。

## 六、对本项目的可执行结论

1. **壳子直接用 Harbor，不要自己再造**：它已有 `claude_code` / `codex` / `hermes` / `langgraph` / `openhands` / `mini_swe_agent` 等 40+ adapter，且每个 adapter 都支持 `self.mcp_servers` 注入——我们的 mock_server 挂进去即可，等于零成本获得"同题多壳对照"这一实验维度；
2. **我们的 MCP server 注入方式已被验证可行**：Claude Code 走 `~/.claude.json`、LangGraph 走 `configurable["mcp_servers"]`、MCP-Universe 走 SDK 的 `mcp_servers=`，三条路都不需要改题面；
3. **必须钉壳子版本并记录**：照 inspect_swe 的 `agentbinary.py` 做法（版本 + checksum），否则闭源 CLI 自动升级会让历史分数不可复现；
4. **题面要显式要求结构化输出**：MCP-Universe 追加 output-format prompt、我们的 `SYSTEM_PROMPT` 附 JSON，是同一必要改造，不算"作弊"；
5. **状态外置优于压缩**：MADE 的 `evaluation_history` 输入字段模式，比让壳子自行 compaction 更适合我们的长程 case——把"已查过哪些候选、已排除哪些"做成结构化上下文回灌，既控 context 又不丢证据；
6. **实验卫生清单直接照抄 Harbor**：禁遥测、session 目录隔离、模型别名全部对齐同一模型、按需禁 prompt caching——这些不做，跨模型对比就不公平；
7. **发布路径：把我们的 benchmark 写成 Harbor task 格式**。`harbor/adapters/` 下已有 26 个外部 benchmark 的适配层，其中包括 **`scienceagentbench/`**（含 `adapter.py`、`llm_visual_judge.py`、`validate_tasks.py`）与 **`medagentbench/`**（含 `adapter.py`、`refsol_pkg/`、`template/environment/medagentbench_cli.py`、`template/solution/oracle.py`）——说明科学与医疗领域的 benchmark 已经在向这个统一 harness 汇聚。我们若同时提供原生薄壳与 Harbor adapter，就能直接接入它的 40+ 壳子矩阵做对照实验。

## 七、遗留与风险

- 上一份报告中"Codex CLI 的 `compact.rs` 路径与 token 阈值"仍未一手核实：本轮拉的是 harbor/inspect_swe 等 harness 仓库，未拉 `openai/codex` 本体（Rust，仓库大）。要引用前需单独 clone 核实；
- `.srcprobe/` 不入库（12 个仓库共约 500MB），本文所有行号对应各仓库 2026-07-31 的 `--depth 1` HEAD，仓库更新后行号可能漂移，故关键处均同时给出符号名与原文引用。
