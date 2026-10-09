# E3 交接材料 · 新契约 trace 固化包（PR-34 判分修复验证用）

> 交接方：E4（harness 线，task-03 §2）｜日期：2026-08-07
> 目的：E3 用这批**新契约 9 工具**实跑的 fixed 类 trace 验证 `evaluate.py` 判分修复（PR-34），**不需要再问 E4 任何问题即可直接跑**。

---

## 1. 快速开始（一条命令）

```bash
cd /Users/jingwu/work/benchmark          # 工作目录必须是仓根
.venv/bin/python task/E4-agent-harness搭建/e3_handover/run_eval.py
```

- 脚本自包含：自动把 `src/benchmark` 加入 sys.path、只读 import `evaluate`（不改判分器）、自动 chdir 到 `dag_poc/` 对齐 evaluate 内部相对路径口径。
- 输出：逐组打印 cp1-cp6 / process / gate / task_score / best / 工具调用数。
- 退出码：`0` = 3 组判分全部可执行（**不代表分数达标**）；`2` = 有组抛异常。

不想用脚本、手动逐组跑判分（等价命令，工作目录 `dag_poc/`）：

```bash
cd /Users/jingwu/work/benchmark/dag_poc
../.venv/bin/python -c "
import json, sys; sys.path.insert(0, '../src/benchmark'); import evaluate
H = '../task/E4-agent-harness搭建/e3_handover'
case, pair = 'templates/fixed_discovery/case_cns_metoprolol.json', 'case_cns_metoprolol__bailian_glm-5.2'
resp = json.load(open(f'{H}/{pair}_response.json')).get('response')
scores, gps, ranked, best = evaluate.score_case(case, f'{H}/{pair}.jsonl', resp)
print(scores, best)
"
```

## 2. 判分器状态与当前实测（2026-08-07 更新）

**判分器状态时点**：`evaluate.py extract_tool_data()` 的双轨抽取（新契约迁移 + legacy 双轨分派）已于 2026-08-07 修复，**正确起点 commit 为 b85f4a7（11:40，任务 #45）**。订正说明：本文初版误写为 df93eca（经 git log 实查，df93eca 为 02:03 的"任务 #41 三维评审合并修复"，非判分双轨抽取）；订正依据为 E3 progress.md §11.4 知会（2026-08-07）。已认新契约 9 工具与 doc_id 口径。本 README 固化时的原始预期（PR-34 修复前 cp1-cp5 全 0）已过期，以下为修复后实跑 `run_eval.py` 的实测值（exit 0，2026-08-07 自跑）：

| 组 | cp1 | cp2 | cp3 | cp4 | cp5 | cp6 | process | task_score | 判读 |
|---|---|---|---|---|---|---|---|---|---|
| cns×bailian/glm-5.2 | 1.00 | 1.00 | 0.36 | 0.40 | 1.00 | 0.50 | 0.653 | 0.300 | cp1-cp5 全部非 0 |
| cns×moonshot/kimi-k3 | 1.00 | 1.00 | 0.91 | 1.00 | 1.00 | 0.50 | 0.882 | 0.300 | cp1-cp5 全部非 0（三组最高） |
| sotalol×moonshot/kimi-k3 | 1.00 | 1.00 | 0.01 | 0.00 | 0.00 | 0.00 | 0.303 | 0.151 | cp3-cp5 偏弱：trace 内 get_property 仅 1 次命中、agent 结论 NO_SOLUTION，证据稀薄所致，**非判分器问题** |

**PR-34 通过标准（task-03 文档 §2 原文口径）**：用本目录这批新 trace 实跑 `evaluate.py`，证明 cp1-cp5 非 0。**该标准已于 2026-08-07 用本批材料实跑达成**（cns 两组 cp1-cp5 全部非 0，见上表）。本批材料继续保留，供 E3 后续对 evaluate.py 的改动做**回归复跑**之用：回归预期 = 上表数字（允许判分器合理演进导致的口径变化，但不得崩溃、不得回退为修复前全 0）。

若复跑结果与上表不一致，先核对 evaluate.py 是否又在新 commit 上演进，再回来找 E4。

**分数判读警示**：这批 trace 里模型的 `answer` 本身不保证等于 ground truth（见 §3，cns 两组模型答 `ec`、gt 为 `aa`），cp 分数反映的是**判分器能否从 trace 提取证据**，不是模型能力结论。

## 3. 固化组清单（3 组 fixed 类，均含判分 trace + 全量证据流 + 答卷）

每组的工具清单**从判分 trace 逐行实数统计**（`ok` = result 不含 error_code），不凭印象。

### 3.1 case_cns_metoprolol × bailian/glm-5.2（推荐首选：submit_answer 交卷、工具最全）

| 字段 | 值 |
|---|---|
| case_id / 模板 | `case_cns_metoprolol` / `dag_poc/templates/fixed_discovery/case_cns_metoprolol.json` |
| 模型 | `bailian/glm-5.2` |
| 收卷通道 / answer / gt | `submit_answer`（服务端 schema 校验通过）/ `ec` / `aa` |
| 调用总量 | 33 次，命中 ok 20 次 |

| 新契约工具 | 调用次数 | 命中 ok |
|---|---|---|
| literature_search | 1 | 1 |
| search_candidates | 16 | 4 |
| get_entity | 1 | 1 |
| get_property | 12 | 12 |
| compare | 1 | 1 |
| submit_answer | 2 | 1 |

### 3.2 case_cns_metoprolol × moonshot/kimi-k3（text_fallback 收卷对照组）

