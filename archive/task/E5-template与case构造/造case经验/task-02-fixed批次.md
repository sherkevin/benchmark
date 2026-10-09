# 造 case 经验 · task-02 fixed 批次（30 道）

| 字段 | 值 |
|---|---|
| 批次 | E5 task-02：fixed 类进场批次，30 道（药物 15 + 材料 15；每域类型1 八道 + 类型2 七道） |
| 落盘位置 | `dag_poc/templates/constraint_satisfaction/case_cs_{drug,material}_0{1..8}.json`、`dag_poc/templates/constrained_optimization/case_co_{drug,material}_0{1..7}.json` |
| 造题脚本 | `src/case_build/{common.py, shared_text.py, build_drug_cases.py, build_material_cases.py}` |
| 日期 | 2026-08-07 |

---

## 1. 切分参数的实际效果

### 1.1 药物侧：`(atom_count // 5, round(logS))`

- 进场文档口径：155 非空格 / 类型1 可用 153 格 / 类型2 可用 92 格（监工 §7 修订后数字）。
- 实际造题时的可用格与选格策略：
  - **type1（约束满足）取格标准**：脚本过滤 `len(m) == 1`，**8 道全部单成员格**（|G|=1 最严语义：合格集恰好 1 个成员，答案平凡唯一）。"约束过滤"解题路径（多成员格靠 dk/constraints 筛出唯一解）本批**未覆盖**——放量时如需覆盖，必须改过滤条件并在题面给出甄别准则，否则多成员格会破坏答案唯一性（task-05 评审正是因此打回材料侧 3 道多成员格）。
  - **type2（约束最优）取格标准**：格内目标量（logS）最优者必须**不并列**（最优唯一才保证 |G|=1 语义）。从 92 个可用格中挑了 7 格，选格时逐格穷举验证最大值唯一。
  - **gt 不重复**：M9b 要求同一 gt 全库出现 ≤1 次，选格时额外保证 30 道题的 gt 两两不同（药物 15 个 gt 互异）。
- 实测坑：`round(logS)` 在 .5 边界上是银行家舍入（Python round），格子归属有歧义。解法见 §2.4。
- **task-05 补充**：constraints 只保留 get_property 可查性质（logS 窗口）；atom_count 是 get_entity 实体字段、不可经性质接口查询，移入 domain_knowledge 写成精确文字判据（详见 §6）。

### 1.2 材料侧：`family × tc 五档 × 元素种类数`

- tc 五档：`<10 / 10–30 / 30–60 / 60–100 / ≥100`；元素种类数由 `formula` 正则提取（如 `Ag0.02Ge2Pd1.98Sr1` → 4 种）。
- 进场口径：73 非空格 / 类型1 可用 69 格 / 类型2 可用 59 格。
- 实际选格策略与药物侧相同（type1 过滤 `len(m)==1`；**task-05 前材料侧 type1 曾误选 3 道多成员格导致答案不唯一，返工后与药物侧对齐，69 个可用格中单成员格充足**），外加两条硬断言（写进 `build_material_cases.py`，写盘前逐条执行）：
  1. **排除 `tc == 0`**（3DSC 中占 30.6%，属非超导样本）——断言合格集中所有成员 `tc > 0`；
  2. **题面禁现 "常压 / ambient / ambient pressure"**（condition 仅 16/5771 条有标注）——题面一律用 `in the candidate database` 限定范围，脚本对生成文本做字符串断言。
- `|G|=1` 语义提醒（最容易搞错）：指"满足约束的候选中目标量最优者唯一"，**不是**候选池只有一个成员。按错误理解会把材料侧容量从 69 格误算成 10 格。每个 case 的 `solvability_verified` 字段落盘了该格全部成员 + 目标量 + 最优唯一的穷举证明。

---

## 2. lint 主要卡点与最难满足的规则

### 2.1 最难：`--strict` 退出码 0 字面不可达（lint 固有行为，已报 E5-01）

