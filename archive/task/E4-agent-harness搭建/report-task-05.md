# E4 · task-05 收口报告（H2 真实 harness 端到端 + PR-40 合跑终验收 + 交卷断链修复）

**核心结论**：三项全部落地并实跑验证——① H2 端到端跑通：选型 claude 2.1.212（qodercli 被内容安全网关拦截，如实登记），fixed + open 各 1 道全链路（读 case → CLI 执行接 9 个 MCP 契约工具 → **cli_trace_adapter.py 转换** → 落 trace → evaluate.score_case 判分），断言 4/4 PASS × 2，adapter 逐字段对账 10/10 PASS × 2，H=2 达成；② PR-40 合跑终验收 **13/13 PASS**（见 §八，已回填 report-task-04.md §四、解除 progress.md ⛔️第3条）；③ 交卷断链双层根因修复并经 dogfooding 复跑 4/4 PASS 验证（submission_channel=submit_answer、答卷过 answer_schema）。全程 ADR-0019 dogfooding 纪律：所有实跑走免费档（H1 侧 Peach-07-17-DogFooding；H2 侧 claude 后端经 idealab 代理，调试不耗付费三模型额度）。

---

## 一、CLI 选型与协议对接结论（§5 风险一，如实报）

| CLI | 版本 | 探测结论 |
|---|---|---|
| qodercli | 1.1.2 | **不可用（如实报）**：MCP 联通冒烟通过后，正式跑被内容安全网关全线拦截——`error_code=406 "This conversation contains sensitive content"`，化学/药物内容全部拒绝；连先前通过的冒烟 prompt 复测也被拒（状态不稳定，非偶发）。留档 raw stream（traces_new/case_co_drug_01__qodercli_raw_stream.jsonl）|
| **claude（Claude Code）** | 2.1.212 | **选用**：原生支持 MCP（`--mcp-config` http 类型直连 mock_server :9000/mcp），9 个契约工具一次接通，无需工具桥。后端模型显示 qwen3.7-max（本机 fetch-intercept hook 路由 idealab 代理），与 H1 同模型家族，增强可比性 |

