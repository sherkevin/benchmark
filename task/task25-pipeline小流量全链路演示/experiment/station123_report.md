# task25 站1–3 报告（选题采样 + 题面构造 + 后端视图）

> 2026-09-17。全部产物落 `task/task25-pipeline小流量全链路演示/experiment/`；
> 零 API、零下载、零机时；冻结输入只读未动；`git status` 仅含本任务落点。
> 三个脚本重跑字节一致（md5 相同，实跑核验）；json 一律 `sort_keys=True`、无时间戳、无随机。

## 1. 产物清单（生成脚本 → 落盘）

| 站 | 脚本（experiment/scripts/） | 产物 | md5 前8位 |
|---|---|---|---|
| 1 | `station1_sample.py` | `tw_distribution.json` | `9ced9768` |
| 1 | 同上 | `sample.json` | `b934c039` |
| 2 | `station2_build_cases.py` | `cases/case_bbb_demo_<eid>/{task.md, answer_schema.json, ground_truth.json, tools.json}` ×6 | 见 §5 |
| 3 | `station3_build_views.py` | `cases/case_bbb_demo_<eid>/view.json` ×6 + `view_selfcheck.json` | 见 §5 / `9d89326c` |

站2 额外产出 `tools.json`（配套工具的机器可读目录）：题面文本与 mock server 工具实现
由同一脚本同一次生成，防手写漂移（BENCH_SPEC 闸3「完整题面 = 题面文本 + 配套工具」；
answer_schema「schema 只写一遍」同一纪律）。站6 直接消费它，不必从题面反解析。

## 2. tw 口径核实（主会话 2026-09-17 裁定项：必须与 ADR-0007 的 56 一致）

**结论：56 口径已精确复现，采样落在该口径上，未触发停机。**

- 权威算法：tw = 7 个家族列中「票 ≠ 实验真值」的族数（空票不计错不计分母），
  加载器 = `src/probe_headless/bbb_variance_decomposition.py` 的
  `load_truth/load_votes/load_excluded`（task21 D2-① 与 task23 裸答交叉复用，
  与 α_i 同源同口径）。
- **群体 = usable-722** = 730 ledger 真值分子 − `truth_unresolved_bbb.json` 排除的
  8 个「真值源行自相矛盾」分子（如 aspirin 两行 label 0/1）。实跑：usable-722 上
  tw≥4 = **56（19 阳 + 37 阴）**，与 ADR-0007 R3.3 台账数逐位一致。
- 59 的来历：all-730 直算得 59，差集恰 3 个 = `z007002`(tw6) / `z015731`(tw4) /
  `z034653`(tw6)，**全部在被排除的 8 个里**。与 `tied_families_no_vote` 列无关
  （差集中仅 z015731 有平票家族；平票→空票在两种口径下同样跳过）。
  ⇒ 59 口径错在未做 truth_unresolved 排除。
- `tw_distribution.json` 落两块：primary（usable-722，直方图 0..7 =
  492/88/49/37/19/12/11/14，档 easy 580 / mid 86 / hard 56）+
  contrast（all-730：583/88/59）+ 差集 3 分子明细 + 口径出处字段
  `calibration_provenance`。脚本内 assert：usable-722 hard ≠ 56 即停机退出。

## 3. 采样（确定性规则，无随机数）

每档按 entity_id 字典序取前 2（usable-722 群体）；工具子集规则：
S ⊆ present（有票家族）、2 ≤ |S| ≤ 6、若 wrong 非空则 S ∩ wrong ≠ ∅
（ADR-0007「子集内仍含错工具」非平凡口径）；按（|S| 升序、家族名字典序）取第一个。

