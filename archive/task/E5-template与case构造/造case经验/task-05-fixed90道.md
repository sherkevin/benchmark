# 造 case 经验 · task-05 fixed 90 道缺口 · 药物侧批次（45 道）+ 材料侧批次（45 道，见 §9）

| 字段 | 值 |
|---|---|
| 批次 | E5 task-05：fixed 缺口批次药物侧，净增 45 道（类型1 +22 + 类型2 +23；药物侧累计 30→75，含 task-02 存量 15 道） |
| 落盘位置 | `dag_poc/templates/constraint_satisfaction/case_cs_drug_09~30.json`（22 道）、`dag_poc/templates/constrained_optimization/case_co_drug_08~30.json`（23 道） |
| 造题脚本 | `src/case_build/build_drug_cases_task05.py`（沿用 task-02 链路，口径升级见 §2）；共用基建 `common.py` / `shared_text.py` |
| 账本 | `src/case_build/task05_drug_ledger.json`（含 run_history：首跑中断实录） |
| 日期 | 2026-08-08 |
| 姊妹批 | 材料侧 45 道 = Task #26（已完成，见 §9；同一套基建，`best_is_unique(members, prop, direction)` 与 opener 全池轮换为两批共用） |

---

## 1. 产出与验收数字

- 投造 45 / 过门 45 / 判废 0（账本 discards 空；选格期剔除的 5 个 round(logS) 歧义格 [(2,-3),(5,-3),(6,-5),(7,-5),(8,-3)] 记入 ambiguity_cells_dropped，属预检留痕非判废）。
- 逐道 lint：45 道每道落盘后立即 `python3 dag_poc/query_lint.py --cases <单题路径> --strict --json`，**violations==0 全数通过**（不看退出码，口径见 §5）。
- 全语料复核（131 case，`dag_poc/templates/*/case_*.json`）：新 45 道相关违规 **0**；G1 无跨批撞格、M9b 无 gt 复用（75 个药物 gt 互异，与存量 30 道 gt 零交集）。其余 352 条违规全来自 legacy 示例 case（case_co_cns_drug 等，E5 边界外已知项）；corpus 级恰 1 条 M9（gt='aa' 出现 2 次）系 legacy 示例既有问题，与新批无关。
- M9a 开头两词实测（药物侧 75 道）：type1 21 种、type2 19 种，均 ≥ 120 道规模下的 18 种门槛（VARIATION_FLOOR=0.3，仅 --freeze-batch 判；本批未跑 freeze，监工统一安排）。
- used_cells 最终规模 75（三元组：存量 30 + 新 45），used_gt 75。
- 类型1 多成员格占比：22 道中 18 道为多成员格（|G| 最大 53），唯一性全部由穷举证明锁定（§3）。

## 2. 共享基建三处改动（Task #25 第一步，材料批直接复用）

1. **`common.py` QUERYABLE_PROPS 扩容**：加入 `atom_count` 与 `element_count`，并把旧注释"不可查询只能塞 domain_knowledge"更正为 E1-05 §6 已注册口径（get_property_A.jsonl 实测 atom_count 74,941 行、element_count 17,252 行，BackendReader 可查）。脚本内加硬断言：候选池每个分子的 atom_count 性质行值 == 实体字段值（回迁前提，实测全库一致）。
2. **`common.py` best_is_unique(members, prop=None, direction="max")**：扩签名支持 max/min 方向与 dict 形态成员；旧单参调用（build_drug_cases.py / build_material_cases.py 三处）向后兼容。
3. **`shared_text.py` opener 池扩容 + 全池轮换**：TYPE1_OPENERS 16→21、TYPE2_OPENERS 14→20；新增句式领域中性、开头两词与既有池两两不同（脚本实测核验）；type2 原"drug 前 7 / material 后 7 不回绕"分用策略弃用，改两域共用一池按 opener_idx 续接轮换（46 道新增下分用策略必然回绕）。本批续接点：type1 索引 16~37、type2 索引 14~36、provenance seq 从 15 起。

## 3. 类型1 口径切换（红线项，task-05 任务书 §3）

