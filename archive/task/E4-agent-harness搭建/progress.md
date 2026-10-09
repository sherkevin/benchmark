# E4 · agent-harness搭建 · progress

> 对应任务板 task_id=12/13/17/19/23/25（task-04 合跑前置探测回填 + harness接口契约.md 补写 + task-05 H2 真实 harness 接入与合跑终验收 + task-06 堵 H2 内置工具泄漏 + task-07 清 adapter 日志冗余 + task-08 H2 llm_call 证据流；上接 task_id=10 task-04 收口）。
> 工作区纪律：只写 task/E4-agent-harness搭建/、src/benchmark/ harness 代码、dag_poc/trace_schema.json；dag_poc/traces/ 只读。

## 已完成

**task_id=25 task-08：H2 llm_call 证据流（逐次 usage，2026-08-10，收口报告 report-task-08.md）**：

- **判断（第一产出，盘上实证）**：claude CLI 默认流的 assistant 事件 usage 恒 0 占位（3 条存量 raw 全扫：139/368/323 条 assistant 非零仅 dh 流 3 条同时刻重复事件，不可靠），只有 result 事件给会话级总数；加 `--include-partial-messages` 后每回合 stream_event.message_delta 带真实 usage——**能拿到，已按补流路径落地**。
- **补流落点（只碰记录不碰行为）**：claude 命令行加开关（只增 stdout 事件种类）；adapter 新增 `_llm_call_from_delta()` 产 llm_call（与 tool_call 共享 seq 交织）；降级字段 null 注明原因（request/timestamp/latency_ms/attempt/reasoning，登记 trace_schema.json `h2_llm_call_degraded`）；对账新增 R10a/b/c；汇总脚本 H2 改逐次累加、拿不到写 null；禁改清单零触碰（含 src/eval_matrix/**，对 E6 只出知会 e6_notice_task08.md）。
- **回归实测（owner 最关心）**：存量流零影响（无 stream_event→不产 llm_call，存量 raw 复跑对账 11 PASS/0 FAIL，R10 如实 INFO 跳过）；同题重跑 5/5 PASS exit 0（316s，llm_call 26 条，tool_call 对账 57/57，封堵/R7 全在位，收卷 submit_answer，判分 exit 0；best=z000608 与上轮 dc 不同属模型答案波动如实登记）。
- **交叉校验（两路独立取数）**：message_delta 逐次累加 vs result 会话级总数，R10c 四项一致（input=10,181/output=11,747/cache_read=1,038,550/cache_creation=51,210）；不等时 FAIL 报出不静默。
- **存量封顶影响面**：task-08 前 H2 产物（dh×claude、co×qodercli、co×claude 旧轮）token 七项计 null 不回填（汇总脚本实测输出 null+原因）；结果表成本列空，论文限定句建议已写入报告 §三问③。环境：实验后杀自起 :9000，备份 *_t08pre.* 留档。

**task_id=23 task-07：清 adapter 日志冗余 + R7 嵌套深度断言（2026-08-09，收口报告 report-task-07.md）**：

- **定性跟进（owner 纠正 PR-53）**：三层壳非 bug——CLI 实际发 `{"arguments": "{真参数}", "session_key": ""}` 2 层合规（MCP 签名即 `def tool(arguments: str, session_key: str)`）；第 3 层是 `t05_h2_run_cli.py`「入参包壳对齐 H1」块后处理加的（task-05 误以为 H1 是包壳形，实测 H1 trace 是平铺）。存量实测仅 2/278 条被包三层（非 99%）。硬约束遵守：CLI→MCP 实际入参形状零改动，只改日志记录。
- **动作 1（adapter 记 trace 去包裹）**：cli_trace_adapter.py 新增 `unwrap_mcp_envelope()`/`args_envelope_depth()`，tool_call 记录处拆壳为与 H1 同构的单层平铺；t05_h2_run_cli.py 删包壳块改 R7 校验。新产形态抽样与 H1 同构（单层平铺业务字段，判分取数无需拆壳）。
- **动作 2（raw 原样性）**：raw 由 CLI 子进程 stdout 直写（t05_h2_run_cli.py L138），adapter 只读无写入路径；抽查 input 仍含 session_key 协议占位（CLI 原样）；md5 指纹留档（报告 §四）。
- **动作 3（R7 断言）**：嵌套深度 >1 报错；旧 R7（timestamp）让位改 R9；R4 同步改拆壳后比较；新增 `--check-converted` 模式对存量转换产物独立跑。存量实跑：包裹旧产物如实 FAIL 报出 2 条（不掩盖，备份 *_t07pre.*），H1 与 baseline 流全过；存量 raw 重转换对账 2 条流各 11 PASS/0 FAIL。
- **存量处理口径**：同题产物 --reprocess 重生成（复用存量 raw，5/5 PASS exit 0，272 条全单层，response 与重生成前逐字相同、通道相同，判分口径完全可比）。
- **同题实跑**（case_co_drug_01×claude，server 重启后跑，422s，5/5 PASS exit 0）：R7 99 条全单层、判分投影 92 条 9 契约工具、判分 exit 0；新产 raw 全量对账 11 PASS/0 FAIL；交卷走 text_fallback（模型行为波动如实登记，前两轮同题为 submit_answer，与形态修改无关——evaluate 只读 result）。输出留档 t07_rerun_output.txt。
- 环境：实验后杀自起 :9000；备份四件套 *_t07pre.*（含 raw）留档。

**task_id=19 task-06：堵 H2 内置工具泄漏（2026-08-08，收口报告 report-task-06.md）**：

- **重清点订正（实质泄漏 0）**：task-05 两条 _full 流 50 条非契约工具事件 = 46 条拒绝信封（信息零返回）+ 4 条 CLI 内部 todo（TaskCreate/TaskUpdate，不触数据不联网）；**无一读 dag_poc/data/**、无一联网**。task-05 report §一「Bash 实读工作区」证实为误判（那是更早 bypassPermissions 探测轮行为），已就地订正。清点+定性脚本 t06_leak_inventory.py（可复跑），逐条举证清单 t06_leak_evidence.md，基准备份 traces_new/*_t05baseline_*。
- **三层封堵全部冒烟实测生效**（诱导 prompt 逼模型主动尝试旁路工具，证据流 t06_smoke_stream*.jsonl）：① --disallowed-tools 补齐 Task*/MCP 资源工具（task-05 遗漏点）；② --settings t06_claude_settings.json permissions.deny（去掉 CLI 参数仍能拒）；③ PreToolUse hook 白名单闸门 t06_tool_gate.py（前两层全失时仍拦，流中可见 hook error BLOCKED 信封）。落地 t05_h2_run_cli.py claude 分支；不动用户级/工作区级 settings。
- **同题重跑验证**（case_co_drug_01×claude，server 重启后跑，998s，4/4 PASS，交卷通道 submit_answer）：MCP 资源工具/Task* 尝试归零；残留 6 条 Read/Bash 尝试全被拒（信息零返回，目标为契约文档非受控数据），逐条举证无害 ⇒ 验收口径达成（实质泄漏 0 + 残留举证）。无需运行时隔离方案（claude 自身依赖网络，无网络隔离不可行）。
- **知会 E6**：其 task-05 §1 前置第 8 项解除，知会文档 e6_notice_task06.md（含 H2 基线配置与排期提醒）。
- **§7 token/日志汇总硬门禁达成**：t06_token_summary.py 可复跑，7 维汇总（总/模型/harness/template/单题分布/缓存命中/重试）从落盘产物算出；当前 14 run 总 token 2,553,752，全程 DogFooding 单价 0；层级注记已写进脚本 docstring（H1 usage 在 llm_call.response.usage；H2 在 _response.json.run_metadata，dogfooding 消息层 usage 恒 0 实测登记）。
- 环境清理：实验后杀 :9000、清 /tmp/t06_* 临时文件。
- **9000 端口进程归属登记（task_id=22，QA 终核纪律项；结论：非 E4 遗留，未代杀）**：QA 观测到 :9000 存活进程 PID 67297（启动 2026-08-08 23:13:51）。E4 实查归属：命令行 `mock_server.py`，环境变量 `DAGPOC_CASE_ID=case_cs_material_05`，父进程 PID 58666（23:07:58 启动）为 `src.eval_matrix.matrix_runner run --models bailian/glm-5.2 --harness H1 ...`——属 **E6 矩阵跑**进程链，且正在轮转 case（QA 两次观测 PID 62501→67297 更替即 matrix_runner 逐 case 重启 server 的行为）。E4 侧事实：task-06 最后实验（同题重跑）约 22:53 结束，收口时已杀自起 server（原 PID 21943，22:36 起）并复测 lsof 为空；PID 67297 启动于 23:13:51，晚于 E4 收口清理，非 E4 所起。「实验结束杀 9000」纪律已执行，该进程由监工/E6 自行处置，后续核验不得归责 E4。
- **小返工（task_id=21，QA 检查 1 实锤）**：t06_leak_inventory.py 的 RUNS 指向当前 _full 流（已被重跑覆盖，泄漏 12→6），与举证文档自述的基线备份数据源不符，仓根复跑输出 44 条≠声明的 50 条。修复：RUNS 拆两套——baseline（两条 *_t05baseline_full.jsonl，佐证 task-05 登记）+ rerun（封堵后当前流，佐证封堵效果），仓根复跑实测输出 baseline 50=46 BLOCKED+4 BENIGN+0 实质、rerun 6 条全拒，与 t06_leak_evidence.md 结论逐字一致（恢复“脚本即证据”）。口径订正一处：举证文档 §结论第4条 BLOCKED 分布 open 33→34 条（漏计 TaskStop×1 拒绝信封，复算实数 fixed 12+open 34=46）。实质结论不变。