| # | case_id | 档 | tw | 真值 label | 工具子集 | 子集内错族 | 子集全错 |
|---|---|---|---|---|---|---|---|
| 1 | case_bbb_demo_z000553 | 易 | 0 | 1 | admet_ai, admetlab | —（分子上无错族） | 否 |
| 2 | case_bbb_demo_z000555 | 易 | 0 | 1 | admet_ai, admetlab | — | 否 |
| 3 | case_bbb_demo_z000282 | 中 | 3 | 1 | admet_ai, admetlab | admet_ai | 否 |
| 4 | case_bbb_demo_z000565 | 中 | 2 | 1 | admetlab, deeppurpose | deeppurpose | 否 |
| 5 | case_bbb_demo_z000364 | 难 | 5 | 0 | admet_ai, admetlab | 两族全错 | **是（伪共识形态）** |
| 6 | case_bbb_demo_z000846 | 难 | 4 | 1 | admetlab, b3clf | 两族全错 | **是（伪共识形态）** |

演示批附注：两道难题的子集恰为「全票一致但全错」（z000364 的 admet_ai、admetlab
同投 1、真值 0；z000846 的 admetlab、b3clf 同投 0、真值 1）——正是 R4.4 要防的
「只看分歧才检验」捷径样本，对演示有利；两道易题子集全对、两道中题子集一半错。
该形态是排序规则的自然结果，非人工挑选。

## 4. 题面与后端视图

**题面**（站2，单一权威生成器模式，仿 qed01_task.py/aedit01_task.py）：只给
分子结构（entity_id + SMILES）与工具目录（每族一个 `predict_bbb_<family>(smiles)`
+ 四个 A 侧库查询工具），预测值必须 agent 自己调用；要求做可用性判断
（`predictions_usable`）+ 选择性作答（`abstain`/`final_label`），弃答代价 c 的
存在与区间如实写进结果规范（公开判分口径，非解题指引）。**不给任何解题指引、
不点明规律**（不提示工具可能不一致、不教检验方法）。答案骨架由
`answer_schema.skeleton()` 生成（字段名唯一权威），并内联跑了
`check_task_md` 同口径断言：骨架 ⊇ 必填字段、⊆ 声明字段，6 题全过。
`answer.json` 预放空白件（`blank_answer` 口径：null/[] 留空）由站6 workspace 铺。

**view.json**（站3，`per_case_view_v1`，结构照搬 38 份现成视图 + fixed 类先例
`case_dr_aedit_01`，未自创 schema）：
- gt：`side="B"`、`entity_ref=null`（答案非库内实体，aedit_01 同型）、
  `composition` = 目标分子匿名域元素计数多重集（视图文件不出现真实化学式）；
- 白名单 12 实体（n=2 族序列 + K=10 干扰项，K 达 ADR-0018 §3.2 下限）：
  构造规则 = A 侧实体中 **∉ 730 池**、数值 logS ≤ −1.0、在 search_candidates
  有记录，按 entity_id 字典序取前 12（确定性）。**构造性排除全部 730 池分子**；
- `literature_doc_ids=[]`（药物线现成视图同型；literature_search 一律 EMPTY_RESULT）。

### 4.1 自检结果（官方 `assert_per_case_view.assert_one_case`，与其 main() 同一入口）

6/6 题各 **16/17 断言过**，唯一失败项全部是 `A6.quality_gate_empty`，明细落
`view_selfcheck.json`。该项是 **open 类质量门**（「A 内无不劣于 gt 的解」需要
gt 的性质阈值），fixed 类 gt 无阈值可构造，必然报「gt.logS 值缺失」——
**与生产先例 `case_dr_aedit_01` 在仓库既有断言日志
`src/data_build/_build_state/per_case_view_assert_result.json` 中的失败签名逐字
一致**，属既有口径限制而非本批缺陷；未改断言代码、未自创 schema。
gt ∉ 可见集由三道实证覆盖：A1（entity_ref ∉ 白名单 + 组成零碰撞 + 注入同组成
实体后扫查变红的非空转自证）、A4 负例（按 gt 组成扫查 BEFORE 全库命中 /
AFTER 视图内返空）、以及本任务语义的 leak_demo（见 §6 泄漏发现，6/6 blocked）。

