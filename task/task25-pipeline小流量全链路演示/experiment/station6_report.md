# task25 站6 · agent 实跑通路报告 + 正式批执行手册

> 2026-09-17。冒烟 **1 run**（预算 ≤3，跑通即停）四环节全通；
> mock server MCP 面阻塞（无 `mcp` 包，属停机条件，未装、已上报）。
> 通路细节与证据见 `harness_readiness.md`，冒烟现场见 `smoke/`、结论见 `smoke_verdict.json`。

## 1. 冒烟结论（四环节通断表）

| 环节 | 状态 | 证据 |
|---|---|---|
| auth（jingwu idealab token） | ✅ 通 | `read_ak()` 走 env 分支（len 32）；HTTP 200；run 1 零 api_error；全程未触 chenxihe settings |
| 工具调用 | ✅ 通 | run 1 实做 18 次预算内 get_property + 免费 element_count，账本 19 条；预算耗尽 nudge 生效 |
| 落盘 | ✅ 通 | `smoke/runs/qwen3.7-max__smoke__20260917-103536/`：answer.json + tool_calls.jsonl + messages.json + run_stats.json |
| 判分（judge_contract） | ✅ 通 | 权威 `case_sc_chain_01/judge.py` 实判 total=0.8253、status=null（此前 0-API 干跑合成满分卷得 1.0） |
| mock server（MCP 9000/9001） | ❌ 阻塞 | `mcp` 包全机缺失；装 = 新依赖 = 停止条件 3。**进程内 Gateway 数据面已验证可用**（search_candidates 命中 z002730） |
| BBB 判分器 | ❌ 不存在 | 需站7 按 qed01 `judge_some.py`/chain `judge.py` 模式新写（含 `L = I(答错) + c·I(弃答)` 口径） |

冒烟模型与 run 数：qwen3.7-max × 1（tag=smoke，temp=0，204.8s）。
冒烟用题：冻结 `case_sc_chain_01`（材料链题）——BBB case 目录尚不存在（站1-3 未落），
链路证明与题目域无关；**注意 `src/probe_headless/case9.json` 是旧坏真值快照，禁用**。

## 2. 交付物清单

```
experiment/
├── harness_readiness.md          # 通路核查（§0 环境三事实 / 逐项实测标注）
├── smoke_harness_wrapper.py      # 最小 wrapper：复用 chain_harness_stream 的 stream_turn/make_tools，
│                                 #   题面与真值改为可配置（注入冻结 GT），不改既有脚本
├── smoke/
│   ├── runs/qwen3.7-max__smoke__20260917-103536/   # 冒烟 run 现场（answer.json + tool_calls.jsonl + …）
│   └── judge_dryrun/                               # 0-API 判分干跑现场（合成满分卷 → 1.0）
├── smoke_verdict.json            # 冒烟结论（确定性字段，sort_keys）
└── station6_report.md            # 本文件
```

## 3. 正式批执行手册（**主会话在 preregistration.json 落盘后执行**；站6 不自行开批）

前提检查（开批前逐条确认）：
1. 站1-3 已落盘：`experiment/sample.json`（6 分子）、`experiment/cases/<case_id>/`
   每题 `task.md 题面 + answer_schema.json + ground_truth.json + view.json`；
2. 站5 `experiment/preregistration.json` 在位（闸6）；
3. 站7 已写好 BBB 判分器（每题一个 `judge.py`，judge_contract 三函数 + CLI 三形态）；
4. mock server 依赖已解：owner 批准 `pip install 'mcp==1.29.0'`（或等价 venv）后
   `cd dag_poc/mcp_mock_server && PORT=9000 python mock_server.py` 起主通道，
   agent 经 `http://localhost:9000/mcp` 连（`src/benchmark/agent_harness.py` 的 MCPClient）；
   **若 mcp 依赖仍不可解**，正式批降级走 chain 直连模式（进程内工具包装 Gateway
   数据面 + per-case view 白名单过滤），须先在 preregistration 里写明该口径。

### 3.1 run 矩阵与预计 run 数

| 批 | 构成 | run 数 |
|---|---|---|
| 带工具批 | 2 模型 × 6 题（some_tools 单档） | 12 |
| 裸答基线批 | 1 模型 × 6 题（同分子同判分口径，无工具） | 6 |
| 合计 | 与 task.md §1 一致（API 总预算 ≤21 含冒烟 1，余量 2 供 api_error 补跑） | **18** |

