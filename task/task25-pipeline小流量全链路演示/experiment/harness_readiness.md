# task25 站6 · harness 通路核查报告

> 2026-09-17 实测。凭据纪律：全程只用 `IDEALAB_AK` 环境变量（从
> `/mnt/ai4sci_develop_fast/jingwu/init` 第 42 行提取，值不落任何输出）；
> 未读、未使用 `~/.claude/settings.json`（chenxihe 软链，scope.md 规则 3 禁用）。

## 0. 环境事实（与任务书预期不符的三点，先说清）

| 事实 | 实测 | 影响 |
|---|---|---|
| 仓库 `.venv` **不存在** | `ls .venv` 无此目录；README/ws_runner 里 `PY = ROOT/.venv/bin/python` 全部悬空 | workspace 路线（ws_runner_*）无法按既有命令跑 |
| `mcp` 包**全机不可用** | 系统 python3.10.14（/usr/local/conda）与 5 个 conda env（abedit/cpa/gears/scdiff/state）均无 `mcp`；仓库无 vendored 副本；`requirements.txt` 钉 `mcp<2`（1.29.0 可用，pip 索引可达） | 一切 `ws_mcp_server_*.py` 与 `dag_poc/mcp_mock_server/*.py`（FastMCP）**起不来**；装它 = 装新依赖 = task25 §4 停止条件 3，**未装** |
| `~/.claude` = chenxihe 软链 | `readlink -f ~/.claude → /mnt/ai4sci_develop_fast/home/chenxihe/.claude` | `ws_isolation.read_auth()` 只认这条路的凭据 ⇒ workspace 路线即使解决 mcp 也**凭据违规**，禁走 |

⇒ **本环境唯一合规的实跑通路** = `chain_harness_stream.py` 的直连路线：
idealab OpenAI 兼容端点（function-calling + SSE 流式）+ **进程内工具**（不经 MCP）
+ AK 走 `IDEALAB_AK` env（`ak.read_ak()` 第一优先级）。冒烟即按此通路执行。

## 1. read_ak() env 分支 —— ✅ 已实测通

```bash
export IDEALAB_AK="$(sed -n 's/^export ANTHROPIC_AUTH_TOKEN=//p' /mnt/ai4sci_develop_fast/jingwu/init | tr -d '\"')"
python3 -c "import sys; sys.path.insert(0,'src/probe_headless'); import ak, os; \
  got=ak.read_ak(); print(got==os.environ['IDEALAB_AK'], len(got))"
# 输出: True 32
```

设了 `IDEALAB_AK` 后 `read_ak()` 返回 env 值（True），长度 32（值不打印）。
`chain_harness_stream.py` 在 **import 时**执行 `AK = read_ak()`（模块级），
所以只要先 export 再启动，就全程走 jingwu 通路、零 chenxihe 接触。

**API 端到端 auth 探针（1 次最小请求，非评测 run）**：
EP `https://idealab.alibaba-inc.com/api/openai/v1/chat/completions`，model `qwen3.7-max`，
HTTP 200、finish=stop、回复 pong、usage 正常返回。✅ 已实测通。

## 2. chain_harness_stream.py 调用方式 —— ⚠️ 仅代码阅读 + 实测发现不可直接用

确切命令行（形式上）：

```bash
export IDEALAB_AK=...   # 不回显
python3 src/probe_headless/chain_harness_stream.py <model> <tag> [temperature]
# 例: python3 src/probe_headless/chain_harness_stream.py qwen3.7-max smoke 0
```

但**题面与真值硬编码**到 `src/probe_headless/case9.json` + `prompt_case9.txt`（`{BUDGET}` 注入）。
实测比对：`case9.json` 是 case_sc_chain_01 的**旧坏真值快照**——预算 34（冻结权威 18）、
measurable 与 held_out 重叠 4 项（O4.047/O4.180/O6.160/O6.270），正是
case README「真值的三个构造缺陷（已修）」里那版；golden 的 stepB_Q1/stepC_truth 也与
冻结 artifacts 不一致。权威真值 =
`cases/materials/chain/fixed_basic_tools/case_sc_chain_01/artifacts/ground_truth.json`，
权威判分器 = 同目录 `judge.py`（judge_contract 契约：只吃 answer.json + tool_calls.jsonl）。

**处置（按任务书边界条款）**：不硬改既有脚本。写了最小 wrapper
`experiment/smoke_harness_wrapper.py`：`import chain_harness_stream as H`，运行时把
`H.TRUTH/MEASURABLE/HELD_OUT/BUDGET` 替换为冻结 GT 内容，复用 `H.stream_turn()`
（流式+限流退避+分片累积）与 `H.make_tools()`（工具语义+账本落盘逐行原实现）；
主循环逐行对齐原 `__main__`（100 turn、NUDGE_MAX=3 双额度、api_error.json 剔除、
answer.json 仅真提交后落盘）；题面按 prompt_case9.txt 同构模板从冻结 GT 数据渲染。

## 3. mock server 启动 —— ❌ 阻塞（mcp 包缺失，属停机条件）

- 主通道 `dag_poc/mcp_mock_server/mock_server.py`（端口 9000，streamable HTTP `/mcp`）：
  ```bash
  cd dag_poc/mcp_mock_server && python mock_server.py     # 默认 PORT=9000
  ```
  实测 `python3 mock_server.py` → `ModuleNotFoundError: No module named 'mcp'`（第 57 行
  `from mcp.server.fastmcp import FastMCP`）。备用通道 `gateway_mcp.py`（端口 9001）同因阻塞。
