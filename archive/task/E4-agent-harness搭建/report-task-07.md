# report-task-07：清 adapter 日志冗余（R7 嵌套深度断言）

> task_id=23 / E4-07 · 收口报告。定性依据：task-07 文档 §0——上轮 PR-53「H2 参数被套三层壳是 bug」定性已纠正：CLI 实际发 `{"arguments": "{真参数}", "session_key": ""}` **2 层合规**（MCP server 工具签名即 `def tool(arguments: str, session_key: str)`，CLI 照 schema 传）；trace 里的第 3 层是**后处理记日志时包壳加的**，不影响实际调用。本任务为日志冗余清理，硬约束：**不改实际调用链路，只改日志记录**。

## 一、问题定位（三层壳的真实成因）

实测定位（与 owner 纠正一致，且更精确）：

| 环节 | 形态 | 说明 |
|---|---|---|
| CLI 实际发出（raw_stream） | `{"arguments": "<真参数JSON串>", "session_key": ""}` 或 `{"arguments": "<真参数JSON串>"}` | 2 层合规，照 MCP inputSchema 传 |
| adapter `convert_cli_stream` | 原样记录 CLI input | task-07 前不做拆解 |
| `t05_h2_run_cli.py`「入参包壳对齐 H1」块 | keys≠{'arguments'} 时再包一层 | **第 3 层的制造者**（task-05 误以为 H1 是包壳形，实测 H1 trace 是平铺业务字段） |

存量实测：`case_co_drug_01__claude` 当前流 278 条中仅 2 条被包成三层壳（input 含 session_key 键的那批），其余为单层壳——与「99% 中招」的旧定性不符。

## 二、修改落点（只动日志记录，调用链路零改动）

| 文件 | 修改 | 证据：不碰调用链路 |
|---|---|---|
| `src/benchmark/cli_trace_adapter.py` | 新增 `unwrap_mcp_envelope()`（记 trace 时拆 MCP 面包壳为单层平铺，拆解失败保原样交 R7 报）与 `args_envelope_depth()`（沿 `{"arguments":"<JSON串>"}` 链计嵌套深度）；tool_call 记录处 `arguments` 改为拆壳后值 | adapter 只在转换落盘路径生效，不参与 CLI↔MCP 的实际请求；CLI 子进程命令行/入参一字未动 |
| `task/E4-agent-harness搭建/t05_h2_run_cli.py` | 删除「入参包壳对齐 H1」块（第 3 层壳来源），改为 R7 校验（嵌套深度 >1 即 FAIL）；`submitted_payload_from_events` docstring 订正（壳形分支仅旧产物兼容） | 该块本就只是转换后日志形态处理，位于 CLI 子进程结束之后 |
| `task/E4-agent-harness搭建/t05_adapter_reconcile.py` | R4 比较语义改为「raw input 经同一拆壳规则后与转换产物相等」；新增 R7（转换后嵌套深度 ≤1）；旧 R7（timestamp）让位改编号 R9；新增 `--check-converted` 独立模式对存量已转换产物跑 R7 | 对账脚本只读 |

禁改清单（evaluate.py/gateway.py/tools_schema.json/answer_schema.json/mcp_mock_server/**/templates/data/共享账本）零触碰。`evaluate.extract_tool_data` 只读 result 不读 arguments（实查确认），判分取数不受 arguments 形态影响。

## 三、R7 断言实现与实跑

实现：`args_envelope_depth(args)` 沿 `{"arguments": "<JSON串>"}` 链计数（0=平铺与 H1 同构，每层包壳 +1）；断言口径按文档 §3——**嵌套深度 >1 报错**。设计动机（文档 §1.1）：R4 深比较只能证明转换前后一致，两边都是包裹形态时照样通过；R7 直接校验形态，拦「两边一致但都错」的漏检。

### 3.1 对存量产物实跑（如实报出，不掩盖）

```text
--check-converted 存量转换产物：
case_co_drug_01__claude.jsonl              FAIL R7 — 超限 2/272 条（三层壳，task-07 前产物）
case_co_drug_01__claude_full.jsonl         FAIL R7 — 超限 2/278 条
case_co_drug_01__claude_t05baseline_full   PASS R7 — 276 条全部单层
case_dh_drug_01__claude.jsonl              PASS R7 — 146 条全部单层
case_cns_metoprolol__qwen3.7-max_full(H1)  PASS R7 — 53 条全部单层
```