**task_id=17 task-05：H2 真实 harness 端到端 + PR-40 合跑终验收 + 交卷断链修复（2026-08-08，收口报告 report-task-05.md）**：

- **H2 端到端跑通（H=2 达成）**：CLI 选型实查——qodercli 1.1.2 被内容安全网关全线拦截（error_code=406，化学/药物内容全拒，状态不稳定，**如实报不硬凑**）；改用 claude 2.1.212（Claude Code），原生 MCP 直连 9 契约工具无工具桥。链路：读 case → claude stream-json → **cli_trace_adapter 转换（硬边界，不直连判分）** → 双轨落盘 → evaluate 判分。fixed（case_co_drug_01）：4/4 PASS、submit_answer 交卷 accepted、best=ea、692s；open（case_dh_drug_01 带 case_id 走视图）：4/4 PASS、1313s、链路全通（答卷被 SCHEMA_VIOLATION 拒收走文本兜底，属模型行为如实登记）。两条 raw stream adapter 逐字段对账各 10/10 PASS（t05_adapter_reconcile.py 可复跑；assistant 级 usage/stop_reason 在 H2 trace 为缺项已如实登记）。复跑脚本 t05_h2_run_cli.py（含 --reprocess）。
- **风险如实登记**：claude --disallowed-tools 无法完全禁住内置工具（fixed 泄漏 12 条/open 泄漏 38 条，含 Bash 实读工作区 .md）——判分投影只留 9 契约工具兜住，_full 流如实保留；H1/H2 对比须注明口径差异。**耗时知会 E6**：fixed 类 H2 692s vs H1 940s 同量级；open 类 H2 实测 1313s 已超 E6 规划值 600s/次 2.2 倍，请重算排期。
- **PR-40 合跑终验收通过（13/13 PASS exit 0）**：t04_verify_view_wiring.py --joint 实跑（2026-08-08 16:14，前置探测三接线标志全 ✅、视图 34 份）：B1 查询返回集⊆白名单零泄漏（实体对 visible_entity_ids、文献 doc_id 对 literature_doc_ids 分开断言）、B2 gt（z064386）不可达。已回填 report-task-04.md §四，⛔️第3条阻塞解除。留档 t05_joint_run_20260808.log。
- **交卷断链修复（owner 实测发现，双层根因）**：① 引导层——run 循环提醒旧文案引导文本输出 + SYSTEM_PROMPT 无步数预算；② 预算层——推理模型 reasoning_tokens 吃满 max_tokens=4096（finish_reason=length 零输出，历史 4/6 text_fallback 即此病灶）。修复：SYSTEM_PROMPT 第4条交卷纪律 + 第7条 payload 包壳硬约束、max_tokens→16384、finish_reason 注入、length 截断提醒 continue、交卷兜底轮。dogfooding 复跑验证 4/4 PASS（940s、steps=15、tool_calls=107(ok=20)、submission_channel=submit_answer、answer=bd、过 answer_schema），脚本 t05_rerun_submit_fix.py。只动 agent_harness.py，禁改清单零触碰。
- 附带：kind 口径扩展（view_ref 非空⟹open，discovery_haystack 22 道不再误判 fixed）+ DAGPOC_CASE_KIND 第三通道 fail-closed（t04 mode A 回归 11/11 PASS）；环境发现——mock_server 交卷终态是进程级状态（每次端到端跑前必须重启 server）。