- 旧口径 `len(m)==1`（候选池只有一个成员）**弃用**；正解语义 = 格内目标量最优者唯一（`best_is_unique`），与成员数无关。依据：任务书 §3 与 query_spec.json capacity_basis 白纸黑字；容量实证：沿用旧口径材料侧类型1 只剩 5 格、撑不起 22 道。
- 多成员格类型1 的答案唯一性由 `common.exhaustive_note` 穷举证明锁定（落盘 solvability_verified：完整过滤口径 + 全部成员全精度目标量 + 最优者唯一断言 + "|G|=1 语义=最优者唯一"明示），证明只进 judge 侧字段、不进题面（防 N7/N8 泄漏）。
- 只改新批路径：`build_drug_cases.py:86-87` 过滤条件同步更正并加弃用注释（该脚本不得重跑，重跑会覆盖已交付 30 道）；**已交付 30 道文件一个字节未动**（git diff 确认：旧 30 道的唯一改动是并行代理 Chris Task #27 的 grid_key 回填，纯增量 +135 行）。

## 4. 结构判据回迁（新批 vs 存量 30 道的核心差异）

- 新 45 道：atom_count 窗口写进 constraints（`{"property":"atom_count","op":">=","<=", ...}`，E1-05 §6 注册后 get_property 可查）；domain_knowledge 不再承担结构判据职能，仅保留 logS/atom_count 的定义性描述与 scope 句。constraints 四件套 = logS 上下界 + atom_count 上下界。
- 存量 30 道：保持方向 B 原样（atom_count 在 dk 文字判据），不动——E6 已在其上跑采样。
- s_feasible.criteria 随之变化：新批 constraints 4 条（op 归一化 ge/le）、无 structural 键；存量批 criteria 带 structural。两批差异 report 需说明（本文件即依据）。
- E6 读取器契约：顶层 `s_feasible_size` 整数 == s_feasible.size == len(members)，脚本写盘断言；新批 s_feasible.size = 格成员数（clean 格保证全库过滤与切格成员集合一致，逐格断言）。

## 5. lint 自检口径（task-02 已批，逐道执行）

- 以 `--strict --json` 的 `summary.violations_total == 0` 为准，**不看退出码**：corpus 级 M9/A1 pending 恒定存在，退出码 0 结构性不可达（E5-01 已登记）。
- glob 坑复提醒：`constraint*` 匹配不到 `constrained_optimization` 目录；本批逐道自检直接传单题绝对路径，整批复核用 `dag_poc/templates/*/case_*.json`。
- 未跑 --freeze-batch（交付冻结由监工统一安排）。

## 6. 跨题查重双保险与首跑实录（最大的坑）

