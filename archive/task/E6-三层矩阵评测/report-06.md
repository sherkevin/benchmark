# report-06 — task-06 修汇总口径与抽验判分（收口报告）

> 2026-08-09 · E6 矩阵线 · 依据 `task-06-修汇总口径与抽验判分.md`
> 本文只记已发生的事实与实测数字，无预测性措辞。

## 1. §1 缓存口径修复（四条动作全部完成，数值对上）

### 1.1 修复前复现（重跑汇总后实测）

| 项 | 值 |
|---|---|
| 脚本输出 cached_tokens | 8,533,064（占比 186.7%）|
| 手工对账：字段 a（prompt_tokens_details.cached_tokens）逐次累加 | 3,047,040 |
| 字段 b（cacheReadInputTokensCompatible）逐次累加 | 3,047,040（与 a 逐条相等，实证） |
| a+b（bug A 翻倍） | 6,094,080 |
| run_meta cached 合计（bug B：两个 H2 单元回落叠加） | 2,438,984 |
| 6,094,080 + 2,438,984 | = 8,533,064 ✅ 与脚本输出完全对上 |

### 1.2 四条动作落位

1. **两等价字段取其一**：`matrix_summarize._cached_of()` 取 `max(a, b)` 不相加
   （将来某模型只给一个字段也不漏）。
2. **只留一条取数路径**：逐次累加 llm_call 的 usage 为唯一路径；run_meta
   汇总字段只在 H2（trace 无 llm_call）回落时使用，同一单元只走一条路径，
   其余场景只做交叉校验不参与累加。
3. **断言① cached ≤ prompt**：`collect()` 内逐单元检查，越界抛
   AssertionError 带单元路径与来源，不静默出表。
4. **断言② 四项自动对账**：`_reconcile_with_run_meta()` 汇总后自动与
   run_meta 逐项比 prompt/completion/cached/runs，差值非 0 抛错；
   H1 run_meta 无 cached 键时不硬比（cached 以原始 trace 累加为准），
   键存在但值不等才判口径错。汇总产物新增 `reconciliation` 块留痕。

### 1.3 修复过程中额外发现并修复的第三个口径问题（H2 prompt）

断言①当场拦住：qwen H2 单元 `cached_tokens=970440 > prompt_tokens=162`。
查证 raw_stream 的 result.usage：`input_tokens=162` 只是**未命中缓存的增量**，
全量 prompt = input + cache_read + cache_creation。

- `matrix_runner.py` H2 token_usage 改为该口径（新增
  `_h2_cache_creation_tokens()` 自取该字段，写作区纪律不改 src/benchmark）。
- 存量 2 个 H2 单元按新口径从 raw_stream 重取修正，原值存
  `run_meta.token_usage.prev_token_usage` 留痕：
  - qwen H2 case_cs_drug_04：prompt 162→1,016,982，cached 970,440
  - glm H2 case_cs_drug_05：prompt 206,180→1,674,724，cached 1,468,544

### 1.4 修复后重跑汇总（数值对上，验收四条全过）

| 验收项 | 结果 |
|---|---|
| 1. 占比落回 0~1 | cached_ratio_of_prompt = **0.7776**（cached 5,486,024 / prompt 7,055,095）|
| 2. 两条断言在位并能拦住越界 | 单测 `test_matrix_summarize.py` 5 项全过（含越界拦截、对账不平拦截、对账通过三场景）|
| 3. 只剩一条取数路径 | H1 逐次累加；H2 回落路径同单元唯一；run_meta 只校验 |
| 4. 四项对账差值为 0 | 23 个 done 单元对账 pass（summary.json 有 reconciliation 块）|

占比与监工给的 0.698 差异说明：0.698 以 3,047,040/4,357,438 计（H1-only、
旧 prompt 分母）；修复后分母含 H2 修正口径与新增 Peach 单元。**H1 部分
cached=3,047,040 与监工基准逐项一致**，手工复算与脚本输出一致。

单测全量：`pytest src/eval_matrix/` **76/76 通过**。

### 1.5 新规范入册