**task_id=15 返工：探测快照过期修正（2026-08-07 21:28）**：

- **返工缘由（教训登记）**：task_id=12 的探测落笔于 21:21:50，但 E3 并行推进甩开了人工快照——落笔时 gateway §10 接线已在工作区（mtime 21:23:29 前后）、drug 系列视图 21:17:42 已落盘，结论落笔即部分过期。QA 实锤 1 项失败（两处「未实施/仅 e105」与实况矛盾）。教训：**探测结论必须带时刻戳与时效声明，前置状态以可复跑脚本的实时输出为准，不写人工快照**。
- 复核后实况（自行实查非照抄 QA，且比 QA 快照又进一步）：gateway.py L681 起 §10 接线块完整在位（ENV_CASE_ID/_resolve_case_view/CASE_VIEW_MISSING/L755 visible_entity_ids 过滤），QA 报的"工作区未提交态"复核时已随 commit 6c14fdd（21:25）入库，本文按提交态记录；per_case_views/ 23 份（drug×15+spare×3+material×4+e105，均已入库）；阻塞结论仍成立但收窄为单一前置（case_open_balanced_superconductor.json 未建）。
- 根治（QA 建议落地）：t04_verify_view_wiring.py `--joint` 增 `probe_preconditions()` 前置自动探测（实时打印 gateway 三接线标志 + 视图目录份数 + 目标视图存在性）；复跑确认 BLOCKED exit 3、探测输出正确，完整输出留档 `t04_joint_probe_20260807.log`；mode A 回归 8/8 PASS。
- 顺带修正两处脚本口径偏差：B1 白名单改以 visible_entity_ids 为准（与 gateway 过滤同基准）；case JSON 查找扩到全 templates 目录（新视图对应 discovery_haystack 类 case）。
- 接缝对照结论：E3 env 通道与 E4 CASE_ID_ENV 同名同义，HTTP 头双方均二期预留——已对齐零改动（契约 §四 已补「已与 E3 实现对齐（commit 6c14fdd 提交态）」）。
- 修订位点：progress.md ⛔️第3条/待对齐项第1条、harness接口契约.md §四（状态句 + 双通道句，均带探测时刻与时效声明）。

