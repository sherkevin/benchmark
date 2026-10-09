# E4 · harness 接口契约（trace / 答卷 / 模型 / 接缝 / 输出 / adapter）

> 状态：✅ 已交付（task_id=13，2026-08-07）。供 E5/E6 及后续接入方并行开工。
> 口径原则：**本文全部内容以已落地实测代码为准**——代码基准为 `src/benchmark/agent_harness.py`（711 行实测在位），机器可读规格一律引用既有 schema 文件路径，不复制粘贴大段 JSON；本文与代码冲突时以代码为准并回报 E4。

**速读总览**：harness 对外契约共六条——① trace 是 JSONL 事件流，双轨落盘（判分投影 + `_full` 全量），格式定稿见 `dag_poc/trace_schema.json` v1.0.0；② 答卷直出 `dag_poc/answer_schema.json` v0.3.0 六字段（fixed 后三字段填 null，open 三扩展字段填真值，operator 仅 add_atom/remove_atom）；③ 模型走 idealab 单一端点、凭据只进 `.env`、温度统一 `temperature_for(model)`；④ case_id 视图接缝 open 类硬报错不静默降级；⑤ trace 输出目录三级优先级解析、默认不落 `dag_poc/traces/`；⑥ 新接任何真实 harness 必须走既有 adapter 共用同一 trace 契约（硬条款，见 §六）。

---

## 一、trace 格式

**小结**：一条 run 落两份 JSONL——判分投影（纯 tool_call、无 type 字段，与历史 63 份 trace 同形）+ `_full` 全量证据流（tool_call + llm_call 交织、含 type）；格式唯一基准是已定稿的 schema 文件。

- **格式基准（只读引用）**：`dag_poc/trace_schema.json`（v1.0.0，status=定稿）。事件结构、文件命名、时间序约定均以其 `event_types` / `file_convention` / `time_ordering` 节为准。
- **tool_call 五要素 + ok**：`trace_id / seq / timestamp / tool / arguments / result / latency_ms` + 派生字段 `ok`。`ok` 口径（`Tracer.record`）：`result` 不含 `error_code` 且不含 `error` → true；异常兜底 `{"error": ...}` 记 false。与 `dag_poc/tools_schema.json` meta.recording.row_format 官方口径一致。
- **llm_call（ACTION_RULES §7 留痕）**：完整 request payload + 完整 response 原样 + `usage`/`finish_reason`（在 response 内）+ `latency_ms` / `attempt` / `seq` / `timestamp` + 索引三元组 `trace_id/case_id/model` + `code_version`（跑批时 git HEAD 短 hash，非 git 环境为 `nogit`）。
- **双轨落盘**：`<case_id>__<model_tag>.jsonl`（判分文件，`save_scoring_view` 投影：只含 tool_call、去掉 type 字段）；`<case_id>__<model_tag>_full.jsonl`（全量流，`save` 原样）；答卷另落 `<case_id>__<model_tag>_response.json`（`{"case_id","model","response","submission_channel",...}`，字段名与 evaluate.py 读取口径不变）。同名旧文件写前先删，防混写。
- **与 evaluate.py 的双格式兼容**：判分器 `extract_tool_data` 逐事件直读 `e['tool']`，只吃判分投影文件（混入无 tool 键的 llm_call 会 KeyError）；`_full` 流是论文证据链，判分器不读。新契约 9 工具与 legacy 的双轨分派由 evaluate.py 侧 `_NEW_CONTRACT_TOOL_NAMES` 名单处理（E3 commit b85f4a7 起），harness 侧无需感知。

## 二、答卷字段

**小结**：答卷直出 answer_schema v0.3.0 六必填字段；收卷双轨——submit_answer 主通道（服务端 schema 校验）+ 最终文本 JSON 兜底。

