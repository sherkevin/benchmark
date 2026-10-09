# E6 · 三层矩阵评测 — progress

> 更新：2026-08-10（task-09 修后重跑 3 道完成：**五条上游修复（PR-61~65）全部实测生效，但四断言①仍未过（cp2 六单元仍全 0）、②硬停线再次触发（H1/H2 均 3 道全错）已停下报监工、③逐道核对完成（本轮无判分器缺陷）、④过**；见 report-09.md。全量矩阵不开，等监工复验发令）。
> ⚠️ 状态行纪律（监工点名）：状态行必须与实际同步。当前实际：**无进程在跑**（task-09 六单元已跑完落盘 spotcheck_runs_t09/，task-07 基线 spotcheck_runs/ 保留未动），矩阵冻结等监工对硬停线与登记项 N1~N3 的裁定。
> 门 2 现默认走 cached 只读校验（不覆写 E1 的 _build_state 产物，E1 迁移期推荐）；实跑重建需显式 `--ab-rebuild`。

## ✅ 已完成（带路径）

| 项 | 产物 |
|---|---|
| 三层矩阵设计定稿（模型 × harness × template，k=3 + 种子策略，容量数字不写死） | `task/E6-三层矩阵评测/matrix_design.md` |
| 总跑测次数与总耗时预估（公式 + 单次耗时 trace 采样依据 + 降级四档适配） | `matrix_design.md` §6 |
| 统计层：两比例检验、Wilson CI、功效阈值动态读 `min_publishable`、fixed/open 显著性区分 API 级强制、估算函数 | `src/eval_matrix/stats.py` |
| 统计层自查测试（n=30 半宽 ±0.18、n=10 ±0.31、每组约 97 题、open 30 只出描述性、fixed 120 功效达标），30/30 通过 | `src/eval_matrix/test_stats.py`、`src/eval_matrix/test_pre_gates.py` |
| 前置三道门编排器（子进程调现成脚本读退出码；门 1 退出码 0/1/2 三态区分；门 3 按 ADR-0016 修订 R1 新口径，回执未就位显式拒绝不静默放行）；机器可读 JSON 报告；干跑模式可用 | `src/eval_matrix/pre_gates.py` |
| 结果表骨架三张（主表 fixed/open 分表 + 双验证分列；过程分拆解 + gate_failed_cps 归因；harness 消融），字段对齐 `evaluate.py aggregate()`，占位数据显著标注 | `task/E6-三层矩阵评测/result_tables/01_main_table.md`、`02_process_breakdown.md`、`03_ablation_harness.md` |
| 本批收口报告 | `task/E6-三层矩阵评测/report-02.md` |
| task-05 前置：跑批框架（分批落盘/四元组判重续跑/抗 PR-35 原子写与 manifest/空卷防线）、七维度汇总脚本（脚本算出可重跑）、落盘契约 README（usage 实际在 `response.usage` 嵌套层） | `src/eval_matrix/matrix_runner.py`、`matrix_summarize.py`、`matrix_runs/README.md` |
| 付费档切换（task-05 明令）：.env `LLM_PROFILE=dogfooding` 已注释，回 idealab（timeout=120s）；框架内置 profile≠idealab 拒跑断言 | `.env`（监工明令例外行） |
| 付费切片试跑（H1 × 10 题，qwen 10 单元 + glm 7 单元 = 17 有效）：空卷 1 份留痕（qwen dh_drug_01，SCHEMA_VIOLATION 拒收，非断链）；续跑实证（杀进程→重启→skip 判重零重复，failed 自动重跑）；单位成本 ≈22 万 tokens/题（3600 次外推 ≈7.9 亿），耗时均值 180.6s；框架暴露并修复 2 处缺陷（残留占端口/open 类 env 注入）+ 上报 1 处 src/benchmark 重试缺口 | `task/E6-三层矩阵评测/report-05-pre.md`、`matrix_runs/`（含 summary.json/manifest.jsonl） |
| H2 接线进 matrix_runner（task-05 正式开跑前置）：claude CLI + adapter 后处理链 + task-06 三层封堵只读引用；冒烟实证发现并修复用户 ~/.claude 插件（ponytail SessionStart hook）persona 污染致空卷 → CLAUDE_CONFIG_DIR per-单元隔离（鉴权 env 用完即删）；qwen/glm H2 冒烟交卷正常；kimi H2 通道 400 提供方错误留痕（见阻塞）；H1 改子进程隔离（verify_case_channel 全局 env 串扰）+ 端口池并发（--workers/--base-port）+ 残留回收只杀占目标端口进程 | `src/eval_matrix/matrix_runner.py`、`matrix_launch.py`（M=4×H=2×150×k=3=3600 单元启动器，干跑已验证） |
| **M4 定档落位（监工裁定 2026-08-09）**：M4=Peach-07-17-DogFooding（免费档/推理模型/600s 超时档，不进裸答基线）；断言放行方案：模型白名单→必需 profile 映射，H1 子进程 import 前注入 LLM_PROFILE（进程 env 优先于 .env），付费模型仍强制 idealab、未知模型拒跑，单测 5/5 通过；启动器扩回 M=4（3600 格位，Peach 900 为四元组续跑位）；Peach H1 冒烟通过（336.6s、交卷非空、profile=dogfooding 生效），Peach H2 不可路由（400 平台业务限制，最小复现确证）格位 failed 留痕 | `matrix_design.md` §1.1/§6.4、`matrix_runner.py`（required_profile）、`test_matrix_profile.py`、`matrix_launch.py` |
| **task-06 §1 缓存口径修复（2026-08-09，含 §6 返工 PR-55）**：bug A 两等价字段相加翻倍已修（取 max）；**bug B 返工**：上轮误把 H2 回落当第二取数路径，run_meta 汇总字段仍进总数且对账两边同源自己对自己（虚假验收）→ 已删回落路径、只留 llm_call 逐次累加一条，H2 无证据流 token 不进 totals 如实标注；断言②改不同源对账（H2 不参与）、新增断言③独立复扫（另一实现只读 trace 重算四项 vs 汇总）；**复算对上监工基准：cached 3,047,040 / prompt 4,363,389 / 占比 0.6983**；负向测试实证：注入双路径叠加断言③确实报错。单测 79/79。根因与对照表见 report-06.md §7 | `matrix_summarize.py`、`matrix_runner.py`、`test_matrix_summarize.py`、`matrix_runs/README.md`、`report-06.md` §7 |
| **task-09 修后重跑 3 道（2026-08-10，任务 #21）**：同 3 道同规则同 harness 重跑（落盘隔离新目录 spotcheck_runs_t09/，task-07 基线零写入）。五条修复逐条实测生效：PR-61（mw/logp/tpsa 出白名单，本轮 0 次相关空转）、PR-62（dh 走 _score_open_discovery，_status not_attempted 留痕）、PR-63（H1 错误信封中文正常含可查清单）、PR-64（H2 llm_call 16/28/24 条落盘）、PR-65（字符串化 keywords 19 次如实被拒）。**四断言：①仍未过（cp2 六单元仍全 0，gt 仍未进答卷候选池）；②硬停线再次触发（H1/H2 均 3 道全错）已停下报监工；③逐道核对完成，本轮无判分器缺陷，失分全在模型行为层；④过（4 单元非零总分）**。新旧对照：EMPTY_RESULT 86→55（率 30.07%→19.71%，残留全是 literature/keywords 检索面窄，非白名单 bug 复发）；**CALL_LIMIT_EXCEEDED 17→19 未归零**（全是对 tc/logS 的放宽重试耗尽，PR-61 三项外的新载体，登记 N1）；UNKNOWN_PROPERTY 7.69%→2.15%；成功率 29.4%→38.4%。dh 道 open 公式首次真实执行：hit_gb=false、grounding_pass=false（H1 edit_op 不复现、H2 无契约交卷）、exists 预算未耗尽。H2 token 两路 identical=true，独立取证定性为 CLI 汇总口径天然相等（非 PR-55 同源），登记 N2。噪声门改动清单只列不改（report-09 §7） | `report-09.md`、`spotcheck_runs_t09/`、`src/eval_matrix/matrix_runner.py`（PR-64 接线+tool_events 适配）、`spotcheck_task07.py`（E6_SPOTCHECK_ROOT+两形态七项+交叉校验） |
| **task-07 抽验判分链路实跑（2026-08-10，任务 #20）**：新题新说明书实跑 3 道 × H1/H2（qwen3.7-max，落盘隔离目录 spotcheck_runs/，正式 matrix_runs 零写入）。挑题固定规则（每类字典序取第 ⌈n/2⌉ 道）：case_cs_drug_15 / case_co_material_15 / case_dh_drug_15。**四断言：① cp2>0 未过（六单元全 0，gt 均未进答卷候选池）；② cp6 硬停线触发——H1 与 H2 均 3 道全错，已按纪律停下报监工；③ 逐道人工对 trace 完成（判分器问题登记 R1 路由/R2 H1 错误信封乱码，其余均 trace 未做到）；④ 过（4 单元非零总分）**。报错率换算对比：UNKNOWN_PROPERTY 34.60%→7.69%（说明书见效）、H2 INVALID_ARGUMENT 22.13%→30.46%（模型用 properties 拒收，说明书已显式防混淆）、成功率 46.4%→29.4%（H2 EMPTY_RESULT 主导）。H2 token 遗留：**llm_call 落不下来（3 道实测 llm_calls=0）登记报监工，未改 harness**。登记项 R1~R4 见 report-07 §9 | `report-07.md`、`src/eval_matrix/spotcheck_task07.py`、`spotcheck_runs/` |

