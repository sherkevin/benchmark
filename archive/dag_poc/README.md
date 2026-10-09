# dag_poc — AI4S 长程 agent 评测 benchmark

## 目录结构

```
dag_poc/
├── src/                    # 可执行代码
│   ├── agent_harness.py    # 薄壳 agent 评测 harness（LLM function calling → MCP → JSONL tracer）
│   ├── evaluate.py         # checkpoint 评分引擎（从 trace + case 算分，含 ID 归一化层）
│   ├── mock_tools.py       # 简易 mock 工具（Python API，不走 HTTP）
│   └── run_all.py          # 批量评测脚本（5 case × 3 模型全矩阵）
│
├── data/                   # 静态数据（非代码）
│   ├── mock_db.json        # mock 数据库（20 ADMET + 17 晶体 + 14 文献，全匿名化手工构造）
│   ├── response_template.json  # LLM 输出模板（仅 candidate_result.ranked_candidates）
│   ├── tool_params.json    # 23 个 MCP 工具的入参出参注册表
│   ├── property_spec.json  # 性质定义通用格式 + 领域清单
│   └── （TOOLS_INVENTORY.md 已删，工具清单见 docs/AGENT_TOOLS.md）
│
├── docs/                   # 设计文档
│   └── INTERFACE_CONTRACT.md  # 接口层契约（符号绑定表 + 答卷契约 + 取数网关）
│
├── templates/              # 模板定义（设计层）
│   ├── fixed_discovery/    # 固定发现类（GT 反构造，判分参照 GT 性质）
│   │   ├── template.json   # 6 checkpoint DAG + reward 表达式 + 变量注册表
│   │   └── case_*.json     # 5 个 case（CNS/心血管/镇静/抗菌/支气管）
│   └── open_discovery/     # 开放发现类（无 GT，objectives 判优）
│       ├── template.json   # 6 checkpoint DAG + reward 表达式
│       └── case_*.json     # 1 个 case（常压超导，含 LaH10 高压陷阱）
│   # ⚠️ task-07 阶段1 归档：fixed_discovery 29 道 / open_discovery 4 道 +
│   # 4 道 pilot 已入 _deprecated/<原目录名>/（见 _deprecated/README.md）；
│   # 活跃语料 150 道 = cs 60 + co 60 + dh 30，手写批逐批覆盖。
│
├── mcp_mock_server/        # mock MCP 服务（HTTP，23 工具与真实同名同签名）
│   ├── mock_server.py      # FastMCP 服务入口（数据走 data/mock_db.json）
│   ├── mock_db_loader.py   # 共享数据加载器（支持真实化学式反查匿名 ID）
│   ├── server.py           # 原始灵枢 server.py（参考，未修改）
│   └── skills/             # 原始灵枢 skills（参考，未修改）
│
└── traces/                 # 评测轨迹与结果（生成物）
    ├── *_response.json     # 每组 (case, model) 的 agent 最终输出
    ├── *.jsonl             # 每组的完整 tool call 轨迹
    └── _all_results.json   # 全部 15+3 组的评分汇总
```

## 快速开始

```bash
# 1. 启动 mock MCP server（端口 9000）
cd mcp_mock_server && python mock_server.py

# 2. 跑一个 case
cd src && python agent_harness.py ../templates/fixed_discovery/case_cns_metoprolol.json http://localhost:9000/mcp qwen3.7-max

# 3. 跑全矩阵
cd src && python run_all.py

# 4. 评分
cd src && python evaluate.py
```