- **字段基准（只读引用）**：`dag_poc/answer_schema.json`（**v0.3.0**，additionalProperties=false）。required 六字段：`candidates / ranking / answer / anchor_molecules / cited_values / edit_op`。
- **fixed 类**：前三字段（candidates/ranking/answer）填真值，后三扩展字段填 null。
- **open 类三扩展字段**：`anchor_molecules`（锁定的锚点分子 ID 列表）、`cited_values`（引用的性质值，须与工具返回一致）、`edit_op`（`{"anchor","operator","detail"}`）。
- **operator 枚举（0.3.0 收窄，硬约束）**：仅 `add_atom` / `remove_atom`（组成算子封闭集，见 answer_schema.json `properties.edit_op.properties.operator.enum`）；`detail` 格式 `<token>` 或 `<token>*<n>`（如 `"F"`、`"F*2"`）。**旧三算子 replace_substituent / substitute_element / ring_closure 已无判分语义，答卷中不得使用**；harness 的 SYSTEM_PROMPT 已按此引导（用了判不过）。
- **收卷双轨（task-02 §4）**：主通道取最后一次服务端 `accepted=true` 的 `submit_answer` payload（`Tracer.submitted_payload`）；主通道无果则从最终文本提取最后一个合法 JSON 块兜底（`_extract_json`）。result 里 `submission_channel ∈ {"submit_answer", "text_fallback", None}`。SCHEMA_VIOLATION 时按 errors 修正只允许重交一次（SYSTEM_PROMPT 约束）。

## 三、模型配置

**小结**：凭据纪律 + 三模型名单 + 温度统一函数 + 重试全留痕，四处均已在代码落地并实测。

- **凭据（纪律红线）**：代码内禁止 key 字面量；唯一来源为仓库根 `.env`（已 gitignore）的 `IDEALAB_API_KEY`（备选名 `LLM_API_KEY`），进程环境变量可覆盖。缺失时 `agent_harness` 导入即抛 RuntimeError。
- **模型名单**：`.env` 的 `IDEALAB_MODELS`（逗号分隔）提供；缺省回退历史口径三档 `qwen3.7-max` / `bailian/glm-5.2` / `moonshot/kimi-k3`（`SUPPORTED_MODELS`）。
- **温度约束**：一律调 `agent_harness.temperature_for(model)`，不要各处各写判断。**kimi-k3 硬约束**：temperature=0 返 HTTP 400 MPE-001（实测），只能取 1；该差异由 `.env` 的 `IDEALAB_TEMPERATURE_OVERRIDE`（格式 `模型名:值[,模型名:值]`）声明。`llm_chat` 未显式传 temperature 时自动按模型解析（task-04 起删除了 moonshot 硬编码判断）。
- **重试留痕**：限流/临时错误（429/Throttling/MPE-429）指数退避最多 5 次（2,4,8,16,30s 封顶）；每次尝试（含失败）都经 `Tracer.record_llm` 落 trace，带 `attempt` 序号与 `latency_ms`，失败记 `{"error","status_code"}`。

## 四、case_id 视图接缝

**小结**：参数名统一 `case_id`；现行通道 `DAGPOC_CASE_ID` 校验 + 会话头 `X-Dagpoc-Case-Id` 预留；open 类缺失/不一致硬报错，无默认值不跳过；通道终确认待 E3 §10 落地。

- **case 身份判定**（`resolve_case_identity`）：case_id 从 case JSON 读（不另建映射）；含 `gt` 字段 → fixed，无 gt → open。
- **open 类硬报错**：缺 case_id，或现行通道 `DAGPOC_CASE_ID` 缺失/与 case_id 不一致 → 抛 `CaseIdentityError`（不静默降级——静默会让 A/B 隔离失守而无人知）。fixed 类仅 WARN 不阻断，但接口路径统一。实测断言 8/8 PASS（`task/E4-agent-harness搭建/t04_verify_view_wiring.py`）。
- **双通道与 mock_server 的关系**：环境变量管"server 启动时装载哪个 case 的 B 分区"（mock_server.py L76 按 cases[case_id] 装载），会话头管"本会话查询走哪个 case 的视图"（E3 §10 gateway 侧，工作区已实施但 HTTP 头留二期，当前生效通道为 env）。二者同值互补；常量 `CASE_ID_ENV` / `CASE_ID_HEADER` 集中在 agent_harness.py 顶部。
- **状态（探测于 2026-08-07 21:28，结论仅对探测时刻负责，前置状态以 `t04_verify_view_wiring.py --joint` 复跑输出为准）**：E3 §10 视图加载**已实施并提交**（commit 6c14fdd，21:25，gateway.py +130 行：ENV_CASE_ID/'DAGPOC_CASE_ID' + _resolve_case_view + CASE_VIEW_MISSING fail-closed；已与 E3 实现对齐——env 通道与 CASE_ID_ENV 同名同义，HTTP 头 X-Dagpoc-Case-Id 双方均定位二期预留）。阻塞收窄为单一前置：**本 harness open case 的视图文件 case_open_balanced_superconductor.json 未建**（per_case_views/ 已 23 份，drug/material 系列均已入库）；断言脚本 `--joint` 已带前置自动探测（实时输出实况，不依赖人工快照），前置到位后一条命令合跑。

