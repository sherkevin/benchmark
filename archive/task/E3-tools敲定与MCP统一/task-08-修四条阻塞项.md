# Task E3-08 — 修四条阻塞全量开跑的缺陷（PR-61/62/63/65）

> 2026-08-10 派发 · 监工 · **关键路径：E6-07 抽验四条断言两条未过，根因三条压在你这一线**

| 字段 | 值 |
|---|---|
| **状态** | ✅ **已交付（四条 PASS，待监工复验 / E6 重跑 3 道）**：PR-61 _queryable_props 动态剔除投影全 None 键 + UNKNOWN_PROPERTY 语义 + viability 断言 5 测试（随 a01a6eb 混装入库）/ PR-62 template_id 显式路由两处 + 防回归 4 测试（随 22e8d99）/ PR-63 charset=utf-8 共用中间件（随 22e8d99）/ PR-65 keywords 拒字符串化数组（随 a01a6eb）。Chris #83 独立实测（HEAD=e968a33）：全量 383 passed / 1 failed / 1 skipped / 5 errors（唯一失败归因 b9a3576 task-07 批次⑤ dh_09 重注册，与 task-08 零交集，二选一待监工裁定）；aggregate 回放 418/54 不变；红线全守；本轮无 LLM 调用。遗留项见 problems.md PR-66~67；收口见 report.md §15，任务 #84 状态对齐 |
| **入库方式（重要登记）** | 本线文件无独立 commit，被外部全仓提交混装（归属偏离第四次）：a01a6eb 裹 PR-61/65 七件、22e8d99 裹 PR-62/63 六件与调研/知会文档、181413b 裹挟 Chris 取证脚本（.r83_inject.py/.r83_routing.py，已删工作区）；三代理逐件/逐 hunk 核验内容完整无篡改，按既往裁定不重写历史，登记见 problems.md PR-67 |

---

## 0. 与项目最终目标的对齐

`goal.md` 第六步（跑三层矩阵）现在被一道闸门挡住：E6-07 抽 3 道验判分，**断言① cp2>0 未过、断言② cp6 全错触发硬停线**。我独立复验后定位到根因，其中三条在工具/判分侧，归你。

**这道闸门救了两天机时**——如果直接开全量，3,600 次跑完才发现全是 0 分。

---

## 1. 先看清 E6-07 抽验的结果（不是它做得不好，是撞出了真问题）

| 断言 | 结果 |
|---|---|
| ① cp2 > 0 | **未过**，六个单元全 0 |
| ② cp6 有对有错 | **触发硬停线**，H1/H2 各 3 道全错 |
| ③ 六个 cp 对 trace | 完成，判分器问题 2 类 |
| ④ 至少 1 道非零总分 | 过 |

**重要前提：题本身是好的。** 我实测——用真实可用的约束一次检索，`case_cs_drug_15` 返 36 条且 gt 排第 24；`case_co_material_15` 返 5 条且 gt 在内。**一次调用就能拿到答案。** 所以问题不在题、不在判分器给分严，在工具链。

---

## 2. 🔴 第一条（PR-61）：`mw`/`logp`/`tpsa` 宣称可过滤，库里 0 条数据

**这是断言①②未过的真根因。**

### 2.1 实测证据

| 调用 | 结果 |
|---|---|
| `mw ge 0`（分子量 ≥ 0，**恒真条件**） | **EMPTY_RESULT** |
| `logp` 全区间 / `tpsa` 全区间 | 同样全空 |
| 对照 `logS ge -6` | OK，2,173 条 |
| 对照 `atom_count ge 10` | OK，74,377 条 |

数据面坐实：扫 `get_property_A.jsonl` 前 20 万行，**这三项各 0 条**。

### 2.2 成因

`gateway.py` L1422 / L1584：

```
filter_only = sorted(qprops - vprops - hidden)
```

这是**动态算出来的**——这三项在 `_queryable_props()` 里但不在真值表里，于是被自动归类为"filter-only"，写进说明书和错误信封。**但它们实际既不能查值、也不能有效过滤。**

### 2.3 后果有多严重（agent 真实行为逐条）

`H1 case_cs_drug_15` 的检索调用：

```
#9  mw 200~500              → 空
#11 mw 300~500 + logS       → 空
#12 mw 250~500 + logS       → 空
#13 mw 150~400 + logS       → 空
#14 mw 100~600（已覆盖全部药物分子）→ 空
```

它以为是自己范围定窄了，一路放宽，全部落空，**最后打满 50 次调用上限**（`CALL_LIMIT_EXCEEDED` 17 次）。

