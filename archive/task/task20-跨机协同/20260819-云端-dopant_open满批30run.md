# 报告：dopant_01·open 满批 30 run 跑完 + 一个挡住所有满批的 harness 误报

| 字段 | 值 |
|---|---|
| 日期 | 2026-08-19 |
| 发件方 | 云端（开发机 Linux） |
| 类型 | 报告 |
| 关联 commit | `01c4cee` |
| 需对方做什么 | ①采信本报告的数据结论 ②**修 `ws_isolation.py` 的 tool_leak 误报**（见 §4，不修则今后每个满批都会在第一个 run 就停批） |
| 回执状态 | ✅ 已回执（→ 本文件，对应本地 20260819-本地-指令-dopant_open满批.md） |

---

> ## ⛔ 2026-08-20 追加限定：本报告数据产自已废止的「免费全量发现」配置
>
> 本报告的 30 个 run 跑在 `search_db` **免费且不分页**的旧配置上。08-20 复盘查明该配置
> 使 open 的发现步白给：单次 `search_db` 返回**全库 30 个**、对目标 Gd 系覆盖率
> **中位 1.000、29/30 恒为 1.0**。因此：
>
> - **规范 §11.1 判据 ④「发现有后果」在该配置下永远无法通过**（恒 1.0 则不存在"低覆盖组"）；
> - **§5 表里"尺子② fixed 0.778 vs open 0.766、差 0.012 正向但噪声级"这条结论失去解释力**
>   ——fixed/open 的唯一合法差是候选可见性，而发现免费时 open 的信息量 ≈ fixed，该变量
>   实际没生效。**不得据此说"这道题 fixed 不优于 open"**，只能说这道题的 open 当时还没做成立。
>
> 该配置已按方案 A 修正（`PAGE_SIZE=4` 分页 + 每页扣 1 预算 + `measurement_budget=23`，
> owner 批、已按 §8 预登记）。本报告的 30 个 run 连同 cprobe/dprobe/smoke 共 **37 个已归档**
> 到 `runs_freeDiscovery_deprecated/`：**不得作为达标证据引用，不得与新配置数据混算。**
>
> **本报告仍然有效的部分**：§2 的逐 run 状态与失败分类口径、§4 的 tool_leak 误报根因分析
> （已据此修好 `ws_isolation.py`）、§5 中"cp3/cp4 仍是阶梯门"的结论（与发现机制无关）。

---

## 速读

**满批 30 run 全部跑完并落盘，数据有效，但被 harness 误判成 `tool_leak` 而在日志层「停批」。**
根因不是数据问题：30 并发下 MCP server 启动慢，claude 在 MCP 就绪前拍了 init 快照，
里面是 `5 内置 + WaitForMcpServers = 6` 个工具，还没挂上 `measure_tc`/`search_db`，
于是 `n_tools=6 ≠ expected=7` 触发 leak。**但 30/30 个 run 实际都调用了两个 MCP 工具、
29/30 正常交卷**——工具从来没缺，只是 init 拍早了。

- **判分**（29 run 入分布，1 个 api_error 剔除）：open 均值 **0.7655**
- **尺子②**：fixed 0.778（本地 64 run）vs open 0.766 → **fixed 略优，差 0.012，正向但在噪声级**；
  推翻了此前 8 run 给的 open 0.799（反向）——那是小样本假象
- **cp3/cp4 仍是阶梯门**：cp3 只在 {0.1, 0.2, 1.0} 跳、cp4 只在 {0.006, 0.014, 0.054, 1.0} 跳，
  **X-6 移出总分的处理，30 run 给出了充分证据**
- **模型间区分度有限**：6 个模型均值挤在 0.66–0.83

---

## 1. 命令与环境

```bash
cd /mnt/ai4sci_develop_fast/jingwu/benchmark && git pull   # HEAD=01c4cee
cd src/probe_headless
../../.venv/bin/python sweep_ws.py ws_runner_dopant_open.py 5 d 2>&1 | tee /tmp/dopant_open_d.log
```

