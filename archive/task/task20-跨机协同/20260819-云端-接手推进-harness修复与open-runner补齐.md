# 云端接手推进：修 tool_leak 误报 + 补齐两个 open runner

| 字段 | 值 |
|---|---|
| 日期 | 2026-08-19 |
| 发件方 | 云端（已接手全权推进，本地工程师暂时下线） |
| 类型 | 报告 |
| 关联 commit | 见本文件入库后的 `git log`（本次提交） |
| 需对方（本地/owner）做什么 | 上线后过目本报告；两个新 open runner 待批一批真跑验证（烧机时，见 §3 遗留） |
| 回执状态 | ✅ 本轮自主完成，无待回执 |

---

## 速读

owner 授权我接手本地工程师的活、全权推进。本轮做完三件**零机时成本**的工程活（都离线自测过）：

1. **修好 tool_leak 系统性误报**——满批的拦路石。改 `ws_isolation.py`：leak 判定从「工具数不等」改为「出现 allowed 集之外的工具」。用现有 30 个 d 批 trace 验证：**旧逻辑误报 26 个 → 新逻辑 0 个**；反向验证混入 `Bash` 仍被抓到。
2. **补齐 `constraint_02·open` runner + MCP server**——8 道题差的两个之一。
3. **补齐 `satisfy_02·open` runner + MCP server**——最后一个。

**第一组 8 道题的 workspace runner 现已全部就绪**（4 case × fixed/open = 8）。

---

## 1. 修 tool_leak 误报（`ws_isolation.py`）

### 病根（满批实测，见 20260819-云端-dopant_open满批30run.md §4）

30 并发下 MCP server 启动被拖慢，claude 在 MCP 就绪前拍 init 快照，工具列表是
`[Edit, Glob, Grep, Read, WaitForMcpServers, Write]`——`5 内置 + WaitForMcpServers 占位`
共 6 个，还没挂上 `measure_tc`/`search_db`。旧判定 `n_tools(6) != expected(7)` → 判 leak，
`sweep` 第一个 run 就停批。**实测 26/30 中招，而 30/30 都真调用了 MCP 工具**。

### 改法

- 新增常量 `TRANSIENT_TOOLS = {"WaitForMcpServers"}`（MCP 未就绪时的瞬态占位工具白名单）。
- leak 判定改为**集合越界**：`allowed = 内置 ∪ 本次注册的 MCP 工具 ∪ TRANSIENT_TOOLS`，
  只有 init 里出现 `allowed` 之外的工具（如 claude 升版新增内置工具）才算 leak。
- 保留 `n_tools` 字段（报告用），`tool_leak.json` 内容改为记录越界工具/实际暴露集/allowed 集。

**保住了原始意图**（抓计划外工具），只消除了"MCP 没就绪导致少算"的假阳性。

### 验证（离线，不烧机时）

| 场景 | 结果 |
|---|---|
| 30 个 d 批 trace 复算 | 旧逻辑 leak=26 → **新逻辑 leak=0** |
| 就绪 init（7 工具） | 不 leak ✅ |
| 未就绪 init（6，含占位） | 不 leak ✅ |
| 混入计划外 `Bash`（8） | **仍判 leak** ✅ |

顺手清掉了 d 批 26 个已被证明是误报的 `tool_leak.json` 标记（runs/ 为 gitignore，不入库）。

## 2. 补齐两个 open runner

| 新文件 | 说明 |
|---|---|
| `src/probe_headless/ws_runner_constraint02_open.py` | constraint_02 open 侧 runner |
| `src/probe_headless/ws_mcp_server_constraint02_open.py` | 配套 MCP server |
| `src/probe_headless/ws_runner_satisfy02_open.py` | satisfy_02 open 侧 runner |
| `src/probe_headless/ws_mcp_server_satisfy02_open.py` | 配套 MCP server |

**设计要点（未自创，忠实移植已验证设计）**：

- 走 **ws 范式**（`measure_tc` + `get_element_count` + 写 `answer.json` 交卷），与
  fixed 侧同接口——不用 openai 版的 `get_property`/`submit_answer`。answer.json 字段
  与 fixed 完全一致（judge 代码级转发 fixed）。
- open 比 fixed 多一个 **`search_db`**，且采用**分页（每页 4 个）+ 每翻一页消耗 1 预算**：
  依据 `openai_constraint02_open_runner.py` 文件头实测——search_db 若免费不分页，
  本库候选都含 O/Cu，搜一次就枚举全库、发现步白给，**open 退化成 fixed（尺子② p=0.84 失效）**；
  分页收费后 p=0.0008 恢复显著。故 `measure_tc` 与 `search_db` **共用一份预算**。
- seed 从 case 的 `measurable[0]` 动态取（**不用不可测最优当 seed 以免泄露答案**；
  注意 openai 版硬写的旧 seed `O6.220Ca1Cu2Ba2Hg1` 已不在当前 truth 里，是旧 pool24 遗留）。

**离线自测（都过）**：语法；search_db 分页与 `has_more`；预算共享计数正确
（constraint：2 页 search + 1 measure = 3；satisfy：1+1=2）；measurable 返 Tc、
unmeasurable 返 `probe_unavailable`；未知组成报错；`get_element_count` 不耗预算；
两题 preflight 均 0 失败；`sweep_ws` 能抓到两个新 runner 的 `CASE_DIR`。

## 3. 遗留

| # | 事项 | 归属 |
|---|---|---|
| 1 | 两个新 open runner 只做了**离线自测**，尚未真跑 agent 验证端到端（要烧机时）。建议各先跑 `--probe` 3 run 探链路，再定满批 | 待 owner 批机时 |
| 2 | `dopant_01·open` 满批 30 run 数据已在库（均值 0.766），本轮修 harness 后可放心作为有效数据引用 | — |
| 3 | 数据缺口仍在：`satisfy_02·fixed` 仅 9 run、`constraint_02·fixed` 批间极差 0.721 需二批 | 待批机时 |
| 4 | 治理层滞后（goal §7 停在 08-09、8 道题收缩未回写 governance）——我列了但本轮未做（owner 让先聚焦 harness+runner） | 云端后续 |

## 4. 本轮改动清单（供 review）

- 改：`src/probe_headless/ws_isolation.py`（tool_leak 判定 + TRANSIENT_TOOLS 常量）
- 新增：上述 4 个 open runner/server 文件
- 改：`task/task19/第一组8道题清单.md`（工程状态表：8 道 runner 全就绪）
- 台账：`TODO.md` T-167
