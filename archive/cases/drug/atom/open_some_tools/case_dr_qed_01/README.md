# case_dr_qed_01 · open · some_tools

**第一组套卷的 open·some 格。母题描述、真值、判分口径见
[fixed·basic 侧 README](../../fixed_basic_tools/case_dr_qed_01/README.md)；
open 侧的配置差异见 [open·basic README](../../open_basic_tools/case_dr_qed_01/README.md)。
本页只记本格差异。**

## 本格 = open 配置 + some 工具档

两条轴各走一格，其余全部不变：

| 项 | 值 |
|---|---|
| 候选可见性 | **open**——不给 `pool.tsv`，用 `list_pool(page)` 分页枚举（每页 4 个，翻全 33 个需 9 页） |
| 工具面 | `list_pool` + `measure_qed` + **`fit_trend`** + **`group_by_scaffold`** |
| 预算 | **17**（= fixed 的 12 + ⌈9 页/2⌉ 枚举补偿，按项目已固化公式）；翻页与测量共用，各扣 1 |
| 题面 / 真值 / 判据 | 与 fixed 侧**同一份**（`ground_truth.json` 只差 `measurement_budget`） |
| 判分器 | `judge_some.py`，**整套转发 basic 档 fixed 的 `judge.py`**，带 golden + `lead_conditions` 双重同源断言 |

两个便捷工具**不发放 QED、不扣预算**。它们的来历与 fixed·some 相同（由 basic 档实测失败导出），
包括 `group_by_scaffold` 那条依据已被推翻的更正——详见
[fixed·some README](../../fixed_some_tools/case_dr_qed_01/README.md)。

## 本格结果（batch T，n=27，均值 0.810）

`cp_extrap` 0.813｜外推答准 22/25 (88%)｜Δ(some−basic) = **+0.293**

| 模型 | n | 均值 |
|---|---|---|
| bailian/glm-5.2 | 3 | 0.955 |
| qwen3-coder-plus | 4 | 0.955 |
| qwen3.5-plus | 5 | 0.955 |
| qwen3.7-max | 5 | 0.955 |
| deepseek-v3.2 | 5 | 0.683 |
| DeepSeek-V3-671B | 5 | 0.443 |

四个模型压在同一个 0.955 上 ⇒ **本档模型区分度很低**（消融档的预期行为）。

记录关（拆细后）：`cp_focus_job1` **1.000**、`cp_focus_job2` **1.000**、掉进联苯陷阱 **0/27**。
即在 open 配置下，模型把 5 个可测同系物**全部测到了**、且选型测量无一投错——
**枚举目录时顺带看清结构，反而让测量投放比 fixed 侧更准**（fixed·some 是 0.992 / 0.979）。

## 与 fixed·some 的对照（尺子②）

| | fixed·some | open·some | 差 |
|---|---|---|---|
| 总分 | 0.847 | 0.810 | +0.037 |

方向为正但量级小。与 basic 档一样（fixed 0.503 vs open 0.516，反而是负的），
**候选可见性这条轴在本母题上没有拉开**——池只有 33 个、`list_pool` 翻几页就全给了，
发现的边际难度有限。这是如实记录，不是缺陷；要不要调发现成本属题目设计改动，
须先预登记（并注意 chain_01 / census 在同类调参上的失败史）。

## 怎么跑

```bash
.venv/bin/python src/probe_headless/ws_runner_qed01_open_some.py <model> [round]
.venv/bin/python cases/drug/atom/open_some_tools/case_dr_qed_01/judge_some.py \
    cases/drug/atom/open_some_tools/case_dr_qed_01/runs --batch
```

preflight 当前**通过**。

## 欠账

§11.3 未满足（只有一个批次，前缀 T）。preflight **通过**；同关 `cp_lead_value` 在另两格
被报饱和一事**已核为误报**（它是门、不阻塞开批、判分不动），详见《第一组 case 清单》〇节。
带提示题面的旧批已归档 `runs_hinted_deprecated/`（30 run，不可与本格混算）。
