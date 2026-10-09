# 04 · 改造 harness：输出路径、工具注册表、失败不中断

Type: task
Status: resolved
Blocked by: 01

## 三个硬伤（已读代码确认）

1. `DEFAULT_OUTPUT_ROOT = REPO_ROOT / "cases/_methodology/tool_conflict_census"` —— `cases/` 已移入 archive，此路径不存在，跑批直接失败
2. `DEFAULT_MODEL_IDS = ("admet_ai_1_4_0",) + tuple(DEEPPURPOSE_MODELS)` 硬编码，且 `model_id not in DEFAULT_MODEL_IDS` 的校验散在 6 处（行 211/220/265/315/357/407）
3. 失败即中断：全文 12 处 `raise RuntimeError`（行 267/274/302/305/310/321/323/329/331/333/335/337）。本轮要的是"失败也记下来继续跑"——这与现有设计相反

## 做什么

1. **输出路径**：改指 `data/bbb_census_v3/`（02 号 ticket 建立）—— **已在 02 号执行中完成**，`tool_conflict_census.py:32` 与另 7 个文件的路径常量都已改指新位置，此项只需回归确认
2. **工具注册表**：新增 `data/bbb_census_v3/tool_registry.json`，每个工具一条记录，字段至少包含
   ```
   tool_id, family, tier(L3|W|V), runtime_path, command(数组),
   endpoint{id, action_rule, threshold}, version_lock{pypi|commit|zenodo_doi},
   asset_sha256[], enabled(bool)
   ```
   把 `DEFAULT_MODEL_IDS` 及 6 处校验改为读注册表
3. **失败模式**：`invoke_worker` 增加 `on_failure="record"` 模式——非零退出码时不 raise，而是把 `status` 与 `failure_reason` 写进结果行，receipt 照常落盘（`exit_code` 字段已有，保留原值）。原 `raise` 行为保留为 `on_failure="abort"`，用于既有产物的复现校验
4. **端点泛化**：`BBB_Martins` 从常量改为读注册表的 `endpoint` 字段
5. **三列输出**：结果行改为 `value` / `status` / `failure_reason` 三件套（ADR-0003 §三）
6. **修 `validate_pool_seal` 的 `generator_sha256` 比对口径**（执行 02 号时发现的第四个硬伤）。`tool_conflict_candidate_pool.py:450` 起的 `validate_pool_seal` 把 `generator_sha256` 与 `sha256_file(generator_path)` 比，而 `generator_path` 默认取 `__file__`，也就是工作树里那份**可变**的生成器文件。后果是任何一次无害编辑（02 号只改了 3 行路径常量）都会让校验失败，把「封条记录完好」和「生成器从未被编辑过」两件事混为一谈——前者是封条该管的，后者不是。

   改法：比对一个钉住的 blob 而非活文件。函数已有 `generator_path` 形参，具备这个能力，缺的是调用侧默认值和一条「封条时刻代码从哪取」的约定。封条时刻那份生成器已提取保存在 `data/bbb_census_v3/sealed_code/tool_conflict_candidate_pool.at_seal.py`（sha256 `f8413d3618f78c865d9fe69e36f64ccf0b0e982e658735d60280722f39b4eda3`，等于封条声称值），可直接作为参照物。

   **不要用重新封条的办法让它变绿**——那会毁掉封条唯一的价值。

## 验收

- 01 号建立的测试基线全部维持原状态（绿的仍绿）
- 用既有 receipt 做回归：以 `on_failure="abort"` 模式重跑 `rebuild_screening_rows_from_receipts()`，重建结果与 `data/bbb_census_v3/` 里的宽表逐位一致
- 注册表里注册一个故意配错命令的假工具，确认 `on_failure="record"` 下它被记为 `install_failed` 且整批继续
- `tool_conflict_candidate_pool.py verify` 全项通过，且 `pool_seal.json` 未被重新生成（`git diff` 对该文件为空）

## 硬约束

不改 `invoke_worker` 的 stdin-JSON 协议与 19 字段 receipt schema——新旧数据要能并表，协议一动就断了。

## Answer

### 逐项落地

1. **输出路径**：回归确认。`tool_conflict_census.py:36` 的 `DEFAULT_OUTPUT_ROOT` 指向 `data/bbb_census_v3/tool_conflict_census`，目录存在，`tool_conflict_candidate_pool.py verify` 能从新路径读到全部产物。
2. **工具注册表**：新建 `data/bbb_census_v3/tool_registry.json`（schema `bbb-tool-registry-v1`），4 条记录，字段与工单列举一致。命令写成 `{runtime}` / `{workers}` 模板而不是绝对路径——冻结的 v2 契约就是因为把跑批主机的绝对路径钉死，才导致 `verify_frozen_census` 只在那一台机器上可过（见下方结构性发现），注册表不重复这个错。
   `DEFAULT_MODEL_IDS` 拆成两件事：4 处校验问的是「这个工具有没有钉住的资产哈希」，属本模块局部性质，改名 `PINNED_PROVENANCE_MODEL_IDS`；另 2 处问的是「这轮普查该跑哪些工具」，改为读注册表的 `enabled_tool_ids()`。混作一谈会让注册表一长就要求冻结的 4 工具普查凑齐全部工具。
