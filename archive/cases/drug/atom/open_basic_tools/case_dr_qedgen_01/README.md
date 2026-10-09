# case_dr_qedgen_01（生成式发现）— **属另一组 case，不计入第一组**

> **⚠️ owner 2026-08-22 定位裁定**：**一组 case = 同一个题面；fixed 与 open 的唯一区别是
> 给没给候选集。** 本题的题面与 `case_dr_qed_01` 不同（答案形态是"报出一个分子"而非
> "预测一个数值"）⇒ **它是另一组 case、属未来要做的事，不计入第一组**。
> 已做的工程与实验（四件套 + 裸答基线 + 3 run 冒烟）**原样留档、不干扰第一组**。
> 第一组的 open 格仍由 `case_dr_qed_01`（同题面、只是不给候选集）承担。

**原设计意图（现归为另一组）：** 与 fixed 侧 `case_dr_qed_01`（预测+选型，评过程）
**不是同一道题**——这是 goal §1.5 类型二（open 发现探索类，评终态）。owner 2026-08-21 认可
定义改述：第一组 = 两类任务各一条 3 档工具梯子（见《第一组 case 清单》〇节 + ADR-0027）。

## 为什么是生成式而不是"判别式 open"

原 `open_basic_tools/case_dr_qed_01` 用一次 `list_pool` 就交出全部 33 个候选 ⇒ 生成式塌成
判别式、与 fixed 无区别，判据② 三档全不成立。而"删 list_pool 改 measure 收任意分子"不能用在
**预测数值**的题上：QED 是粗描述符函数，通用 oracle 会被近似物一次调用绕过（[ADR-0027](../../../../../governance/adr/0027-预测型题不配通用oracle可测集必须闭合.md)）。
把答案形态从**数值**换成**分子**，命中判定是精确集合成员判断 ⇒ 无近似物泄漏。

## 一句话

A 侧可测：单环苯甲酰胺家族（8 头部 × 链长 n=2..8，全域最高 QED **0.792**）。
agent 要**推理出一个 A 中不存在、QED≥0.85 且无结构警报的新分子**，用存在性接口（只回 bool）
验证。A 里没有任何东西够 0.85 ⇒ 只能**生成**、不能检索。

## 真值（rdkit 确定性，`tools/gen_qedgen_case_gt.py` 可重跑复现）

| 项 | 值 |
|---|---|
| A 侧可测 | 56 个单环苯甲酰胺（8 头部 × n=2..8），全域最高 QED 0.7919 |
| B 侧存在性库 | 24 个加环/并环族（agent 不可测，只能问在不在） |
| 合格集 `G_B` | **6 个**：biphenyl n2/n3、diphenyl_ether n2/n3、phenyl_cyclohexyl n3、quinolinyl n3（QED≥0.85 且 ALERTS=0） |
| 近失诱饵 | naphthyl（0.834）、benzofuran（0.833）在 B 库但 QED<0.85 ⇒ 存在≠合格 |
| spec | QED ≥ 0.85 且 ALERTS = 0（收紧约束，owner 定；n≥4 触发警报 ⇒ 天然逼向短链） |
| 预算 | measure_qed 12 次；check_molecule_exists ≤10 次（§2b max_attempts） |

## 判分（goal §2b 唯一口径）

    open_score = 𝟙[hit(G_B)] × 𝟙[grounding_pass]

- **hit**：提交分子规范化后 ∈ `G_B`，且经 rdkit 复核 QED≥0.85、ALERTS=0（存在≠合格）。
- **grounding**（三条可执行断言）：① anchor ∈ A；② cited_values 与账本 measure_qed 真实返回
  容差内一致（不得编造）；③ anchor 与 proposed 都含苯甲酰胺母核 + 账本里有对 proposed 的成功
  `check_molecule_exists`。
- 二值 × 二值 = 二值 {0,1}：连续信号来自**命中率**与 **attempts 分布**（§2b「attempts 计入指标」）。

