# report-05-pre：跑批框架搭建 + 付费切片试跑（task-05 前置部分）

日期：2026-08-09（执行跨 8 日夜至 9 日凌晨）｜执行：E6 专家（任务 #14）
范围：**仅 task-05 前置部分**——跑批框架 + 切片试跑；全量 3600 次未开。
所有数字由 `src/eval_matrix/matrix_summarize.py` 从落盘日志算出
（`matrix_runs/summary.json`，可重跑复现），非手记。

## 1. 框架交付清单

| 交付物 | 路径 | 要点 |
|---|---|---|
| 跑批框架 | `src/eval_matrix/matrix_runner.py` | 分批落盘（每单元跑完即原子写 run_meta + 追加 manifest）；四元组 (模型, harness, case, 轮次) 判重续跑；failed 重启重跑、done 跳过；日志目录按四元组 `matrix_runs/<模型>/<harness>/<case>/round_NN/`；mock server per-case 起停；空卷检查 + `--halt-on-empty` |
| 汇总脚本 | `src/eval_matrix/matrix_summarize.py` | 七维度（总 token 三列/按模型/按 harness/按 template 类型/单题分布/缓存/重试失败）+ PRICE_TABLE 成本折算（单价来源如实标注） |
| 落盘契约 | `task/E6-三层矩阵评测/matrix_runs/README.md` | usage/finish_reason 实际嵌套层级表（监工实测教训：在 `e.response.usage` / `e.response.choices[0].finish_reason`，不在事件顶层）；reasoning_tokens、cached_tokens 及 glm 兼容字段路径 |
| 抗还原 | — | 本仓非 git 仓库、跑批目录在 E6 写作区内；tmp+rename 原子写；manifest.jsonl 追加式台账（PR-35 教训） |

11 项留痕字段全在（Tracer.record_llm 既有实现，框架不改）；usage 的
prompt/completion/total 分列 + reasoning/cached 明细原样保留于 `_full.jsonl`。

## 2. 付费档切换（task-05 头部明令）

`.env` 的 `LLM_PROFILE=dogfooding` 已注释（监工明令的唯一例外行），回落
idealab 付费档：`profile=idealab，models=[qwen3.7-max, bailian/glm-5.2,
moonshot/kimi-k3]，timeout=120s`。框架内置硬断言：profile≠idealab 拒绝开跑。

## 3. 切片试跑实测（付费）

切片设计：H1 × 10 题（cs×4 + co×3 + dh×3，覆盖三类 template）。
首试模型 bailian/glm-5.2 受阻（见 §6），按降级路径换同档 qwen3.7-max 补齐，
glm 侧 cs/co 也跑满，最终两模型合计 17 有效单元。

| 模型 | 有效单元 | 空卷 | failed | tokens/题均值 | 耗时均值 |
|---|---|---|---|---|---|
| qwen3.7-max | 10（含 1 空卷留痕） | 1 | 0 | 207,637 | 185.0s |
| bailian/glm-5.2 | 7（cs/co 全齐，dh 全灭） | 0 | 3 | 237,639 | 174.3s |
| 合计 | 17 | 1 | 3 | 219,991 | 180.6s（中位 169.4） |

### 3.1 空卷检查（逐份确认）

- 17 份有效单元全部 `submission_channel=submit_answer` 正常交卷，
  `answer_empty=false`，唯 1 例外如下。
- **空卷 1 份**：qwen3.7-max × case_dh_drug_01（305.7s，59 次工具调用）。
  根因（留痕核查）：模型唯一一次 submit_answer 的 payload 缺 open 类必填
  字段 `mol`，server 返 `SCHEMA_VIOLATION` 拒收，模型剩余步数耗尽未修正。
  **属模型交卷 schema 行为，非交卷断链**（同 harness 同批 8 份 cs/co 全正常
  交卷）。按纪律已停批留痕，meta.answer_empty=true，全量流内 submit 尝试
  原样可查。glm 首跑曾现「空卷假象」，查明为残留 mock_server 占端口致中途
  拒连（模型实际已提交），框架修复后重跑为正常卷（§4）。

### 3.2 断点续跑实证（杀进程 → 重启 → 判重）

- 切片进行至 2 单元落盘后 `pkill` 杀进程；子 mock_server 残留占端口被新
  防御捕获（拒绝启动），回收后重启。