- 前缀 `d`（轮次 d1–d5）；模型池 6 个；`MAX_WORKERS=30`，30 run 全并发
- 先跑了 `--probe` 3 run 探路（前缀 `dprobe`，均正常，均分 0.737），方向对了才上满批
- 满批 30 run 全部落盘到 `cases/materials/atom/open_basic_tools/case_sc_dopant_01/runs/`

## 2. 逐 run 状态（30 run）

| 状态 | 数量 | 说明 |
|---|---|---|
| 正常交卷 | 28 | 判分入分布 |
| 交卷但空 predictions | 1 | `DeepSeek-V3-671B d5`：search_db 查 3 次没命中就放弃、measure_tc 一次没调 → 能力失败，按口径 B **计 0 分入分布** |
| api_error | 1 | `qwen3-coder-plus d5`：HTTP 400「模型提供方限流」→ **剔除，不算能力失败** |
| **`returncode: -9`（进程被杀）** | **0** | 本地那批全灭的症状，云端一次没出现 |
| tool_leak 标记 | 26 | **全部误报，见 §4** |

MCP 工具实际调用核查（30/30）：每个 run 的 `tool_calls.jsonl` 里 `measure_tc + search_db` 调用数均 >0，
调用量 3–35 次不等。**没有任何一个 run 真的缺 MCP 工具。**

## 3. 判分结果

判分器 = 本地已修好的版本（`total = (cp1_class + cp2_numeric)/2`，cp3/cp4 只记录不进总分）。

### 3.1 按模型聚合（29 run，api_error 已剔除）

| model | n | 均值 | 轮间极差 | min~max |
|---|---|---|---|---|
| qwen3-coder-plus | 4 | **0.829** | 0.116 | 0.762~0.877 |
| qwen3.7-max | 5 | **0.812** | 0.176 | 0.754~0.930 |
| deepseek-v3.2 | 5 | **0.806** | 0.176 | 0.758~0.934 |
| qwen3.5-plus | 5 | **0.755** | **0.006** | 0.753~0.759 |
| bailian-glm-5.2 | 5 | **0.747** | 0.093 | 0.676~0.769 |
| DeepSeek-V3-671B | 5 | **0.658** | 0.883 | 0.000~0.883 |
| **全体** | **29** | **0.7655** | — | — |

DeepSeek-V3-671B 的均值和极差都被那个空交卷的 0 分 run 拉动；剔掉它后其余 4 轮是 0.792~0.883（极差 0.091）。

### 3.2 逐 run 逐关（节选，完整见判分器 `--batch` 输出）

cp1 绝大多数是 0.857（7 个信息点命中 6 个），少数满分 1.000；cp2 在 0.49~0.90 之间铺开——
**四关里区分度几乎全靠 cp2 承载**，与探路批的判断一致。

## 4. 关键：tool_leak 是误报，会挡住今后每一个满批

这条比数据本身更要紧，因为**不修，30 并发的满批永远在第一个返回的 run 就被判 leak 而停批。**

### 现象

`sweep_ws.py` 汇总只打印 1 行就停批：

```
{"model": "qwen3.7-max", "round": "d1", ..., "n_tools": 6, "tool_leak": true, ...}
!! tool_leak：工具暴露数与预期不符，停批。先跑 probe_claude_models.py 核对 BUILTIN_TOOLS
```

（注：因为 `ThreadPoolExecutor.__exit__` 会等所有已提交线程跑完，30 个 run 其实都跑完落盘了，
只是 `ex.map` 一 break 就不再打印后续结果。所以数据在，日志只有 1 行。）

### 根因（已逐 run 核实）

`ws_isolation.py:222` 取 trace 里 `subtype=="init"` 事件的工具数：

```python
if d.get("subtype") == "init" and d.get("tools"):
    n_tools = len(d["tools"])
```

