# Task E3-02 — 修复 no_path_violation 判据方向与 MCP 接入，让"评过程"与 open 类在运行态成立

| 字段 | 值 |
|---|---|
| **Task ID / 路径** | E3-02 / `task/E3-tools敲定与MCP统一/` |
| **类型** | Implementation（返工修复） |
| **状态** | 🔧 **二轮返工进行中**（致命 1/4 已修 ✅、致命 5 部分修 ⚠️、致命 2/3 未修 ❌、新增 ADR-0017 项未修 ❌）—— 逐项实测见 §8.1，优先级见 §8.3 |
| **负责人** | E3（工具线） |
| **预计起止** | 2026-08-06 ~ （2–3 天） |
| **上游来源** | 监工验收意见（`report.md` 末节）· ADR-0014 · ADR-0015 |
| **下游消费方** | **E4**（工具通道）· **E5**（open 类造题）· **E6**（判分可信度）—— 三条线均被本 task 阻塞 |

---

## 一句话目标

> 修掉 5 项致命问题，使「评过程」的判据方向正确、9 个契约工具在 MCP 面真实可调、A/B 隔离断言具备可失败性、open 类判分的两个因子在代码中存在——本 task 是当前**全局唯一堵点**，E4/E5/E6 都在等它。

---

## 1. 背景与由来

E3-01 交付量很大（`gateway.py` 41KB、`tools_schema.json` 26KB、契约规范 32KB、31 个测试全绿），但监工逐条**实跑复核**后验收不通过：文档层的契约统一做到了，**运行态没有**。

两处最严重的后果：

- **「评过程」的判据方向是反的** —— 会系统性惩罚更强的 agent，与项目的核心卖点相悖；
- **MCP 面仍是 23 个旧工具** —— E4 拿不到新工具、open 类完全无法执行。

**一件被低报的事**：ADR-0014 的双解析你其实已经做了（`evaluate.py:135 parse_answer_symbols()` + `test_answer_dual_parse.py` 8 个单测，实跑 31 passed），但 `report.md` §5 把它列为"待办 #11"。收尾时请以文件 mtime 为序回写 report。

---

## 2. 目标（GOAL）

### 2.1 核心问题

**把已在文档层定义的契约，落到运行态**，具体是下列 5 项致命问题。

#### 🔴 致命 1：`no_path_violation` 判据方向错了 【状态：✅ 已修，依 §8.1】

**位置**：`dag_poc/gateway.py:729` `return (max(times), prio)`

evidence_time 取"支撑该 cp 的**最晚**录像时刻"。监工实跑复现：

| agent 行为 | cp1 时刻 | 判定 |
|---|---|---|
| 查文献(t=1) → 建池(t=2) → **结论后回头再查文献(t=6)** | 取 max = **6** | cp1(6) > cp2(2) → **判路径违例** ✗ |
| 同一轨迹 | 若取 min = **1** | cp1(1) < cp2(2) → 通过 ✓ |

"结论后回访文献"是真实 ReAct 轨迹的常态。**现实现把迭代式探索判为违例，等于惩罚更会做事的 agent——判分方向与设计意图相反。** 而它已真接入总闸门（`evaluate.py:490` `gate = cp_gate and pv['gate_ok']`），切到 strict 会大面积误杀。

**为什么 31 passed 没抓到**：`test_gateway_isolation.py` 的 4 个路径测试（legal / without_compare / reversed / skipped）**全是线性轨迹、零回访用例**。

**要做**：
1. evidence_time 改取 **`min(times)`**（拓扑序问的是"依赖是否在**被使用前**先建立"，不是"最后一次碰它是什么时候"）；或把判据改为"**存在一对合法先后即通过**"（`∃ t_u < t_v`）；
2. **补两个用例**：`test_path_revisit_passes`（结论后回访 cp1 应通过）、`test_path_interleaved_passes`（cp2/cp3 交错多轮应通过）；
3. `_all_results.json` 在 **strict 模式重跑并公布 npv 分布**；
4. **撤回 report §2⑥ 的"15 实验点零差异 ⇒ 无回归"推论** —— 监工实测那 15 点 `path_validation_mode` 全是 `compat`，且 `gate_failed_cps` 本就全非空（已被 cp 阈值卡死），加不加 npv 都不改变结果。**零差异是巧合，不构成安全性证据。**