**这一条同时解释三件事**：`EMPTY_RESULT` 86 次（占全部报错 42%、第一大错误码）、cp2 全 0（候选池永远拿不到）、cp6 全错（没走到选答案那步）。

### 2.4 要做的

| # | 动作 |
|---|---|
| 1 | **二选一，不许两边都留**：把三项从 `_queryable_props()` 彻底移出（说明书 / 错误信封 / 参数白名单**三处同步**），**或**请 E1 补数据后再宣称可用 |
| 2 | **错误码语义也要改**：无数据的性质应报 `UNKNOWN_PROPERTY`（"这字段不能用"）而非 `EMPTY_RESULT`（"没找到结果"）。后者会误导 agent 反复放宽重试——实测正是如此，白烧 14 次调用 |
| 3 | **补机器断言**：对外声称可过滤的**每一项**，都要验证"至少存在一个阈值使其返回非空"，否则 CI 报错 |

第 3 条是重点：**这个 bug 能活到现在，就是因为没有任何断言检查"宣称可用的东西是不是真可用"。**

**结果（任务 #84 回写）**：✅ 已交付（随 a01a6eb 混装入库，归属登记见 problems.md PR-67）：选项①落地——`_queryable_props()` 动态剔除投影全 None 键，mw/logp/tpsa 出白名单、报 UNKNOWN_PROPERTY（不再误导 agent 反复放宽重试）；tools_schema v0.3.2 九处 + smoke 两处 + docstring 三处同步；viability 机器断言 5 测试带注入负例自证，Chris 另四路独立注入实证可证伪性。遗留：replay 漂移实查 ok→UNKNOWN_PROPERTY 11 次（有效单元格 11、调用 14 全 mw），与规格预期 ±14 有口径差（当年跑在旧 mock_db 记 ok 非 EMPTY_RESULT），已如实登记（见 report.md §15.4）。

---

## 3. 🔴 第二条（PR-62）：open 三门判分从未执行过

### 3.1 实测

`evaluate.py` L302：

```
is_open = 'gt' not in case
```

而 **30 道手写 dh 题全部带 `gt` 键**（答案留档本就应该）⇒ `is_open = False` ⇒ 直接进 `_score_fixed`。

**我加重一层**：`discovery_case_ids()` 实测**正确注册了全部 30 道**（38 个里含 30），但路由在上一层就把它们劫走了 ⇒ **`_score_open_discovery`（32 行，含 hit/G_B/grounding/attempt）永远不被调用。**

### 3.2 严重性

goal §2b 的

$$\text{open\_score} = \mathbb{1}[\text{hit}(G_B)] \times \mathbb{1}[\text{grounding\_pass}]$$

其中：
- $\mathbb{1}[\cdot]$：指示函数，条件成立取 1、不成立取 0；
- $\text{hit}(G_B)$：提交的分子属于该 case 落盘的 B 侧合格集；
- $\text{grounding\_pass}$：产物落地验证通过（anchor ∈ A、引用值与工具返回一致、`edit_op(anchor)` == 提交分子）；

**这个公式一次都没跑过。** 而"两类任务分列判分"是本 benchmark 的核心设计——open 类评终态、fixed 类评过程，这是我们区别于同类工作的立论点。

### 3.3 归属与改法（我已定死，不用再讨论）

**归你（判分器），不归 E5。** 题里写 gt 是对的，答案必须留档。

| # | 动作 |
|---|---|
| 1 | 判据换成**显式类型字段**（`template_id` 或 case 里的 `case_kind`） |
| 2 | **不得再用"字段在不在"这类隐式判断**做类型路由 |
| 3 | 按我们已有的规矩落**机器断言**：断言 30 道 dh 全部路由到 open 分支 |

第 3 条参照 `test_resolve_composition_task05.py` 那个模式——裁定要能被 CI 防回归。

**结果（任务 #84 回写）**：✅ 已交付（随 22e8d99 混装入库）：显式 `template_id=='discovery_haystack_v1'` 路由两处——主路由 L302 + `__main__` aggregate 第二处 L1901（调研补发现，任务书未提，不同改会造成判分路由与聚合口径分裂）；DISCOVERY_TEMPLATE_ID 常量 + case_is_discovery 纯函数两处共用；用 template_id 不用 case_kind（实查 case_kind 全库 0/150 不存在，按实物口径报备）。Chris 实测：30 道 dh 全进 _score_open_discovery，fixed 120/旧 open 4 零变化；test_dh_routing_task08.py 4 测试防回归。

