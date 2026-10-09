# E4 · task-06 收口报告 — 堵 H2 内置工具泄漏（task_id=19）

> 状态：✅ 已收口（2026-08-08）。对应任务书 `task-06-堵H2内置工具泄漏.md`；全程 DogFooding 免费额度。
> 一句话结论：**task-05 登记的 50 条非契约工具事件重新清点后实质泄漏为 0（46 条拒绝信封 + 4 条 CLI 内部 todo，无一触达 dag_poc/data、无一联网）；仍按任务书要求做了三层运行时禁止并同题重跑验证——重跑后 MCP 资源工具/Task* 尝试归零，残留 6 条 Read/Bash 尝试全部被拒（信息零返回），验收口径=泄漏计数 0（实质泄漏）+ 残留逐条举证无害。**

---

## 一、逐条清点与定性（交付物 1）

**口径订正**：重新清点 task-05 落盘的两条 `_full` 流（实数 fixed 12 条 / open 38 条，与 QA 上轮 fixed 4 条的差异 = 是否计入 MCP 资源工具），并**订正 task-05 report §一的误判**：

- task-05 report 写「含 Bash 实读工作区 .md」——**不成立**。那发生在更早的 bypassPermissions 探测轮（v1，已弃用）；现行 acceptEdits+disallowed-tools 参数组合下，所有 Bash/Read/Glob/Grep 尝试均收到拒绝信封。
- 50 条自动定性（脚本 `t06_leak_inventory.py`，可复跑）：
  - **BLOCKED 46 条**：返回为 `No such tool available` / `Resource not found` / `Directory listing is not enabled` / `No resources found` / `Unknown resource` 拒绝信封，**信息零返回**；
  - **BENIGN_TASK_TOOL 4 条**：open 跑中 TaskCreate×2/TaskUpdate×2 曾成功执行（task-05 时 Task* 未在 disallowed 名单）——入参/返回均为 todo 文案本身，不读文件、不联网；
  - **TOUCH_DATA 0 条、NETWORK 0 条**：DATA_PAT/NET_PAT 扫描 + 人工复核，无一读到 `dag_poc/data/**`，无一联网。
- 逐条举证清单（工具名/入参/返回/与受控数据的距离判断）：`t06_leak_evidence.md`（可复核），机器可读版 `t06_leak_inventory.json`；task-05 基准流已备份为 `traces_new/*_t05baseline_full.jsonl` 防覆盖。

**按任务书 §2.1 判断标准：无一条触达受控数据 ⇒ task-05 已落盘结果的有效性不受影响；但仍须做运行时禁止升级（§3：判分投影过滤 ≠ 运行时禁止）。**

## 二、堵漏方案与实测生效证据（交付物 2）

三层纵深，全部落在 `t05_h2_run_cli.py` 的 claude 分支，且**每层独立冒烟实测过**（诱导 prompt 让模型主动尝试 Bash/Read/TaskCreate/ListMcpResourcesTool 等，证据流 `t06_smoke_stream*.jsonl`）：

| 层 | 机制 | 实测证据 |
|---|---|---|
| ① CLI 参数 | `--disallowed-tools` 黑名单**补齐 TaskCreate/TaskUpdate/TaskStop/TaskGet/TaskList/TaskOutput 与 ListMcpResourcesTool/ReadMcpResourceTool/ReadMcpResourceDirTool**（task-05 遗漏的正是这批）+ `--allowed-tools mcp__lingshu-mock` 白名单 | smoke1：Bash 尝试 → `No such tool available` |
| ② settings 文件 | `--settings t06_claude_settings.json`：`permissions.deny` 拉黑全部内置工具（CLI 参数缺失时仍能拒） | smoke2：**去掉** --disallowed-tools 后 Bash/Read/TaskCreate 仍全被拒 |
| ③ PreToolUse hook | `t06_tool_gate.py` 白名单闸门：只放行 `mcp__lingshu-mock__*`，其余 exit 2 + 拒绝信封回灌模型（前两层全失时的兜底，且无需穷举工具名） | smoke3：**同时去掉** deny 名单与 disallowed-tools，TaskCreate 被 hook 拦下（流中可见 `hook error ... [t06 tool gate] BLOCKED`） |

配置与脚本：`t06_claude_settings.json`、`t06_tool_gate.py`（均在写作区内；不动用户级 `~/.claude/settings.json` 与工作区 `.claude/settings.local.json`）。hook 与 idealab 路由的 fetch-intercept hook 无冲突（本流中 SessionStart hook 正常触发）。

## 三、同题重跑验证（交付物 3，实测输出）

同一道题 case_co_drug_01 × claude（fixed），封堵生效后重跑（mock_server 按 case 重启，进程级终态纪律）：

```
[H2] CLI 结束：returncode=0 实测耗时=998s
PASS H1 CLI产出tool_call事件>0 — 278 条
[H2] 全量流工具面实况: ['Bash', 'Read', 'compare', 'get_entity', 'get_property',
                        'literature_search', 'search_candidates', 'submit_answer']
[H2] 判分投影过滤非契约工具事件 6 条（仅存于 _full 流）
PASS H2 判分投影均为9契约工具（前缀已剥） — 272 条，过滤 6 条内置工具
PASS H3 收卷通道 — submission_channel=submit_answer
PASS H4 判分器能吃（score_case 出分） — best=dc {cp1: 1.0, cp3: 0.611}
结论：PASS 4 / FAIL 0
```