## 🔄 进行中

- **任务 #17 基线补齐——已按红线停采（2026-08-09 16:30）**：依据 task-06 §4 红线 + 150 道旧题将全部重写（owner 定方案一，E5 未开工），旧 case 采样作废，继续烧付费配额无意义 → 优雅停止采样进程（PID 2079）与守望进程，已采 **2086/4500** 份留痕与断点保留（失败记录 2033 条在 failures.jsonl），停止原因已写入 /tmp/e6_sample_watch.log。**新题集交付后按新 case 重排采样计划**（含重新评估缺口与配额）。停止前滚动复判结论不变：open 45 满 k 全干净、fixed 出结论部分全 pass@10=0，0 污染。
- **任务 #15 全量矩阵——红线冻结（不许全量开跑）**：开跑五条硬前置第 5 条「150 道手写题全部完成」未满足，拿旧 case 跑 3600 次全部作废。启动器保持就绪（`matrix_launch.py` 干跑验证 3600 格位），**不开**；本轮未跑冒烟（runner 结果目录为常量无法指向临时目录，既有 H2 单元 raw_stream 复算已等价验证新口径）。允许冒烟 ≤2 次/不入正式目录的窗口保留，等题集重写完成后按新前置重评开跑。
- ~~**task-07 抽验判分（硬停线触发待监工裁定）**~~——已由 task-09 修后重跑接续（R1~R4 五条修复全部实测生效，见下条）。
- **task-09 修后重跑 3 道（2026-08-10 实跑完成，硬停线再次触发待监工复验）**：五修复全生效但断言①②仍不过——失分点已从工具链转移到模型行为层（检索未把 gt 进池、tc/logS 放宽重试、H2 参数名漂移）。登记项 N1（CALL_LIMIT_EXCEEDED 新载体 tc/logS）、N2（H2 token 两路 CLI 口径天然相等）、N3（H2 参数名漂移是否需说明书锚定示例）。**监工复验发令前不开全量**。见 report-09.md。
- 等 E5 30 道验证批题集落盘后：用真实题集回归三道门编排器与结果表渲染链路（本批只用存量 37 个旧 case 做了门禁实跑观察，未跑正式实验）；
- E2-04 容量订正（156/154/91 → 155/153/92）落盘中——代码与文档均动态读 `query_spec.json`，落盘后无需返工，仅需复核一次。
- **task-05 正式开跑前置就绪状态（2026-08-09 更新）**：H1/H2 双链路均已接线并冒烟实证（含并发 8 路端口池）；但**全量开跑被 task-06 §4 红线冻结**（等 150 题重写完成）。
- **task-04 裸答基线满额 150 复判收口（2026-08-08 第二轮）**：题集满额 150/150（fixed 120 = cs/co 各 60，open 30 = dh drug/material 各 15），代码名单已扩容至满额（含「未采样」如实标注态）。G_B 注册扩到 b_partition.cases 35 条（dh 30 + fixture 2 + spare 3），dh 30 道与 case JSON 逐一比对 30/30 一致；s_feasible_size 120/120 落盘。满额重判（纯离线）：**open 0 污染（旧批 45 条满 k 全干净，新增 45 条未采样）；fixed 0 废弃（70/360 出结论全 pass@10=0，16 采样不完整 + 4 零样本不硬判，274 未采样）**。修一处候选提取正则缺陷（多段组成串在后续 `]` 处截断致 resolve 漏判命中）+ 2 回归单测，单测 66/66。新增侧 dogfooding 冒烟（dh_material_01/02 × 2）与命中正负向离线验证通过。**付费采样继续挂起**（未获 task-04 文档明确授权，不擅动）：满额缺口 3247/4500（旧批 97 + 新增 105 道 ×3×10=3150），等监工授权后断点续跑。见 report-04.md §9。