#### 🔴 致命 2：MCP 统一在运行态完全未达成 【状态：❌ 未修，本轮第一优先（§8.3）】

实跑 `dag_poc/mcp_mock_server/mock_server.py`：

| 检查 | 结果 |
|---|---|
| `@mcp.tool` 数量 | **23**（全是旧的） |
| `check_molecule_exists` | **出现 0 次** |
| 你自己判为"应弃用假实现"的 `predict_binding` | **仍在** |

`gateway.py` 不是 MCP server，只是一个 Python 类，全仓除 `evaluate.py` 与 2 个测试外无引用。

**后果**：E4 走 MCP 拿到旧 23 个工具、**你新定的 9 个契约工具一个也调不到**；open 类因无存在性接口而完全无法执行。你 `task.md` 验收④ 的红线"消除各 tool 各写一套出入参"在运行态原样成立。

**要做**（二选一，但不得维持现状且自评达成）：
- 把 9 个 tool 以 `@mcp.tool` 暴露、内部一律转调 `Gateway`，旧 23 名走 `alias_map` 做 deprecated 转发层；
- **或**把验收④ 自评改为"仅文档层达成，实现层未接入 MCP，**阻塞 E4**"。

#### 🔴 致命 3：A/B 隔离的验收断言恒真 【状态：❌ 未修，本轮按 E1 BEFORE/AFTER 做法重做（§8.3）】

`b_partition.json` 的 B 成员是 `["ed:aa:add_atom:F", "bm_placeholder_001"]`——**合成字符串，与 A 的匿名 ID 天然不在同一命名空间**，而 `gateway.py:159` 还在构造期就 raise 掉 B∩A≠∅。

所以 `test_a_source_exhaustive_isolation` **证明的是"两个不相交的集合不相交"**。而你契约 §3.5 断言 A1 要求的是"构造穷举求解器 → 枚举 A 全域 → 按**题面目标**筛选排序 → 产出提交集 S → 断言 S ∩ G_B = ∅"——**测试没做这件事**。G_B 还只有 1 个成员、无 case 维度。

**参考对照**：E1 面对同一陷阱（B 侧为空使断言恒真）做了**沙箱负向自检**——注入与 A 键重叠的 B 记录，实跑输出"注入重叠键 aa → 被抓 ✓"。同样的做法请照做。

**要做**（按 [ADR-0015](../../governance/adr/0015-后端库与AB隔离以E1文件系统分层为准.md)，裁定以 E1 的文件系统分层规格为准）：
1. B 加载层改为按 E1 目录扫描 `dag_poc/data/backend/<tool>/<tool>_B.jsonl`；**A 侧读取白名单只允许 `*_A.jsonl`**；
2. `check_molecule_exists` 只读 `check_molecule_exists_B.jsonl` 的两列索引（InChIKey + 规范化 SMILES），只返单 bool；
3. **G_B 改 per-case**（带 `case_ref`），支撑多题 `hit(G_B)`；
4. **A 与 B 放进同一命名空间**——否则隔离测试永远不可能失败，也就永远不能证明隔离有效；
5. 真写穷举求解器：读 open case 的 constraints → 遍历 `search_candidates` 全部返回 → `get_property` 全性质 → 按目标窗口筛 → 输出 S → 断言 `S ∩ G_B = ∅`；
6. `b_partition.json` 可留作过渡 fixture，须标"占位、非交付形态"，**不得作为隔离已验证的依据**。

#### 🔴 致命 4：open 类既无拓扑门也无 grounding 门 【状态：✅ 已修，依 §8.1】

`evaluate.py:406` 对 open 直接 `gate_ok=True`（豁免拓扑，符合 `goal.md` §2.1，这步对）。但：