- 重启日志实证：`[skip] case_cs_drug_12 全部 1 单元已完成（断点续跑判重）`
  等 skip 共 6 次，done 单元零重复；failed 单元（ReadTimeout）自动重跑，
  cs_material_20 第三次重跑成功。

### 3.3 单题耗时（对照 E4 基准）

- 实测 H1 付费档：均值 180.6s、中位 169.4s、最大 313.0s（dh_material_01）。
- 高于 E4 的 H1=102s（dogfooding 档、不同模型），排期**按 H2=692s 口径**：
  3600 次 × 692s ≈ 2.49M s ≈ 28.8 机日；并发 8 路 ≈ 3.6 天（task-05 §3.1
  起步并发），并发度与机器数按全量开跑前监工核定。

## 4. 框架试跑中暴露并修复的缺陷（全部留痕）

| # | 缺陷 | 处置 |
|---|---|---|
| 1 | 残留 mock_server 占 9000 端口，readiness 误判就绪 → 中途拒连 → 空卷假象（glm cs_drug_01 首跑） | start_server 启动前查残留、自动回收本框架 mock_server、非本框架占用拒绝启动；受污染单元标 failed 重跑覆盖，manifest 追加 invalidate 留痕 |
| 2 | harness 侧 verify_case_channel 校验 runner 本进程环境变量，dh（open 类）fail-closed 报 CaseIdentityError | 框架每 case 同步注入 DAGPOC_CASE_ID/DAGPOC_CASE_KIND 到本进程；修复后 qwen dh 三题全部正常交卷 |
| 3 | `llm_chat` 重试仅覆盖 HTTP 状态码，requests 网络异常（ReadTimeout）不重试直接抛（**src/benchmark 不在本任务写作区，未改，报监工**） | 依赖续跑机制 failed 重跑兜底；全量前建议补网络异常重试 |

## 5. 单位成本核算与 3600 外推（脚本算出）

- 试跑 17 有效单元合计：prompt 3,583,315 / completion 156,542 /
  total 3,739,857（reasoning_tokens=0，两模型均非推理模式）。
- **单位成本**：约 22.0 万 tokens/题（prompt 占 95.8%），与 E4 实测
  「单跑 218k token 量级」一致。
- **3600 次外推**：total ≈ 7.9 亿 tokens（prompt ≈ 7.6 亿 / completion ≈ 0.33 亿）。
- **费用**：三付费模型单价仓库内无权威计费表，PRICE_TABLE 来源栏如实标
  「待 idealab/moonshot 计费口径核实」，当前不出费用数字；核到实价改表
  重跑 `matrix_summarize.py` 即得。
- 缓存读数：cached_tokens 累计 4,988,416 > prompt 总量（比值 1.39），为
  逐次调用缓存读数累加所致（同前缀多次命中重复计），仅作缓存行为参考、
  不当成本折扣（README §5 已注明）。

## 6. glm-5.2 可用性问题（如实标注，不静默改口径）

- glm dh 三题（open 类）首调用 ReadTimeout（idealab timeout=120s）**连续
  3 轮全灭**（每单元各失败 3 次）；同批 cs/co 七题全部完成（含 cs_material_20
  前两次超时、第三次成功）。小请求探测正常（2.9s），判定为该模型在 dh 类
  首调用（带 tools schema 的首包）上稳定超 120s，叠加 §4-3 重试缺口。
- 处置（按 task-02 降级路径，未静默换口径）：试跑切片换同付费档
  qwen3.7-max 补齐 10 题；glm dh 三单元保持 failed 留痕待续跑判重。
  **全量前必须决策**：glm dh 是调 timeout/补重试后重验，还是降出矩阵。

## 7. 全量 3600 就绪状态

**H1 侧：就绪（有 3 项前置）**。框架经 17 单元 + 杀进程续跑实证；
qwen/kimi 可直接开，glm 待 §6 决策。
**H2 侧：未就绪**——框架 H2 接线位预留（显式报错不静默降级），
E4 `t05_h2_run_cli` 链路接入为 task-05 正式开跑的前置任务。

前置三项：①glm dh 可用性决策（§6）；②`llm_chat` 网络异常重试（需监工
授权改 src/benchmark，或全量靠续跑兜底并接受 3600 中概率性重跑开销）；
③三模型计费单价核实（影响成本报告，不阻塞开跑）。
