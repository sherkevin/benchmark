# ADR-0019 — debug/联调阶段统一走 DogFooding 免费模型

| 字段 | 值 |
|---|---|
| **状态** | Accepted |
| **日期** | 2026-08-08 |
| **决策者** | owner（jingwu） |
| **执行** | 全部六条线 |

---

## 1. 决议

**逻辑 debug 与联调阶段一律用 DogFooding 免费模型；只有正式跑实验矩阵时才切回付费模型。**

owner 原话：「我们在正式跑测试之前，在做整个逻辑上的 debug 和测试时，先使用 dogfooding 模型；后面真实跑实验矩阵时，再换新的模型进去。」

---

## 2. 配置（已实测跑通）

| 项 | 值 |
|---|---|
| 模型名 | **`Peach-07-17-DogFooding`** |
| 端点（OpenAI 兼容） | `https://idealab.alibaba-inc.com/api/openai/v1` |
| 端点（Anthropic 协议） | `https://idealab.alibaba-inc.com/api/code` |
| key | 见 `.env` 的 `DOGFOODING_API_KEY`（备用 `DOGFOODING_API_KEY_ALT`，两套实测均可通） |
| 开关 | `.env` 里 `LLM_PROFILE=dogfooding`；正式跑矩阵时注释掉该行 |

来源：CC Switch 的 `~/.cc-switch/cc-switch.db` → `providers` 表。

### 2.1 切换方式

`agent_harness._load_llm_config()` 读 `LLM_PROFILE`，命中 `dogfooding` 就整组替换 key / model / endpoint，**下游任何调用方式都不用改**。切回付费只需注释掉 `.env` 那一行。

---

## 3. 实测记录（踩过的坑，照抄可省时间）

| # | 现象 | 结论 |
|---|---|---|
| 1 | `Qwen3.7-Max-DogFooding` → HTTP 400 `CE-012 模型已下架` | **该模型已下线，不要再用** |
| 2 | `Peach-07-17-DogFooding[1M]`、`peaching-07-17-dogfooding` → `CE-001 模型不存在` | **模型名必须精确写 `Peach-07-17-DogFooding`**，不加后缀、不改大小写 |
| 3 | 原 `IDEALAB_API_KEY`（前缀 `677b9f`）→ `PL-001 平台业务限制` | **必须换 DogFooding 那把 key**，老 key 对本模型无权 |
| 4 | 同时发 `x-api-key` 与 `Authorization` → 401 | 网关只接受**其中一个**，不可并存 |
| 5 | `Qwen3.6-Plus-DogFooding` / `qwen3-coder-plus` → `IRC-001 该模型需要授权` | 模型存在但本 key 无权；`IRC-001` 与 `CE-001` 含义不同，别混 |
| 6 | 真题跑测 `Read timed out (timeout=120)` | **推理模型需要更长窗口**。已把超时改为配置驱动（`LLM_REQUEST_TIMEOUT`），dogfooding 档默认 **600s** |

### 3.1 与既有三个付费模型的差异

**这个模型的坑比现有三个都少：**

| 项 | `Peach-07-17-DogFooding` | `moonshot/kimi-k3` |
|---|---|---|
| 正文位置 | `content` 直接可用 | `content` 空、要回落 `reasoning_content` |
| `temperature=0` | 接受 | **返 400 MPE-001**，只能取 1 |
| function calling | 支持，实测正确调出 `get_entity({"id":"z000374"})` | 支持 |

---

## 4. 端到端实跑证据

跑 `case_co_drug_01`（H1 薄壳 harness + 31 工具的 MCP mock）：

| 指标 | 值 |
|---|---|
| 步数 / LLM 调用 | 14 / 14 |
| 工具调用 | **67 次**（链路完全打通） |
| token | 218,293（prompt 197,725 + completion 20,568） |
| 留痕 | `request`/`response`/`latency_ms`/`attempt`/`seq`/`timestamp`/`code_version` **7 项全在**，`response.usage` 与 `response.id` 也在 |

### 4.1 这一跑立刻暴露了一个真问题

**`submission_channel=text_fallback`、`final_text=None`** —— 模型跑了 14 轮、调了 67 次工具，但**没走 `submit_answer` 交卷，最终答卷是空的**。

**这正是 debug 阶段该发现的东西**，而且是免费发现的。归 E4 在 task-05 一并查（是 prompt 没把交卷要求说清、还是 `submit_answer` 的 schema 让模型不敢调）。

---

## 5. 附带发现（与本决议无关但需登记）

`dag_poc/mcp_mock_server/server.py`（704 行，25 个 `@mcp.tool`）**跑不起来**：它 `from src.context import lite_ctx_vars`，而该模块在本仓库不存在（是从灵枢 MCP 拷来的原版参考文件）。

**实际可用入口是 `mock_server.py`**（453 行，31 个 `@mcp.tool`）。`server.py` 建议标注为「参考用，不可执行」或移出可执行路径，避免后人踩。

---

## 6. 环境提示

本仓库的 `mcp` 包装在 `./.venv`（python 3.12）。系统 `python3` 是 3.9，装不了 `mcp`（要求 ≥3.10）。跑 MCP server 与端到端合跑都要用 `./.venv/bin/python`，且 `mock_server.py` 需在其所在目录启动。