而 leak run 的唯一 init 事件，工具列表是：

```
['Edit', 'Glob', 'Grep', 'Read', 'WaitForMcpServers', 'Write']
```

**是 `5 个内置 + WaitForMcpServers`，不含 `measure_tc`/`search_db`。**
`WaitForMcpServers` 是 claude code 在 MCP server 尚未就绪时挂的占位工具——它出现本身就说明
「这张 init 快照拍摄时 MCP 还没连上」。30 并发抢资源，MCP server 启动被拖慢，
于是绝大多数 run 的 init 都拍在 MCP 就绪之前，`n_tools` 停在 6，`expected=7` → 判 leak。

对照证据：那 4 个 `n_tools=7` 的 run（glm d3/d4、qwen3.7-max d3/d4）恰好是 MCP 在 init 前就绪的，
所以不 leak。而**探路批（3 并发）几乎不触发**，正是因为并发低、MCP 启动快。

`tool_leak.json` 里现有的 note 写的是「claude 升版新增内置工具」——**方向反了**：
不是多了内置工具，是少了还没连上的 MCP 工具。

### 建议修法（归本地，`ws_isolation.py`）

三选一，我倾向第 1 个（最小、最稳）：

1. **判 leak 前先剔除瞬态占位工具**：若 init 的 tools 里含 `WaitForMcpServers`，
   说明 MCP 未就绪，这张快照不可用于计数——改取「最后一个**不含** `WaitForMcpServers` 的 init 事件」，
   或直接把 `WaitForMcpServers` 从计数集里排除并等一个更晚的 init。
2. **改用实际暴露集判 leak**：leak 的本意是「有非法工具混进来」，那就检查 trace 里**被调用过**的工具
   是否都在 `BUILTIN_TOOLS ∪ MCP` 内，而不是拿 init 快照的**数量**去对。
3. 起 agent 前显式等 MCP 就绪（治本但依赖 claude 行为，成本高）。

在修好之前，满批数据需要像本报告这样**人工核实每个 run 是否真的调用了 MCP 工具**才能采信。

## 5. 直接回答你下发时的三个问题

| # | 问题 | 结论 |
|---|---|---|
| 1 | 尺子②（fixed 优于 open）正还是反？ | **正向但极小**：fixed 0.778 vs open 0.766，差 **0.012**，在噪声级。8 run 时的 open 0.799（反向）是小样本假象，29 run 推翻了它 |
| 2 | 单模型跨轮稳不稳？ | 多数稳：qwen3.5-plus 极差仅 0.006、glm 0.093；qwen3.7-max/deepseek-v3.2 约 0.176。6 模型均值挤在 0.66–0.83，**模型间不太分高下**——这道题本身区分力有限，和 fixed 侧「挤在 0.75–0.85 窄带」同症 |
| 3 | X-6 修完后 cp3/cp4 是否仍同分？ | **仍是阶梯门**，诊断完全坐实：cp3 全场只取到 {0.1, 0.2, 1.0} 三个值，cp4 只取到 {0.006, 0.014, 0.054, 1.0}。它们不该进总分，30 run 是充分证据 |

## 6. 遗留

| # | 事项 | 归属 |
|---|---|---|
| 1 | **修 `ws_isolation.py` 的 tool_leak 误报**（§4），否则满批跑不通 | 本地 |
| 2 | dopant_01 这道题「模型间区分度有限」是否是它本身的问题（fixed/open 双侧同症），要不要调题面/判据加大区分 | 本地 / owner |
| 3 | 下一批跑什么（constraint_02 已就绪但 fixed 侧批间极差 0.721，稳定性存疑） | 等本地判分本批后定 |
| 4 | 原始日志 `/tmp/dopant_open_d.log` 与 30 个 run 目录都在云端，需要哪段点名 | — |

**按纪律跑完这一批就停**，等你看完判分再定下一批。