- 现象：任何一批 case 在 `--strict` 下退出码恒为 2。
- 根因（均在 `dag_poc/query_lint.py`，E5 无权改，已报监工）：
  - L777–784：`check_a_side_identity` 全绿分支也无条件 `append pending`（A1 扫描留痕），无任何方式规避；
  - L927：`exit_code = 1 if violations else (2 if (strict and pendings) else 0)`——pendings 非空即 2。
- 处置：监工 2026-08-07 批准达标线为"**违规 0 条 + pending 仅 A1 信息性留痕（退出码 2）**"。实测本批达标：非 strict 退出码 0；strict + `--freeze-batch e5_task02` 违规 0、pending 仅 A1 一条、退出码 2。
- 给下一批的经验：**不要为退出码 0 返工**，按达标线验收；新批次同样用 `--freeze-batch <批次名>` 把 M9 变体率从 pending 转为实判。

### 2.2 M7：旧字段名陷阱（题面不得点名派生字段）

- `screened_pool` / `ranked_candidates` 已降为派生字段，题面点名即违规（现有 37 个 legacy case 在此有 41 条违规）。
- 处置：题面交付段只写 `candidates, ranking, answer` 三个字段（D 段固定句 `Submit three fields: candidates, ranking, and answer.`），脚本统一拼装，从源头杜绝。

### 2.3 M9a：开头两词变体率 ≥ 0.3（不能一个句式套 30 题）

- 规则：开头两词模式种类 ÷ 该类题数 ≥ 0.3，且 M9b 要求同一 gt 全库出现 ≤1 次。
- 实测结果（task-05 重生成后）：type1 变体率 **16/16 = 1.000**，type2 变体率 **13/14 = 0.929**，均远超 0.3；gt 全库唯一通过。
- 实现：`shared_text.py` 内置轮换表——type1 备 **16 种动词开头**（drug 用前 8、material 用后 8，全部互不相同），type2 备 **14 种介词短语开头**（各域按序取 7，不回绕；task-05 从 10 条扩到 14 条——原 10 条导致材料侧回绕复用药物侧开头）。下一批放量前需先扩表：放量 90 道 fixed 时每类需 ≥ ceil(题数×0.3) 种开头两词。

### 2.4 边界歧义：`round(logS)` 的 .5 舍入

- 格子边界（如 logS = -9.5 归入 round=-10 还是 -9）在脚本与 lint 之间若口径不一致会产生跨格泄漏。
- 解法演进：最初在落盘循环内逐格断言集合等价（round 分组 vs 约束区间成员），不等即断言中断——但这会让**半成品已落盘**才报错。**task-05 改为选格期预检**：歧义格在选格前整体剔除并打印留痕（本批实测剔除 5 个），落盘循环内不再有该断言失败路径。

### 2.5 宽 glob 撞 legacy case（M4/M7/M10 违规全来自旧题）

- 用 `dag_poc/templates/con*/**` 之类宽 glob 跑 lint 会撞上 4 个 legacy case，产生 M4/M7/M10 违规——这些违规全部来自旧题，与新 case 无关。
- 解法：新 case 一律带**编号后缀**（`case_cs_drug_01` 形态），lint 用**精确 glob** `dag_poc/templates/con*/case_c?_*_??.json` 圈定本批 30 道。legacy case 迁移属边界外，未处理（见 report 遗留问题）。

---

## 3. 脚本批量 vs 手工的取舍结论

**本批结论：骨架批量 + 题面措辞人工轮换。**

- **骨架批量**（`common.py` + 两个 build 脚本）：格子切分、穷举可解性证明、tc>0 / ambient / 非空三条硬断言、provenance 绑定、写盘前 lint 镜像自检——这些全部脚本化，30 道一次跑通，零结构性返工。事实证明批量脚本是可行的默认方式（task.md §4 优先级 1 成立）。
- **人工介入点**：题面措辞的轮换不能完全交给模板拼装——M9a 要求开头两词多样化，纯模板循环会产生机械感。做法是脚本从轮换表（16 + 14 种人工撰写的开头）按序取用，**人工负责轮换表本身的措辞质量与领域适配**（如材料侧 objective 表述与药物侧不同），脚本负责拼装与断言。
- 给下一批的推论：放量 90 道时骨架脚本可直接复用，主要人工成本在**扩句式轮换表**与**provenance 文献池扩容**（现池 10 篇：药物 6 + 材料 4；30 道循环引用尚可，120 道需扩池以免背书同质化）。

