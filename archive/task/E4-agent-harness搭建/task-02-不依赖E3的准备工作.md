# E4 · task-02 — 不依赖 E3 的 harness 准备工作（先做骨架与多模型接入）

| 字段 | 值 |
|---|---|
| **派发** | 监工，2026-08-06 |
| **状态** | 🟢 **已开启（依赖已解除，2026-08-06）** |
| **前提** | ✅ **E3 已验收通过，MCP 面有 9 个契约工具可调** |
| **不做** | 见 §1 更新说明 |

---

## 1. 为什么现在能开工，以及哪部分必须等

**你的上游 E3 验收不通过**，具体影响：

| E3 的问题 | 对你的影响 |
|---|---|
| `mock_server.py` 仍是 23 个旧工具、`check_molecule_exists` 出现 **0 次** | **你走 MCP 拿到的是旧工具，新契约的 9 个一个也调不到** |
| `gateway.py` 不是 MCP server，全仓无人引用 | 工具通道的最终形态未定 |

**所以：工具通道部分必须等 E3。** 但 harness 的其余部分**不依赖工具通道**，现在就能做——等 E3 修好时你只需接上通道，而不是从零开始。

---

## 2. 本批做四件（都不碰工具通道）

### 2.1 多模型接入层

按 `goal.md` §6 步 6 的三层矩阵，harness 要能跑多个模型。**先把模型接入层做完**：

- 统一的 chat/completion 调用封装（模型名、温度、max_tokens、重试、超时）；
- 支持**至少 3 个模型**（参考现有 `dag_poc/traces/` 里的命名：`qwen3.7-max`、`bailian_glm-5.2`、`moonshot_kimi-k3`）；
- **凭据从环境变量读，不得硬编码进代码**；
- 失败重试与退避策略，且**重试次数要记进 trace**（后续分析要区分"模型能力不行"与"网络抖动"）。

### 2.2 trace 落盘格式（这一步最关键）

**判分器吃的是 trace，trace 格式错了整条链路白跑。**

- 参考现有 `dag_poc/traces/*.jsonl` 的既有形态（63 份历史 trace）；
- 每条工具调用记录必须含：**调用序号（时间序）、工具名、入参、返回、是否命中**；
- **时间序必须准确**——E3 的 `no_path_violation` 靠它做拓扑序比对（且该判据当前方向错误、正在返工，你的 trace 只要保证时间序真实即可，不要自己实现判据）；
- 与 `dag_poc/answer_schema.json`（ADR-0014 裁定为唯一字段来源）对齐：答卷部分须能产出 `candidates` / `ranking` / `answer`，open 类另加 `anchor_molecules` / `cited_values` / `edit_op`。

### 2.3 工具通道的**适配层接口**（只定接口，不接实现）

定义一个薄适配层，让 harness 不直接依赖 MCP 或 Gateway 的具体形态：

- 接口只需 `call_tool(name, args) -> dict`；
- **两个后端实现留桩**：MCP 通道（等 E3）、直连 Gateway（等 E3 决定是否暴露）；
- 这样 E3 无论最终选哪种，你改一个适配器即可，不动 harness 主体。

### 2.4 单 case 冒烟跑（用现有 23 个旧工具，只为验证管道通）

用**现有 `mock_server.py` 的旧工具**跑通一个 case 的完整链路：读 case → 发 query → agent 调工具 → 落 trace → 输出答卷。

**目的只是验证管道**，不是产出可用结果。**明确标注"用旧工具跑的冒烟测试，非正式实验"**，不要写进任何结果表。

---

## 3. 交付标准

| # | 标准 |
|---|---|
| 1 | 多模型接入层可用，≥3 个模型实测各跑通 1 次，凭据走环境变量 |
| 2 | trace 落盘格式定稿，含调用序号/工具名/入参/返回/命中标志，**时间序真实** |
| 3 | 答卷输出与 `answer_schema.json` 字段对齐（fixed 三字段 + open 三扩展字段） |
| 4 | 工具适配层接口定稿，两个后端留桩 |
| 5 | 单 case 冒烟跑通，**明确标注为"旧工具、非正式实验"** |
| 6 | report 收口：明确列出"哪些部分在等 E3、等到后需改哪几处" |

## 4. 边界

**只写** `task/E4-agent-harness搭建/`、harness 代码（建议 `src/harness/`）。

**不改**：`gateway.py`/`evaluate.py`/`tools_schema.json`/`answer_schema.json`/`mcp_mock_server/**`（E3 的——**发现问题写进 progress.md 报监工**）、`dag_poc/templates/**`（E5 的）、`dag_poc/data/**`（E1 的）、`query_spec.json`/`query_lint.py`（E2 的）、共享账本（`TODO.md`/`problems.md`/总览/`goal.md`/`governance/adr/**`）。

进度写自己的 `progress.md`。

## 5. 已生效的裁定（与你相关）

- **[ADR-0014](../../governance/adr/0014-答卷字段契约以answer_schema为唯一来源.md)**：答卷字段以 `answer_schema.json` 为唯一来源；旧字段 `screened_pool`/`ranked_candidates` 降为派生字段。**你的答卷输出按新字段做。**
- **[ADR-0015](../../governance/adr/0015-后端库与AB隔离以E1文件系统分层为准.md)**：A/B 隔离靠文件系统分层，读取层不 open B 文件。**harness 不得直接读 `dag_poc/data/backend/**`**，一切数据访问走工具通道。

---

# §7 依赖解除通知（监工，2026-08-06）—— 本批范围已扩大

**本 task 原写成"不依赖 E3 的准备工作"，因为当时 E3 的 MCP 没接。现在 E3 已验收通过，限制解除。**

实测 E3 的交付：`@mcp.tool` **23 → 31**，9 个契约工具**全部暴露**（literature_search / search_candidates / get_entity / get_property / compare / submit_answer / check_molecule_exists / edit_molecule / normalize_structure），内部走 `_contract_call` 统一转调 Gateway，不是空壳。测试基线 126 passed。

## 范围变更

| 原写法 | 现在改为 |
|---|---|
| §2.3 "工具适配层**只定接口，不接实现**，两个后端留桩" | **直接接 MCP 通道**（9 个工具已可调），适配层仍保留（便于将来换后端），但要有一个能跑的实现 |
| §2.4 "用现有 **23 个旧工具**跑单 case 冒烟，标注为非正式实验" | **用 9 个新契约工具跑单 case 冒烟**，这次算正式的管道验证 |
| 不做端到端 | **要跑通端到端**：读 case → 发 query → agent 调新工具 → 落 trace → 输出答卷 |

## 两件新增的注意事项

1. **A 侧数据已匿名化**（E1-03 交付）。agent 现在看到的 `formula` 长这样：`[16*]15[19*]25[22*]3[81*]1`，`inchikey` 是 `None`。**这是正确的**（防污染要求），harness 不要试图"补全"或"还原"这些字段。
2. **`mcp` 包当前未装**——E3 那边有 1 个 e2e 测试因此 skip。你若要走真实 MCP 通道，需要先装包（E3-03 也在做这件事，可以协同）。

## 其余不变

§2.1 多模型接入层、§2.2 trace 落盘格式（判分器吃的是 trace，格式错整条链路白跑）仍是本批重点，标准不变。`no_path_violation` 的判据 E3 已修好（evidence_time 取 min），你的 trace **只要保证时间序真实**，不要自己实现判据。
