# E4 · task-03 收口报告（trace 落盘防护与证据固化）

**核心结论**：task-03 五条交付标准全部达成——新 trace 落盘已迁出 `dag_poc/traces/` 至本任务独立目录 `task/E4-agent-harness搭建/traces_new/`，输出路径三级可配置（显式参数 > `TRACE_OUT_DIR` > 默认值），最小冒烟实跑证明新路径生效且 `dag_poc/traces/` 零新增写入（195 文件清单/mtime/内容 md5 冒烟前后逐一比对一致）；3 组 fixed 类冒烟证据已固化为 E3 可直接复跑的 PR-34 验证材料（含逐组工具清单实数统计与 md5 自证）。

---

## 一、交付标准五条逐项核对

| # | 标准 | 状态 | 证据 |
|---|---|---|---|
| 1 | trace 落盘位置迁移（新 trace 在 task/E4-* 下；dag_poc/traces 不再被写入） | ✅ | 默认目录改 `task/E4-agent-harness搭建/traces_new/`（agent_harness.DEFAULT_TRACE_DIR）；最小冒烟实测落盘于此（§三）；迁移用"复制 + 保留原件"（traces_new 27 文件从 smoke_evidence 快照复制），未 move/删除 dag_poc/traces 任何文件 |
| 2 | 输出路径可配置（不写死，默认指向自己目录） | ✅ | `resolve_trace_dir(trace_dir)`：显式参数（`run_case(trace_dir=...)` / `run_all.py --trace-dir`）> 环境变量 `TRACE_OUT_DIR` > DEFAULT_TRACE_DIR；三级优先级实测正确（§三-3）；run_all.py 已删写死的 TRACE_DIR |
| 3 | 冒烟证据固化（≥2 组 fixed，E3 可直接取用） | ✅ | `task/E4-agent-harness搭建/e3_handover/`：3 组 fixed（cns×glm-5.2、cns×kimi-k3、sotalol×kimi-k3）× 3 文件（判分 trace + _full + 答卷）；README 含一键复跑命令与通过标准；run_eval.py 实测 exit 0 |
| 4 | 新旧目录分工说明（供 E3/E6 取用） | ✅ | §二 |
| 5 | report 收口（含还原事件完整时间线） | ✅ | §四 |

## 二、新旧目录分工（E3/E6 必读）

| 目录 | 状态 | 内容 | 谁用 |
|---|---|---|---|
| `dag_poc/traces/` | **只读历史资产**，E4 不再写入 | 63 份旧契约 trace（判据回归 fixture，ADR-0014 修订 R1）+ task-02 冒烟重跑后的 9 组产物（与 smoke_evidence 快照逐一 md5 核验一致，0/27 diff） | E3/E6 只可只读取用；**任何写入都违反纪律** |
| `task/E4-agent-harness搭建/traces_new/` | **E4 新 trace 默认落盘点**（可配置） | 9 组 task-02 冒烟产物副本 + task-03 最小冒烟新产物；格式见 dag_poc/trace_schema.json | E6 跑测编排从这里读（或用 TRACE_OUT_DIR/--trace-dir 指定别处） |
| `task/E4-agent-harness搭建/e3_handover/` | **E3 专用固化包**（md5 自证） | 3 组 fixed 判分材料 + README + run_eval.py + MD5SUMS | E3 验 PR-34：`.venv/bin/python task/E4-agent-harness搭建/e3_handover/run_eval.py`（仓根执行） |
| `task/E4-agent-harness搭建/smoke_evidence/` | 快照（防还原备份） | 与 dag_poc/traces 现状逐文件 md5 一致 | 备查 |

**路径可配置用法**（E6 注意）：

```bash
# 默认（落 traces_new/）
.venv/bin/python src/benchmark/run_all.py case_cns_metoprolol --models qwen3.7-max
# 环境变量覆盖
TRACE_OUT_DIR=/some/dir .venv/bin/python src/benchmark/run_all.py case_cns_metoprolol
# 显式参数覆盖（最高优先）
.venv/bin/python src/benchmark/run_all.py case_cns_metoprolol --trace-dir /some/dir
```

## 三、最小冒烟验证（§1 要求 4，实跑证据）

1 组最小冒烟：`case_cns_metoprolol × qwen3.7-max`（2026-08-07 11:23:57–11:26:07，mock_server 按 case 重启，DAGPOC_CASE_ID=case_cns_metoprolol）。

**结果**：exit 0，steps=15，tools=38(ok=18)，channel=submit_answer，answer=ec（gt=aa），tokens=163375。

**证据 1 —— 新路径生效**：run_all 打印 `trace 落盘目录: /Users/jingwu/work/benchmark/task/E4-agent-harness搭建/traces_new`；三件套新产物（.jsonl / _full.jsonl / _response.json）mtime 11:26 落于 traces_new/。

**证据 2 —— dag_poc/traces/ 零新增写入**：冒烟前（11:23:30）与冒烟后分别对 dag_poc/traces 全目录取证并比对：

- 195 文件的「文件名 + size + mtime」清单 diff 为空；
- 全部文件内容逐文件 md5 的聚合值冒烟前后一致：`eb7629e587e64c7898376e46d5ef6e3a`；
- 即：无新增、无删除、无改写、无 mtime 触碰。

**证据 3 —— 可配置三级优先级实测**（resolve_trace_dir）：无参 → `.../traces_new`；`TRACE_OUT_DIR=/tmp/…` → 生效；显式参数 → 覆盖 env。