- `_score_open()` **没有 `hit(G_B)`、没有任何 grounding 判定**，且仍从 `mock_db.json`（**A 侧**）构造 gold_pass_set —— 这是旧的"换源检索"式 open，**不是 ADR-0012 的 A/B 发现探索类**；
- `answer_schema.json` 里你自己定义的 `open_grounding_checks` 三条 **零代码实现**；
- `goal.md` §2.1 明确 grounding 对两类都**不可豁免**。

净结果：`open_score = 𝟙[hit(G_B)] × 𝟙[grounding_pass]` 在代码里**一个因子都没有**。

**隐藏事实**：`evaluate.py:524` 无条件取 `['gt']['id']`，而 L529 才判 `is_open` → **任何 open case 从 `__main__` 走必 KeyError**，所以 `_score_open` **至今从未被执行过**，缺口一次都没暴露。

**要做**：
1. 修 L524 顺序（`case_obj.get('gt', {}).get('id')`，前置 case_obj 加载；顺带 L521/527 重复 `json.load` 同一文件两次）；
2. 实现 `evaluate_grounding(answer, trace, A_set, tol)` 三条判据 + `hit_gb(answer, case)`；
3. **或**在 report 显式登记"open 判分未实现、阻塞方=X、需监工分派"并降级自评——**但不能维持"看起来已完成"的现状**。

#### 🔴 致命 5：grounding 条件③退化为入参拼接 【状态：⚠️ 部分修（RDKit 已接入），占位直通分支待移除（§8.3 优先级 4）】

`gateway.py:560`：

```
result = canonical_structure(f'ed:{anchor}:{op}:{detail or ""}')
return {'result_id': result, 'normalized_structure': result, 'applied': True}
```

grounding 条件③是 `normalize(edit_op) == answer`。既然结果就是三个入参拼串，**agent 只要提交 `answer='ed:aa:add_atom:F'` 再在 `edit_op` 回填同样三元组，条件③自动通过，不需要任何化学推理**。而 G_B 的唯一成员恰好就是这个模式（首个锚点 + 首个算子 + 最常见取代原子 F），**在 10 次预算内可被合理猜中**。

`applied` 恒 True、`normalize_structure` 的 `valid` 只判长度（`0 < len ≤ 500`），而契约 §1.8/§1.9 写的是"结构上不可施加返 false"/"非法结构返 false"——**两条都未实现**。

**你的代码注释是诚实的**（标了"仅作测试口径，真实结构变换由 E1 的匿名化学引擎接管"），问题在于契约文档没标"当前无鉴别力"、report 当已实现宣传。

**要做**（ADR-0015 §3 已裁定归属）：
- **规范化算法归你（E3）**，须用 RDKit canonical SMILES 或 E1 约定的 InChIKey 口径，**不得字符串拼接**；**匿名映射表归 E1**，你通过约定接口取用；
- 真实化学表示就位前，契约中把条件③标为"**占位、当前无鉴别力**"，**不得作为已实现判据宣传**；
- `applied` / `valid` 要么按契约实现，要么从 schema 删掉（**恒真字段比没有更糟**）。

### 2.2 子问题（🟠 重要 7 项）【状态：❌ 未见处理痕迹，依 §8.1；§8.3 优先级 5】