**harness接口契约.md 交付（task_id=13，2026-08-07）**：

- 路径：`task/E4-agent-harness搭建/harness接口契约.md`（78 行，金字塔结构：速读总览 + 每节一句话小结打头）。
- 六节全覆盖，全部以已落地实测代码为准（基准 agent_harness.py 711 行实读）：① trace 格式（引 trace_schema.json v1.0.0，tool_call 五要素+ok、llm_call 留痕、双轨落盘与 evaluate.py 双格式兼容）；② 答卷（answer_schema v0.3.0 六字段，operator 仅 add_atom/remove_atom，收卷双轨）；③ 模型配置（.env 凭据/三模型/temperature_for/重试留痕）；④ case_id 接缝（open 硬报错 + 待 E3 终确认状态）；⑤ 输出路径三级优先级；⑥ adapter 硬条款（新接真实 harness 必走 cli_trace_adapter.py 共用同一契约，含 ok 字段补齐责任）；附基准文件清单。
- 机器可读部分一律引用 schema 文件路径，未复制大段 JSON。

**task-04 合跑前置二次探测（task_id=12，2026-08-07）**：

> ⚠️ 本条探测结论（两前置均未到位）已被 task_id=15 返工修正：落笔时 E3 §10 接线与 drug 系列视图已在工作区/已落盘，结论部分过期；现行口径以 task_id=15 条目与 ⛔️ 节第 3 条为准。

