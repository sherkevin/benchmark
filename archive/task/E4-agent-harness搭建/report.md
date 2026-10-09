# E4 · task-02 收口报告（agent harness：多模型接入 + trace 定稿 + 新契约端到端冒烟）

**核心结论**：task-02 六条交付标准全部达成——3 模型 × 3 case 共 9 组端到端冒烟全部跑通（读 case → 发 query → agent 调 9 新契约工具 → 落 trace → 出 answer_schema v0.2.0 答卷），判分器对 9 条 trace 全部可执行（退出码 0）。**但必须严格区分两件事**：①"管道验证通过"（本次已证）；②"判分适配未就位"——evaluate.py extract_tool_data 只认旧工具名/旧出参，新契约 trace 送判分时 cp1-cp5 证据集系统性为空（实测 fixed 组全 0），**冒烟分数不得误读为模型能力**。该缺口 E4 无权修（禁改 evaluate.py），已报监工裁定。

---

## 一、交付标准六条逐项核对

| # | 标准 | 状态 | 证据 |
|---|---|---|---|
| 1 | ≥3 模型各实测跑通 1 次，凭据走环境变量 | ✅ | 9 组实测（§三表）；代码内零 key 字面量，`IDEALAB_API_KEY` 环境变量优先、fallback 只读 `src/llm_api_test/config.json`（agent_harness.py `_load_llm_config`，缺 key 直接 RuntimeError） |
| 2 | trace 定稿含五要素（调用序号/工具名/入参/返回/是否命中）且时间序真实 | ✅ | tool_call 事件 = {seq, tool, arguments, result, ok, timestamp, latency_ms}；`ok` 由 result 是否含 error_code 派生（对齐 tools_schema.json meta.recording.row_format）；seq 单调 + timestamp 双保证时间序；定稿文件 `dag_poc/trace_schema.json` |
| 3 | 答卷与 answer_schema 对齐（fixed 三字段 + open 三扩展） | ✅ | SYSTEM_PROMPT 要求六字段全交（fixed 后三字段 null）；收卷主通道 submit_answer（服务端 schema 校验）、兜底文本提取；9 组答卷均含 candidates/ranking/answer（open 组含扩展字段，见 smoke_evidence 的 _response.json） |
| 4 | 适配层 MCP 后端有能跑实现 | ✅ | `ToolBackend.call_tool(name, args) -> dict` 接口；`MCPBackend` 真实实现（http://localhost:9000/mcp，含契约工具 arguments JSON 字符串封装）；`GatewayBackend` 留桩 |
| 5 | 新工具冒烟跑通（正式管道验证） | ✅ | §7 修订口径：9 个新契约工具端到端，非旧工具非正式实验；list_tools 实测 31 工具、9 契约名全在场 |
| 6 | 列明剩余缺口 | ✅ | §五 |

## 二、改造清单落点（改造复用 src/benchmark/agent_harness.py，未重写）

| 落点 | 内容 |
|---|---|
| `agent_harness.py::_load_llm_config` | API key 环境变量化（IDEALAB_API_KEY/LLM_API_KEY），删除旧 L34 字面量兜底；endpoint 可由 IDEALAB_ENDPOINT 覆盖 |
| `agent_harness.py::SUPPORTED_MODELS / CONTRACT_TOOL_NAMES` | 3 模型名单、9 契约工具名常量 |
| `agent_harness.py::Tracer.record` | tool_call 事件新增 `ok` 命中标志 |
| `agent_harness.py::Tracer.save_scoring_view / token_usage / submitted_payload` | 判分投喂投影（纯 tool_call、无 type 字段，与 evaluate.py 输入契约同形）、token 汇总、submit_answer 收卷提取 |
| `agent_harness.py::ToolBackend / MCPBackend / GatewayBackend` | 工具通道适配层（§2.3） |
| `agent_harness.py::AgentHarness.setup` | 契约工具对 LLM 暴露 tools_schema.json 真实入参 schema（替换 MCP 面 {arguments:string} 包壳）；描述仍从 list_tools 动态取 |
| `agent_harness.py::SYSTEM_PROMPT` | 旧工具名 → 9 新契约工具工作流 + 六字段交卷要求 |
| `agent_harness.py::run_case` | 双轨落盘（判分文件 + _full 全量证据流）、落盘前清同名旧文件、response 增补 submission_channel/tool_calls_ok/token_usage |
| `src/benchmark/run_all.py` | 重写为冒烟批跑入口（3 case × 3 模型，按 case 提示 DAGPOC_CASE_ID） |
| `dag_poc/trace_schema.json` | 新建定稿（两类事件全字段/必填性 + 双轨文件约定 + 与录像行格式、_event_args_ret 双格式的兼容声明 + 判分缺口登记） |

## 三、冒烟结果表（3 case × 3 模型，全部实测）

**读法警示**：cp1-cp5≈0 是判分侧缺口（不认识新契约工具名/出参形状），不是模型能力结论；open 组非零分来自 legacy open 路径，同样只证管道可判、不证三门闭环。

