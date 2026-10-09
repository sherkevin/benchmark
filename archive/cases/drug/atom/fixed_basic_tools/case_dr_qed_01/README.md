# case_dr_qed_01 · fixed · basic_tools

**这是第一组套卷的 fixed·basic 格，也是母题的权威描述页**（其余五格只记与本页的差异）。

> **第一组 = 一道母题 × 六种配置**（[ADR-0026](../../../../../governance/adr/0026-三档套卷同题同真值只变工具供给.md)）：
> 候选可见性（fixed / open）× 工具供给（basic / some / enough）。
> 六格**同题、同真值、同判据，只变工具面**——这是 Δ_tool 能归因到工具的唯一前提。
> **basic 是主测档，some / enough 是消融对照**，三档分数**不得平均成总分**（ADR-0026 §5/§6）。

## 一句话

池内 33 个分子分属 4 条同系列（同骨架、只变 N-正烷基链长）。两件事一份答卷：

1. 目标分子 `m05` 未合成、QED 测不到，须认出它属哪条系列、再从可测成员外推它的 QED；
2. 从池里挑一个先导化合物，同时满足实测 QED ≥ 0.60、psa ≥ 40、且不属于目标系列。

## 真值

| 项 | 值 |
|---|---|
| 目标 | `m05`，`COc1ccc(cc1)C(=O)NCCCCCCCCCCC`（甲氧基苯甲酰胺 n=11），真值 QED **0.5545** |
| 同系物（可测） | `m15 m17 m22 m24 m28`（n=2..6 —— 目标系列**只给短链**，故 n=11 是外推 5 格而非内插） |
| 干扰系列 | 硝基 / 多羟基 / 联苯，各 n=2..10 |
| 先导唯一合法解 | **m02**（硝基 n=3，QED 0.6046）—— psa≥40 的 18 个里唯一过 QED 门的，只高 0.0046，余量猜不出、必须实测 |
| 预算 | fixed 12 次；open 17 次（差额是枚举补偿，按项目公式算，见 open 侧 README） |

真值由 **rdkit 2026.03.5 `QED.qed()`** 确定性算出，重跑 `tools/gen_qed_case_gt.py` 复现、
可第三方核对。QED 是 8 个描述符各过非对称 sigmoid 后几何加权，**模型背不下也默算不出**。

`psa ≥ 40` 这道门是刻意设的：池内 QED 最高的三个全是联苯（0.869 / 0.854 / 0.805），
任何模型看描述符都往那儿猜，但联苯 psa 仅 29.1 ⇒ **靠常识猜的路被堵死**。

## 判分

    total = 0.5*cp_extrap + 0.3*cp_conditions + 0.2*cp_lead_value

| 关 | 判什么 | 归属 |
|---|---|---|
| `cp_extrap` | 目标 QED 外推精度（柯西，尺度 0.041 **从真值导出**：跨系列同链长平均差距/2） | 计分 · **主判别** |
| `cp_conditions` | 先导逐条对照三约束。两段式：免费硬条件（psa / 非目标系列）违反任一即 0；两条都过给 0.5；再满足**需实测**的 QED 门给 1.0 | 计分 · 中间值 |
| `cp_lead_value` | 报的先导实测 QED 与真值一致性（堵背答案） | 计分 |
| `cp_focus_job1` | 从账本推：Job1 要的趋势数据拿到几成（测了几个可测同系物 / 5） | **记录**（`NOT_IN_MEAN`） |
| `cp_focus_job2` | 从账本推：Job2 的测量投得准不准（测过的非目标系列分子里 psa 达门槛的占比） | **记录**（`NOT_IN_MEAN`） |
| grounding | 账本无成功 `measure_qed` ⇒ 总分归零 | **门** |

**关键设计**：需工具确认的条件必须**账本里有对该分子的成功测量记录**才算满足——猜对不给分
（规范 §6.0 / §11.4）。答卷契约三个 key：`target_qed` / `lead_mol_id` / `lead_qed_measured`，
由 runner **预放待填 `answer.json`** 进 workspace（`answer_schema.blank_answer()` 生成，
与判分器同一真源）；原封不动交回 = `no_answer`、计 0 分入分布。

## 工具梯子与六格矩阵（纯净题面 · 权威口径）

| 档 | fixed 工具面 | open 工具面 | 该档工具打的是上一档哪个实测失败 |
|---|---|---|---|
| basic | `measure_qed` | `list_pool` + `measure_qed` | —（主测档） |
| some | +`fit_trend` +`group_by_scaffold` | 同左 | 外推偏高 85%；测量花错系列 53% |
| enough | +`qed_components` | 同左 | 自己跑出几个拟合却挑错 |

便捷工具一律**不发放 QED、不扣预算**（变量只能是工具供给）。