---

## 4. 🟠 第三条（PR-63）：H1 错误信息乱码，你上轮的成果被抵消了一半

**现象**：`UNKNOWN_PROPERTY` / `SCHEMA_VIOLATION` 的中文说明（**里面装着可查性质清单**）在 H1 trace 中是乱码。`err.encode('latin-1').decode('utf-8')` 可还原。

**根因**：mock_server 的 HTTP 响应没声明 `charset=utf-8`，requests 回退 ISO-8859-1。H2 不受影响。

**为什么这条重要**：你上一轮把说明书从 516 字补到 6,142 字，还把可查清单写进了错误信封——**这一半的功夫在 H1 链路上被乱码抵消了**。agent 运行时读到的是乱码，等于没告诉它。

**动作**：响应头声明 `charset=utf-8`。
**验收断言**：H1 trace 里的错误文本能直接读出中文性质名。

**结果（任务 #84 回写）**：✅ 已交付（随 22e8d99 混装入库）：CharsetUtf8AsgiMiddleware 共用中间件，mock_server/gateway_mcp 两处 wrap；中文直读实证（test_charset_task08.py）。调研疑点如实登记：H1/H2 实同走 mock_server.py 单链路，乱码差异在客户端解码（requests 回退 ISO-8859-1 vs CLI 按 JSON 规范 UTF-8），任务书设问与实物不符（见 task-08-research.md §6）。

---

## 5. 🟡 第四条（PR-65）：keywords 传成字符串没被拒

**现象**：H1 agent 把 keywords 数组序列化成字符串传入（如 `"[\"solubility\"]"`），**未被拒绝**；且 agent 普遍不用 `constraints`。

**动作**：入参校验拒掉字符串化数组，报 `INVALID_ARGUMENT`，并在错误信封里给出正确形态示例。

**边界不变**：**不做宽容解析**。承既定裁定——猜字段名、猜格式不属于 benchmark 该考的能力，但我们也不替 agent 纠错，只把话说清楚。

**结果（任务 #84 回写）**：✅ 已交付（随 a01a6eb 混装入库）：keywords 字符串化数组/非 str 拒绝 INVALID_ARGUMENT，错误信封带正确形态示例；不做宽容解析（test_keywords_validation_task08.py）。

---

## 6. 明确不做的事（红线）

| 不做 | 原因 |
|---|---|
| 不改题 | 150 道已冻结（lint 冻结批次退出码 0）。我已验证题是好的 |
| 不放宽判分 | cp2 全 0 是工具链的错，不是判分严。修工具，不是调分数 |
| 不做同义词模糊匹配 | 承既定裁定 |
| 不动 harness | PR-64 归 E4，已另派 |
| 不自行开全量矩阵 | 你修完 → 我复验 → E6 重跑 3 道 → 断言过了才开 |

---

## 7. token 与日志留痕（ACTION_RULES §7.5）

若本任务涉及 LLM 实跑验证，落盘按七项齐全（prompt / completion / total / reasoning / cached / llm_calls / 失败重试），汇总须可用脚本复算，report 给出复算命令。

若只是本地改代码 + 跑测试，则不涉及，report 里写明"本轮无 LLM 调用"即可——**不要为了凑格式编数字**。

---

## 8. 交付物

| # | 交付物 | 验收口径 |
|---|---|---|
| 1 | PR-61 四项（三处同步 + 错误码语义 + 断言） | `mw ge 0` 要么返非空、要么报 `UNKNOWN_PROPERTY`；断言能拦住"宣称可用但实际全空" |
| 2 | PR-62（显式路由 + 断言） | 30 道 dh 全部走 open 分支，`_score_open_discovery` 被真实调用 |
| 3 | PR-63（charset） | H1 trace 错误文本直读中文 |
| 4 | PR-65（拒字符串化数组） | 传字符串化数组报 `INVALID_ARGUMENT` |
| 5 | report-08.md | 每条给改前改后实测对比 |

---

## 9. 验收方式

独立实测，不采信自述（PR-59 的教训）。重点复验：

- 自己跑 `mw ge 0`、`logp`、`tpsa`，看返回是否合理
- 自己拿一道 dh 题跑 `score_case`，确认走的是 open 分支
- 故意注入一个"宣称可过滤但无数据"的性质，**断言必须报错**（断言本身要被验证过才算交付）
- 读 H1 trace 的错误字段，看中文是否可读

**修完不要自己开全量矩阵**——交回我，我复验后让 E6 重跑那 3 道。