| 字段 | 值 |
|---|---|
| case_id / 模板 | `case_cns_metoprolol` / 同上 |
| 模型 | `moonshot/kimi-k3` |
| 收卷通道 / answer / gt | `text_fallback`（未走 submit_answer，从末轮文本提取）/ `ec` / `aa` |
| 调用总量 | 30 次，命中 ok 18 次 |

| 新契约工具 | 调用次数 | 命中 ok |
|---|---|---|
| literature_search | 1 | 1 |
| search_candidates | 15 | 3 |
| get_entity | 2 | 2 |
| get_property | 11 | 11 |
| compare | 1 | 1 |

### 3.3 case_cardiac_sotalol × moonshot/kimi-k3（NO_SOLUTION 组）

| 字段 | 值 |
|---|---|
| case_id / 模板 | `case_cardiac_sotalol` / `dag_poc/templates/fixed_discovery/case_cardiac_sotalol.json` |
| 模型 | `moonshot/kimi-k3` |
| 收卷通道 / answer / gt | `text_fallback` / `NO_SOLUTION` / `ba` |
| 调用总量 | 35 次，命中 ok 6 次 |

| 新契约工具 | 调用次数 | 命中 ok |
|---|---|---|
| literature_search | 1 | 1 |
| search_candidates | 17 | 2 |
| get_entity | 2 | 2 |
| get_property | 14 | 1 |
| submit_answer | 1 | 0（服务端未 accepted，走兜底） |

> 选组说明：task-03 文档建议 sotalol 选 qwen3.7-max，但该组实测 ok 仅 2（search_candidates 23 次 0 命中、get_property 0 调用），PR-34 修复后也无法产出非 0 的 cp3-cp5，故改选证据更充分的 moonshot/kimi-k3 组。两组 cns 均已固化，满足"至少 2 组 fixed 类"。

## 4. 文件清单与 trace 格式说明

| 文件 | 说明 |
|---|---|
| `<pair>.jsonl` | **判分投喂文件**：纯 tool_call 投影，每行 `{seq, tool, arguments, result, ok, timestamp, latency_ms}`，**无 type 字段**——与 `evaluate.py load_trace/extract_tool_data` 的输入契约同形（历史 63 份旧 trace 也是这个形状）。送判分用这个。 |
| `<pair>_full.jsonl` | **全量证据流**：tool_call 与 llm_call 交织，每行带 `type` 字段（llm_call 含 attempt/token usage）。排查 agent 行为用这个，**不要直接喂 evaluate.py**（`extract_tool_data` 遇 llm_call 会 KeyError）。 |
| `<pair>_response.json` | 答卷：`{response: {candidates, ranking, answer, ...}, submission_channel, tool_calls_ok, token_usage, ...}`；`response` 字段即 answer_schema v0.2.0 结构，判分时作为 `response` 参数传入。 |
| `run_eval.py` | §1 的一键判分复跑脚本。 |
| `MD5SUMS` | 9 个固化文件的 md5 清单（防篡改自证，§6）。 |

schema 定稿：`dag_poc/trace_schema.json`（E4 task-02 交付）。

## 5. 环境前置（已就位，无需重装）

- `.venv`（python 3.12.13）已在仓根建好，evaluate.py 依赖可直接跑。
- 跑判分**不需要**起 mock_server（9000 端口）——trace 已固化，判分是离线复算。

## 6. 完整性自证（md5 + 行数/字节数，2026-08-07 固化时生成）

```
ec21e771d2a093b436072677e9f64b6a  case_cardiac_sotalol__moonshot_kimi-k3.jsonl        (35 行 / 18670 字节)
9828d532d5c8d531bbae2111264f46dc  case_cardiac_sotalol__moonshot_kimi-k3_full.jsonl    (50 行 / 1049673 字节)
6dd66094909dba51aaa95c985a5c605e  case_cardiac_sotalol__moonshot_kimi-k3_response.json (282 行 / 6678 字节)
a31bbc405c68a05abacee6af9d96ea97  case_cns_metoprolol__bailian_glm-5.2.jsonl           (33 行 / 18672 字节)
9f788486fb772abf8567571ec17f98be  case_cns_metoprolol__bailian_glm-5.2_full.jsonl      (46 行 / 790567 字节)
601596cf4c496b785ff2abe0635f4502  case_cns_metoprolol__bailian_glm-5.2_response.json   (217 行 / 4045 字节)
bce595479c3bbd60d17fab0bf8f1b821  case_cns_metoprolol__moonshot_kimi-k3.jsonl          (30 行 / 14678 字节)
c493ebe9fd1441f6d18e7a267edd707c  case_cns_metoprolol__moonshot_kimi-k3_full.jsonl     (44 行 / 861972 字节)
5d1ed6be1f964250ab0fc22aeb39c573  case_cns_metoprolol__moonshot_kimi-k3_response.json  (229 行 / 4169 字节)
```

一键校验：

```bash
cd /Users/jingwu/work/benchmark/task/E4-agent-harness搭建/e3_handover
diff <(md5 -r case_*.jsonl case_*_response.json | sort) <(grep -v '^#' MD5SUMS | sort)  # 无输出 = 未被篡改
```

## 7. 背景速览（只读参考）

- 这批 trace 是 2026-08-07 task-02 端到端冒烟产物，原始落点在 `dag_poc/traces/`，当晚遭外部进程 git 还原（见 PR-35 / E4 report-task-03），已重跑并快照到 `task/E4-agent-harness搭建/smoke_evidence/`，本目录即从快照固化；task-03 起新 trace 默认落 `task/E4-agent-harness搭建/traces_new/`（`TRACE_OUT_DIR` 可配）。
- 判分缺口详情：`problems.md` PR-34；E4 侧证据与判读见 `task/E4-agent-harness搭建/report.md` §五-1。