模型建议：qwen3.7-max + bailian/glm-5.2（均在 ws_isolation.MODEL_POOL 已验证池）；
裸答模型 = 两者之一（预登记里钉死）。temperature=0。

### 3.2 逐条命令

带工具批（12 run；`<case_id>` ∈ 站2 产出的 6 题；BBB 题面/工具由站2/站3 定，
下列命令模板以 wrapper 模式为准——每题的题面路径与工具实现注入点由站3 交付后填入）：

```bash
export IDEALAB_AK="$(sed -n 's/^export ANTHROPIC_AUTH_TOKEN=//p' /mnt/ai4sci_develop_fast/jingwu/init | tr -d '\"')"
cd /mnt/ai4sci_develop_fast/jingwu/benchmark

for M in qwen3.7-max bailian/glm-5.2; do
  for C in <case_id_1> <case_id_2> <case_id_3> <case_id_4> <case_id_5> <case_id_6>; do
    python3 "task/task25-pipeline小流量全链路演示/experiment/bbb_harness.py" \
        "$M" "tools__$C" 0 --case "$C"          # 站3 交付后由主会话落地 bbb_harness.py
  done
done
```

裸答批（6 run；同题面、无工具、答案直接落 answer.json）：

```bash
for C in <case_id_1> ... <case_id_6>; do
  python3 "task/task25-pipeline小流量全链路演示/experiment/bbb_harness.py" \
      <裸答模型> "naked__$C" 0 --case "$C" --no-tools
done
```

（`bbb_harness.py` = 本次 `smoke_harness_wrapper.py` 的正式化：题面从
`experiment/cases/<case_id>/` 读、工具面接 Gateway+view 白名单、
交卷走 answer.json 文件而非 submit_answer——与 workspace 范式判分口径对齐。
本次冒烟已验证其全部通路构件；正式化改动集中在题面注入与工具后端两处。）

### 3.3 落盘路径约定（对齐 chain_harness_stream.py:191 命名）

```
experiment/runs/<model 斜杠换连字符>__<tag>__<YYYYmmdd-HHMMSS>/
├── answer.json          # 唯一答案来源
├── tool_calls.jsonl     # 工具账本 {"tool","args","result"} 每行一条（裸答批为空文件）
├── messages.json        # 全对话轨迹（回查证据）
├── meta.json            # model/tag/case/预算 等确定性元数据
└── run_stats.json       # 汇总（json.dumps sort_keys=True）
```

tag 约定：`tools__<case_id>` / `naked__<case_id>`。api_error.json 或 tool_leak.json
在位 ⇒ 判分整条剔除（口径 B），可用余量 2 run 补跑同格。

### 3.4 判分命令（每题唯一权威判分器，禁止 LLM judge）

```bash
# 单 run
python3 experiment/cases/<case_id>/judge.py experiment/runs/<run_dir>
# 批量（一个批目录）
python3 experiment/cases/<case_id>/judge.py experiment/runs --batch
# 汇总（sort_keys=True 落盘）→ experiment/scores.json，随后站7 出 demo_report.md
```

## 4. 阻塞点（需 owner/主会话处置）

1. **`mcp` 包缺失**（阻塞 mock server 9000/9001 与 workspace 路线）。解法二选一：
   a) 批准安装 `mcp==1.29.0`（pip 索引实测可达，requirements.txt 已钉此口径）；
   b) 正式批走 3.2 注所述降级直连模式（进程内 Gateway + view 白名单），预登记写明。
2. **workspace 路线凭据违规风险**：`ws_isolation.read_auth()` 只读 chenxihe 软链的
   settings.json。若正式批要走 workspace 路线，需 owner 裁定给 ws_isolation 增加
   合规凭据注入（env/jingwu settings）——属改既有口径，站6 未动。
3. **BBB 判分器缺位**：站7 交付前正式批无法判分（冒烟已验证 judge_contract 链路本身通）。
4. `case9.json` 为旧坏真值快照（预算 34、清单重叠）——**任何新代码不得引用它**；
   权威真值只在 case 目录 artifacts 下。