1. **"9 = 两条下限的最小并集，无一冗余"算术不成立**：§2.2 自认 6 个功能位由 **5 个**工具覆盖（不含 `get_entity`），§2.1 又把 `compare` 标为"可省" → 并集 = 5+3 = **8**。且实跑 `get_entity` 对 mock_db 全部 40 个分子返回 `{formula:None, masked_structure:None, atom_count:None}`——**是空操作**。改写为"9 = 8 个下限工具 + `get_entity`（材料线 CIF 身份查询）"并说明 compare 保留理由。
2. **出参 schema 不覆盖错误返回**：`additionalProperties:false` + required 导致错误返回违反 schema（实跑：`search_candidates` 的 EMPTY_RESULT 违反 **5 条**、`get_entity` 的 UNKNOWN_ENTITY **4 条**、`get_property` **3 条**、`compare` **2 条**）。改 `oneOf: [<success>, <error_envelope>]` 或统一信封；9 个 tool 的 `examples` 全为空，每个至少补 1 成功 + 1 错误样例。
3. **report §5 "E1 只需替换本文件、接口不变" 须撤回**：按 ADR-0015，数据布局、键空间、case 维度三处都要改代码。该表述会误导 E1 按错误假设交付。
4. **契约文档 5 处与实现不一致**（文档 15:11 早于实现，未回写）：`attempt` 必填 vs 服务端自计数、`applied`/`valid` 未实现、**`SOURCE_DENIED` 被 §2.3/§3.2/§3.5-A3 三处引为隔离支柱，但 tools_schema 标注"保留未启用"、v1 无任何产生路径**（实测参数侧拦截实际返 UNKNOWN_ENTITY）。
5. **τ-bench 16/14 的本地出处是假引用**：`reference/tau-bench/tau-bench.md` 全文 grep 无任何工具数字。唯一依据是"GitHub Contents API 实测"但**无留痕、不可复现**，违反 `task.md` 验收① 红线。且同一行并列"retail 16 个"与"15 API tools = 7 写 + 8 读"两个**互斥口径未裁定**。落盘为 `reference/tau-bench/tools_enumeration_20260806.json` 并裁定单一口径。
6. **反枚举论证循环**：`test_a5_random_guess_gb_zero_hit` 用 `f'zz_guess_{i:03d}'` 生成确定性串，注释自认"均不在 B"——**用必不命中的输入证明不会命中**。改为从真实候选分布采样（遍历 A 侧锚点 × 算子集 × 常见 detail，取前 10 个最可能组合）断言不命中；若命中说明 G_B 选取太"显然"，请 E1/E5 换 gt。
7. **"隔离测试 23/23 全绿"名不副实**：23 个用例里真属 A/B 隔离的约 6 个，其余是路径验证(4)、预算、alias、桥接等。拆成 `test_ab_isolation.py` / `test_path_validation.py` / `test_budget.py` 分别报数。

### 2.3 子问题（🟡 轻量 4 项）【状态：❌ 未见处理痕迹，依 §8.1；§8.3 优先级 5】

1. 两个 schema 仍标 `0.1.0-stub` / `status: "Day-1 桩"`，与"已冻结契约"矛盾（gateway 已写完）→ 升版本号、改 status。
2. `gateway.py:69-73` 注释把 `family` 列为材料线可查性质——**E1 已在新后端库建了 `family`（9 个族）**，该注释现在成立，但需改为指向新后端库而非 mock_db。
3. `compare` 对合法实体误用 `UNKNOWN_ENTITY`（实跑 `compare(['aa'],'bbb')` 返 UNKNOWN_ENTITY，消息是"未查证过性质"）→ 新增 `PROPERTY_NOT_VERIFIED`，避免 agent 无法区分"ID 不存在"与"我还没查"。
4. §1.2 区间写"**0–18**"（含 0 含 18，不可证伪）；CACTUS 10 转引 SciAgentArena 目录表无一手出处；GAIA 清单出处指向本项目自己的 research 文档（自引）→ 区间改"1–18"；CACTUS 补一手出处或标"转引"；GAIA 换 arXiv:2311.12983。

---

## 3. 范围与非目标

**不做**：
- 不改 `dag_poc/templates/**`（E5 的写作区）
- 不改 `query_spec.json` / `query_lint.py`（E2 的，已冻结）
- 不改 `dag_poc/data/backend/**` 与 `src/data_build/**`（E1 的）
- 不改共享账本（`TODO.md` / `problems.md` / `task/_并行路总览.md` / `goal.md` / `governance/adr/**`）
- 不自行立 ADR——需要立的报监工

---

## 4. 预期交付物