- 结论：两前置（E3 §10 视图加载 / open case 视图文件）**均未到位**，不空等不越位；探测依据（文件/行号/commit）已更新进 ⛔️ 节第 3 条。
- 接缝自查：DAGPOC_CASE_ID（mock_server.py L76）与 X-Dagpoc-Case-Id 均与 E4 实现一致，零调整。
- 附带处理：E3 §11.4 知会的 commit 指针订正（df93eca→b85f4a7）已落实到 e3_handover/README.md、run_eval.py、report-task-03.md（明细见待对齐项第 2 条）。

**task-04（上轮，已收口）**：

- harness→gateway case_id 视图接线（§1 主任务，PR-40 harness 一半），全部落在 `src/benchmark/agent_harness.py`：
  - `resolve_case_identity(case)`：case_id 从 case JSON 读（不另建映射）；按 gt 字段存在性判 open/fixed；**open 缺 case_id → CaseIdentityError（无默认值、不跳过）**；
  - `verify_case_channel(case_id, kind)`：open 类校验现行通道 `DAGPOC_CASE_ID` 与当前 case 一致，缺失/不一致报错；fixed 类仅 WARN 不阻断（接口路径统一）；
  - 会话级预留：`MCPBackend(case_id)` → 每个 MCP 请求携带 HTTP 头 `X-Dagpoc-Case-Id`（供 E3 §10 gateway 按会话加载视图）；
  - `run_case` 接线：resolve → verify → Tracer/Harness 携带 case_id。
- 接缝对齐结论：参数名 `case_id`（E3 task-05 §10.3 原文）；现行通道用文档中已存在的 `DAGPOC_CASE_ID`（mock_server 按 case 装载 B 分区即读它），会话头 `X-Dagpoc-Case-Id` 为预留；E3 若另行裁定通道，改 `CASE_ID_HEADER`/`CASE_ID_ENV` 常量一行对齐。
- 验收断言脚本落盘并实跑：`t04_verify_view_wiring.py` —— mode A **8/8 PASS exit 0**（含缺 case_id 报错、通道缺失/不一致报错、通道一致放行、fixed 识别/不阻断、会话头接线、真实 open case 取 case_id）；`--joint` 实跑正确报 BLOCKED exit 3（视图文件不存在）。合跑命令与断言口径见 report-task-04.md §四。
- 回归冒烟（改造后管道正常）：case_cns_metoprolol × qwen3.7-max 实跑 exit 0（steps=14/tools=39(ok=18)/111s，落 traces_new/）；留痕达标：_full.jsonl 14 条 llm_call 逐条含 code_version=958dfa0、case_id、model、usage、finish_reason（本轮补齐 `code_version()` 与 Tracer 索引三元组，trace_schema.json 同步增 case_id/model/code_version 字段声明）。
- operator 枚举收窄（0.2.0→0.3.0，E3 知会照办）：SYSTEM_PROMPT 只引导 add_atom/remove_atom（detail 格式 `<token>` 或 `<token>*<n>`），明示旧三算子 replace_substituent/substitute_element/ring_closure 已无判分语义不得使用；枚举以 dag_poc/answer_schema.json L77 现文为准（只读引用）。
- task-03 状态行修复（第六次滞后收口）：task-03 文档头部状态行 → ✅ 已收口；task-04 文档头部状态行同步；task.md §9 状态板补注 + 新增 task-04 行（表头 owner 原文零改动未回退）；自查无其他滞后点。
- 收口报告：report-task-04.md。