| case | 模型 | 状态 | steps | tools(命中 ok) | 收卷通道 | answer | gt | tokens | evaluate cp1..cp6 | evaluate |
|---|---|---|---|---|---|---|---|---|---|---|
| case_cns_metoprolol | qwen3.7-max | ✅跑通 | 15 | 36(17) | None（未交卷） | - | aa | 153065 | 0/0/0/0/0/0 | exit 0 |
| case_cns_metoprolol | bailian/glm-5.2 | ✅跑通 | 13 | 33(20) | submit_answer | ec | aa | 131048 | 0/0/0/0/0/0 | exit 0 |
| case_cns_metoprolol | moonshot/kimi-k3 | ✅跑通 | 14 | 30(18) | text_fallback | ec | aa | 142669 | 0/0/0/0/0/0 | exit 0 |
| case_cardiac_sotalol | qwen3.7-max | ✅跑通 | 15 | 27(2) | submit_answer | NO_SOLUTION | ba | 133313 | 0/0/0/0/0/0 | exit 0 |
| case_cardiac_sotalol | bailian/glm-5.2 | ✅跑通 | 15 | 35(1) | text_fallback | NO_SOLUTION | ba | 139086 | 0/0/0/0/0/0 | exit 0 |
| case_cardiac_sotalol | moonshot/kimi-k3 | ✅跑通 | 15 | 35(6) | text_fallback | NO_SOLUTION | ba | 153061 | 0/0/0/0/0/0 | exit 0 |
| case_open_balanced_superconductor | qwen3.7-max | ✅跑通 | 14 | 31(10) | text_fallback | xb | -（open 无 gt） | 133520 | 0/0.167/1.0/0.4/0/0（legacy open 路径） | exit 0 |
| case_open_balanced_superconductor | bailian/glm-5.2 | ✅跑通 | 15 | 71(12) | submit_answer | xb | - | 164246 | 同上 | exit 0 |
| case_open_balanced_superconductor | moonshot/kimi-k3 | ✅跑通 | 15 | 63(11) | text_fallback | xb | - | 177266 | 同上 | exit 0 |

**open case 性质声明（必写）**：case_open_balanced_superconductor 是旧"约束最优型"open；真 A/B 发现型 open case 目前为 0（等 E5）。本次 open 冒烟**只验证管道通，不验证 open 三门判分闭环**（hit(G_B)/anchor∈A/edit_op 复现 answer 需真 open case 与材料侧身份键数据）。

### 判分执行记录（命令与退出码）

```bash
# 驱动（逐条调 score_case/open_case_metric，禁改区只读）：9/9 可执行、0 异常
.venv/bin/python task/E4-agent-harness搭建/_run_smoke_eval.py   # exit 0
# evaluate.py 原生批扫（cwd=dag_poc）：可执行、不崩溃
cd dag_poc && ../.venv/bin/python ../src/benchmark/evaluate.py  # exit 0
```

明细落盘：`task/E4-agent-harness搭建/_smoke_eval_results.json`。

### trace 样例路径

- 判分文件：`dag_poc/traces/case_cns_metoprolol__qwen3.7-max.jsonl`（纯 tool_call 投影）
- 全量证据流：`dag_poc/traces/case_cns_metoprolol__qwen3.7-max_full.jsonl`（tool_call + llm_call 交织，含 attempt/token）
- 答卷：`dag_poc/traces/case_cns_metoprolol__qwen3.7-max_response.json`
- **快照（防外部还原，见 progress.md 范围外发现 1）**：`task/E4-agent-harness搭建/smoke_evidence/`（9 组 × 3 文件 = 27 份）

### 冒烟命令（可复现）

```bash
python3.12 -m venv .venv   # 已建
.venv/bin/pip install -i https://mirrors.aliyun.com/pypi/simple/ "rdkit>=2025.09" "mcp>=1.9,<2.0" uvicorn fastapi requests
# 终端 1（每个 case 重启一次，装载对应 B 分区/case.props）
cd dag_poc/mcp_mock_server && DAGPOC_CASE_ID=case_cns_metoprolol ../../.venv/bin/python mock_server.py
# 终端 2
.venv/bin/python src/benchmark/run_all.py case_cns_metoprolol   # 默认 3 模型全跑
```

## 四、已知坑核对（任务书五条全部遵守）

1. kimi-k3 强制 temperature=1：保留（llm_chat 首行），实测无 MPE-001；
2. 429/Throttling 指数退避：保留，重试 attempt 记入 llm_call 事件；
3. 模型名斜杠落盘转下划线：保留（run_case model_tag），与 evaluate.py 批扫命名一致；
4. 旧 63 份 trace 只参考格式不作基线：新 trace 落盘前清同名旧文件；
5. 实验后已杀 mock_server（端口 9000 已释放）、清理 /tmp 临时日志。

## 五、剩余缺口清单

1. **evaluate.py 判分适配（阻塞，报监工裁定）**：extract_tool_data 只认旧工具名与旧出参形状——literature_search 新契约返 papers[].doc_id 而判分读 doi；search_candidates/get_property/get_entity/compare 不在其名单。新契约 trace 的 cp1-cp5 证据系统性为空（fixed 组实测全 0）。E4 禁改 evaluate.py，需判分侧适配新契约工具面后重判。
2. **真 A/B 发现型 open case 缺失（等 E5）**：open 三门判分闭环（hit(G_B) × grounding 三判据）无法端到端验证；材料侧身份键/G_B 数据未冻结前 grounding_mode 只能落占位口径。
3. **E3-03 装包协同状态**：E4 已在本任务 .venv 装好 mcp（1.29.0，注意 2.x 移除 mcp.server.fastmcp 须钉 <2.0）并实跑 e2e 通道；E3-03 若在其环境补装，建议同版本口径。
4. 辅助观察（非阻塞，详见 progress.md 范围外发现）：case 题面不含合法性质名导致大量 UNKNOWN_PROPERTY 试错；mock_db 关键词检索口径偏窄导致大量 EMPTY_RESULT。两者属 E2/E5/E1 范畴。
5. **外部还原风险**：dag_poc/traces 已跟踪文件曾被外部流程 git 还原为旧作废内容（2026-08-07 01:35:53），已重跑并留快照，请监工排查还原源。
