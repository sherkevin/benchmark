# Agent tools 云端实测核查：能用的都能用，但有一条要澄清

> **一句话：清单里的 9 个契约工具在云端 9/9 实测可用（不是读文档，是逐个真调过）；benchmark 实际在跑的 16 个 case 侧 MCP server 也 16/16 可加载；灵枢那 30 个扩展工具确认云端不可用，与文档记载一致。**
> 2026-08-21。owner 提出："之前是在本地测的，怀疑现在这些 tool 还能不能用"，遂全量实测。

## 一、结论速览

| 层 | 数量 | 云端实测 | 说明 |
|---|---|---|---|
| **契约工具**（`dag_poc/mcp_mock_server/mock_server.py`） | 9 | **9/9 可用** | 逐个真调，返真值 |
| **case 侧 MCP server**（`src/probe_headless/ws_mcp_server_*.py`） | 16 | **16/16 可加载、工具面正确** | **这才是 benchmark 实际在用的** |
| **灵枢扩展工具** | 30 | **0（不可用）** | `server.py` 导入即失败：`No module named 'src.context'` |

## 二、一条必须澄清的事：我们的 benchmark 不用那 9 个契约工具

这点之前的文档没讲清楚，容易误解：

- `AGENT_TOOLS.md` 里的 9 个契约工具（`get_property` / `search_candidates` / `submit_answer` …）
  属于 **dag_poc 契约层**，是给"完整矩阵"路线用的。
- **第一组套卷实际跑的是 `ws_mcp_server_*.py`**，走 **stdio** 直启（`ws_isolation.run_case()` 把
  server 路径写进 run 目录的 `mcp.json`，由 claude code 拉起）。它们自带 `measure_qed` /
  `measure_tc` / `list_pool` 等题目专用工具，**和那 9 个没有调用关系**。

⇒ 两层都测了，两层都没问题；但要知道**第一组的结论只依赖第二层**。

## 三、9 个契约工具的实测明细

全部用真实签名调用（参数名以 MCP schema 为准），返回均为库内真值：

| 工具 | 调用 | 返回 |
|---|---|---|
| `search_candidates` | `keywords="z002730", k=3` | 命中 1 条，带 `structure_hint` |
| `get_entity` | `id="z002730"` | `C13H10N2O4`、29 原子、inchikey |
| `get_property` | `id="z002730", props=["logS","atom_count"]` | `logS=-2.6758`、`atom_count=29` |
| `compare` | `ids=[两个], key="logS"` | `["z033990","z002730"]` 正确降序 |
| `literature_search` | `query="aqueous solubility"` | 真实 DOI 文献 |
| `check_molecule_exists` | `molecule_id="z001542"` | `exists=false`，带剩余尝试数 |
| `edit_molecule` | `anchor_id="z018571", add_atom F` | 返回改后结构 |
| `normalize_structure` | 阿司匹林 SMILES | `valid=true`，规范化 ID |
| `submit_answer` | 完整答卷对象 | `{"accepted": true}` |

**两个"看起来失败"的其实是正确行为**，不是故障：

- `compare` 先报 `UNKNOWN_ENTITY`——它要求**先用 `get_property` 查证过该性质**才准排序。
  补上前置调用后正常返回。这是设计的防幻觉约束。
- `submit_answer` 先报 `SCHEMA_VIOLATION`——是我 payload 结构没按 `answer_schema.json` 写
  （`candidates` 元素要对象不是字符串、`measurements` 用 `name` 不是 `prop`）。按 schema
  改对后 `accepted=true`。**这说明 schema 校验是真在工作的。**

**教训记一笔**：我第一轮全用猜的参数名调，9 个里 8 个报 `TypeError`，差点误判"工具坏了"。
**参数名必须以 MCP schema 为准**——它的 description 里本来就带真实示例值。

## 四、16 个 case 侧 server

全部可加载、工具面与各自档位一致：

| server | 工具面 |
|---|---|
| `qed01` / `satisfy02` / `constraint02` / `dopant` / `edit01` / `constraint` | 1 个（`measure_qed` 或 `measure_tc`） |
| `qed01_open` | 2（`list_pool` + `measure_qed`） |
| `chain_open` / `satisfy02_open` / `constraint02_open` / `dopant_open` / `edit01_open` | 2（发现工具 + 测量） |
| `qed01_some` | 3；`qed01_enough` / `qed01_open_some` | 4；`qed01_open_enough` | 5 |

（`dopant_open` 首轮报 `KeyError: measurement_budget`，查明是**我测试脚本喂错了真值**
——给 open server 喂了 fixed 的真值文件；用正确真值加载正常。不是工具缺陷。）

## 五、灵枢 30 个扩展工具：确认不可用

`dag_poc/mcp_mock_server/server.py` 导入即失败：`ModuleNotFoundError: No module named 'src.context'`。
该模块属灵枢仓库（mac 本地 `/Users/jingwu/…`），云端没有；且清单记载它还依赖内网
Diamond / KeyCenter。**与 `AGENT_TOOLS.md` 的记载一致，不是新问题。**

当前第一组不接入这批工具，**不构成阻塞**。

## 六、一个新发现（记录，暂不阻塞）

把契约层 `mock_server.py` 以 **streamable-http** 起在 9100 端口时，**进程存活但 90 秒不绑定端口**，
`toolcheck/probe_all.py` 因此连不上。stdio 路径（benchmark 实际用的）完全正常。

由于第一组不依赖 HTTP 形态，此项**记录待查**，不影响当前结论。