## 五、输出路径

**小结**：`resolve_trace_dir` 三级优先级，默认落本任务独立目录，不再写 `dag_poc/traces/`。

- 优先级（实测，`resolve_trace_dir(trace_dir)`）：**显式参数 > 环境变量 `TRACE_OUT_DIR` > 默认 `task/E4-agent-harness搭建/traces_new/`**（`DEFAULT_TRACE_DIR`，返回绝对路径）。
- 批跑入口 `run_all.py` 提供 `--trace-dir` 参数并打印实际落盘目录。
- 背景：`dag_poc/traces/` 是 git 跟踪的历史判据 fixture（ADR-0014 修订 R1 不得改写）且曾发生外部还原事件；新 trace 一律落 traces_new（task-03 成果，延续有效）。

## 六、adapter 要求（给 E6 / 任何新 harness 接入方）——硬条款

**小结**：新接真实 harness（商用 CLI agent 等）必须走既有 adapter 转成同一 trace 契约，不另立格式。

1. **硬条款**：二期接入真实 harness 时，其原始输出**必须**经 `src/benchmark/cli_trace_adapter.py`（stream-json → 本契约事件流）转换后再投喂判分与评估，共用 §一的 trace 契约与 §二的答卷契约；**禁止另立 trace 格式或判分投影**。现支持 QoderCLI / Claude Code 的 `-o stream-json`（事件结构一致，自动识别）；新 CLI 若事件结构有差异，扩展 adapter 本体而非旁路。
2. **转换后必须满足的最低形状**：每条事件含 `type=tool_call` / `trace_id` / `seq` / `timestamp` / `tool` / `arguments` / `result`；工具名剥掉 `mcp__lingshu-mock__` 前缀（`MCP_PREFIX`）。
3. **ok 字段补齐责任**：adapter 现版不派生 `ok`（实测在位）；接入方投喂判分前须按 §一口径补齐（`result` 无 error_code/error → true），或经判分投影同形校验后再入库。
4. **答卷提取**：`extract_final_response`（candidate_result 正则 → ```json``` 块倒序兜底）+ `extract_run_metadata`（num_turns/成本/token，Claude Code 有、QoderCLI 可能无）。
5. **落盘**：adapter 产物与 response 一并落 §五 解析出的目录，文件命名沿用 §一 file_convention。

---

## 附录 · 基准文件清单

| 契约面 | 基准文件 | 版本/状态 |
|---|---|---|
| trace 格式 | `dag_poc/trace_schema.json` | v1.0.0 定稿 |
| 答卷 | `dag_poc/answer_schema.json` | v0.3.0（operator 收窄） |
| 工具面 | `dag_poc/tools_schema.json`（9 契约工具，`CONTRACT_TOOL_NAMES` 同录于 harness） | 0.2.0 |
| 实现 | `src/benchmark/agent_harness.py`（711 行） | task-04 收口态 |
| adapter | `src/benchmark/cli_trace_adapter.py`（另有 `qodercli_trace_adapter.py` 历史件） | 在位 |
| 验收断言 | `task/E4-agent-harness搭建/t04_verify_view_wiring.py`（mode A 8/8 PASS；`--joint` 待前置） | 可复跑 |