**协议对接结论**：claude 的 MCP 工具协议与我们 9 个契约工具**直接兼容**（工具名 `mcp__lingshu-mock__<tool>` 前缀由 adapter 剥除），无额外工具桥工作量。两处协议摩擦如实登记：
1. **入参形状**：模型首次交卷发平铺六字段，被服务端 `INVALID_ARGUMENT 未知入参` 拒收；按 SYSTEM_PROMPT 第7条（payload 包壳硬约束）第二次包壳提交即 accepted（fixed 实测）。open 题模型自造 cited_values 形状，两次 SCHEMA_VIOLATION 后交卷机会用尽——属模型答卷质量范畴，非协议不兼容。
2. **内置工具旁路**：`--permission-mode acceptEdits + --allowed-tools mcp__lingshu-mock + --disallowed-tools Bash,Read,...` **无法完全禁住**内置工具——fixed 跑泄漏 12 条（Bash×1/Read×1/MCP 资源读×10），open 跑泄漏 38 条（含 Glob/Grep/TaskCreate）。Bash 实读的是本工作区 task/*.md 文件（旁路工具面、可能触及 gt，与 H1 tools-only 链路不完全可比）。处置：判分投影只留 9 契约工具（evaluate 按工具名集合分派，混入即 MIXED 归 legacy 致证据丢失）；_full 流如实保留全部事件作证据链。**H1/H2 对比时须注明此口径差异**。

> **[task-06 订正，2026-08-08]** 本条「Bash 实读工作区 task/*.md」经 task-06 重新清点（`t06_leak_inventory.py`）证实为误判：那是更早 bypassPermissions 探测轮（v1）的行为，现行 acceptEdits+disallowed-tools 参数组合下 50 条非契约工具事件全部为拒绝信封/CLI 内部 todo，实质泄漏 0 条。封堵升级与重跑验证见 `report-task-06.md`。
   - 已排查根因之一：`--allowed-tools/--disallowed-tools` 是 variadic 参数，prompt 作位置参数会被吞进工具列表致限制静默失效——已改为 prompt 走 stdin；改后 MCP 工具调用正常，但内置工具仍有少量泄漏（claude 权限模型实测行为，不再硬凑）。

## 二、H2 端到端结果表（§1 主任务，契约 §六硬条款：必经 adapter，不直连判分）

链路：读 case → claude `-p --output-format stream-json`（--mcp-config 接 mock_server）→ raw stream-json → **cli_trace_adapter.convert_cli_stream** → 解壳/ok 补齐/入参包壳对齐 H1 → 双轨落盘（判分投影 + _full）→ evaluate.score_case / open_case_metric。复跑脚本 `t05_h2_run_cli.py`（支持 `--reprocess` 对已有 raw 流重算后处理，无需再调 LLM）。

| case（kind） | 收卷通道 | adapter 对账 | 判分退出码 | 判分结果 | 实测耗时 |
|---|---|---|---|---|---|
| case_co_drug_01（fixed） | **submit_answer**（第1次平铺拒收→第2次包壳 accepted=true） | **10/10 PASS** | 0 | best=ea（cp1=1.0/cp2=0/cp3=0.706/cp4=0），ranking 7 候选 | **692s** |
| case_dh_drug_01（open，带 case_id 走视图） | text_fallback（3 次提交均被拒，见 §一摩擦1；终文本兜底成功） | **10/10 PASS** | 0 | score_case 出分（cp 全 0，fixed 判分口径对 open 本就预期弱）；open_case_metric gate=False score=0.0 | **1313s** |

- 断言（两跑均 4/4 PASS）：H1 事件数>0（276/184 条）｜H2 判分投影纯 9 契约工具（264/146 条，过滤 12/38 条内置工具）｜H3 收卷通道非空｜H4 判分器能吃。
- open 类已验「带 case_id 走视图」通道：server 以 `DAGPOC_CASE_ID=case_dh_drug_01 DAGPOC_CASE_KIND=open` 启动，harness 侧 resolve/verify 识别为 open（kind 口径扩展：view_ref 非空 ⟹ open）。
- 产物（traces_new/）：`case_*__claude.jsonl`（判分投影）/ `_full.jsonl` / `_raw_stream.jsonl`（对账基准）/ `_response.json`。
- open gate=False 判读：答卷交卷失败（SCHEMA_VIOLATION）+ 文本兜底答卷未达 open 判分门槛——模型行为证据，不是链路问题；链路本身（adapter→判分器）exit 0 全通。

## 三、adapter 转换对账（§5 风险二，落盘可复跑：t05_adapter_reconcile.py）

两条正式 raw stream 各 10/10 PASS：R1 tool_use 数==转换事件数（276=276 / 184=184）｜R2 id 无重复｜R3 前缀剥除全对｜R4 arguments 深比较全等｜R5 tool_result 配对率一致（276/276）｜R6 result 深比较全等｜R7 timestamp 保真｜R8a/b result 事件标量元数据与 usage（input/output_tokens）不丢。**如实登记缺项（R8c）**：assistant 级 usage/stop_reason 在 H2 trace 为缺项（adapter 现版只产 tool_call 不落 llm_call），判分不受影响（判分不吃 llm_call），E6 做 token 成本分析时须改用 raw stream 的 result 事件元数据（input_tokens 650563/447921、output_tokens 29035/50804、total_cost_usd 已提取在案）。

## 四、H1/H2 耗时对比（§5 风险三，供 E6 重算排期）

| 组合 | 实测耗时 | 备注 |
|---|---|---|
| H1 × Peach-07-17-DogFooding × case_co_drug_01 | 940s | steps=15，tool_calls=107(ok=20)，推理模型长思考 |
| **H2 × claude × case_co_drug_01** | **692s** | num_turns=277，契约工具调用 264 次——调用更多但单轮更轻 |
| **H2 × claude × case_dh_drug_01（open）** | **1313s** | open 探索量大（146 次契约调用） |

**报 E6**：fixed 类 H2 实测（692s）与 H1（940s）同量级、未超 E6 规划值 600s/次太多；但 **open 类 H2 实测 1313s 已超规划值 2.2 倍**——若矩阵含 open × H2 组合，按 600s/次排期会低估约一半，请以本表实测值重算。另：H2 claude 单跑 token 消耗大（fixed 跑 input 65 万/output 2.9 万），批量跑需估量配额。

## 五、交卷断链修复（任务三，owner 实测发现，复跑验证通过）

**现象**（owner）：case_co_drug_01 链路通（14 步/14 LLM/67 工具调用）但 submission_channel=text_fallback、final_text=None——67 次工具调用后没走 submit_answer。

**双层根因（均实锤）**：
1. **引导层**：run 循环倒数第二步提醒旧文案「直接输出最终答案和 JSON」引导模型走文本输出而非 submit_answer；SYSTEM_PROMPT 无步数预算，模型 60-100+ 次工具调用耗尽轮次仍不收敛。
2. **预算层（更深）**：推理模型 Peach 的 reasoning_tokens 计入 completion 预算——max_tokens=4096 下思考阶段吃满预算（实测 completion=8194、reasoning_tokens=4096、finish_reason=length、**零输出**），交卷 tool_call 永远发不出。历史 6 组 trace 4/6 text_fallback 即此系统性病灶。

**修复（只动写作区 agent_harness.py）**：SYSTEM_PROMPT 第4条交卷纪律（约15步预算、剩余不足3步必须收敛交卷）+ 第7条 submit_answer 入参形状硬约束（必须 `{"payload": {...}}` 包壳）；run 循环提醒文案改指向 submit_answer；finish_reason 注入 message（OpenAI 格式原在 choice 上，不注入则截断检测永远落空）；length 截断不视为自然完成（提醒 continue 立即交卷）；主循环后交卷兜底轮；llm_chat max_tokens 默认 4096→16384。kind 口径同步扩展（view_ref 非空 ⟹ open）+ DAGPOC_CASE_KIND 第三通道 fail-closed 校验（t04 mode A 回归 11/11 PASS）。

**复跑验证（t05_rerun_submit_fix.py，dogfooding × case_co_drug_01）：4/4 PASS**——C1 submission_channel=submit_answer｜C2 答卷非空｜C3 jsonschema 全量校验通过｜C4 answer=bd 非空；实跑 940s、steps=15、tool_calls=107(ok=20)。产物落 traces_new/case_co_drug_01__Peach-07-17-DogFooding*。

## 六、环境/状态发现（登记）

1. **mock_server 交卷终态是进程级状态**：同一 server 进程内第二次交卷恒 DUPLICATE_SUBMISSION（跨运行残留，v4 实测实锤）——每次端到端跑前必须重启 server。已按此纪律执行后续所有跑次。
2. claude CLI 实查要点（可复跑参考）：不认 `-o`，需 `--output-format stream-json --verbose`；prompt 走 stdin（variadic 参数吞位置参数坑）；`bypassPermissions` 无视 --allowed-tools（改用 acceptEdits）。
3. traces_new/ 内 qodercli 空产物为选型探测留档（被 406 拦截的 raw stream 11.8KB 含网关报错）。

## 七、交付标准核对（task-05 §4）

| # | 交付物 | 状态 | 证据 |
|---|---|---|---|
| 1 | H2 端到端跑通（≥1 fixed + ≥1 open） | ✅ | §二（fixed submit_answer 交卷出分；open 链路全通带 case_id 走视图） |
| 2 | H1/H2 契约一致性 | ✅ | §一/§三：同一 adapter 契约、同判分器、arguments 形状对齐（入参包壳）、SYSTEM_PROMPT 同源；差异口径（内置工具泄漏）如实注明 |
| 3 | PR-40 合跑终验收 | ✅/见下 | §八（实跑输出贴录，回填 report-task-04.md §四） |
| 4 | 知会 E6 | ✅ | §四耗时表即知会内容（open×H2 超规划值 2.2 倍），progress.md 同步 |
| 5 | report 收口 + 状态行更新 | ✅ | 本报告 + progress.md 三段 + task-05 文档状态行 |

## 八、PR-40 合跑终验收（t04_verify_view_wiring.py --joint，实跑 13/13 PASS exit 0）

**结论：通过**。B1（open 题查询返回集 ⊆ 白名单）零泄漏、B2（gt 不可达）达成，前置探测三接线标志全 ✅。已回填 report-task-04.md §四，progress.md ⛔️第3条阻塞解除。

实跑环境（2026-08-08 16:14-16:22）：mock_server 以 `DAGPOC_CASE_ID=case_dh_drug_01 DAGPOC_CASE_KIND=open` 重启；H1 × Peach-07-17-DogFooding（dogfooding 档）实跑 case_dh_drug_01（30 事件、交卷通道 submit_answer）。留档：t05_joint_run_20260808.log。

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

口径注：B1 实体 ID 对 visible_entity_ids 白名单、文献 doc_id 对 literature_doc_ids 白名单分开断言（papers 的 doc_id 是 DOI 非实体 ID）；白名单与 gateway §10 过滤同基准。

## 九、复跑命令汇总

```bash
cd /Users/jingwu/work/benchmark
# H2 端到端（每次先重启 mock_server；fixed 不带 KIND，open 带 KIND=open）
./.venv/bin/python task/E4-agent-harness搭建/t05_h2_run_cli.py dag_poc/templates/constrained_optimization/case_co_drug_01.json --cli claude
./.venv/bin/python task/E4-agent-harness搭建/t05_h2_run_cli.py dag_poc/templates/discovery_haystack/case_dh_drug_01.json --cli claude
# adapter 对账
./.venv/bin/python task/E4-agent-harness搭建/t05_adapter_reconcile.py <raw_stream.jsonl>
# PR-40 合跑终验收
./.venv/bin/python task/E4-agent-harness搭建/t04_verify_view_wiring.py --joint
# 交卷修复复跑验证
./.venv/bin/python task/E4-agent-harness搭建/t05_rerun_submit_fix.py
```
