# E4 · task-04 收口报告（PR-40 harness 一半：per-case 视图接线 + task-03 状态行修复）

**核心结论**：harness 半边全部完成并实跑验证——case_id 从 case JSON 读入（不另建映射），以「现行通道 `DAGPOC_CASE_ID` 校验 + 会话头 `X-Dagpoc-Case-Id` 预留」双通道传入 gateway 会话；open 类拿不到 case_id 或通道不一致**直接报错**（`CaseIdentityError`，无默认值、不跳过），fixed 类接口路径统一不阻断；8/8 断言实跑全过、fixed 回归冒烟管道正常、留痕字段按 ACTION_RULES §7.1 补齐（code_version/索引三元组实测在场）。**合跑（PR-40 完整验收）已由 task-05 完成（2026-08-08，13/13 PASS，见 §四）**：当时阻塞原因为 E3 gateway §10 未实施、open case 视图文件未建，后随 E3 交付与改用带视图的 case_dh_drug_01 彻底解除。task-03 状态行已修复（第六次滞后到此为止）。

---

## 一、交付标准四条逐项核对

| # | 标准 | 状态 | 证据 |
|---|---|---|---|
| 1 | harness 传 case_id（从 case JSON 读；open 缺失时报错不静默） | ✅ | `resolve_case_identity()` 从 case JSON 读 case_id 并按 gt 字段存在性判 open/fixed；open 缺 case_id → CaseIdentityError；`verify_case_channel()` 对 open 校验通道一致性，缺失/不一致同样报错。实测 §三 |
| 2 | 与 E3 的参数名对齐（report 写清接缝约定） | ✅ | §二（参数名 `case_id`，头名 `X-Dagpoc-Case-Id`，通道依据 E3 task-05 §10 与 mock_server 现行机制；E3 未定通道 → 待对齐项已登记 progress.md） |
| 3 | 合跑实测（待 E3 到位） | ✅ 完成 | task-05 实跑 13/13 PASS（B1 零泄漏/B2 gt 不可达），见 §四 |
| 4 | task-03 收口 + 状态行更新 | ✅ | §五（三处状态行修复清单 + 自查结论） |

## 二、接缝对齐结论（与 E3 文档的对应依据）

**结论**：参数名统一为 `case_id`；传递采用「现行通道校验 + 会话头预留」双轨，E3 §10 到位后无需 harness 再改。

| 接缝面 | 约定 | 依据 |
|---|---|---|
| 参数名 | `case_id`（与 case JSON 字段同名，不建映射） | E3 task-05 §10.3 原文「gateway 会话按传入的 `case_id` 加载对应视图（`dag_poc/data/backend/per_case_views/<case_id>.json`）」；E4 task-04 §1.3 要求 2 |
| 现行通道 | 环境变量 `DAGPOC_CASE_ID`（server 启动时装载，harness 侧校验与当前 case 一致） | E3/E4 文档中**已存在的唯一通道**：`dag_poc/mcp_mock_server/mock_server.py` L74-76 按该变量装载 B 分区 `cases[case_id]`；owner 指示「E3 未写明则先用文档中已存在的通道做实现」 |
| 会话级预留 | HTTP 请求头 `X-Dagpoc-Case-Id`，MCPBackend 建立会话时注入、每个 MCP 请求携带 | mock_server 的启动时环境变量无法表达"会话"语义；E3 §10 要求的是"gateway **会话**按 case_id 加载视图"，请求头是 harness→gateway 会话级传参的标准位；mock_server 忽略未知头，不影响现状。E3 若裁定用别的通道（如 initialize 参数），只需改 `CASE_ID_HEADER` 一个常量 |
| 与 DAGPOC_CASE_ID 的关系 | 二者同值、互补：环境变量管"server 启动时装载哪个 case 的数据"（B 分区），请求头管"本会话查询走哪个 case 的视图"（E3 §10）。run_case 对 open 类强制校验环境变量与 case_id 一致，防"server 装载的 case 与跑的 case 不同"造成隔离错配 | mock_server.py L38/L74-76 机制实测 |

**待对齐项（已登记 progress.md）**：E3 §10 未写明 case_id 传递通道与头名；本实现按上表先行，E3 落地后若参数名/通道不一致，harness 侧改 `CASE_ID_HEADER`/`CASE_ID_ENV` 常量即可，一行对齐。

