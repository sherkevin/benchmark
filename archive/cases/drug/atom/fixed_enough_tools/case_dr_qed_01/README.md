# case_dr_qed_01 · fixed · enough_tools

**第一组套卷的 fixed·enough 格。母题描述、真值、判分口径见
[fixed·basic 侧 README](../../fixed_basic_tools/case_dr_qed_01/README.md)——本页只记差异。**

## 与 some 档的唯一差别：再多一个工具

| 项 | fixed·some | fixed·enough |
|---|---|---|
| 题面 / 真值 / 判据 / 预算 | — | **全部相同**（预算同为 12） |
| 工具面 | `measure_qed` + `fit_trend` + `group_by_scaffold` | 同左 + **`qed_components`** |
| 判分器 | `judge_some.py` | `judge_enough.py`，同样**整套转发 basic 档 `judge.py`**，带双重同源断言 |

`qed_components` **只对已实测过的分子开放**，不发放目标分子的分项——否则等于把答案算给它。
同样不扣预算。

## 它是从 some 档实测失败导出的（ADR-0026 §3 第二遍）

some 档剩下的失败不是"不会拟合"，而是**在自己跑出的几个拟合结果里挑错了**——
7 个低分 run 中 5 个来自同一模型，它同时拿到 0.5416（对）与 0.695（错），报了后者。

⇒ 配 `qed_components`：把 QED 拆成 8 个分项，让 agent 看见"是哪一项在变"，
从而有依据判断哪个拟合更可信。

**兑现**：外推答准 85% → **93%**。但 **Δ 只有 +0.047**，且该工具**只有约一半 run 去调**（15/29）。

## 核心结论：工具梯子到头了

| Δ_tool | some − basic | **enough − some** |
|---|---|---|
| fixed | +0.344 | **+0.047** |

边际收益塌了一个数量级。两条硬证据说明**剩下的推理不是缺工具，是缺不了的**：

1. **统计学工具帮倒忙**。本想配"交叉验证自动选阶数"，先在真值上验：

   | degree | LOO-CV 误差 | 外推值 | 柯西分 |
   |---|---|---|---|
   | 1 | **0.0231（最小）** | 0.6950 | 0.078 |
   | 2 | 0.0401 | 0.5416 | **0.910** |

   **CV 推荐的是错的那个**——因为测量窗口 n=2..6 内关系本就近似直线，曲率只在外推段才显现。
   纯统计的模型选择在这道题上有害，故该工具被否掉、没进 enough 档。

2. **真正的坎是领域知识，而任何供给它的工具都会顺带泄答案**。QED 分项显示
   **ALOGP 项 n=2→6 还在变好（+0.124）、n=6→11 反转崩塌（−0.509）**。
   拐点从 5 个 QED 数值里看不出来——要知道亲脂性有最优区、且目标的 alogp=4.96 已越过它。
   我们能给的最大帮助（`qed_components`，且只对已实测分子开放）只值 +0.047。
   再往下给（比如直接给目标的分项）就等于把答案算给它。

⇒ **这道题的不可再分内核 = "识别出外推段会发生机制反转"。**

## 本格结果（batch U，n=29，均值 0.894）

`cp_extrap` 0.844｜外推答准 27/29 (93%)

| 模型 | n | 均值 |
|---|---|---|
| qwen3.5-plus | 5 | 0.955 |
| qwen3.7-max | 5 | 0.951 |
| bailian/glm-5.2 | 5 | 0.925 |
| qwen3-coder-plus | 5 | 0.925 |
| deepseek-v3.2 | 5 | 0.872 |
| DeepSeek-V3-671B | 4 | 0.701 |

**这一格是反向验证**：工具补齐后六个模型基本拉平到 0.87–0.955（仅 671B 0.701），
**说明 basic 档测出的 0.319–0.627 那个差距，确实来自被工具替代掉的那些推理步骤**。
也因此本档没有模型区分度——消融档的预期行为。

记录关：`cp_focus_job1` 0.979、`cp_focus_job2` 0.991、掉进联苯陷阱 2/29。

## 怎么跑

```bash
.venv/bin/python src/probe_headless/ws_runner_qed01_enough.py <model> [round]
.venv/bin/python cases/drug/atom/fixed_enough_tools/case_dr_qed_01/judge_enough.py \
    cases/drug/atom/fixed_enough_tools/case_dr_qed_01/runs --batch
```

preflight **通过**。（同关 `cp_lead_value` 在 fixed·some 与 open·enough 会被报饱和，
**已核为误报**——它是门、诚实作答者接近满分是应有行为，preflight 的门识别启发式认不出
柯西平滑 + 加法进总分的门。不阻塞开批、判分不动，详见《第一组 case 清单》〇节。）

## 欠账

§11.3 未满足（只有一个批次，前缀 U）。带提示题面的旧批（含当时的补批）已归档
`runs_hinted_deprecated/`（60 run，不可与本格混算）。