> §2b 原文 grounding 条③是"edit_op(anchor) 规范化 == 提交分子"，需封闭算子执行器；v1 未建，
> 用"共享母核 + 存在性已验"这个**可执行代理**替代，并按 §6.0 **不判**自由文本 `edit_op`（仅诊断记录）。
> 这条差异如实登记，非含糊。

## 质量门（造题期，已离线过）

| 门 | 判据 | 实测 |
|---|---|---|
| ④第一条 | A 全域无满足 spec 的解（否则退化检索） | A 峰 0.7919 < 0.85 ✓（生成器 assert） |
| ④第二条 | 线索可达：anchor∈A + ≤2 跳 + evidence_set 在 A | 苯甲酰胺 n=3 --加环--> biphenyl n=3 ∈ G_B；evidence 3 分子均在 A ✓ |
| §2b 命门 | 存在性接口对 A 内分子必返 false | 实测 A 分子 exists=false、B 分子 true ✓ |
| ⑤ 答案不可背 | 裸答基线 k≥10 命中 G_B 即污染废弃 | ✅ **过**：14 个有效样本（deepseek-v3.2 8 + qwen3.7-max 6，无工具）**0 命中 G_B**；模型都停在单环苯甲酰胺、无一跳到两环合格骨架 |

判分器对抗自测（离线）：正解 1.0 / 近失 naphthyl 0.0 / 空账本 0.0（grounding 挂）/ 编造引用值
0.0 / 锚点不在 A 0.0 / 空模板 no_answer。

## 怎么跑

```bash
.venv/bin/python tools/gen_qedgen_case_gt.py         # 重建真值（含质量门④断言）
.venv/bin/python src/probe_headless/preflight.py cases/drug/atom/open_basic_tools/case_dr_qedgen_01
.venv/bin/python src/probe_headless/ws_runner_qed01gen.py <model> [round]
.venv/bin/python cases/drug/atom/open_basic_tools/case_dr_qedgen_01/judge.py <run_dir>
.venv/bin/python cases/drug/atom/open_basic_tools/case_dr_qedgen_01/judge.py <批目录> --batch
```

## 状态

工程就绪且经冒烟验证：GT + server + runner + judge + schema；preflight 0 失败、命门离线全过、
判分器对抗自测过、**裸答基线 k=14 零命中已过**、verify_isolation **19/19**。

**冒烟（`--probe` 3 run，2026-08-21）：工程全绿但 0/3 命中，暴露一个待 owner 定的难度问题。**

| 冒烟项 | 结果 |
|---|---|
| returncode / api_error / tool_leak / 交卷 | 3/3 =0 / false / false / 全交 ✓ |
| 两个工具都可用且被用 | measure_qed 12–15 次、check_molecule_exists 10–13 次/run ✓ |
| open_score | **0 / 0 / 0** |

**诊断（读了三个 run 的账本+答卷，§6.0）**：三个模型都在**单环空间**打转（变取代基/变链长）或改**酰胺侧**
（qwen3.7-max 换吗啉/哌嗪、qwen3.5-plus 反而删了链），**无一想到"给苯环再加一个芳环"**——
而那正是过 0.85 的唯一路（QED 的芳环项奖励第二个环，是 QED 特有机制、非通用成药性直觉）。
每次 check_molecule_exists 都返 False：他们正确地在探存在性，只是没探到 B。**这不是 harness bug**
（工具/隔离/grounding 全对，正解离线判 1.0），是**难度/可发现性**问题。

**待 owner 定（两条路，n=3 分不出是"校准得好"还是"太难"）**：
1. **直接上正式批**测命中率——生成式发现本就该低命中，hit-rate 就是信号；风险是 0/30 无区分度。
2. **先松难度**再上批：把 spec 门槛 0.85→0.83（G_B 纳入 naphthyl/benzofuran、更易达），
   gate④ 仍成立（A 峰 0.792<0.83）但**须重跑裸答基线**（G_B 变大、命中风险升）。
   注意 owner 的纯净题面规矩 ⇒ **不能在题面加"加环"提示**，可发现性只能靠难度校准。

some/enough 档待 basic 定稿后按失败模式导出（ADR-0026 §3）。