## 三、缺 case_id 报错路径实测证据（t04_verify_view_wiring.py mode A，实跑 8/8 PASS）

```
PASS A1 open缺case_id报错          # resolve_case_identity：无 gt（open）且无 case_id → CaseIdentityError
PASS A2a open通道缺失报错          # DAGPOC_CASE_ID 未设置 → CaseIdentityError
PASS A2b open通道不一致报错        # DAGPOC_CASE_ID=case_y ≠ case_x → CaseIdentityError
PASS A3 open通道一致放行
PASS A4a fixed识别 — case_cns_metoprolol/fixed
PASS A4b fixed通道不一致不阻断     # 仅 WARN，接口路径统一走 verify_case_channel
PASS A5 会话头X-Dagpoc-Case-Id — {'X-Dagpoc-Case-Id': 'case_x'}
PASS A6 open真实case取case_id — case_open_balanced_superconductor/open
结论：PASS 8 / FAIL 0（exit 0）
```

复跑命令：`cd /Users/jingwu/work/benchmark && .venv/bin/python task/E4-agent-harness搭建/t04_verify_view_wiring.py`

## 四、合跑终验收（task-05 实跑完成，13/13 PASS，阻塞解除）

> **2026-08-08 回填（task_id=17/task-05）**：两前置均已到位——① E3 gateway §10 已实施并入库（per_case_views 引用 / ENV_CASE_ID 通道 / CASE_VIEW_MISSING fail-closed 三接线标志实测全 ✅）；② 目标 open case 视图已建（改用带视图的 discovery_haystack 类 case_dh_drug_01；旧默认 case_open_balanced_superconductor 的视图至今未建，不再等它）。本节下方旧阻塞登记仅作历史留档。

**实跑结果（2026-08-08 16:14-16:22，完整输出留档 t05_joint_run_20260808.log）**：

```bash
cd /Users/jingwu/work/benchmark
# mock_server 先以目标 case 重启：DAGPOC_CASE_ID=case_dh_drug_01 DAGPOC_CASE_KIND=open
.venv/bin/python task/E4-agent-harness搭建/t04_verify_view_wiring.py --joint
```

```
[前置探测 @ 2026-08-08 16:14:44]
  gateway §10 per_case_views 引用: ✅
  gateway §10 ENV_CASE_ID 通道: ✅
  gateway §10 CASE_VIEW_MISSING fail-closed: ✅
  视图目录实况: dag_poc/data/backend/per_case_views 共 34 份
  目标视图文件 case_dh_drug_01.json: ✅
[合跑] DAGPOC_CASE_ID=case_dh_drug_01 DAGPOC_CASE_KIND=open model=Peach-07-17-DogFooding
PASS B1 查询返回集⊆白名单 — 泄漏 0 次
PASS B2 gt不可达 — gt=z064386
trace: traces_new/case_dh_drug_01__Peach-07-17-DogFooding.jsonl
结论：PASS 13 / FAIL 0（mode A 11 项 + B1/B2，exit 0）
```

**断言口径（task-05 修订后）**：B1 = trace 中每次 A 侧查询（5 个契约查询工具一个不绕）返回的实体 ID ⊆ 视图白名单（**visible_entity_ids** 为准，与 gateway §10 过滤同基准；缺失时回落 family_sequence + distractors），文献 doc_id 另对 literature_doc_ids 白名单分开断言（papers 的 doc_id 是 DOI 非实体 ID）；B2 = gt（view.gt.entity_ref，本 case 为 z064386）不出现在任何查询返回中。实跑 H1 × Peach-07-17-DogFooding（dogfooding 免费档，ADR-0019）30 事件、交卷通道 submit_answer，B1 零泄漏、B2 达成——**PR-40 完整验收通过**。

---

**历史阻塞登记（已解除，留档）**：落笔时（2026-08-07）两前置都不满足：1. E3 gateway §10 未实施；2. open case 视图文件未建（仅 E1 示例）。后经 task_id=12/15 探测修正与 E3 交付推进，收窄为单一前置（旧默认 case 视图未建），task-05 改用带视图的 case_dh_drug_01 后彻底解除。

## 五、task-03 状态行修复清单（第六次滞后，本次一并自查）

