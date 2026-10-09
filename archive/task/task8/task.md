# task8 · 判分符号的接口层（不删符号，补基础设施）

## 0. 当前状态（2026-08-01 回顾核查）

**未完成，无 report。** 三件交付物中仅一件落地：

| 交付物 | 状态 |
|---|---|
| `dag_poc/docs/INTERFACE_CONTRACT.md`（符号绑定表 + 答卷契约全文） | ✅ 已落地 |
| `dag_poc/gateway.py`（取数网关） | ❌ **不存在** |
| `dag_poc/answer_schema.json`（全局答卷 schema） | ❌ **不存在** |
| `no_path_violation`（路径验证符号） | ❌ **代码中未实现**（全仓无引用） |

**部分符号已由另一条路径落地**：ADR-0011 的答卷契约拆分（`screened_pool`/`ranked_candidates`）已在 `dag_poc/templates/*/template.json` 与 `src/benchmark/evaluate.py` 实现，覆盖了原 `pool`/`pass_set` 语义。故本 task 的剩余缺口是 **`gateway.py` + `answer_schema.json` + `no_path_violation`** 三项。

**验收标准仍未达成**：cp2–cp6 与 `no_path_violation` 在真实 trace 上全部可求值、且三类轨迹（金标准/空转/陷阱）区分正确——`no_path_violation` 未实现，故无法验收。


> 上游：owner 2026-07-30 指令——"`pool ⊆ snapshot.universe`、`agent.value[c][p]` 这些都是同样的问题，你做的事儿不是把它删掉……你要正面面对，详细解释它，把用处跟作用先明确下来，然后看有什么比较好的解决方案，而不是一味地删掉。我觉得本质是因为你缺少了一个……"
> 决议：`governance/adr/0008-判分符号不删补接口层.md` ｜ 规范全文：`dag_poc/docs/INTERFACE_CONTRACT.md`

## 1. 目标

把 checkpoint `logic` 里跑不起来的符号**全部保住并变成可执行**，方式是补建它们缺失的那一层基础设施，而非删除符号。

**判定完成的标准**：cp2–cp6 与 `no_path_violation` 全部能在真实 trace 上求值，且金标准轨迹全过、空转轨迹不全过、陷阱轨迹在 cp3 或 cp4 亮红。

## 2. 诊断（已完成）

符号分三层：**世界层**（快照/case，✅ 已就绪）、**答卷层**（agent 交什么，❌ 从未定义）、**录像层**（调了哪些接口，❌ 从未定义）。
悬空符号 = 后两层缺失的症状，**不是错误符号**：`pool` / `agent.value[c][p]` / `agent.pass_set` / `ranking` / `fail_property` / `best` / `grounded()` / `search_calls` / `history` / `no_path_violation`。

**缺的那"一个" = 接口层**，而 owner 把它钉得更准：**缺一份标准的 LLM JSON 输出契约，而且它必须长在 template 里**——没有它就无法确定 response 里有没有 `agent.best` 这个 key，template 本身就是不完整的。三件套：**答卷契约（answer_contract + JSON Schema strict，已入 template）** + 取数网关（考场 + 录像）+ 全局符号绑定表（variables 字典的正确形态，提到全局一层）。

## 3. 子任务

| # | 子任务 | 状态 | 交付物 | Todo |
|---|---|---|---|---|
| 1 | 符号用处与作用逐条说清 + 三个根因 + 方案对比（闭合宇宙 A / 开放宇宙 B） | ✅ | `dag_poc/docs/INTERFACE_CONTRACT.md` §1–§5 | T-15 |
| 2 | 决议留痕（不删符号；`pool ⊆ universe` 改归属；PR-02 覆盖面备注；PR-05 口径收紧） | ✅ | ADR-0008 + `problems.md` PR-15/16/17 | T-15 |
| 3 | 实现取数网关（4+1 工具、录像、查证预算）+ 答卷单语法校验 | ⬜ | `dag_poc/gateway.py` | T-16 |
| 4 | **template 补完整**：`answer_contract`（JSON Schema strict + 三条交卷通道 + 失败码 + derived_bindings）入 template；case 物化 enum/条数 + `gold_expected` QA 锚；五个 cp 改为可执行 logic 并声明 `reads`（含四处 bug 修复） | ✅ | `templates/drug_candidate_discovery/{template,case_001_cns}.json` | T-17 |
| 5 | 引擎按绑定表求值（重算 gold、跑 5 个 cp、`evidence_time` 做路径验证） | ⬜ | `dag_poc/engine.py` | T-10 |
| 6 | 快照扩容 + 预算参数（8 → 60–200，拉开过程分区分度） | ⬜ | `dag_poc/snapshot/*.json` | T-18 |
| 7 | 三条对照 trace + 质量门 | ⬜ | `dag_poc/` trace + grade 报告 | T-11 |

## 4. 不做什么（边界）

- ❌ **不删任何符号**，不因"跑不起来"就下线任何 checkpoint（红线 1）。
- ❌ **不上 LLM 抽取器**从散文里抽答卷字段（等于把 LLM judge 从后门放进来，红线 3）。
- ❌ **v1 不做真实外部检索 + 身份解析**（B 方案留 v2；v1 走"发现式开放"）。
- ❌ **不依赖灵枢后端真假**（ADR-0004）：网关是我方出卷设施。

## 5. 已知风险

1. `get_property` 返回真值 ⇒ cp3 考"真查 + 如实转述"而非预测能力；难度必须由宇宙规模 + 查证预算承担（否则撞红线 1 的软化版）。
2. `smiles_masked` 掩码若可逆，agent 可反推真实分子名绕过检索（红线 5 失效）。
3. 材料线快照的 `condition` 是枚举型约束，需补枚举比较口径后才能走本契约。
4. **工作区无 git 仓库**（`git status` 报 not a repository），`ACTION_RULES` §1.5「先 commit 再执行」当前无法执行；本 task 只做新增文档与账本追加（可人工回退），但引擎/网关落地前建议先建仓。
