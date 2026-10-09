# TEMPLATE_SPEC — benchmark 模板与 case 的强制规范

> **定位**：任何 template 和 case 在投入使用前，必须逐条通过本规范的检查。本规范是唯一的合规判据——所有权威来源（`goal.md`、ADR、ACTION_RULES）中关于模板的要求在此汇总并落地为可检查条目。
>
> **权威顺序的唯一正文在 [`governance/DECISIONS.md`](../../governance/DECISIONS.md) 开头**，本文不重述；
> 项目目标与方法论正文在根目录 `goal.md`（v3.0.1）。
>
> ⚠️ **本文成文时假定的 "GT-first 反查构造法" 已被 2026-08-28 判分范式决议作废**
> （`governance/决议_20260828_判分范式作废与RSI含训练.md`）。本规范**只约束存量 template 与 case**，
> **不得据它造新的"藏标准答案"题**。

---

## 一、一句话总纲

**template = 带槽位的泛化骨架 + checkpoint DAG + reward 算分表达式；case = 只填槽位、不改骨架。**

---

## 二、硬边界（`goal.md` §3，违反即废）

| # | 边界 | 含义 | 检查方式 |
|---|---|---|---|
| 1 | **单 Agent** | 被测对象是一个智能体，非多智能体协同 | 每个 case 只跑一个 agent |
| 2 | **一次性输入** | 任务信息一次给全，执行中无模拟用户对话、无长期记忆检索 | query 是唯一输入，无后续追问 |
| 3 | **hard 中间 checkpoint** | 判定必须是可执行的（数值容差/集合成员/结构解析/约束函数/状态比对），**不用 LLM judge、不用人工主观分** | 每个 cp 的 reward 必须是 Python 可执行表达式，返回 [0,1] |

**唯一例外**：仅当某语义点确实无法规则化时才允许 LLM judge，且必须配 κ 校准（人评一致率）并报告。

---

## 三、template 必须包含的四个必需槽位

| 槽位 | 含义 | 例 |
|---|---|---|
| `target_domain` | 为哪个应用域找候选 | "中枢神经系统"、"常压超导" |
| `property_constraints` | 候选必须满足的约束列表 | `[{property:"bbb", op:"==", threshold:1}]` |
| `target_metric` | 排序与择优的目标量 | `"-{caco2}"`（越大越好） |
| `candidate_space` | 候选来源声明 | `"开放空间（agent 自行检索建池）"` |

**检查**：template.json 的 `slots` 里必须有这四个 key（或等价的语义映射，如 open 类用 `objectives` 替代 `target_metric`、用 `constraints` 替代 `property_constraints`，但必须在 slots 中显式声明映射关系）。

---

## 四、checkpoint DAG 规范

| 规则 | 含义 |
|---|---|
| 每个 cp 有 `id` / `name` / `pre[]` / `next[]` / `weight` / `pass_threshold` / `reward` | pre/next 都可多个（真 DAG，不是链） |
| 每个 cp 必须有 **`verify.type`** | `value_match`（数值容差比对）/ `set_judgment`（集合成员判定）/ `process_metric`（过程指标统计）。**禁用 `keyword_match` 和 `llm_judge`**（除非符合硬边界第 3 条的例外） |
| `reward` 是 Python 运算表达式，返回 [0,1] | 0=没通过、1=完全通过、中间=部分得分 |
| reward 中所有 `{}` 变量必须在 template 的 `vars` 区注册 | 标明来源（response JSON key / tool call 入参出参 / case 常量）和类型，不允许未定义直接用 |
| 分叉的两支之间无边 → 可任意换序 | 等价路径不受罚 |

---

## 五、case 构造规范（GT-first 四步法）

| 步 | 要求 | 检查 |
|---|---|---|
| ① 找分子反查性质 | 从科学数据库选一个已知优秀的分子作 GT 锚，反查其性质作为真值 | case 有 `gt.id` + `gt.properties` + `provenance`（真值来源可复核） |
| ② 构造专属 query | 反向构造自然语言任务，**隐去答案与具体数值**（用 rebuild 模糊描述） | query 中不出现 GT 的真名和精确数值 |
| ③ 模板+DAG | 所有 case 复用同一标准化 DAG，不复制 checkpoint | case 只含 slots 填充值 + query + constraints + gt，不含 checkpoint_dag |
| ④ 终态+路径双验证 | 终态验证（答案对不对）+ 路径验证（过程对不对） | DAG 的 pre/next 依赖关系定义了合法拓扑序 |

