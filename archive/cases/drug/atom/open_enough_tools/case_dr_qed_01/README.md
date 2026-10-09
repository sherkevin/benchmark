# case_dr_qed_01 · open · enough_tools

**第一组套卷的第六格（open + enough）。母题描述、真值、判分口径见
[fixed·basic 侧 README](../../fixed_basic_tools/case_dr_qed_01/README.md)；
open 侧配置差异见 [open·basic README](../../open_basic_tools/case_dr_qed_01/README.md)；
`qed_components` 的来历与"梯子到头"的论证见
[fixed·enough README](../../fixed_enough_tools/case_dr_qed_01/README.md)。本页只记本格。**

## 配置

| 项 | 值 |
|---|---|
| 候选可见性 | **open**——`list_pool(page)` 分页枚举（每页 4 个，翻全 33 个需 9 页） |
| 工具面 | `list_pool` + `measure_qed` + `fit_trend` + `group_by_scaffold` + **`qed_components`**（**5 个，全矩阵最多**） |
| 预算 | 17（枚举补偿，与 open 其余两档相同） |
| 题面 / 真值 / 判据 | 与 fixed 侧同一份 |
| 判分器 | `judge_enough.py`，**整套转发 basic 档 fixed 的 `judge.py`** + 双重同源断言 |

## 本格结果（batch V，n=25，均值 0.887）

`cp_extrap` 0.782｜Δ(enough−some) = **+0.077**

| 模型 | n | 均值 |
|---|---|---|
| qwen3-coder-plus | 5 | 0.955 |
| qwen3.5-plus | 5 | 0.955 |
| qwen3.7-max | 5 | 0.953 |
| bailian/glm-5.2 | 4 | 0.884 |
| DeepSeek-V3-671B | 1 | 0.805 |
| deepseek-v3.2 | 5 | 0.705 |

**一处如实标注**：`cp_extrap` 0.782 略**低于** open·some 的 0.813。这在跑间方差内，
**不作解读**——不要据此声称"多给工具反而变差"。

记录关：`cp_focus_job1` **1.000**、`cp_focus_job2` 0.969、掉进联苯陷阱 2/25。

> 本格早前报 0.885 (n=24)：当时 glm 有一个 run 因思考溢出挂着（规范 §12 已知坑）。
> 该 run 已落地、被 glob 自动纳入 ⇒ 现为 **0.887 (n=25)**，结论不变。

## 六格闭合后的 Δ_tool 全貌

| Δ_tool | some − basic | enough − some | 合计 |
|---|---|---|---|
| fixed | +0.344 | +0.047 | +0.391 |
| **open** | **+0.293** | **+0.077** | **+0.371** |

两侧形状一致：**前一档工具值很多，后一档几乎不值** ⇒ 工具梯子到头。

## preflight 会报一条 `cp_lead_value` 饱和 —— 已核为误报

本格 `cp_lead_value` **25/25 满分**，闸门2 因此报该关无区分度。
**已核为误报，不阻塞开批，判分公式不动。**

`cp_lead_value` 是**门**（堵背答案），诚实作答者接近 100% 通过是它该有的样子；
preflight 的门识别启发式只认"0/1 两档且以乘法出现"的关，认不出这种柯西平滑 + 加法进总分的门。
详见《第一组 case 清单》〇节同名小节。

## 怎么跑

```bash
.venv/bin/python src/probe_headless/ws_runner_qed01_open_enough.py <model> [round]
.venv/bin/python cases/drug/atom/open_enough_tools/case_dr_qed_01/judge_enough.py \
    cases/drug/atom/open_enough_tools/case_dr_qed_01/runs --batch
```

## 欠账

§11.3 未满足（只有一个批次，前缀 V）；`cp_lead_value` 饱和待裁定。
带提示题面的旧批（含当时补批）已归档 `runs_hinted_deprecated/`（60 run，不可与本格混算）。