报出的 2 条超限即 task-07 前的包裹产物（已备份 `*_t07pre.*` 留档）。存量 raw 流用新 adapter 重转换对账各 **11 PASS / 0 FAIL**（含新 R4 拆壳语义、R7、R9），证明第 3 层壳源于转换后包壳块而非 raw。

### 3.2 存量处理口径

task-07 文档未要求重生成全部存量；执行口径：**存量包裹产物不掩盖（R7 如实 FAIL 可复现）+ 同题产物用 --reprocess 重生成**（复用已落盘 raw，后处理与实跑同一代码路径）。重生成后判分投影 272 条全部单层、判分 exit 0，且与重生成前完全可比（见 §四）。

## 四、raw_stream 原样性 + 重处理/重跑验证

**raw 原样性证据**：
- 写入链路：`t05_h2_run_cli.py` L138 `subprocess.run(cmd, stdout=f)`——raw 由 CLI 子进程 stdout 直写，adapter 全程只读无写入路径（grep 实查）；
- 内容抽查：raw 中 tool_use.input 保持 CLI 原样（含 `session_key` 协议占位字段，样例 `{"arguments": "{\"domain\": \"drug\", \"type\": \"small_molecule\"}", "session_key": ""}`）；
- 指纹：case_co_drug_01 raw md5=0261c909ee2df3a8c1a59bbd13e635b4、case_dh_drug_01 raw md5=6d6ef8fff7ac7abce1a00be661bbb67e（另有 `*_raw_stream_t07pre.jsonl` 备份与原件逐字节一致）。

**--reprocess 重处理验证**（复用存量 raw，不调 LLM）：5/5 PASS exit 0——R7 278 条全部单层、收卷通道 submit_answer、判分出分；response 与重处理前逐字相同、通道相同（判分口径完全可比）。

**形态同构抽样**：
```text
新H2判分投影 search_candidates arguments= {"domain": "drug", "type": "small_molecule"}
H1基准      literature_search  arguments= {"query": "...", "max_results": 5}
```
均为单层平铺业务字段，判分取数无需拆壳。

**同题实跑**（交付物 4，case_co_drug_01×claude，server 重启后跑，输出留档 t07_rerun_output.txt）：

```text
[H2] 启动 CLI（stream → case_co_drug_01__claude_raw_stream.jsonl）@ 16:17:16
[H2] CLI 结束：returncode=0 实测耗时=422s
PASS task-07 R7 arguments嵌套深度≤1（与H1同构单层） — 99 条全部单层
PASS H1 CLI产出tool_call事件>0 — 99 条
PASS H2 判分投影均为9契约工具（前缀已剥） — 92 条，过滤 7 条内置工具
PASS H3 收卷通道 — submission_channel=text_fallback
PASS H4 判分器能吃（score_case 出分） — best=None task相关分={'cp1': 1.0, 'cp2': 0.0, 'cp3': 0.0, 'cp4': 0}
结论：PASS 5 / FAIL 0（exit 0）
```

实跑后追加验证：新产 raw 全量对账 11 PASS/0 FAIL（R4 拆壳语义/R7/R9 全过）；新产判分投影与 _full 流 R7 均全单层；新 raw 仍含 session_key 95 处（CLI 原样保留）。

如实登记（非本任务回归）：本轮交卷走 text_fallback（前两轮同题为 submit_answer），属模型交卷行为波动；判分 exit 0 链路不受影响。工具面实况含 Bash/Glob/Monitor/Read 尝试 7 条（封堵机制仍在位，仅存于 _full 流，判分投影已过滤）。

**判分可比性说明**：存量判分投影已用 --reprocess 重生成（同一 raw、同一判分器），重生成前后 response 逐字相同、通道相同；实跑轮为独立新样本（模型行为波动属常态，与形态修改无关——evaluate 只读 result，arguments 形态变化不影响取数，实查确认）。

## 五、遗留与交接

- 存量 H1 产物（qwen/glm/kimi 系列）本就平铺，R7 全过，无需处理；
- R 编号变更（旧 R7→R9）仅 E4 对账脚本内部口径，已在本报告与脚本 docstring 登记，无外部消费方；
- 二期新 harness 接入同一 adapter 即自动获得拆壳与 R7 校验（契约 §六 adapter 硬条款不变）。