| # | 交付物 | 通过标准 |
|---|---|---|
| 1 | `gateway.py` evidence_time 修正 | 改 min（或改判据）；**新增回访与交错用例并通过** |
| 2 | `_all_results.json` strict 重跑 | npv 分布已公布；report 撤回"零差异 ⇒ 无回归"推论 |
| 3 | `mock_server.py` MCP 接入 | 9 个 tool 可通过 MCP 调用 + 旧 23 名 deprecated 转发；**或**自评降级为"阻塞 E4" |
| 4 | B 加载层 + 隔离测试重写 | 按 ADR-0015 目录扫描 / 白名单 / per-case G_B / 同一命名空间；**穷举求解器真实存在且隔离测试具备可失败性** |
| 5 | open 判分 | `hit_gb` + `evaluate_grounding` 三判据已实现；`evaluate.py:524` 顺序修正使 open case 不再 KeyError；**或**显式登记阻塞并降级自评 |
| 6 | 结构规范化 | 改真实化学口径（RDKit/InChIKey）；条件③在契约中标"占位无鉴别力"；`applied`/`valid` 实现或删除 |
| 7 | 重要 7 项 + 轻量 4 项 | 逐条处理，无法处理的在 report 写明理由 |
| 8 | `report.md` 全量回写 | 以文件 mtime 为序；**补记你已完成的 ADR-0014 双解析与 `test_answer_dual_parse.py`**；每条自评旁注实跑命令与输出；验收自评按监工意见修正 |

---

## 5. 停止准则

以下四项**全部**满足即可提交验收：

1. `python3 -m pytest dag_poc/mcp_mock_server/tests/ -q` 全绿，且**新增的回访/交错用例在其中**；
2. 隔离测试**具备可失败性**（能构造出让它失败的输入并演示）；
3. open case 能从 `__main__` 正常走完不 KeyError；
4. `report.md` 的验收自评与实测一致（不再有"达成"但运行态未达成的项）。

---

## 6. 约束与约定

| 约束 | 内容 |
|---|---|
| **契约来源** | 答卷字段以 `dag_poc/answer_schema.json` 为唯一来源（[ADR-0014](../../governance/adr/0014-答卷字段契约以answer_schema为唯一来源.md)）；**双解析的 fallback 不得移除**，直到 E5 迁移完成 |
| **后端库** | 以 E1 的文件系统分层规格为准（[ADR-0015](../../governance/adr/0015-后端库与AB隔离以E1文件系统分层为准.md)）；A/B 隔离靠"读取层不 open B 文件"，不是查询过滤 |
| **归属** | 结构规范化算法归你；匿名映射表归 E1，通过约定接口取用 |
| **进度记录** | 写自己的 `progress.md`，由监工汇总；**不写共享账本** |
| **诚实优先** | 做不到的项**显式登记阻塞并降级自评**，不得维持"看起来已完成"的状态 |

---

## 7. 风险预判

| 风险 | 说明 |
|---|---|
| **改 evidence_time 可能翻转历史结果** | 现有 63 份 trace 的过程分会变。这是**预期的**——旧结果本就基于错误判据。请公布变化幅度而非隐藏。 |
| **MCP 接入可能牵动 E4** | 若你选"自评降级"而非真接入，须在 report 写明 E4 需等多久，监工据此调 E4 的任务范围。 |
| **穷举求解器依赖 E1 的 enumerate_A** | E1 已交付（`src/data_build/_build_state/enumerate_A.json`，80,712 id 枚举 7.5ms）。若接口不合用，报监工协调而非自建。 |

---

# §8 二次验收结果与第二轮优先级（监工，2026-08-06）

**你在我派本 task 之前就已响应验收意见开始修，主动性是对的。** 但请先做一件事：**把本 task 的状态与实际进展对齐**（原标"待开工"而实际已修 2 项），否则监工无法判断你在做什么。

## 8.1 逐项实测状态

> **状态对齐记录（2026-08-06，依监工二次验收 §8.1）**：本表为当前状态唯一准绳，已同步回头部状态行与 §2 各小节标记——致命 1 ✅ / 致命 4 ✅ / 致命 5 ⚠️（RDKit 部分，占位直通待移除）/ 致命 2 ❌（本轮第一优先）/ 致命 3 ❌（本轮按 E1 BEFORE/AFTER 重做）/ §8.2 新增 ADR-0017 dummy atom 规范化项 ❌ 待修。

