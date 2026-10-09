# case_dr_qed_01 · open · basic_tools

> **📌 本格就是第一组的 open 格**（owner 2026-08-22：一组 case = 同一个题面，fixed 与 open
> 的唯一区别是给没给候选集）。08-21 曾说它"被生成式题取代"——**那个说法已被推翻**：生成式题
> 题面不同、属另一组。
>
> **2026-08-22 修正**：查出本格预算曾是 **17**（旧"枚举补偿"）而 fixed 是 12，违反"同题面"。
> 已统一为 **12**；旧 30 run 归档 `runs_budget17_deprecated/`（留证不删），**本格待重跑**。

**第一组套卷的 open·basic 格。母题描述、真值、判分口径全部见
[fixed·basic 侧 README](../../fixed_basic_tools/case_dr_qed_01/README.md)——本页只记差异。**

## 与 fixed 侧的关系：同一道题，只换候选可见性

这一点曾被搞错过，故写在最前面。

| 项 | fixed·basic | open·basic |
|---|---|---|
| 题目（Job1 外推 + Job2 联合约束选先导） | 相同 | 相同 |
| `ground_truth.json` | — | **与 fixed 逐字段相同，只差 `measurement_budget`** |
| `answer_schema.json` 契约 | `target_qed` / `lead_mol_id` / `lead_qed_measured` | **完全相同** |
| 判分器 | `judge.py`（唯一权威） | `judge_qed01_open.py` **整套转发 fixed 的 judge**，带 golden + `lead_conditions` 双重同源断言 |
| 候选池 | 直接给 `pool.tsv`（33 个全可见） | **不给**，用 `list_pool(page)` 自己枚举 |
| 工具面 | `measure_qed` | `list_pool` + `measure_qed` |
| 预算 | 12 | **17** |

**预算差额是枚举补偿，不是放水**：按项目已固化的公式（同 chain_01 / dopant）
`= fixed 测量数 12 + ⌈翻页数 9 / 2⌉ = 17`。33 个候选、每页 4 个需翻 9 页，
若同为 12 则翻完只剩 3 次测量、题目不可解。实测枚举 9 页看全 33 个后**还能测 7 个**（需 6 个）
⇒ 可解，且发现仍有实打实代价（判据④）。翻页与测量**共用同一份预算**，每次成功各扣 1。

## 两处曾经的设计错误，留痕以免重犯

1. **发现原语选错**：原用 `search_pool(query, page)` 子串搜索，实测把"会不会猜中结构片段"
   混进了本该只考"候选可不可见"的轴——qwen3.7-max 连试 `S(=O)(=O)N` / `c1ccncc1` /
   `C(=O)O` / `CC(=O)N` 四个子串全 0 命中、预算耗尽、题目做不成。
   已改为 **`list_pool(page)` 纯分页枚举**。废 run 归档 `runs_pureExtrap_deprecated/`。
2. **曾被改成另一道题**：一度把 open 侧退化为"纯外推版"（2 个 key、无先导选型），
   导致两侧不同题、**破坏了 Δ_tool 的归因前提**。已按 ADR-0026「同题只换配置」修回。

⇒ 现在两侧是同一道题，**尺子②（fixed vs open）在本套卷上是可比的**。

## 本格结果（batch Q，n=23，均值 0.516）

| 模型 | n | 均值 |
|---|---|---|
| qwen3.5-plus | 4 | 0.657 |
| deepseek-v3.2 | 5 | 0.578 |
| qwen3.7-max | 5 | 0.570 |
| bailian/glm-5.2 | 5 | 0.526 |
| DeepSeek-V3-671B | 2 | 0.279 |
| qwen3-coder-plus | 2 | 0.160 |

**同样不得用来排模型**（单批、每模型 n=2–5、未按 §11.2 逐检查点 + 身份先验检验）。

**一个如实标注的观察**：open·basic 0.516 与 fixed·basic 0.503 **几乎持平**，
尺子②在本格上没有拉开。这与 chain_01 时期"翻页成本足够高才拉得开"的经验一致——
本格 9 页/17 预算的枚举成本占比约 53%，但候选池只有 33 个、结构信息在 `list_pool`
返回里一次给全，发现的边际难度有限。**是否要为拉开尺子② 而调发现成本，属题目设计改动，
须先预登记、且不得因分数不好看而回调**（§10 第 2/4/8 条）。

## 欠账

1. **§11.3 未满足**（只有一个批次，前缀 Q）。owner 2026-08-21 裁定先不补批。
2. preflight 报的 `cp_lead_value` 饱和（本格 20/23 满分）已核为**误报**：它是**门**
   （堵背答案），诚实作答者接近满分是应有行为；preflight 的门识别启发式认不出柯西平滑 +
   加法进总分的门。**不阻塞开批，判分公式不动。** 详见清单〇节。

**已办结**：`cp_focus` 已拆成 `cp_focus_job1` / `cp_focus_job2`（只需重判、总分未变）。
本格实测 `cp_focus_job1` **0.922**、`cp_focus_job2` **1.000**、掉进联苯陷阱 **0/23**
——**open 侧的测量投放比 fixed 侧更准**（fixed·basic 是 0.852 / 0.994），
因为枚举目录时顺带看清了结构。旧口径那个 0.612 是分母假象，已废。

## 怎么跑

```bash
.venv/bin/python src/probe_headless/preflight.py cases/drug/atom/open_basic_tools/case_dr_qed_01
.venv/bin/python src/probe_headless/ws_runner_qed01_open.py <model> [round]
.venv/bin/python cases/drug/atom/open_basic_tools/case_dr_qed_01/judge_qed01_open.py \
    cases/drug/atom/open_basic_tools/case_dr_qed_01/runs --batch
```

**参数已冻结**：预算 17、PAGE_SIZE 4、QED_SCALE 0.041（与 fixed 同源）——不再改。

## 归档目录（可回查，不可与终稿混算）

| 目录 | 是什么 |
|---|---|
| `runs/` | **权威**：纯净题面 batch Q，30 run |
| `runs_hinted_deprecated/` | 带提示题面批 |
| `runs_pureExtrap_deprecated/` | "纯外推版"（与 fixed 不同题）时期的 90 run —— 发现原语选错会怎样的证据 |
| `runs_v1_deprecated/` / `runs_v2lead_deprecated/` | v1/v2 冒烟 |