**泄漏计数对比**（同一清点口径 `t06_leak_inventory.py`）：

| 项 | task-05 基准跑 | task-06 重跑 |
|---|---|---|
| 非契约工具事件 | 12 条（含 MCP 资源工具 8 条） | **6 条（Read×4 + Bash×2，全为尝试读契约文档）** |
| MCP 资源工具尝试 | 8 条 | **0**（deny 名单生效） |
| Task* 工具尝试 | 0（fixed）/ open 曾成功 4 条 | **0** |
| 实质泄漏（触数据/联网） | **0** | **0** |
| 残留 6 条逐条举证 | — | 全部为拒绝信封 `No such tool available: Read/Bash ... not enabled`，尝试目标是 INTERFACE_CONTRACT.md / answer_schema.json / property_spec.json（契约文档，非 dag_poc/data），**且信息零返回** ⇒ 按任务书 §2 要求 4 逐条举证无害（见 `t06_leak_evidence.md` 重跑节） |

**验收结论**：实质泄漏计数 0；残留 6 条均为被拒尝试（信息零返回），逐条举证无害。CLI 自身机制已足以封堵，**无需动用运行时隔离（无网络/隔离 cwd）方案**——且 claude 自身 API 依赖网络，无网络隔离本就不可行；agent 侧联网工具 WebFetch/WebSearch 在三层名单中均已禁。

**附注**（如实登记）：重跑 998s vs 基准 692s，耗时上升主要是模型 thinking_tokens 波动（2185 vs 181 条 thinking 事件）与 6 次被拒尝试消耗轮次，非封堵机制开销（hook 每次调用为本地 python，毫秒级）；best 由 ea→dc 属模型行为波动（判分器吃同一投影链正常出分，链路未退化）。

## 四、知会 E6（交付物 4）

E6 的 task-05 §1 **前置第 8 项**（「H2 内置工具泄漏：E4 报 fixed 跑泄漏 12 条，已派 E4-06 堵」）结论如下，知会文档落盘 `e6_notice_task06.md`：

1. **可以解除**：重清点实质泄漏为 0（task-05 时点即无一条触达 dag_poc/data/联网），且封堵已升级并同题重跑验证（MCP 资源工具/Task* 归零，残留 Read/Bash 尝试全被拒、信息零返回）。
2. H2 半边矩阵的隔离性缺口已堵：现行 `t05_h2_run_cli.py` 的三层封堵是 H2 矩阵跑的基线配置。
3. 遗留提醒：模型仍会偶发尝试 Read/Bash（被拒、耗轮次），H2 排期宜按实测耗时留足（见 §六 token 汇总的耗时数据）。

## 五、交付物索引

| 交付物 | 文件 |
|---|---|
| 逐条清点+定性脚本 | `t06_leak_inventory.py`（可复跑） |
| 逐条举证清单 | `t06_leak_evidence.md` + `t06_leak_inventory.json` |
| 封堵三件套 | `t06_claude_settings.json` + `t06_tool_gate.py` + `t05_h2_run_cli.py`（claude 分支） |
| 冒烟证据 | `t06_smoke_stream.jsonl` / `_stream2` / `_stream3` + stderr 日志 |
| 重跑产物 | `traces_new/case_co_drug_01__claude_{raw_stream,_full,.jsonl,_response}`（基准备份 `*_t05baseline_*`） |
| §7 token/日志汇总 | `t06_token_summary.py`（可复跑）+ `t06_token_summary.json` |
| 知会 E6 | `e6_notice_task06.md` |

## 六、§7 token/日志汇总（任务书硬门禁）

脚本 `t06_token_summary.py` 从落盘产物算出（可复跑复现），当前 14 个 run 摘要：

| 维度 | 数值 |
|---|---|
| 总 token | prompt 2,331,395 / completion 222,357 / **total 2,553,752** |
| 按 harness | H1：11 run、160 calls、2,010,674 total；H2：3 run、465 turns、543,078 total、wall 合计 2322s |
| 按模型（top） | Peach-DogFooding 703,834 / claude 543,078 / kimi-k3 472,996 |
| 按 template | fixed 9 run 1,325,588；open 5 run 1,228,164 |
| 单题分布 | 中位数 140,877；超长题（>3×中位数）：case_co_drug_01×Peach（449k）、case_dh_drug_01×claude（499k） |
| 缓存命中 | H1 cached/prompt=76.3%；H2 cache_read 为全轮次累计口径（脚本已注明不可与 H1 同层比较） |
| 重试/失败 | 625 calls 中 attempt>0 仅 2 次，失败响应 0 |
| 成本 | **全程 DogFooding 单价 0，成本 0**（H2 run_metadata.total_cost_usd 是 CLI 官方价表折算，非实际计费） |

层级注记（§7.5 反例预防）：H1 的 usage 在 `llm_call.response.usage`（含 cached_tokens/reasoning_tokens 原样）；H2 的 token 在 `_response.json.run_metadata`（dogfooding 通道 raw stream 消息层 usage 恒 0，实测登记）。

## 七、纪律自查

- 写作区：仅动 task/E4-agent-harness搭建/ 与（无）src/benchmark；禁改清单（evaluate.py/gateway.py/tools_schema.json/answer_schema.json/mcp_mock_server/**/templates/data/共享账本）零触碰。
- 环境口径：全程 dogfooding；server 每次端到端前重启；实验后杀 9000、清临时文件（见 progress 收口条）。