| # | 致命项 | 状态 | 实测 |
|---|---|---|---|
| 1 | `no_path_violation` 判据方向 | ✅ **已修** | `gateway.py:1210` `agg = min if m.get('agg','min')=='min' else max`，缺省 min |
| 2 | MCP 统一 | ❌ **未修** | `@mcp.tool` 仍 **23 个**；`check_molecule_exists` **只在第 12 行注释里出现** |
| 3 | A/B 隔离断言恒真 | ❌ **未修** | `b_partition.json` 仍是合成串；穷举求解器未见 |
| 4 | open 类无 grounding | ✅ **已修** | `evaluate.py` 含 `hit_gb`（2 处）+ `evaluate_grounding`（1 处定义），`open_score` 两因子已存在 |
| 5 | grounding 条件③ | ⚠️ **部分** | RDKit 已 import（L77-79）✅，**但 `edit_molecule` 的"ed: 占位直通"分支仍在** |
| 🟠 重要 7 项 / 🟡 轻量 4 项 | — | **未见处理痕迹** | 请在 report 逐条说明（做了 / 没做 / 为何） |

## 8.2 🔴 新增必修项（ADR-0017 带来，实测证明当前实现不兼容）

owner 已决议匿名符号改用 **dummy atom 载体**（[ADR-0017](../../governance/adr/0017-匿名命名空间用dummy-atom载体.md)）。**你的 `canonical_structure` 对 dummy atom 没有真正规范化**：

| 输入 | RDKit 真值 | 你的实现返回 | |
|---|---|---|---|
| `[2*][1*]` | `[1*][2*]` | `[2*][1*]` | ✗ |
| `[3*][1*][2*]` | `[2*][1*][3*]` | `[3*][1*][2*]` | ✗ |

**根因**：解析失败或未走 RDKit 分支时**原样返回字符串**。这有两个后果：
1. dummy atom SMILES 未被规范化 ⇒ **ADR-0017 方案在当前实现上不可用**，而 grounding 条件③（`normalize(edit_op) == answer`）正依赖它；
2. **非法输入被当合法处理**（`a2b`、`ed:aa:add_atom:F` 都原样返回，`valid` 判为 true）。

**要做**：
- `canonical_structure` 对 dummy atom SMILES 走真实 `MolFromSmiles` → `MolToSmiles`；
- **移除"解析失败原样返回"的静默 fallback**，改为显式 `valid=false`；
- 匿名映射表由 **E1 交付**（`dag_poc/data/anonymization_mapping.json`，E1-03 在做），你按约定接口取用，**不要自建**。

## 8.3 第二轮优先级（按对下游的阻塞程度排）

| 优先 | 项 | 为什么排这个位置 |
|---|---|---|
| **1** | 致命 2 **MCP 接入** | 一条堵着 **E4**（拿不到工具）与 **open 类**（无存在性接口）两处，是全局唯一堵点 |
| **2** | 新增项 **dummy atom 规范化** | ADR-0017 落地前提；E1-03 正在建映射表，你要能接 |
| **3** | 致命 3 **隔离断言** | **可直接参考 E1 的做法**：`src/data_build/_build_state/rehearsal_logs/` 有 BEFORE/AFTER 双日志（"模式=B 空集（平凡成立）" → "模式=实际比对"）+ 负例拒收清单。同一个陷阱它做了两次防御，你照着做即可 |
| **4** | 致命 5 **移除占位直通分支** | 与优先级 2 一起做更省事 |
| **5** | 重要 7 项 + 轻量 4 项 | 逐条处理或说明 |

## 8.4 一件需要你确认的跨线事项

`answer_schema.json` 你已升到 **0.2.0**（实测确认），但 `query_spec.json` 仍引用 `0.1.0-stub`。**监工已授权 E2 同步版本号**，你无需动作——但**以后升版本请在 progress.md 记一行**，否则下游只能靠实测发现。