**task-03（上轮，已收口）**：

- 落盘路径可配置改造（§1 要求 2、3）：
  - `agent_harness.py` 新增 `DEFAULT_TRACE_DIR`（= `task/E4-agent-harness搭建/traces_new/`）与 `resolve_trace_dir(trace_dir)`，优先级：显式参数 > 环境变量 `TRACE_OUT_DIR` > 默认值；`run_case(trace_dir=None)` 缺省走解析；
  - `run_all.py` 删除写死的 `TRACE_DIR = dag_poc/traces`，新增 `--trace-dir` 参数，批跑入口打印实际落盘目录；
  - `dag_poc/trace_schema.json` 增 `trace_dir_resolution` 字段，file_convention/answer_contract 改为 `<trace_dir>` 口径。
- 9 组冒烟证据迁移（复制不动原件）：`task/E4-agent-harness搭建/traces_new/`（27 文件，从 smoke_evidence 快照复制；dag_poc/traces/ 原件零改动）。
- E3 交接材料固化（§2）：`task/E4-agent-harness搭建/e3_handover/` —— 3 组 fixed 类（≥2 达标）：
  1. case_cns_metoprolol × bailian/glm-5.2（submit_answer 交卷，33 调用/20 命中，推荐首选）；
  2. case_cns_metoprolol × moonshot/kimi-k3（text_fallback，30 调用/18 命中）；
  3. case_cardiac_sotalol × moonshot/kimi-k3（NO_SOLUTION，35 调用/6 命中；文档建议的 sotalol×qwen 组实测 ok 仅 2、证据不足，已换组并在 README 说明）。
  - 含 README.md（逐组 case_id/模型/工具清单实数统计/复跑命令/当前预期/PR-34 通过标准）、run_eval.py（一键判分复跑，实测 exit 0、cp1-cp5 全 0 符合修复前预期）、MD5SUMS（9 文件 md5，一键校验已验证无 diff）。
- 最小冒烟验证（§1 要求 4）：case_cns_metoprolol × qwen3.7-max 实跑（11:23:57–11:26:07，exit 0，steps=15/tools=38(ok=18)/channel=submit_answer）；
  - 新路径生效：三件套新产物 11:26 落 traces_new/，run_all 打印落盘目录为 traces_new；
  - dag_poc/traces/ 零新增写入：冒烟前后 195 文件的文件名/size/mtime 清单 diff 为空、全内容聚合 md5 冒烟前后一致（eb7629e587e64c7898376e46d5ef6e3a）；
  - resolve_trace_dir 三级优先级实测（默认/env/显式参数）均正确。
- 收口文档：report-task-03.md（含还原事件完整时间线、新旧目录分工）；task.md 追加 task-03 状态（未覆盖 owner 表头原文）。

**task-03 收口后环境变化登记（task_id=9，2026-08-07）**：

- 并行线已于今日修复 evaluate.py 双轨抽取（正确起点 commit b85f4a7，11:40；初版误写 df93eca，已按 E3 §11.4 订正；工作区另有未提交修改）：extract_tool_data 已认新契约 9 工具与 doc_id 口径。
- **PR-34 通过标准已用本批 e3_handover trace 实跑达成**（E4 自跑 run_eval.py，exit 0）：cns×glm-5.2 cp1=1.00/cp2=1.00/cp3=0.36/cp4=0.40/cp5=1.00；cns×kimi-k3 cp1=1.00/cp2=1.00/cp3=0.91/cp4=1.00/cp5=1.00；sotalol×kimi-k3 cp1=1.00/cp2=1.00/cp3=0.01/cp4=0/cp5=0。
- **判读注意事项**：sotalol 组 cp3-cp5 偏弱系 trace 证据稀薄（get_property 仅 1 次命中、agent 结论 NO_SOLUTION），非判分器问题；不得据此认为判分适配不完整。
- 已同步修订：e3_handover/README.md §2（预期表换为实测值 + 状态时点）、run_eval.py 注释、report-task-03.md §五/§七；MD5SUMS 仅覆盖 9 个数据文件（未改动，校验和不变）。材料保留供 E3 回归复跑。

