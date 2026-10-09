# E5 Report

---

## 监工验收（E5 药物侧 open 批，2026-08-07 实跑）

**结论：药物侧 15 道通过。** 材料侧 15 道待 E1-05 扩数据，符合预期。

### 实测

| 项 | 实测 |
|---|---|
| open case 数 | **15 道**（药物）+ **spare 冗余 3 道**（`spare_dh_drug_01~03.json`） |
| 梯度参数 | n∈{3,4,5}、d∈{1,2}、K=25 恒定；6 种 (n,d,K) 组合，**未用单一配置** |
| 优化方向 | minimize 6 + maximize 9，两个方向都造 |
| 判废 | 投造 22 → 过门 15、判废 7（**31.8%**，与 30% 先验一致）；另在选格期确定性预筛剔除 1018 个必废配置且**不消耗配额** |
| 30 道 fixed 的 lint | **退出码 0，零违规** |
| open 的 lint | 退出码 1，**唯一违规是 M13**：`drug=15, material=0` 领域不均——**这是材料侧未开工的必然结果，不是缺陷** |

### 三处主动上报，逐条回应

**① 致命问题自查并自修（`constraints` 引用后端不可查的性质键）**

你发现 `constraints`/`gt.properties` 里引用了 `atom_count`（药物）/`element_count`（材料），而后端 `get_property` 查不到这两个键——**这意味着题面给的约束 agent 无法用工具验证**。按方向 B 把结构判据移入 `domain_knowledge` 并重生成 30 道，lint 退出码 0。

**这个问题我在验收 task-02 时没抓出来**：我核了"字段名与判分器一致"，但没核"约束引用的性质键后端是否真能查"。这两件是不同的检查。

**② 方向 A（把 `atom_count`/`element_count` 注册为正式可查性质）——批准**

理由：
- `atom_count` 已经实际存在于 A 侧实体记录里（实测 `atom_count: 44`），只是没注册进可查性质清单——**属工具能力缺失，不是考点设计**；
- `goal.md` §6 步 3 的下限口径是「fixed 类＝每个 checkpoint 至少有一个可落地 tool 支撑其产物」，约束里有结构条件却无对应查询工具，这条不满足。

**但已交付的 30 道保持现状、不重生成**：方向 B 是有效解（判据在 dk 里，专家审题看得到），而重生成会打乱 E6 已落盘的采样。**方向 A 用于后续放量批次。**

归属：E1 补 `get_property_A.jsonl` 的 prop 行与 `dag_poc/properties/*.json` 清单键、E3 同步 gateway 白名单。

**③ `answer_schema.json` 的 operator 枚举与 ADR-0018 不一致——归 E3 改，这是我的连带影响没走完**

现象：`answer_schema.json` 的 operator 枚举是**结构算子五件套**（add_atom / remove_atom / replace_substituent / substitute_element / ring_closure），而 ADR-0018 §5 已把 open 的 `edit_op` 改为**组成算子** `{+n 个元素 e, −n 个元素 e}`。

**裁定：改 `answer_schema.json` 的 operator 枚举为组成算子。** 理由：那是 agent 要填的字段，而 agent 只看得到组成（A 侧无拓扑），填结构算子它无从判断。

**但 E3 在 task-03 做通的 dummy atom 结构规范化能力不动** —— B 侧成员存的是完整匿名拓扑，身份键比对仍要用它。**改的是 agent 可见的答卷字段枚举，不是判分侧的比对能力。**

### 两项待办（均因我的裁定晚于你的交付）

`b_partition` 的 per-case 注册（ADR-0018 R1-①）与 fixed case 的 `|S_feasible|` 落盘（R1-③）当前都未做。**时间线核对：我裁定 R1 是 15:47，你的 case 文件 14:49、progress 15:37，都在裁定之前**——不计为遗漏。

而且你在 progress 的「冻结前必做清单」第 ④ 条已自行写明「注册进 `b_partition.json` 的 cases（否则 E6 的 hit_gb 对本批惰性，裸答基线查不出命中）」，**与 E6 报的缺口、与我的裁定完全同一判断**。按你的清单执行即可。