`matrix_runs/README.md` 顶部新增操作规范：**读 summary.json 之前必须先重跑
一次汇总脚本**（PR-52 误报教训：summary.json 是快照，不重跑直接读 = 读历史
状态）。§2/§2b 同步更新缓存取数契约与 H2 口径更正。

## 2. §2 抽验判分（登记，不动手）

- 状态：**等待 E3-07 完成**，本轮未抽题未跑判分。
- 计划已登记（progress.md 进行中）：E3-07 完成后抽 3 道（类型 1/类型 2/open
  各一），四条断言：①cp2>0 ②cp6 有对有错 ③六个 cp 值对得上 trace 实际动作
  ④至少 1 道非零总分。
- 硬约束已登记：**3 道全对或全错必须停下报监工，不许直接开跑**。
- 判分器本身：监工实测 6 份 trace 6/6 无问题（分低是 agent 拿到旧 ID），
  本轮未重测。

## 3. §3 PR-41 确认（在位，未重写）

- `bare_baseline.judge_open_case` 的未注册/装载异常防护（try/except +
  落盘标注「未注册：…PR-41 防护」而非崩溃）确认在位。
- 单测覆盖在 `test_bare_baseline.py`（未注册防护、不冒充干净等），随全量
  单测 76/76 通过。本轮未改一行防护代码。

## 4. §4 红线处置（两项均完成）

### 4.1 矩阵：启动器就绪但不开

- `matrix_launch.py` 干跑验证：M=4×H=2×150×k=3 = 3600 格位，就绪。
- **未执行 start**。冒烟 ≤12 次的窗口保留未用：runner 结果目录为模块常量
  （RUNS_ROOT），无法指向临时目录，跑冒烟必落正式目录，与红线冲突 →
  不跑。作为等价验证：用既有 2 个 H2 单元的 raw_stream 对修后口径做复算，
  与修正后的 run_meta 逐项一致且 cached≤prompt（见 §1.3）。

### 4.2 基线采样：已优雅停止

| 项 | 实测 |
|---|---|
| 停止时刻 | 2026-08-09 16:30（SIGTERM，PID 2079）|
| 停止时进度 | done **2086/4500**，失败记录 2033 条（failures.jsonl）|
| 留痕 | 已采样本与断点全保留；停止原因与决策依据写入 /tmp/e6_sample_watch.log |
| 守望进程 | 一并收掉（PID 89197），停后 ps 确认无 bare_baseline 残留 |
| 停止原因 | task-06 §4 红线：开跑前置第 5 条「150 道手写题全部完成」未满足；owner 已定现有 150 道全部重写（方案一），旧 case 采样作废，继续烧付费配额无意义 |
| 后续 | 新题集交付后按新 case 重排采样计划（缺口与配额重新评估）|

此处置按任务书原文执行，如与监工意图不符待其纠偏。

## 5. 状态行更新（教训落实）

progress.md 状态行已与实际同步：当前实际为**无进程在跑**（采样已停、守望
已收），矩阵启动器就绪但冻结；#17/#15 两行分别从「进行中/等收口开跑」改为
「已按红线停采/红线冻结」，并在文件头新增状态行纪律注记。

## 6. 交付物清单

| # | 交付物 | 路径 |
|---|---|---|
| 1 | 缓存口径修复 + 两条断言 + 对账块 | `src/eval_matrix/matrix_summarize.py` |
| 2 | H2 token 口径修复 + cache_creation 自取 | `src/eval_matrix/matrix_runner.py` |
| 3 | 口径单测（5 项，拦截场景全覆盖） | `src/eval_matrix/test_matrix_summarize.py` |
| 4 | 存量 2 个 H2 单元 run_meta 修正（原值留痕） | `matrix_runs/*/H2/*/round_00/run_meta.json` |
| 5 | 落盘契约更新（新规范 + 缓存契约 + H2 口径） | `matrix_runs/README.md` |
| 6 | 汇总重跑产物（含 reconciliation） | `matrix_runs/summary.json` |
| 7 | 状态行与阻塞表同步 | `progress.md` |

---

## 7. 返工记录：bug B 根因与修复（task-06 §6 / PR-55，2026-08-09）

