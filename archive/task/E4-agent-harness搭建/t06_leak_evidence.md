# E4 task-06 — H2 内置工具泄漏逐条举证清单（可复核）

> 生成时间：2026-08-08 22:41；数据源：task-05 落盘的两条 `_full` 流（已备份为 `*_t05baseline_full.jsonl`）。
> 清点脚本：`t06_leak_inventory.py`（可复跑，同时清点两套流：baseline 基线备份流 + rerun 封堵后当前流，输出与本清单逐字一致），机器可读清单：`t06_leak_inventory.json`。

## 判定口径（task-06 §2.1）

- **BLOCKED（已拒未遂行）**：返回为拒绝信封（`No such tool available` / `Resource not found` / `Directory listing is not enabled` / `No resources found` / `Unknown resource`），**信息零返回**，agent 未拿到任何数据 ⇒ 不构成信息泄漏。
- **BENIGN_TASK_TOOL**：CLI 内部任务管理工具成功执行，入参/返回均为 todo 文案本身，不读文件、不联网、不触 `dag_poc/data/**` ⇒ 逐条举证后可放行（已在 task-06 封堵中一并禁掉以免耗轮次）。
- 全部 50 条中：**触达 dag_poc/data 的 0 条，联网的 0 条**（DATA_PAT/NET_PAT 自动扫描 + 人工复核）。

## 汇总：fixed 跑 12 条 / open 跑 38 条（QA 上轮口径 fixed 4 条为纯本地内置工具计数；本清单以 `_full` 流实数为准，含 MCP 资源工具）

## fixed（case_co_drug_01×claude，task-05 H2 端到端跑）

| seq | 工具 | 入参（截断） | 返回（截断） | 定性 | 与受控数据的距离 |
|---|---|---|---|---|---|
| 3 | Read | {"path": "/Users/jingwu/work/benchmark/dag_poc/docs/INTERFACE_CONTRACT.md"} | "<tool_use_error>Error: No such tool available: Read. Read exists but is not enabled in this context. Use one  | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 4 | Bash | {"command": "cat /Users/jingwu/work/benchmark/dag_poc/docs/INTERFACE_CONTRACT.md \| head - | "<tool_use_error>Error: No such tool available: Bash. Bash exists but is not enabled in this context. Use one  | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 7 | ListMcpResourcesTool | {"server": "lingshu-mock"} | "No resources found. MCP servers may still provide tools even if they have no resources." | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 121 | ReadMcpResourceTool | {"server": "lingshu-mock", "uri": "file:///Users/jingwu/work/benchmark/dag_poc/templates/f | "MCP error 0: Unknown resource: file:///Users/jingwu/work/benchmark/dag_poc/templates/fixed_discovery/case_001 | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 128 | ReadMcpResourceTool | {"server": "lingshu-mock", "uri": "dag_poc/docs/INTERFACE_CONTRACT.md"} | "Resource not found: dag_poc/docs/INTERFACE_CONTRACT.md — it may have been deleted or the URI is stale. Re-run | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 129 | ReadMcpResourceTool | {"server": "lingshu-mock", "uri": "dag_poc/property_spec.json"} | "Resource not found: dag_poc/property_spec.json — it may have been deleted or the URI is stale. Re-run ListMcp | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 130 | ReadMcpResourceTool | {"server": "lingshu-mock", "uri": "dag_poc/answer_schema.json"} | "Resource not found: dag_poc/answer_schema.json — it may have been deleted or the URI is stale. Re-run ListMcp | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 137 | ReadMcpResourceDirTool | {"server": "lingshu-mock", "uri": "dag_poc/"} | "Directory listing is not enabled in this build." | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 174 | ListMcpResourcesTool | {} | "No resources found. MCP servers may still provide tools even if they have no resources." | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 272 | ReadMcpResourceTool | {"server": "lingshu-mock", "uri": "case_config"} | "Resource not found: case_config — it may have been deleted or the URI is stale. Re-run ListMcpResourcesTool t | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 273 | ReadMcpResourceTool | {"server": "lingshu-mock", "uri": "domain_knowledge"} | "Resource not found: domain_knowledge — it may have been deleted or the URI is stale. Re-run ListMcpResourcesT | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 274 | ReadMcpResourceTool | {"server": "lingshu-mock", "uri": "task_config"} | "Resource not found: task_config — it may have been deleted or the URI is stale. Re-run ListMcpResourcesTool t | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |

