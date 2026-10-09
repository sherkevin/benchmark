# case_dr_aedit_01 · fixed · some_tools（本题锚定档）

**A-Edit 原子题：结构微改 → logS 方向预判。owner 2026-08-26 走查首组，挂在
「先导化合物优化」长程链的核心迭代节点上（ACTION_COVERAGE §3.8 登记，闸0~闸2
实跑依据见该节）。**

## 题目

一个 run = 20 道独立子题（q01..q20，12 单改 + 8 双改）。每题给母分子 SMILES +
实测 logS + 子分子 SMILES + 修改描述，答子分子 logS 升还是降（higher/lower）。
**子分子全是库外新分子**（InChIKey 断言不在 74,245 库键集内），查不到，
唯一活路是从库内证据对子归纳出规律再应用。

真值：单改 = 全库实测拟合的六条规律方向；双改 = 两条规律拟合 Δ 相加取符号，
且只取安全区白名单内的组合（全库 532 对实测，|拟合Δ和|≥0.6 档方向一致率
94~100%，`tools/gate2_a_edit_additivity_scan.py`）。题集生成与全部自检见
`tools/build_a_edit_prototype_v2.py`（种子 20260826）→ `task/a_edit_walkthrough/prototype_v2.json`；
case 四件套由 `tools/build_a_edit_case.py` 确定性转写（零 API，含出库/平衡/符号断言）。

## 三档与判别口径（owner 2026-08-26 二轮裁定，DECISIONS §2.6⑥）

| 档 | 工具面 | 角色 |
|---|---|---|
| basic | 无（裸答） | **不立正式 case**：已完成裸答探针留痕 `task/a_edit_walkthrough/naked_probe_v2.txt`（qwen/glm 多数表决满分、deepseek 95%——教科书规律强模型本来就会，**饱和是设计前提不是缺陷**） |
| some（本格） | 库查询三件套（`search_candidates` / `get_entity` / `get_property`），证据视图 469 | **主区分档**：测"会不会用 ≤50 次调用挖出证据、归纳规律再应用" |
| enough | some + `predict_admet`（恒给对值） | 对照档：验证信息充足可解、给工具收益上界 |

**判别 = 档差 S(some) − S(basic)，不是单档绝对分。** preflight 闸门6 的
「裸答 ≥0.9 ⇒ 降级」对本题不适用（判分器声明 `TIER_DELTA_DISCRIMINATION`，
preflight 据此降为 WARN）；撒网 / 零工具两关仍是硬判。

## 反穷举（结构性，不靠判分补）

视图 = 六规则证据分子并集 455 + 20 母分子 = **469**（`tools/build_a_edit_case.py`
扫描实测）；网关全局调用上限 **50**。翻页列全视图需 ~10 次、逐条查性质需 455 次
≫ 50——且 20 题答案根本不在库里，穷举结构性不可达。

## 工具面实况（包装期复核更正）

视图内实体经 `get_entity` 返回的是**分子式 + atom_count + inchikey，不含 SMILES**
（早前走查记录"masked_structure 透传真实式"以本格实测为准更正）。证据挖掘路径因此是
**分子式差配对**：六条规律在分子式差上坍缩成三类（−H+Cl、±C1H2、−CH+N），
每类方向一致、恰好覆盖题集全部考点——题面给修改描述，agent 要挖的是"这类
编辑在数据里系统性地把 logS 推向哪边"。`search_candidates` 的 keywords 只匹配
实体 ID 子串（化学上无用），constraints 可按 logS 等 12 项真值性质过滤。

## 判分（judge.py，唯一权威；enough 档薄壳转发）

`total = cp_all × grounding 门`

- `cp_all` = 20 题判对率（每题离散二值，**零超参**）——唯一计分关；
- `cp_single` / `cp_double` = 单/双改诊断（`NOT_IN_MEAN`：含芳N子集真值全 higher，
  常数答案可打满，**任何子集拆分永不作判别关**）；
- `grounding` = 门：账本须含 ≥1 次成功领域调用（owner 裁定：查过的不必在答案里
  附证据 id，门直接读账本/trace）；零工具 ⇒ 总分归零。

基线（闸门6 实测）：撒网 0.500（全报 higher + 合成账本）/ 零工具 0.000（门拦截）/
裸答 cp_all 1.000（探针留痕重判，档差判别的 S(basic) 对照）。

## 运行

    python src/probe_headless/ws_runner_aedit01_some.py <model> [round]
    python src/probe_headless/sweep_ws.py ws_runner_aedit01_some.py <轮次> r --probe

题面由 `src/probe_headless/aedit01_task.py` 统一生成（两档同一题面，防漂移毁档差归因）；
MCP 服务 `ws_mcp_server_aedit01_some.py`（视图缺载即拒绝启动，不回落全库）。

## 视图断言适配登记（src/data_build/assert_per_case_view.py，2026-08-26 实测）

本题视图是全库**首个 fixed 类视图**；断言集中 A1/A2/A3/A5/A7 全部 ✓（含 469
全量 sweep 与匿名红线全零），唯二 ✗ 为 open 类外推题的专属语义，对本题不适用：

- `A4.negative_gt_absent`：假设 `gt.composition` 是匿名元素组成多重集（可按组成
  扫查构造"BEFORE 全库可触达 gt"的负例）。本题的 `gt.composition` 是**答案多重集**
  `{"higher":10,"lower":10}`，不是化学组成——BEFORE 扫查恒空，负例口径不成立。
- `A6.quality_gate_empty`：为"在 A 内找不到不劣于 gt 单值的解"的外推题设计，
  需 `gt.entity_ref` 指向单实体取单值作阈值。本题答案是 20 个方向判断、
  `entity_ref=None` 无单值，阈值结构性不可构造。

这两条的实质保证已由别处覆盖：子分子不在库（builder 自检 + A1）、可见集过滤
与匿名（A2/A3/A5/A7 全 ✓）。**断言脚本未改**——它是 E5 open 类共享工具，为其
新增 fixed 类分支属语义变更，留待 owner 裁定；登记于此，防后人把这两条 ✗
误读成泄漏或质量问题。