## 四、还原事件完整时间线（PR-35，供排查还原源）

均为 2026-08-07（task-02 冒烟夜，本机时间）：

| 时刻 | 事件 |
|---|---|
| ~00:59 | 首启 mock_server（端口 9000），开始 9 组端到端冒烟，trace/答卷陆续落 `dag_poc/traces/`（新写内容覆盖同名旧作废文件） |
| 冒烟期间 | cns、sotalol 六组（fixed 类，均为 git 已跟踪文件）落盘完成；open 三组同日落盘（git 未跟踪） |
| **01:35:53** | **六组新 trace/答卷被整体还原为旧作废内容**——还原后与 git HEAD **逐字节一致**；未被 git 跟踪的 open 组未受影响。特征指向 `git checkout -- dag_poc/traces/`（或等价的文件级还原），且只作用于 git 跟踪文件 |
| 01:35:53 后 | E4 发现异常：response 内容变回旧作废内容（mtime 与内容矛盾）；随即重跑 cns+sotalol 六组，并把 9 组产物快照到 `task/E4-agent-harness搭建/smoke_evidence/`（脱离 git 跟踪区） |
| 事后核实 | **owner 确认从未执行过 git checkout**；还原源未查明，登记 problems.md **PR-35** |
| 连带后果 | E3 `test_answer_dual_parse.py` 以 `traces/case_cns_metoprolol__qwen3.7-max_response.json` 为 fixture，还原后该文件 `response` 变 `None`，**5 个测试失败** |
| 2026-08-07 11:23–11:26（task-03） | 最小冒烟期间 dag_poc/traces 未再发生还原/写入（前后 md5 一致），可作为还原源排查的参考基线：还原并非持续定时触发 |

**已知特征汇总（给排查者的线索）**：① 只还原 git 已跟踪文件，未跟踪文件幸免；② 还原结果与 HEAD 逐字节一致（非部分写坏）；③ 责任人（owner/E4）均未执行 checkout；④ task-03 时段未复现。无论还原源是什么，本 task 的落盘迁移已使 E4 交付物不再暴露于该风险下。

## 五、交接材料要点（给 E3）

- 路径：`task/E4-agent-harness搭建/e3_handover/`（README.md 为总入口，**不需要再问 E4**）。
- 一键复跑：`cd /Users/jingwu/work/benchmark && .venv/bin/python task/E4-agent-harness搭建/e3_handover/run_eval.py`（实测 exit 0）。
- **判分器状态更新（2026-08-07，收口后环境变化）**：evaluate.py 双轨抽取已由并行线修复（正确起点 commit b85f4a7，11:40；初版误写 df93eca，经 E3 progress.md §11.4 知会与 git log 实查订正），认新契约 9 工具与 doc_id 口径，本报告收口时声明的“cp1-cp5 全 0”已过期；修复后自跑实测：cns×glm-5.2 cp1=1.00/cp2=1.00/cp3=0.36/cp4=0.40/cp5=1.00，cns×kimi-k3 cp1=1.00/cp2=1.00/cp3=0.91/cp4=1.00/cp5=1.00，sotalol×kimi-k3 cp1=1.00/cp2=1.00/cp3=0.01/cp4=0/cp5=0（证据稀薄非判分器问题）。详见 README §2。
- **PR-34 通过标准：已于 2026-08-07 用本批材料实跑达成**（cns 两组 cp1-cp5 全部非 0）。
- 工具清单均从判分 trace 逐行实数统计（README §3 逐组列表，含调用次数/命中 ok）。
- 完整性自证：`MD5SUMS`（9 文件 md5 + 行数/字节数，README §6 同录；一键校验命令已实测无 diff）。
- 选组偏差说明：文档建议 sotalol 用 qwen3.7-max 组，实测该组 ok 仅 2（get_property 0 调用），PR-34 修复后也无法产出非 0 的 cp3-cp5，改选证据更充分的 sotalol×kimi-k3，已在 README §3 注明。

## 六、纪律自查

- 写作区：仅动 task/E4-agent-harness搭建/、src/benchmark/agent_harness.py、src/benchmark/run_all.py、dag_poc/trace_schema.json；**dag_poc/traces/ 全程零写入**（有 md5 证据）。
- 禁改清单未触碰：evaluate.py / gateway.py / tools_schema.json / answer_schema.json / mcp_mock_server/** / templates / data / 共享账本。
- task.md：仅在文末追加 task-03 状态，owner 表头原文（"✅ task-02 已收口…"）未回退。
- 实验后：mock_server 已杀（端口 9000 释放），/tmp 基线快照留作核验证据。

## 七、剩余缺口

1. **PR-34（已实测达成）**：evaluate.py extract_tool_data 判分适配已由并行线修复（双轨抽取起点 commit b85f4a7，11:40；初版误写 df93eca，已按 E3 §11.4 知会订正）；用本批交接材料实跑，cns 两组 cp1-cp5 全部非 0（glm-5.2：1.00/1.00/0.36/0.40/1.00；kimi-k3：1.00/1.00/0.91/1.00/1.00），sotalol 组 cp3-cp5 偏弱（0.01/0/0，trace 证据稀薄所致，非判分器问题）。材料保留供 E3 回归复跑。
2. **PR-35 还原源排查（归监工）**：时间线见 §四；E4 侧防护已落地，不再阻塞。
3. 真 A/B 发现型 open case 缺失（等 E5），open 三门判分闭环未验证。