- 机制：`used_cells = {(template_type, target_domain, grid_key)}` 初始集 = 已有 30 道重算格（药物 [atom_count//5, round(logS)]、材料 [family, tc_band, element_count]，与 backfill_grid_key.py 同公式；文件已有 grid_key 则交叉核验逐位一致，30/30 通过）；used_gt 初始 30 个，M9b 保 gt 全库唯一。
- **坑（首跑实证，账本 run_history 留痕）**：used_cells 按三元组排除时，旧 co 题占用的格子在类型1 池里不被排除——同一格内 logS max 最优者与模板类型无关，类型1 新题选中该格后最优者正是旧 co 的 gt，落 3 道后断言中止（gt 复用 z002880）。修复：选格排除改为按 **(domain, grid) 跨类型**（occupied_grids），G1 三元组仍按 template_type 记账。3 道半成品删除后重跑，45 道一次全过。
- 给材料批的推论：同一格的最优者（tc max）同样与模板类型无关，材料批选格必须同样按 (domain, grid) 跨类型排除。

## 7. 容量实证（本批选格前探针）

- 候选池 2,299（logS 非空分子）；非空格 155、干净格 150（剔 5 歧义格）。
- 口径切换后：类型1 可用池 = clean ∧ 最优唯一 ∧ 未占用 = 135（150 − 存量 15 占格，跨类型排除）；类型2 可用池 = 再叠 成员≥3 ∧ 与本批 cs 格互斥，取 23 后仍余 30+。22+23 均充足，未降任何标准。

## 8. 遗留事项

- E2 grid_key 契约（query_spec.json grid_key_contract v1.0）已提交：本批 45 道 grid_key 逐位按契约落盘（两位 int，bool/str 拒收语义由 lint G1 兜底），与 Chris Task #27 回填的存量 30 道同契约，全语料 G1 零违规。
- 材料侧 45 道（Task #26）已完成，见 §9；注意 §6 的跨类型排除教训与 element_count 回迁（材料结构判据，17,252 行已注册）均已落实。
- legacy 示例 case 的既有违规（材料批复核后 353 条）与 corpus 级 gt='aa' M9 条，归 E5 边界外已知项，未处理。

---

## 9. 材料侧批次（Task #26，2026-08-08，45 道）

| 字段 | 值 |
|---|---|
| 产出 | `case_cs_material_09~30`（22 道）+ `case_co_material_08~30`（23 道）；fixed 累计 120 道达标 |
| 脚本/账本 | `src/case_build/build_material_cases_task05.py` / `task05_material_ledger.json`（含首跑撞车实录 run_history） |
| 数字 | 投造 45 / 过门 45 / 判废 0；逐道 lint violations==0；类型1 多成员格 20/22（|G| 最大 95，case_cs_material_26；705 为类型2 最大格）；终态 used_cells=120、used_gt=124 |
| 续接 | type1 opener 38~59、type2 37~59、provenance seq 30~74（药物批已占至 37/36/29） |

### 9.1 element_count 口径锚定注册行（本批最大决策点）

- 背景：E1 注册的 element_count 性质行（17,252 行）= **互异 [N*] label 数**（`build_property_counts.element_count_of` 用 set）；而 E5 `common.element_count` = token 计数（findall 不去重）。实测全库 143 个晶体同 label 多次出现（如 `[65*]0.5[65*]4`），两口径分叉。
- 决策：新批切格/约束/穷举一律取**注册行值**——agent 经 get_property 查得的是注册值，判分合格集必须与之一致；若用 token 口径切格，题面约束与后端真值错位，穷举证明即失真。
- 存量影响核验：旧 15 道材料 case 的 s_feasible.members 与 gt 实测均不含 143 个分叉实体 → 存量零影响；另加逐道实体级断言：新题 s_feasible.members 与旧 15 道成员集求交必空（防新旧口径造成隐形同集）。
- dk 描述同步改口径：新批写 "distinct element species, i.e. distinct [N*] labels"（存量 dk 的 "distinct tokens" 表述不动）。

### 9.2 首跑撞车坑：used_gt 必须含 open 批 gt（新教训，补 §6）

- 实证：首跑 45 道落盘后 corpus lint 查出 M9：`case_co_material_12` gt=c04551 与材料 open 批 `case_dh_material_02` 撞车。根因：used_gt 初始集只扫了 75 道 fixed，而 **M9 corpus 查重跨 fixed/open**（open 类无切格不入 used_cells，但 gt 级必须排除）。
- 修复：① used_gt 并入 `discovery_haystack/case_dh_material_01~04` 的 gt；② 选格池增 `gt_free` 预筛——格内 tc 最优者即该格新题 gt，若已在 used_gt 则整格剔除（否则落盘期断言中止）；③ 重跑覆盖，45 道一次全过。证据：`src/case_build/_evidence_m9_corpus_20260808.json`。
- 推广结论：**任何新批的 used_gt 初始集 = 全语料已有 gt（fixed + open + spare），不只是同模板类型**。

### 9.3 family=='未知' 不入选题池

- E1-05 UCI 扩容 11,481 条 family='未知'；语义空洞且 task-04 既有判例不使其入题面约束。选题池剔 38 个未知格（账本 family_unknown_cells=38，pool_exclusions 留痕，非判废）；终态类型1 池 53/类型2 池 47（终态口径，含材料 open 4 道 gt 排除），对需求 22/23 均富余。

### 9.4 选格策略与容量

- 沿用 task-02 材料 diverse_pick（家族多样性轮转、每家族取最小合格格、FAMILY_PRIORITY 序）；池层预筛（最优唯一/未占用/gt_free/成员数下限）后轮转。
- 实测容量（选格前探针）：非空格 111；剔未知后终态类型1 可用 53、类型2 可用 47（终态口径，含材料 open 4 道 gt 排除；co ⊆ cs 池，cs 先选 22 后 co 仍余 25 ≥ 23）；与 Sam 核算（88/71，含未知格）方向一致，未降任何标准。