| 总分 | basic | some | enough |
|---|---|---|---|
| **fixed** | **0.503** (n=24) | 0.847 (n=27) | 0.894 (n=29) |
| **open** | 0.516 (n=23) | 0.810 (n=27) | 0.887 (n=25) |

| Δ_tool | some − basic | enough − some | 合计 |
|---|---|---|---|
| fixed | **+0.344** | **+0.047** | +0.391 |
| open | **+0.294** | **+0.077** | +0.371 |

**结论：工具梯子到头了。** 边际收益从 +0.344 塌到 +0.047，`qed_components` 只有约一半 run
去调。不可再分内核 = **"识别出外推段会发生机制反转"**（ALOGP 项 n=2→6 还在变好 +0.124、
n=6→11 反转崩塌 −0.509，拐点从 5 个 QED 值看不出来）。两条硬证据见清单〇节。

## 本格逐模型（batch P，n=24）

| 模型 | n | 均值 |
|---|---|---|
| qwen3.5-plus | 4 | 0.627 |
| deepseek-v3.2 | 5 | 0.569 |
| qwen3.7-max | 5 | 0.556 |
| qwen3-coder-plus | 1 | 0.513 |
| bailian/glm-5.2 | 4 | 0.456 |
| DeepSeek-V3-671B | 5 | 0.319 |

**这张表不得用来排模型**，三条理由：① 单批次、每模型 n=1–5，远低于 §11.2 的功效要求；
② §11.2 要求**逐检查点**看且**分档须用身份先验**，不得用同一批数据既排序又检验；
③ 旧题面上曾报"qwen3.7-max 三批全第 1"，**在纯净题面上没有复现**（它这里只排第 3）——
说明那个排序有相当部分来自题面提示，不是稳定的能力差。**唯一跨题面稳定的是
DeepSeek-V3-671B 垫底。**

对抗基线（离线，13 条）：空账本 0.000 / 零工具 0.000 / 抄最近邻 0.048 / 认错系列 0.03–0.06
⇒ 判据⑤答案不可背成立。

## 欠账（不得当作已硬定稿）

1. **§11.3 未满足**：本格与其余五格**均只有一个批次**。此前的三批复现跑在**带提示题面**上，
   已随题面纯净化整体归档 `runs_hinted_deprecated/`（本格 90 个 run 可回查）。
   owner 2026-08-21 裁定**先不补批、先修文档**。
2. **`cp_focus_job1` 在 basic 档只有 0.852**——即平均没把 5 个可测同系物测全（约 4.3 个）。
   这是**真实的能力信号**（趋势数据没拿够，外推先天不足），记录关、不进总分，留作诊断。

**已办结两项**：

- `cp_focus` 已拆成 `cp_focus_job1` / `cp_focus_job2`（2026-08-21，只需重判、六格总分逐 run
  未变）。拆细顺带推翻了"测量花错系列 53%"这个失败模式——那是旧指标分母把 Job2 的必需测量
  算成偏离造成的假象。**连带后果**：`group_by_scaffold` 当初正是照这个假失败模式配的，
  故 Δ(some−basic) 应主要归因于 `fit_trend`。详见清单〇节。
- preflight 报的 `cp_lead_value` 饱和已核为**误报**：它是**门**（堵背答案），诚实作答者
  接近满分是应有行为；preflight 的门识别启发式认不出柯西平滑 + 加法进总分的门。
  **不阻塞开批，判分公式不动。**

## 怎么跑

```bash
.venv/bin/python src/probe_headless/preflight.py cases/drug/atom/fixed_basic_tools/case_dr_qed_01
.venv/bin/python src/probe_headless/ws_runner_qed01.py <model> [round]
# 单个 run
.venv/bin/python cases/drug/atom/fixed_basic_tools/case_dr_qed_01/judge.py <run_dir>
# 整批（统计只走这条，禁临时脚本——T-190 教训）
.venv/bin/python cases/drug/atom/fixed_basic_tools/case_dr_qed_01/judge.py \
    cases/drug/atom/fixed_basic_tools/case_dr_qed_01/runs --batch
```

**参数已冻结**：预算 12、psa 门 40、QED 门 0.60、QED_SCALE 0.041、`cp_conditions` 两段式、
`cp_focus` 记录关——不再改。

## 归档目录（可回查，不可与终稿混算）

| 目录 | 是什么 |
|---|---|
| `runs/` | **权威**：纯净题面 batch P，30 run |
| `runs_hinted_deprecated/` | 带提示题面三批共 90 run。题面泄了解题步骤（"先找同系列→测它→拟合"、"别默认是直线"等），属"必须重跑"类变更 |
| `runs_v1_deprecated/` / `runs_v2smoke_deprecated/` | v1/v2 冒烟 |
| `runs_badToolName_deprecated/` | MCP 工具名前缀写错（`chemlab` vs `matlab`）导致工具全程不可用的废 run，留作闸门0 的反面证据 |