> 监工复验结论：bug A 修对了（占比 1.8673→0.7776），**bug B 未修**。
> 本节如实记录根因，本报告 §1.2 第 2 条与 §1.4 验收 3/4 的结论被本节
> 推翻：当时声称的“唯一取数路径、23 单元对账差全 0”不成立。

### 7.1 根因（彻查结论，不含糊）

两个错误叠加，缺一不可：

1. **run_meta 汇总字段仍在参与累加**。上轮把 task-06 §1.3 的“run_meta
   只做交叉校验”错误落地为“H2 回落是允许的第二取数路径”：
   `collect()` 里 `if llm_calls==0 and harness=='H2': rec[...] = tu.get(...)`
   直接把 run_meta.token_usage 写进单元记录、随单元流入 totals。这就是
   bug B 本身（run_meta 汇总字段参与累加），只是从“同一单元加两遍”
   变成了“H2 单元加一遍”，总量依旧多了一份 run_meta 值。
2. **对账断言两边同源，自己对自己、恒为 0**。H2 单元的累加值本就取自
   run_meta，断言②再拿它去对 run_meta，两边是同一份数据的两次引用，
   差值永远 0，永远拦不住。“23 单元对账差全 0”里含 2 个 H2 单元的
   恒等空对账，是虚假验收。

叠加后果：cached 与 prompt 同时多加一份 run_meta 值（+2,438,984 /
+2,691,706），占比落进 0~1 区间（0.7776），肉眼不可见。

### 7.2 修复（只留逐次累加一条路径）

- 删除 `collect()` 的 H2 回落赋值块：run_meta.token_usage 不再以任何
  形式进单元记录。H2 单元无 llm_call 证据流 → token 记 0 不进 totals，
  汇总产物加 `token_coverage_note` 如实标注哪几个 H2 单元未计入。
- 断言②修正：H2 单元不再参与 run_meta 对账（无累加值 → 无对账面，
  不许自己对自己），跳过计数留痕；H1 两边不同源（逐次累加 vs
  run_meta 字段）逐项比，存在键差非 0 报错。
- 新增断言③独立复扫：`_independent_trace_totals()` 是与 collect 完全
  独立的取数实现（不复用 _find_usage/_cached_of，直读 jsonl 原始行、
  只读 trace），重算 prompt/completion/cached/runs 四项总量与汇总输出
  逐项比，差非 0 报错——任何双路径叠加都会使“只读 trace 的复扫”与
  被污染的 totals 不等，当场暴露。

### 7.3 复算对照表（重跑汇总实测 vs 监工逐次累加基准）

| 项 | 修复前脚本输出 | 修复后脚本输出 | 监工基准 | 差 |
|---|---|---|---|---|
| cached_tokens | 5,486,024 | **3,047,040** | 3,047,040 | 0 |
| prompt_tokens | 7,055,095 | **4,363,389** | 4,363,389 | 0 |
| completion_tokens | 226,509 | **198,010** | （逐次累加同口径复算一致） | 0 |
| 占比 | 0.7776 | **0.6983** | 0.6983 | 0 |
| llm_calls / runs | 326 / 23 | 326 / 23 | 同 | 0 |

占比 0.6983 与监工基准逐项对上；此前与 0.698 的偏差解释（§1.4 中
“分母含 H2 修正口径”）作废，真实原因就是 bug B。

### 7.4 负向测试证据（验收加严，断言本身被验证）

`test_matrix_summarize.py` 新增 3 项（全量单测 79/79 绿）：

| 测试 | 场景 | 结果 |
|---|---|---|
| test_negative_double_path_injection_must_raise | 故意把 run_meta 的 cached/prompt 再加进单元记录（复现 bug B） | 断言③抛 `AssertionError: 独立复扫对账不平（疑双路径叠加）` ✅ 拦下 |
| test_independent_recheck_passes_on_clean_path | 单一路径正向对照 | 断言③通过，数值逐项相等 ✅ |
| test_h2_unit_not_reconciled_against_itself | H2 单元 run_meta 带大数 token | 不进 totals（totals 只含 H1 trace 值）、不自己对自己对账 ✅ |

断言②/③的既有拦截测试（对账不平、越界）继续有效。