---

## 4. "库变即重验"实战记录

- **事件**：造题会话期间，后端库 `get_property_A.jsonl`（27,606 行）被外部更新过（快照变动）。
- **处置**：按 task.md §7 坑 2 的纪律（"库一变，所有 case 的可解性都要重验"），对 30 道全部重新穷举：独立验证方基于**当前库**重算可解性，**30/30 通过**（每 case 的格成员、目标量、最优唯一性均与落盘证明一致或按新库重落证明）。
- 经验：
  1. `solvability_verified` 字段必须带日期与库快照描述（行数），重验时可比对；
  2. 重验脚本独立于造题脚本（验证方不复用构造方代码），本批由独立验证方基于当前库重算；
  3. 放量批次建议在造题前**冻结并记录库快照行数/hash**，造题期间监控文件变动，变动即触发全量重验，不要攒到最后。

---

## 5. provenance 文献池（ADR-0016，30/30 真实论文）

- 药物侧 6 篇：ESOL（doi:10.1021/ci034243x）、AqSolDB（doi:10.1038/s41597-019-0151-1）、Yalkowsky（doi:10.1002/jps.2600690814）、Huuskonen（doi:10.1021/ci9901338）、TDC（arXiv:2102.09548）、Avdeef Wiki-pS0（doi:10.5599/admet.766）。
- 材料侧 4 篇：Stanev 2018（doi:10.1038/s41524-018-0085-8）、3DSC 2023（doi:10.1038/s41597-023-02721-y）、Hamidieh 2018（doi:10.1016/j.commatsci.2018.07.052）、Yu 2023（doi:10.1021/acsomega.2c06324）。
- 全部经联网核实；独立验证方抽查 6 篇全部真实。

---

## 6. task-05 返工记录：致命问题与唯一性缺口（2026-08-07）

三维独立评审打回两类问题，全部在 E5 边界内修复并全量重生成 30 道：

1. **致命：constraints/gt.properties 引用后端不可查性质键**——药物 15 道含 `atom_count`（get_entity 实体字段）、材料 15 道含 `element_count`（formula 互异 [N*] 计数派生），均不在 `get_property_A.jsonl`/性质清单/gateway 白名单，agent 调 get_property 会得 UNKNOWN_PROPERTY。**采用方向 B 修复**：constraints 只留可查性质（药物 logS 窗口；材料 family + tc 档），结构判据移入 domain_knowledge 写精确文字判据，gt.properties 删除结构键；穷举证明明示完整可行性过滤口径 = constraints 可查约束 ∧ dk 结构判据。备选方向 A（把两键注册为正式可查性质）涉 backend，归 E1/E3，已登记 progress.md E5-02。
2. **唯一性缺口：材料 type1 三道多成员格**（合格集 2/2/3 成员但题面无甄别准则）——选格加 `len(m)==1` 过滤与药物侧对齐，重选后 8 道全单成员格、家族多样性保持。
3. **脚本加固 3 项**：round(logS) 歧义格改选格期预检剔除（不落盘后中断）；穷举证明目标量改全精度 %.6g（原两位小数截断致显示并列、judge 无法复算唯一性）；TYPE2_OPENERS 从 10 扩到 14 条互异开头（原材料侧回绕复用药物开头）。
4. **教训**：造题前必须核对性质可查性三源（properties/*.json 清单、get_property_A.jsonl 实际 prop、gateway 白名单），实体字段/派生量不是性质；type1 无 objectives 时多成员格 = 答案不唯一，默认必须单成员格除非题面给出甄别准则。