## ⛔️ 阻塞（当前）

| 阻塞项 | 影响 | 解除条件 |
|---|---|---|
| **🔴 task-09 硬停线再次触发（2026-08-10）**：五条修复全部实测生效，但修后重跑 H1/H2 仍均 3 道全错（cp6/open_score 全 0），断言① cp2 仍全 0；本轮无判分器缺陷，失分全在模型行为层；登记 N1（tc/logS 放宽重试耗尽 19 次）N2（H2 token 两路天然相等）N3（H2 参数名漂移） | 全量矩阵继续冻结 | 监工复验 + 对 N1~N3 的裁定与是否再查根因 |
| **🔴 红线冻结（task-06 §4，2026-08-09）**：不许全量开跑矩阵——开跑前置第 5 条「150 道手写题全部完成」未满足，owner 已定现有 150 道全部重写（E5 最重的活未开始），拿旧 case 跑 3600 次全部作废；基线采样同因停采 | 矩阵与采样全部冻结；框架/启动器就绪不开 | E5 新题集交付并通过五道前置后，按新 case 重排采样与开跑计划 |
| 等 E5 30 道验证批题集 | 无合格题集，正式跑测不能开 | E5 交付验证批且过门 1（题面规范） |
| 等专家回执 | 门 3b（专家覆盖 100%，ADR-0016 修订 R1）blocked，显式拒绝放行；分母现为 cases_glob 匹配的 case 文件数（case 粒度） | 回执按约定落盘 `task/E2-query风格与题量定数/expert_signoff/*.json`（门禁报告已给格式约定） |
| **待 owner 仲裁①**：门 3b 粒度口径——case 粒度（当前实现，机器可判） vs 题量粒度 total_questions=150，最终口径待定；同批上报回执目录 SIGNOFF_DIR 约定（本门禁新立） | 仲裁前门 3b 按 case 粒度判定；若裁为题量粒度需改分母来源 | owner 裁定后单点改 `pre_gates.py` 分母逻辑 |
| **待监工仲裁②**：cp5 口径 gap——query_spec `per_type[type1].scoring` 声称 5 checkpoints 无 cp5，与 evaluate.py 现行实现（CP_WEIGHTS/CP_THRESHOLDS 对一切 fixed case 恒含 cp5）不一致 | 仲裁前 type1 的 process/gate 按现行实现口径解读（cp5 恒 0 拉低分数），不得自行改实现 | 报监工请 E3/owner 仲裁（已登记于 result_tables/02_process_breakdown.md ⚠️ gap） |
| 容量订正 E2-04 落盘中 | 不阻塞代码（动态读取）；阻塞"容量数字引用"类结论 | `query_spec.json → capacity_basis` 订正落盘 |
| 门 1 现状 fail（存量 37 case 未迁移，M10 违规 37 条等） | 即使其余门过，题面规范门也会拒跑 | E5 迁移/重写存量 case，query_lint --strict 退出码归 0 |
| **裸答基线等待项①：E3 resolve 映射（ADR-0018 R1-②）**——✅ **已解除（2026-08-08）**：evaluate.resolve 已交付（监工实测 resolve(z064386)→17 实体、一对多成立），E6 侧 R1-② 接线位已接真实实现并补单测（一对多/空交集/未注册防护） | — | — |
| **裸答基线等待项②：E5 fixed `\|S_feasible\|` 落盘（R1-③）**——✅ **已解除（2026-08-08）**：E5 已逐题落盘 `s_feasible_size`（30/30，读取契约首选键命中），fixed 判定已可出结论（采样补齐后重跑 judge） | — | — |
| **fixed 命中判分器依赖（PR-34 相关，已如实记录不阻塞）**：evaluate.py 现无 cs/co 自由文本命中专用入口；E6 命中判定仅组装判分器既有组件（normalize_id 归一 + _score_fixed cp6 文义 best==gt_id），未自写字符串比对，判定结果显式标 `hit_judgment_active=false`、生效以判分器支持为前提 | 不阻塞「等落盘」状态机；正式废弃结论出具时需判分器支持背书 | E3 修完 PR-34 后复核一次 |
| ~~**dh 15 道复检状态**~~——✅ **已解除（2026-08-07 E5 收口）**：E5 progress 登记独立验证完成（首轮 13/15 全绿，09/14 泄漏修复后复检 5 项全过，15 道可冻结），裸答判定结论不再挂「待复检」前缀 | — | — |
| **H2 端到端（E4 task-05）**：跑通前不按 H=1 收口消融表、不拿 H1 自比充当消融（matrix_design.md §2 自设规矩），消融表维持占位 | 不阻塞污染清单；阻塞矩阵正式开跑的 H=2 档 | E4 端到端跑通后明确知会 |
| **glm-5.2 dh 类可用性（试跑实证，待决策）**：glm 在 dh（open 类）首调用 ReadTimeout 连续 3 轮全灭（cs/co 全过）；判定为该模型带 tools 首包稳定超 idealab 120s timeout，叠加 llm_chat 不重试网络异常的缺口。试跑已按降级路径换 qwen 补齐，未静默改口径 | glm dh 3 单元 failed 留痕待续跑判重；不阻塞 qwen/kimi 开跑 | 监工决策：调 timeout/补重试后重验，或降出矩阵 |
| **kimi-k3 与 Peach H2 通道不可用（冒烟实证，2026-08-09）**：claude CLI --model 最小复现（无 tools 无 prompt 附加）稳定返 400（kimi：模型提供方错误；Peach：平台业务限制）；同参 qwen/glm 路由正常。判定为 idealab anthropic 协议通道不支持该两模型名，非框架问题 | kimi H2 450 + Peach H2 900 格位按 failed 留痕如实报，不静默改口径不硬凑；H1 侧两模型不受影响（均已冒烟通过） | 上游通道支持后四元组判重续跑 |
| ~~**M4 弱档名单未定**~~——✅ **已解除（2026-08-09 监工拍板）**：M4=Peach-07-17-DogFooding，落位完成（见已完成表） | — | — |
| **llm_chat 网络异常重试缺口（src/benchmark 不在 E6 写作区，未改）**：重试仅覆盖 HTTP 状态码，ReadTimeout 直接抛；试跑中 6 次 ReadTimeout failed 均因此 | 全量 3600 中概率性 failed 重跑开销（续跑机制可兜底） | 监工授权改 src/benchmark，或接受续跑兜底 |
| **三模型计费单价待核实**：仓库内无权威计费表，PRICE_TABLE 来源栏如实标「待核实」，当前费用只到 token 量级不出金额 | 不阻塞开跑；阻塞成本报告出金额 | 核到 idealab/moonshot 实价改 PRICE_TABLE 重跑汇总即得 |
| ~~**H2 接线（task-05 正式开跑前置）**~~——✅ **已解除（2026-08-09）**：claude CLI + adapter 后处理链接入 matrix_runner，qwen/glm H2 冒烟交卷正常（见已完成表） | — | — |
| **付费档采样——挂起→已停采（2026-08-09 16:30，红线处置）**：此前状态为挂起等授权；task-06 §4 红线 + 150 题重写定案后，旧 case 采样作废，采样进程已优雅停止（已采 2086/4500 留痕断点保留）。新题集交付后按新 case 重排采样计划（缺口与配额重新评估，旧清单作废） | 无进程在烧配额；最终污染清单待新题集重采后出 | E5 新题集交付 → 重排采样 → 补齐 → 终判 |

> 注：旧 task 文档中的"等 E3"阻塞已解除——E3 已验收通过（evidence_time 取 min、open_score 双因子就位、evaluate.py open case 不再 KeyError），见 `report-02.md` §2。