## 5. 全部产物 md5（前8位）

```
9ced9768  tw_distribution.json
b934c039  sample.json
9d89326c  view_selfcheck.json
（每题；answer_schema.json 六题同一文件）
f3abdc40  cases/*/answer_schema.json
z000282: task.md 3a444003  ground_truth.json 45c12e5b  tools.json 2da3106e  view.json 519c6d38
z000364: task.md 0c0d4092  ground_truth.json fc2405e6  tools.json 6d9db9e5  view.json 626cdc2a
z000553: task.md 09095dc7  ground_truth.json 14f0e226  tools.json 8bdac05b  view.json f2d3aadd
z000555: task.md e569a50d  ground_truth.json 2acc7183  tools.json acfce0c2  view.json e0b34ec1
z000565: task.md 7462ab27  ground_truth.json 0195b3a3  tools.json 68f61545  view.json 5eb0a5f9
z000846: task.md 358d1d56  ground_truth.json 2c20b012  tools.json 3fe7f2a1  view.json e65992f0
70283a8b  scripts/station1_sample.py
e9bb9fd8  scripts/station2_build_cases.py
e08ec738  scripts/station3_build_views.py
```

复现命令（仓库根）：
```
python3 task/task25-pipeline小流量全链路演示/experiment/scripts/station1_sample.py
python3 task/task25-pipeline小流量全链路演示/experiment/scripts/station2_build_cases.py
python3 task/task25-pipeline小流量全链路演示/experiment/scripts/station3_build_views.py
```

## 6. 口径问题登记（含一项重要泄漏发现，站6 必须处置）

1. **【重要】A 侧后端对全部 730 池分子携带 `prop='bbb'` 且值 == 实验真值**
   （实跑核验 730/730 一致）。⇒ 任何白名单若含池分子，agent 一次
   `get_property(id, ["bbb"])` 即直接读到答案。处置：
   ① 本批 view 白名单构造性排除全部 730 池分子（§4）；
   ② leak_demo 逐题实证：目标分子 BEFORE（无视图）get_entity 可达、
   get_property('bbb') 返回真值 / AFTER（视图层）一律 UNKNOWN_ENTITY，6/6 blocked；
   ③ 已写入各题 `tools.json.leakage_note`：**站6 mock server 必须 fail-closed
   装载 view（缺视图拒绝启动，先例 `ws_mcp_server_aedit01_some.py`），
   predict 工具的值只能出自冻结 divergence CSV 的对应家族列，禁止从后端
   bbb 属性出**。该问题不触发停机（本批已有确定性处置且未改任何既有口径），
   但**验收批（≥5000 题）出题前须正式裁定**：后端 A 侧 bbb 属性行是保留
   （则一切 fixed BBB 题的视图必须永久排除池分子）还是迁移 B 侧——归主会话/owner。
2. **tw 双口径（56 vs 59）**：已核实，56 = usable-722 权威口径（§2），差集 3 分子
   全部为 truth_unresolved 排除项；未自行拍板新口径，`tw_distribution.json`
   同时落两块 + 出处字段供追溯。
3. **A6 断言对 fixed 类不适用**：既有断言集的 open 类质量门，生产先例同签名失败
   （§4.1）。如实记录，不改断言代码。
4. 站6 报告（并行 agent 所写）成文时本批 case 目录尚未落盘，其「BBB case 目录
   尚不存在」表述已过时；其判分器/mock server 待办现在可以直接消费
   `experiment/cases/*/`（题面 + ground_truth + tools.json + view.json 齐备）。

## 7. 停机条件核对

- 每档分子数：易 580 / 中 86 / 难 56，均 ≥2 —— 未触发「档内不足 2」停机。
- 56 口径核实成功 —— 未触发「配对无法核实」停机（差集已解释到分子级）。
- 无新工具/新依赖、无 API、无既有口径改动、冻结输入未动。