3. **失败模式**：`invoke_worker(..., on_failure="record")` 落地。拆出 `run_worker_attempt()` 单次执行不抛异常，把失败归到 ADR-0003 §三 的枚举：进程起不来 → `install_failed`（没有 pid 与退出码，不伪造 receipt）、超时被杀 → `timeout`、非零退出或 stdout 非法 JSON → `runtime_error`。receipt 照常落盘，`exit_code` 保留真实值。
   重试按 §四 的 1s/4s/16s，但**只重试 `timeout` 与 `runtime_error`**：起不了进程是确定性失败，重试四次是同一个结果。重试轮的 receipt 落在 `run_receipts/retries/attempt{n}/`，不覆盖首次尝试。
   `on_failure="abort"` 保持原语义且**不重试**——既有产物的复现校验要求 receipt 集合与轮次一一对应，补跑一次就多一份 receipt，把可重放这条线打断。
4. **端点泛化**：端点契约的权威定义留在代码里（`BBB_ENDPOINT` / `ENDPOINTS`），注册表只按 `id` 引用并复述 `action_rule` 与 `threshold`，载入时两侧必须一致。反过来（让注册表当权威）等于允许改一行 JSON 就悄悄改判定口径。`normalize_probability` 与 `validate_endpoint_evidence` 都改为从传入的端点契约取阈值和类别含义，不再读模块常量。
5. **三列输出**：新增 `build_wide_table()` / `write_wide_table()`，`run_census` 每次跑批多写一份 `wide_table.csv`。列名即 `<round>__<tool_id>__value|status|failure_reason`，失败留空而非哨兵值。窄表里的行级 `failure` 映射为 `runtime_error`；复现轮没覆盖到的分子记 `not_attempted`，与「跑了但失败」区分开。
   **没有改 `screening_ledger.jsonl` 的形状**：宽表是派生产物。改窄表会同时破坏冻结产物的逐位复现和并表能力。
6. **封条口径**：已修（`validate_pool_seal` 的 `generator_path` 默认解析到产物同目录的快照，不再回落 `__file__`）。**工单正文里的路径已过时**：那份钉住的 blob 现在在 `data/bbb_census_v3/tool_conflict_candidate_pool/bbb_v1/generator.at_seal.py`（sha256 `f8413d36…`，实测等于封条声称值），不在 `sealed_code/` 下。`PROVENANCE.md` 的三处引用已同步更正。

### 验收结果

| 验收项 | 结果 |
| --- | --- |
| 01 号测试基线绿的仍绿 | **通过**。基线 47 passed / 1 skipped / 14 subtests → 现 **58 passed / 1 skipped / 14 subtests**，增量全是本次新增的 11 条测试 |
| 以 `abort` 模式重跑 `rebuild_screening_rows_from_receipts()`，重建结果与既有产物逐位一致 | **通过**（`test_published_artifacts_are_reproduced_from_receipts`，6 件产物逐一比对） |
| 注册表里配错命令的假工具记为 `install_failed` 且整批继续 | **通过**（`test_registered_broken_tool_is_recorded_and_batch_continues`：临时注册表两个坏工具，两个都被记下，批次跑完） |
| `candidate_pool verify` 全项通过且 `pool_seal.json` 未重新生成 | **部分通过**。`git diff` 对 `pool_seal.json` 为空；封条链与 400 候选、A/B/C 分层全部复算一致。但 `naked_baseline` 一项无法在本机复算——转录目录 `runs_naked/` 被 `.gitignore:40` 的 `cases/**/runs_*/` 排除，git 历史里从未存在过，只在当初跑批的主机上。verify 现在把这一项如实报成 `naked_baseline_check: transcripts_unavailable` / `status: partially_verified`，并回报封条里记的 `run_root` 与 `runs_tree_sha256`，而不是崩掉或默默判绿 |

宽表另有一条真实数据证据：从已发布的 730 行窄表投影出宽表，逐行核对 discovery 四个工具全 `success`、概率落在 [0,1]、无失败原因，复现轮恰好对无初始冲突的分子记 `not_attempted`（`test_wide_table_projects_the_published_ledger`）。

### 两个结构性发现（工单 05 及后续跑批要按此设计）

1. **census 产物是机器绑定的。** `verify_frozen_census` 比对预登记里冻结的 worker 绝对路径（`/mnt/ai4sci_develop_fast/...`），所以它只在原跑批主机上可过。在别处复现必须从契约里取命令，而不是从 `make_model_specs()` 取——回归测试已经这么做了。新跑批用注册表的相对模板命令，可以不继承这个问题。
2. **`code_freeze()` 把 harness 自身哈希进契约，等于让普查脚本一次性写死。** 任何后续编辑都会让同一个 census_id 的复现校验失败。要么给它和候选池同样的「快照存产物旁」待遇，要么明确接受「一次普查对应一份代码快照」。本工单未动这一点，留给工单 06 起正式跑批前裁定。

## Comments
