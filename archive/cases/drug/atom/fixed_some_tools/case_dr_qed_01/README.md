# case_dr_qed_01 · fixed · some_tools

**第一组套卷的 fixed·some 格。母题描述、真值、判分口径见
[fixed·basic 侧 README](../../fixed_basic_tools/case_dr_qed_01/README.md)——本页只记差异。**

## 与 basic 档的唯一差别：工具面多两个

| 项 | fixed·basic | fixed·some |
|---|---|---|
| 题面 / 真值 / 判据 / 预算 | — | **全部相同**（预算同为 12） |
| 工具面 | `measure_qed` | `measure_qed` + **`fit_trend`** + **`group_by_scaffold`** |
| 判分器 | `judge.py`（唯一权威） | `judge_some.py`，**一行判分逻辑不新写、整套转发 basic**，带 golden + `lead_conditions` 双重同源断言 |

两个便捷工具**都不发放 QED、都不扣预算**——变量只能是工具供给本身。

## 这两个工具是怎么来的（ADR-0026 §3）

**不是预设的，是从 basic 档实测失败模式导出的**：

| basic 档实测失败 | 占比 | 配的工具 | 它替掉了什么推理 |
|---|---|---|---|
| 外推偏高（把下降段按直线外推） | **85%** | `fit_trend(xs, ys, x_query, degree)` | 把"归纳趋势并外推"变成一次调用。**但喂哪些点、用几阶仍由 agent 决定** |
| 测量花在非目标系列 | 53%（**该数已被推翻，见下**） | `group_by_scaffold()` | 按骨架分组，省去自己认系列 |

**兑现**：外推答准率 **4% → 85%**，两个工具 **26/26 run 全部调用**。

> `screen_candidates` 曾是候选，被换下——它打的"违反免费硬条件"在纯净题面新基线上只剩 11%、
> 退出前二。这正是 ADR-0026 §3 的实际运作：**基线变，工具清单跟着变**。

## ⚠️ `group_by_scaffold` 是照着一个测量假象配的（2026-08-21 更正）

导出它的依据是"测量花错系列 53%"，而那个 53% 来自旧 `cp_focus` = 实测分子 ∩ 目标同系列 / 实测分子。
**该定义把 Job2 强制要求的测量算成了"偏离"**——Job2 明确要求挑一个**非目标系列**的先导并实测它。

按两个 Job 拆开重算后：

| 分量 | fixed·basic | fixed·some |
|---|---|---|
| `cp_focus_job1`（拿到几成趋势数据 / 5 个同系物） | 0.852 | 0.992 |
| `cp_focus_job2`（选型测量投在 psa 合格候选上的占比） | 0.994 | 0.979 |
| 掉进联苯陷阱的 run（测了可免费排除的分子） | 1/24 | 3/27 |

⇒ **模型从来没有"测错系列"**，它们在正常做 Job2。那个失败模式不存在。

**这不影响 Δ_tool 的诚实性**（工具确实给了、分数确实这么变），但它意味着
**Δ(some−basic) = +0.344 应主要归因于 `fit_trend`，而非 `group_by_scaffold`**。
两工具各自的贡献未做拆分实验，**不得声称 `group_by_scaffold` 有效**。

## 本格结果（batch R，n=27，均值 0.847）

`cp_extrap` 0.783｜外推答准 22/26 (85%)｜Δ(some−basic) = **+0.344**

| 模型 | n | 均值 |
|---|---|---|
| qwen3-coder-plus | 3 | 0.955 |
| qwen3.5-plus | 5 | 0.955 |
| bailian/glm-5.2 | 4 | 0.861 |
| DeepSeek-V3-671B | 5 | 0.806 |
| deepseek-v3.2 | 5 | 0.789 |
| qwen3.7-max | 5 | 0.764 |

**注意分布已高度集中**：26 个有效 run 里多数落在同一个 0.955。给了 `fit_trend` 之后
模型间差距基本消失 ⇒ **本档几乎没有模型区分度**，这是消融档的预期行为（basic 档才是主测）。

**逐模型 Δ 相差极大**（coder +0.408 / qwen3.5-plus +0.002），机制是 `fit_trend` 让调用者
自选拟合阶数：用 degree=2 的拿到接近真值的外推、倚赖 degree=1 的原样复现 basic 档的直线错误
⇒ **"这关系是直的还是弯的"这个科学判断仍留在 agent 身上**。

## preflight 会报一条 `cp_lead_value` 饱和 —— 已核为误报

本格 `cp_lead_value` **26/26 满分**，闸门2 因此报"该关 100% 满分、无区分度"。
**已核为误报，不是缺陷，不阻塞开批，判分公式不动。**

理由：`cp_lead_value` 是**门**（堵背答案——报的实测值必须与真值一致，猜的人拿不出这个数），
**诚实作答者接近 100% 通过正是它该有的样子**。preflight 的门识别是启发式，只认"0/1 两档
且以乘法出现"的关，认不出这种**柯西平滑 + 加法进总分**的门。属工具侧已知限制。

详见《第一组 case 清单》〇节同名小节（含一次误判的留痕与固化纪律）。

## 怎么跑

```bash
.venv/bin/python src/probe_headless/ws_runner_qed01_some.py <model> [round]
.venv/bin/python cases/drug/atom/fixed_some_tools/case_dr_qed_01/judge_some.py \
    cases/drug/atom/fixed_some_tools/case_dr_qed_01/runs --batch
```

## 欠账

§11.3 未满足（只有一个批次，前缀 R）；`cp_lead_value` 饱和待裁定。
带提示题面的旧批已归档 `runs_hinted_deprecated/`（30 run，不可与本格混算）。
