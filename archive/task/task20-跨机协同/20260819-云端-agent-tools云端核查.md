# agent tools 云端安装核查

| 字段 | 值 |
|---|---|
| 日期 | 2026-08-19 |
| 发件方 | 云端（开发机 Linux，已接手全权推进） |
| 类型 | 报告 |
| 关联 commit | 见本文件入库后的 `git log` |
| 需对方做什么 | 过目；确认灵枢 MCP 是否确定不接入（见 §4） |
| 回执状态 | ✅ 本轮自主完成 |

---

## 速读

owner 提醒「这些 tools 之前在本地 mac，迁到云端要核查是否装齐」。逐层实测结论：

- **第一组 8 道题在用的 3 个工具**（`search_db` / `measure_tc` / `get_element_count`）：**云端完全 ready**，7 个 `ws_mcp_server_*.py` 全部 import + list_tools 通过，依赖只需 `mcp` + 标准库，真值数据齐全。
- **完整矩阵要用的 9 个契约工具**（dag_poc `mock_server.py`）：原本**只差一个 `rdkit`**，本轮已 `pip install rdkit`（2026.03.5）并钉进 `requirements.txt`；装后 mock_server 9 工具暴露面验证通过。86M 后端数据早已随仓库迁齐、未被 gitignore。
- **灵枢 MCP 的 23/34 个工具**：依赖 mac 本地仓（`/Users/jingwu/...`）+ 内网 Diamond/KeyCenter 配置，云端无法运行；但项目定位它为**历史参考、当前不接入**（CLAUDE.md：与本项目双领域不匹配、仅数量级参考），无需补。

**一句话：agent 工具在云端的能力已对齐本地，唯一动作是补装 rdkit（已完成）。**

## 1. 工具分三层，别混

| 层 | 工具 | 用途 | 定义位置 |
|---|---|---|---|
| A · 第一组8题在用 | `search_db`、`measure_tc`、`get_element_count`（去重 3 个） | 当前主线：第一组 8 道题（4 case × fixed/open） | `src/probe_headless/ws_mcp_server_*.py` |
| B · 完整矩阵契约层 | 9 个：`literature_search` `search_candidates` `get_entity` `get_property` `compare` `submit_answer` `check_molecule_exists` `edit_molecule` `normalize_structure` | goal §6 步3「敲定 9 个工具」，跑 150 道 dag_poc 题矩阵用 | `dag_poc/mcp_mock_server/mock_server.py` 的 `CONTRACT_REGISTERED_NAMES` |
| C · 历史参考 | 灵枢 MCP 23/34 个 | 数量级参考，**不接入本项目** | 灵枢仓（mac 本地）+ `research/AGENT_TOOLS.md` |

权威工具清单文档：`research/AGENT_TOOLS.md`（一 tool 一节，含状态/入出参/时延）。`dag_poc/TOOLS_INVENTORY.md` 已删，指针指向它。

## 2. 云端实测（`.venv/bin/python`，py3.10.14，mcp 1.29.0）

| 核查项 | 结果 |
|---|---|
| 7 个 `ws_mcp_server_*.py`（dopant/constraint02/satisfy02/chain × fixed/open） | ✅ 全部 import + `list_tools()` 通过 |
| `mock_server.py`（9 契约工具） | 装 rdkit 前 ❌（`ModuleNotFoundError: rdkit`）→ 装后 ✅ 9 工具暴露面验证通过 |
| `server.py` | ❌ `No module named 'src'`——**符合预期**，它是灵枢拷来的参考文件、非入口（CLAUDE.md 已注明，入口是 mock_server.py） |
| 后端数据 `dag_poc/data/backend/`（86M：search_candidates_A 35M / get_entity_A 34M / get_property_A 17M / literature_search_A 1.1M） | ✅ 在仓库、未 gitignore（只排除 `data/sources/`） |
| 各 case `ground_truth.json`（实测 12 份） | ✅ 全在 |

## 3. 依赖清单（云端现状）

- **A 层（ws 工具）**：`mcp<2` + 标准库。✅ 全在。
- **B 层（9 契约工具）**：`mcp` + `numpy` `httpx` `uvicorn` `pydantic` `matplotlib`（已随 mcp 装）+ **`rdkit`（本轮补装）**。✅ 现全在。
- **agent harness（跑 agent 本身，非工具后端）**：若走 dag_poc 的 openai/anthropic harness 另需 `requests` `openai` `anthropic` + API 密钥；第一组 8 题走 `ws_isolation` 直接调 `claude` CLI，不需要这些。**当前主线不阻塞**。

## 4. 遗留 / 待确认

| # | 事项 | 说明 |
|---|---|---|
| 1 | 灵枢 MCP 是否确定不接入 | 文档一致说"仅数量级参考、不接入"。若将来要接，需在云端另配灵枢仓 + 内网凭据——那是 owner 侧环境事。**默认不动。** |
| 2 | 9 契约工具尚未在云端真跑过 agent | 本轮只验证了 import + 暴露面；真跑（dag_poc 150 题矩阵）要烧机时，按"不盲目跑实验"纪律，等排到那一步再验 |
| 3 | agent harness 的 openai/anthropic/requests + 密钥 | 只有走 dag_poc harness 才需要；第一组 8 题不需要。用到时再补 |

## 5. 本轮动作

- `pip install rdkit`（2026.03.5），验证 mock_server.py 9 工具暴露面通过。
- `requirements.txt` 追加 `rdkit` + 注释（避免下台机器重踩）。