**反向构建 4 纪律**（ADR-0005，否则造出垃圾题）：
① query 自然（套模板）② 必须长程（多步，非单值）③ 防记忆泄漏（挑冷门/扰动；原列的“匿名化”手段已随 **ADR-0023** 取消，改靠任务设计——答案不写题面 + 裸答基线核验）④ 答案可发现（能从起点推出，非孤立死记）。

---

## 六、mock 机制（防污染红线）

> **口径更新（ADR-0023，2026-08-15 全库回滚真实态）**：“分子匿名化”已废止——只改元素名/抹名字但保留结构防不住（结构即指纹），且与测结构推理相冲突。现行口径如下表；防污染改靠**任务设计**（答案不写题面、必须多步推理+工具探索）+ **裸答基线核验**。时间差声明：任务设计层全面落地前不对外声称抗记忆污染。

| 规则 | 含义 |
|---|---|
| **实体 ID 句柄** | 库内统一用序号句柄（`z`+6 位数字 / `c`+5 位数字）作为实体键；但**化学身份真实可见**（formula/InChIKey/SMILES 均为真值，ADR-0023）——句柄仅为索引，不再承担防背诵职能 |
| **真值在数据库** | 性质真值存 A 侧离线库（oracle），B 侧不可查；映射表保留作判分反查用途（`_mapping_scoring_only`），agent 不可见 |
| **接口返回值即真值** | 所有性质数据来自数据库查询接口，不质疑其准确率 |
| **执行顺序判定依据真实 tool call 记录** | 不是 agent 主动提交的作业格式，而是 trace 中实际发生的调用序列 |

---

## 七、质量门（每个 case 必须同时满足）

| 门 | 含义 | 检查 |
|---|---|---|
| **必要性** | 金标准轨迹回放必须全过 | 一个完美 agent（查了所有该查的、筛对了、排对了、选对了）应得满分 |
| **区分度** | 负例/空转轨迹不应全过 | 一个什么都不做或乱做的 agent 不应得高分 |
| **防泄漏** | 实例内容指纹防变体泄漏 | 同一 case 的不同实例不能仅靠改名字就通过 |
| **诚实性** | 无解题 agent 应声明无解 | 当确无满足全部约束的候选时，agent 必须输出 `NO_SOLUTION` 而非瞎编 |

---

## 八、两类模板的差异

| | fixed_discovery | open_discovery |
|---|---|---|
| 判优参照 | GT 锚的性质（越接近越好） | objectives（xxx 越高/越低越好） |
| slots 特有 | `gt`（id + properties + rebuild） | `objectives`（property/direction/weight）+ `constraints` |
| 正解 | 唯一 GT 实体 | 可行集中 objective_score 最优者（可不唯一） |
| cp6 判定 | 命中 gt.id = 1.0 | `int(constraints_ok) × objective_score` |
| 适用场景 | 有确定答案的发现题（"找到那个满足条件的分子"） | 开放优化题（"找到 Tc 最高的常压超导体"） |

---

## 九、当前缺口（2026-07-31 审查发现）

| # | 缺口 | 影响 | 状态 |
|---|---|---|---|
| 1 | 两个 template 的 slots 均缺 `candidate_space` | 四槽位不全 | 待补 |
| 2 | fixed template 的 slots 未注册 `target_metric` | 四槽位不全（open 有 objectives 等价物，fixed 没有） | 待补 |
| 3 | 全部 12 个 checkpoint 缺 `verify.type` | 违反 DAG 规范 | 待补 |
| 4 | antibiotic case 仅 1 条约束，区分度不足 | agent 不需筛选就能"对"，测不出过程 | 待加强 |

---

## 十、红线（`goal.md` §5，命中即偏离，立即叫停）

1. 不出"不带 checkpoint、只看终答"的退化版本
2. 不拿通用 agent 任务（SWE/网页/办公）凑数
3. 不用 LLM-as-judge 凭知识硬判、不要 GT
4. 不跳过调研直接造题
5. 不用训练分布内的热门分子当答案而不做防污染处理