| 文件 | 修复 |
|---|---|
| `task-03-trace落盘防护与证据固化.md` 头部状态行 | ⚠️"实际已完成但状态行滞后" → ✅ 已收口（2026-08-07） |
| `task-04-视图接线与task03收口.md` 头部状态行 | 🟡 待开工 → ✅ 已收口（本 task 自身状态，同步更新） |
| `task.md` §9 状态板 | task-03 行补注"QA 核验六项全过、状态行滞后已修复、PR-34 已实跑达成"；新增 task-04 行 |

**自查结论**：E4 维护的全部状态位点已逐一核对——task.md 表头 owner 原文（"✅ task-02 已收口"）零改动未回退；progress.md 为持续更新体无状态行；trace_schema.json 的 status 字段是格式定稿声明非任务状态；无其他滞后点。

## 六、harness 改动落点（src/benchmark/agent_harness.py，全部在写作区）

| 落点 | 内容 |
|---|---|
| `CASE_ID_HEADER` / `CASE_ID_ENV` / `CaseIdentityError` | 接缝常量与专用异常（对齐全文命名） |
| `resolve_case_identity(case)` | 从 case JSON 读 case_id；按 gt 字段存在性判 open/fixed；open 缺 case_id 报错 |
| `verify_case_channel(case_id, case_kind)` | open：通道缺失/不一致 → 报错；fixed：仅 WARN 不阻断（接口路径统一） |
| `MCPClient(session_headers)` / `MCPBackend(case_id)` | 会话头 X-Dagpoc-Case-Id 每个 MCP 请求携带 |
| `code_version()` | git HEAD 短 hash（ACTION_RULES §7.1 code_version 字段） |
| `Tracer(case_id, model)` / `record_llm` | llm_call 事件补 case_id/model/code_version 索引字段（实测在场：958dfa0） |
| `llm_chat` / temperature | 删除 moonshot 硬编码判断，统一走 `temperature_for(model)`（ACTION_RULES §7.3） |
| `run_case` | 接线入口：resolve_case_identity → verify_case_channel → Tracer/Harness 携带 case_id |
| `SYSTEM_PROMPT` | 补 open 类答卷指引；**operator 枚举按 answer_schema 0.3.0 收窄**：只引导 add_atom/remove_atom（detail 格式 `<token>` 或 `<token>*<n>`），明示旧三算子 replace_substituent/substitute_element/ring_closure 已无判分语义不得使用（E3 知会，照办；枚举值以 dag_poc/answer_schema.json L77 现文为准，该文件禁改只读引用） |
| `dag_poc/trace_schema.json` | llm_call 增 case_id/model/code_version 三字段声明（required=false，向后兼容历史 trace） |

**回归冒烟（改造后管道正常 + 留痕达标）**：case_cns_metoprolol × qwen3.7-max 实跑 exit 0（steps=14/tools=39(ok=18)/111s，落 traces_new/）；_full.jsonl 14 条 llm_call 逐条含 code_version=958dfa0、case_id、model、usage、finish_reason，字段齐全。注：本组本轮 answer=NO_SOLUTION（task-03 时同组合为 ec），模型行为有波动，不影响管道验证结论。

## 七、纪律自查

- 写作区：仅动 src/benchmark/agent_harness.py、task/E4-agent-harness搭建/**、dag_poc/trace_schema.json；
- 禁改清单零触碰：evaluate.py / gateway.py / tools_schema.json / answer_schema.json（只读引用其 L77 枚举）/ mcp_mock_server/** / templates / data / query_spec / 共享账本；E3 写作区只读参考（task-05 文档 §10、progress.md）；
- `dag_poc/traces/` git status 零改动（收口时实测）；
- 实验后 mock_server 已杀（端口 9000 释放）、临时日志已清；
- LLM 留痕按 ACTION_RULES §7 全程执行（完整 request/response + usage/finish_reason + code_version）。

## 八、剩余缺口

1. **合跑阻塞（等两前置）**：E3 gateway §10 视图加载 + E5 open case 视图文件；到位后按 §四 命令合跑并回填结果。
2. **接缝待 E3 确认**：case_id 传递通道/头名以 E3 §10 落地为准，不一致时改 harness 常量一行对齐（progress.md 已登记）。
3. 真 A/B 发现型 open case 缺失（等 E5），合跑用 case 以届时已有视图文件的 open case 为准。