- 91MB 库在 `dag_poc/data/backend/`（86M）+ `anonymization_mapping.json`（16M），文件齐。
- **进程内数据层可用（已实测，不经 MCP 协议）**：`dag_poc/gateway.py` 纯 stdlib 可 import
  （init 约 43s，含 rdkit 结构解析告警）；`Gateway().call('search_candidates',
  {'keywords':'z002730','k':2})` 命中返回 `{id: z002730, entity_type: molecule,
  structure_hint: FORMULA:C13H10N2O4;ATOMS:29}`；`check_molecule_exists` 信封正常。
  `src/data_build/per_case_view.py` 的 `ViewedBackendReader('case_dh_drug_01')` 可实例化，
  38 份 per-case view 在位（`dag_poc/data/backend/per_case_views/`）。
- 结论：**mock 后端数据面通、MCP 传输面阻塞**。阻塞原因单一且可解（owner 批准
  `pip install 'mcp==1.29.0'` 或提供带 mcp 的 venv 即恢复），但按纪律本任务未装。
  冒烟链路不依赖它：chain 类工具是进程内实现（`make_tools`），账本格式与
  MCP server 落盘格式相同（`{"tool","args","result"}` jsonl），judge_contract 通吃。

## 4. agent 连 mock server 的接线 —— 仅代码阅读（被 §3 阻塞）

两条既有接线，都要求 mcp 包：

| 路线 | 接线方式 | 凭据来源 | 本环境状态 |
|---|---|---|---|
| dag_poc harness（`src/benchmark/agent_harness.py`） | HTTP：`MCPClient(url='http://localhost:9000/mcp')` POST + SSE；server 用 `PORT=9000 python mock_server.py` 起 | `.env` 的 `IDEALAB_API_KEY`/`LLM_API_KEY`（或 dogfooding 档 `DOGFOODING_API_KEY`） | server 起不来 ⇒ 阻塞 |
| workspace（`ws_isolation.run_case`） | stdio：mcp.json 写 `{"command": PY, "args": [ws_mcp_server_*.py], "env": {CASE_FILE, WORKSPACE}}`，claude CLI（/usr/bin/claude 2.1.274 在位）挂 MCP 子进程 | `ws_isolation.read_auth()` **只读** `~/.claude/settings.json` = chenxihe 软链 | 凭据违规 + PY(.venv) 悬空 + mcp 缺 ⇒ 三重阻塞 |

若 owner 解 mcp 依赖后走 workspace 路线，还需给 `ws_isolation` 提供合规凭据注入
（如允许 `read_auth` 读 `IDEALAB_AK`/jingwu settings 的小改——属改既有口径，须 owner 裁定）。

## 5. BBB 可用性判断题的判分器 —— ❌ 无现成，**需站7 写**

- 全仓无 `judge_*bbb*`；`src/probe_headless/bbb_*.py`（constant_answer_baseline /
  variance_decomposition / naked_difficulty_crossref）是基线与方差分解脚本，
  均无 `judge_one/score/classify` 判分三件套。
- `cases/` 下无 BBB case 目录（drug/atom 只有 qed_01、aedit_01、triage_01 等）。
- 可用模板：`cases/drug/atom/fixed_some_tools/case_dr_qed_01/judge_some.py` 模式
  （转发唯一权威 judge.py + judge_contract 的 load/classify/score/report/batch_rows），
  以及 chain 的 `judge.py`（本次冒烟已实测其 CLI 三种给法中的 run_dir 形态）。
- task25 §2 站7 的判分口径 `L = I(答错) + c·I(弃答)`（c 网格）没有现成实现，站7 新写。

## 6. 判分链路（judge_contract）—— ✅ 已实测通（0 API 干跑 + 冒烟 run 实判）

- 干跑：合成满分 answer.json（真值来自冻结 artifacts）+ 现成 trace 账本
  （`trace/qwen3.7-max_calls.jsonl` 拷贝）→ `judge.py <run_dir>` 输出 total=1.0，
  四关 cpA/cpB/cpC/cpD 全 1.0（现场留档 `experiment/smoke/judge_dryrun/`）。
- 实判：冒烟 run 1（真 LLM）→ total=0.8253（见 smoke_verdict.json）。
- 契约确认：只吃 `answer.json` + `tool_calls.jsonl` 两文件路径；`api_error.json`/
  `tool_leak.json` 在位即整条剔除（口径 B）；未交卷计 0 分不剔除。

## 7. 汇总表

| 环节 | 状态 | 证据 |
|---|---|---|
| read_ak() env 分支 | ✅ 已实测通 | True + len=32（§1） |
| idealab API auth | ✅ 已实测通 | HTTP 200 pong（§1） |
| chain_harness_stream 直连通路 | ✅ 已实测通（经 wrapper） | 冒烟 run 1（§2、smoke/） |
| mock server（MCP 9000/9001） | ❌ 阻塞：无 mcp 包（停机条件，未装） | ModuleNotFoundError（§3） |
| mock 后端数据面（Gateway 进程内） | ✅ 已实测通 | search_candidates 命中（§3） |
| agent×MCP 接线（两条路线） | ❌ 阻塞（依赖 mcp 包/凭据路线违规） | §4 |
| BBB 判分器 | ❌ 无现成，需站7 写 | §5 |
| judge_contract 判分链路 | ✅ 已实测通 | 干跑 1.0 + 实判 0.8253（§6） |