## open（case_dh_drug_01×claude，task-05 H2 open 跑）

| seq | 工具 | 入参（截断） | 返回（截断） | 定性 | 与受控数据的距离 |
|---|---|---|---|---|---|
| 23 | TaskCreate | {"subject": "Find and analyze homologous series for drug solubility task", "description":  | "Task #1 created successfully: Find and analyze homologous series for drug solubility task" | BENIGN_TASK_TOOL | 未触达——CLI 内部 todo，入参/返回均为任务文案，不读文件不联网 |
| 24 | TaskUpdate | {"taskId": "1", "status": "in_progress"} | "Updated task #1 status" | BENIGN_TASK_TOOL | 未触达——CLI 内部 todo，入参/返回均为任务文案，不读文件不联网 |
| 70 | TaskStop | {"task_id": "1"} | "<tool_use_error>No task found with ID: 1</tool_use_error>" | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 84 | TaskUpdate | {"taskId": "1", "status": "completed"} | "Updated task #1 status" | BENIGN_TASK_TOOL | 未触达——CLI 内部 todo，入参/返回均为任务文案，不读文件不联网 |
| 85 | TaskCreate | {"subject": "Understand tool schemas and find task data", "description": "Read INTERFACE_C | "Task #2 created successfully: Understand tool schemas and find task data" | BENIGN_TASK_TOOL | 未触达——CLI 内部 todo，入参/返回均为任务文案，不读文件不联网 |
| 87 | ReadMcpResourceDirTool | {"server": "lingshu-mock", "uri": "file:///Users/jingwu/work/benchmark/dag_poc/docs/"} | "Directory listing is not enabled in this build." | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 93 | Bash | {"command": "find /Users/jingwu/work/benchmark/dag_poc -name \"INTERFACE_CONTRACT.md\" -o  | "<tool_use_error>Error: No such tool available: Bash. Bash exists but is not enabled in this context. Use one  | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 94 | Bash | {"command": "find /Users/jingwu/work/benchmark -name \"mock_server.py\" -o -name \"server. | "<tool_use_error>Error: No such tool available: Bash. Bash exists but is not enabled in this context. Use one  | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 95 | ReadMcpResourceTool | {"server": "lingshu-mock", "uri": "file:///Users/jingwu/work/benchmark/dag_poc/docs/INTERF | "MCP error 0: Unknown resource: file:///Users/jingwu/work/benchmark/dag_poc/docs/INTERFACE_CONTRACT.md" | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 100 | ListMcpResourcesTool | {"server": "lingshu-mock"} | "No resources found. MCP servers may still provide tools even if they have no resources." | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 102 | Glob | {"pattern": "**/mock_server.py", "path": "/Users/jingwu/work/benchmark"} | "<tool_use_error>Error: No such tool available: Glob. Glob is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 103 | Glob | {"pattern": "**/server.py", "path": "/Users/jingwu/work/benchmark"} | "<tool_use_error>Error: No such tool available: Glob. Glob is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 104 | Glob | {"pattern": "**/INTERFACE_CONTRACT.md", "path": "/Users/jingwu/work/benchmark"} | "<tool_use_error>Error: No such tool available: Glob. Glob is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 109 | Grep | {"pattern": "logS\|solubility\|target.*window", "path": "/Users/jingwu/work/benchmark/dag_ | "<tool_use_error>Error: No such tool available: Grep. Grep is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 110 | Grep | {"pattern": "get_property\|def get_property", "path": "/Users/jingwu/work/benchmark/src",  | "<tool_use_error>Error: No such tool available: Grep. Grep is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 123 | Grep | {"pattern": "logS\|solubility.*window\|target.*solubility", "path": "/Users/jingwu/work/be | "<tool_use_error>Error: No such tool available: Grep. Grep is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 124 | Grep | {"pattern": "\"get_property\"", "path": "/Users/jingwu/work/benchmark", "glob": "*.py", "o | "<tool_use_error>Error: No such tool available: Grep. Grep is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 125 | Read | {"file_path": "/Users/jingwu/work/benchmark/src/data_build/mock_server.py", "limit": "100" | "<tool_use_error>Error: No such tool available: Read. Read exists but is not enabled in this context. Use one  | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 126 | Glob | {"pattern": "**/*server*.py", "path": "/Users/jingwu/work/benchmark"} | "<tool_use_error>Error: No such tool available: Glob. Glob is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 129 | Glob | {"pattern": "**/answer_schema.json", "path": "/Users/jingwu/work/benchmark"} | "<tool_use_error>Error: No such tool available: Glob. Glob is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 130 | Glob | {"pattern": "**/mock_server*.py", "path": "/Users/jingwu/work/benchmark"} | "<tool_use_error>Error: No such tool available: Glob. Glob is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 131 | Glob | {"pattern": "**/*open*task*", "path": "/Users/jingwu/work/benchmark"} | "<tool_use_error>Error: No such tool available: Glob. Glob is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 132 | Read | {"file_path": "/Users/jingwu/work/benchmark/dag_poc/templates/open_discovery/template.yaml | "<tool_use_error>Error: No such tool available: Read. Read exists but is not enabled in this context. Use one  | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 133 | Read | {"file_path": "/Users/jingwu/work/benchmark/dag_poc/templates/fixed_discovery/template.yam | "<tool_use_error>Error: No such tool available: Read. Read exists but is not enabled in this context. Use one  | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 134 | Bash | {"command": "find /Users/jingwu/work/benchmark -type f \\( -name \"*.json\" -o -name \"*.y | "<tool_use_error>Error: No such tool available: Bash. Bash exists but is not enabled in this context. Use one  | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 137 | Grep | {"pattern": "get_property", "path": "/Users/jingwu/work/benchmark/src", "output_mode": "fi | "<tool_use_error>Error: No such tool available: Grep. Grep is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 138 | Grep | {"pattern": "get_property", "path": "/Users/jingwu/work/benchmark/dag_poc", "output_mode": | "<tool_use_error>Error: No such tool available: Grep. Grep is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 153 | ReadMcpResourceTool | {"server": "lingshu-mock", "uri": "dag_poc/docs/INTERFACE_CONTRACT.md"} | "Resource not found: dag_poc/docs/INTERFACE_CONTRACT.md — it may have been deleted or the URI is stale. Re-run | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 154 | ReadMcpResourceTool | {"server": "lingshu-mock", "uri": "dag_poc/answer_schema.json"} | "Resource not found: dag_poc/answer_schema.json — it may have been deleted or the URI is stale. Re-run ListMcp | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 157 | Grep | {"pattern": "solubility.*window\|target.*logS\|logS.*window", "path": "/Users/jingwu/work/ | "<tool_use_error>Error: No such tool available: Grep. Grep is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 158 | Grep | {"pattern": "domain_knowledge\|target_solubility\|solubility_window", "path": "/Users/jing | "<tool_use_error>Error: No such tool available: Grep. Grep is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 162 | ReadMcpResourceTool | {"server": "lingshu-mock", "uri": "dag_poc/templates/open_discovery/case_101_supercon.yaml | "Resource not found: dag_poc/templates/open_discovery/case_101_supercon.yaml — it may have been deleted or the | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 163 | ReadMcpResourceTool | {"server": "lingshu-mock", "uri": "dag_poc/docs/INTERFACE_CONTRACT.md"} | "Resource not found: dag_poc/docs/INTERFACE_CONTRACT.md — it may have been deleted or the URI is stale. Re-run | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 169 | Read | {"file_path": "/Users/jingwu/work/benchmark/dag_poc/templates/open_discovery/case_101_supe | "<tool_use_error>Error: No such tool available: Read. Read exists but is not enabled in this context. Use one  | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 170 | Read | {"file_path": "/Users/jingwu/work/benchmark/dag_poc/docs/INTERFACE_CONTRACT.md"} | "<tool_use_error>Error: No such tool available: Read. Read exists but is not enabled in this context. Use one  | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 171 | Read | {"file_path": "/Users/jingwu/work/benchmark/dag_poc/answer_schema.json"} | "<tool_use_error>Error: No such tool available: Read. Read exists but is not enabled in this context. Use one  | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 176 | Grep | {"pattern": "solubility\|logS.*window\|target.*window", "path": "/Users/jingwu/work/benchm | "<tool_use_error>Error: No such tool available: Grep. Grep is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |
| 177 | Grep | {"pattern": "logS", "path": "/Users/jingwu/work/benchmark", "output_mode": "content", "hea | "<tool_use_error>Error: No such tool available: Grep. Grep is not available in this context. Use one of the av | BLOCKED | 未触达——工具调用被 CLI/MCP 拒绝，信息零返回 |

## 结论

1. task-05 登记的两处 `_full` 流共 50 条非契约工具事件：**46 条 BLOCKED（拒绝信封，信息零返回）+ 4 条 BENIGN_TASK_TOOL（CLI 内部 todo）**。
2. **实质泄漏 0 条**：无一读到 `dag_poc/data/**`，无一联网。task-05 report §一所担心的『Bash 实读工作区』发生在更早的 bypassPermissions 探测轮（v1），现行 acceptEdits+disallowed-tools 参数组合下不存在。
3. 虽无实质泄漏，仍按 task-06 要求做运行时禁止升级：三层封堵（见 report-task-06.md）+ 同题重跑验证。
4. 残余代价登记：46 条拒绝尝试消耗了模型轮次（fixed 12 条 / open 34 条，含 TaskStop×1 拒绝信封；task_id=21 复算订正，原写 open 33 条漏计 TaskStop），是 H2 耗时偏高的原因之一。

## task-06 封堵生效后同题重跑（case_co_drug_01×claude，2026-08-08 22:37 起跑，998s，4/4 PASS）

残留 6 条非契约工具事件，逐条举证：

| seq | 工具 | 入参 | 返回 | 定性 | 与受控数据的距离 |
|---|---|---|---|---|---|
| 8 | Read | {"file_path": "/Users/jingwu/work/benchmark/dag_poc/docs/INTERFACE_CONTRACT.md"} | "<tool_use_error>Error: No such tool available: Read. Read exists but is not enabled in this context. Use one of the available too | BLOCKED | 未触达——调用被 CLI 拒绝（No such tool available），信息零返回；尝试目标为契约文档（INTERFACE_CONTRACT.md/answer_schema.json/property_spec.json），非 dag_poc/data |
| 9 | Read | {"file_path": "/Users/jingwu/work/benchmark/dag_poc/answer_schema.json"} | "<tool_use_error>Error: No such tool available: Read. Read exists but is not enabled in this context. Use one of the available too | BLOCKED | 未触达——调用被 CLI 拒绝（No such tool available），信息零返回；尝试目标为契约文档（INTERFACE_CONTRACT.md/answer_schema.json/property_spec.json），非 dag_poc/data |
| 39 | Bash | {"command": "cat /Users/jingwu/work/benchmark/dag_poc/docs/INTERFACE_CONTRACT.md 2>/dev/null \| head | "<tool_use_error>Error: No such tool available: Bash. Bash exists but is not enabled in this context. Use one of the available too | BLOCKED | 未触达——调用被 CLI 拒绝（No such tool available），信息零返回；尝试目标为契约文档（INTERFACE_CONTRACT.md/answer_schema.json/property_spec.json），非 dag_poc/data |
| 40 | Bash | {"command": "cat /Users/jingwu/work/benchmark/dag_poc/property_spec.json 2>/dev/null \| head -100",  | "<tool_use_error>Error: No such tool available: Bash. Bash exists but is not enabled in this context. Use one of the available too | BLOCKED | 未触达——调用被 CLI 拒绝（No such tool available），信息零返回；尝试目标为契约文档（INTERFACE_CONTRACT.md/answer_schema.json/property_spec.json），非 dag_poc/data |
| 155 | Read | {"file_path": "/Users/jingwu/work/benchmark/dag_poc/property_spec.json"} | "<tool_use_error>Error: No such tool available: Read. Read exists but is not enabled in this context. Use one of the available too | BLOCKED | 未触达——调用被 CLI 拒绝（No such tool available），信息零返回；尝试目标为契约文档（INTERFACE_CONTRACT.md/answer_schema.json/property_spec.json），非 dag_poc/data |
| 156 | Read | {"file_path": "/Users/jingwu/work/benchmark/dag_poc/answer_schema.json"} | "<tool_use_error>Error: No such tool available: Read. Read exists but is not enabled in this context. Use one of the available too | BLOCKED | 未触达——调用被 CLI 拒绝（No such tool available），信息零返回；尝试目标为契约文档（INTERFACE_CONTRACT.md/answer_schema.json/property_spec.json），非 dag_poc/data |

**重跑结论**：MCP 资源工具/Task* 尝试归零；残留 6 条全为被拒尝试，实质泄漏计数 0。