**task-02（上轮，已收口）**：9 组端到端冒烟、trace_schema.json 定稿、report.md 六条标准全勾；详见 report.md。

## 进行中

- 无。task_id=12/13/17/19/23/25 已收口，等监工核验。

## ⛔️ 阻塞

1. **【已解除，PR-34】evaluate.py extract_tool_data() 判分适配**：并行线已于 2026-08-07 修复（双轨抽取起点 commit b85f4a7，11:40；初版误写 df93eca，已按 E3 §11.4 知会订正）；通过标准已用 e3_handover 材料实跑达成（cns 两组 cp1-cp5 全非 0，数字见已完成节）。材料保留供 E3 回归复跑。
2. 真 A/B 发现型 open case 目前为 0（等 E5）：open 三门判分闭环无法端到端验证。
3. **【已解除，2026-08-08】task-04 合跑（PR-40 完整验收）**：task-05（task_id=17）实跑 t04_verify_view_wiring.py --joint **13/13 PASS exit 0**：前置探测三接线标志全 ✅、目标视图 case_dh_drug_01.json 在场（改用带视图的 discovery_haystack 类 case，旧默认 case_open_balanced_superconductor 视图未建不再等）；B1 查询返回集⊆白名单零泄漏、B2 gt（z064386）不可达。结果已回填 report-task-04.md §四，留档 t05_joint_run_20260808.log。
   - 历史脉络（留档）：2026-08-07 21:28 探测时两前置都不满足；task_id=15 返工后收窄为单一前置（旧默认 case 视图未建）；task-05 换 case 后彻底解除。教训不变：探测结论必须带时刻戳，前置状态以可复跑脚本实时输出为准。

## 待对齐项（接缝）

1. **case_id 传递通道/头名（已对齐，2026-08-07 21:28 探测）**：E3 裁定 F2 仅实现 env 通道 DAGPOC_CASE_ID（与 E4 CASE_ID_ENV 同名同义，已随 commit 6c14fdd 入库），HTTP 头 X-Dagpoc-Case-Id 双方均留二期——接缝零冲突零改动。
2. **【已处理，2026-08-07】E3 §11.4 知会：e3_handover 交接材料 commit 指针订正**：双轨抽取正确起点为 b85f4a7（11:40），初版误写 df93eca（实为 02:03 三维评审合并修复，git log 实查证实）。已订正：e3_handover/README.md §2、run_eval.py（文档串+判读输出）、report-task-03.md §五/§七；progress.md 本文件另两处同步。

## 范围外发现（不顺手修，仅登记）

0. **回归冒烟答案波动**：case_cns_metoprolol × qwen3.7-max 本轮 answer=NO_SOLUTION（task-03 时同组合为 ec），管道验证不受影响；答案稳定性属模型行为范畴，如持续异常可提请 E2/E5 关注题面区分度。
1. **dag_poc/traces 还原源仍未查明（PR-35 另一半）**：2026-08-07 01:35:53 六组新 trace 被整体还原为与 git HEAD 逐字节一致；owner 核实未执行过 git checkout。本 task 只做落盘防护这一半（已完成）；还原源排查归监工。task-03 冒烟期间（11:23–11:26）dag_poc/traces 未再发生还原/写入（前后 md5 一致），可作为排查参考基线。
2. **case query 不含性质名**：UNKNOWN_PROPERTY 大量试错（冒烟 ok 率 6-20/30+），属 E2 题面 / E5 case 构造 / 错误信封设计范畴。
3. mock_db 关键词检索口径偏窄（search_candidates 大量 EMPTY_RESULT），属 E1/E5 数据范畴。
