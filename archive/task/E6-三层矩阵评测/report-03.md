# E6-03 裸答基线 — report-03（第二轮：上游交付检测 + 按已落盘部分重跑判定）

> 更新：2026-08-07 夜（第二轮；第一轮为 fixed 口径收敛 R1-③） · 出处：`goal.md` §2b · ADR-0018 §9 项 8 与**修订 R1**（§7 R1-①/②/③）· task-03 任务书
> 判据写死不得调松（goal §2b）；污染 case 只出判定结论、绝不修改 case（重造归 E5）。

## 0. 一句话结论

**本轮出真实判定结论 0 条，但判定链已实际接通一半**：上游 g_b per-case 注册已全量到货（15/15 dh，真实数据无占位），open 判定 45/45 生效；然而 dh 采样尚为 0/450，45 条均如实标「干净（采样不完整）」；fixed 侧 |S_feasible| 仍 0/30 未落盘，90 条维持「等待 E5 落盘」；E3 resolve 映射未交付。采样续跑已重启（实测 277/1350，唯一进程）。

---

## 1. 裁定确认（ADR-0018 修订 R1，逐字核对）

### 1.1 R1-③ fixed 口径（本任务主体，已逐字确认）

`governance/adr/0018-open类构造方式per-case-AB配对.md` 修订 R1 §「R1-③ fixed 候选池的分母取『完整过滤邻域』」原文判定式：

\[
p_{\text{base}} = \frac{1}{\lvert S_{\text{feasible}}\rvert}, \qquad \text{废弃条件：}\ \text{pass@10} > 2\,p_{\text{base}}
\]

其中：
- \(S_{\text{feasible}}\)：该 case 下满足题面全部硬约束的候选集合（类型1 为合格者集合，类型2 为筛选后待排序的集合）——即「完整过滤邻域」；裁定原文明确否决另两个备选口径（格内成员集 → type1 单成员格 \(p_{\text{base}}=1\) 判据恒不触发；全 A 库 → 退化为绝对判据）；
- \(\lvert\cdot\rvert\)：集合势（候选集合元素个数）；
- \(p_{\text{base}}\)：候选池内单次随机抽样恰好抽中 gt 的概率（等概率抽样下 = 1/集合规模）；
- \(\text{pass@10}\)：无工具裸答 10 次中命中 gt 的比例（命中次数/10）；
- 阈值 2×：沿用 goal §2b 原文，不改；裁定原文写作「>」，即**严格大于，恰等于 2·p_base 不废弃**（本批单测含该边界用例）。

### 1.2 |S_feasible| 落盘位置约定

- **归属与时机**：ADR-0018 R1-③「配套要求」：E5 按当前库快照复算每个 fixed case 的 \(|S_{\text{feasible}}|\) 并**落盘为 case 字段**；对应任务书 `task/E5-template与case构造/task-04-材料侧open首批与三项收尾.md` §4.2（交付物 #5：30 道全覆盖，co 系列处置有说明）。
- **字段名**：E5 文档只写「落盘为 case 字段」，**未明文冻结字段名**（E5 task-04 状态「待开工」）。E6 侧读取契约（`bare_baseline.s_feasible_of`，已落码留痕）：依次读 `s_feasible_size` / `s_feasible` / `feasible_set_size`，兼容其它键名含 `feasible` 的正整数字段（排除 `normalizer` 等字符串字段）；均未命中 → 该 case 标「等待 E5 落盘」。读到哪个键在判定结果 `s_feasible_field` 字段留痕。**E5 落盘时若用契约外字段名，E6 只需在该读取序补一行。**

### 1.3 R1-②（open，继续等）

组成串 → 实体 ID 映射归 E3 判分侧，一对多（同分异构全收），判定式 `hit_gb = 1[resolve(c_submit) ∩ G_B ≠ ∅]`。E6 侧只留函数签名级接线位，不实现（见 §2）。

---

## 2. 代码改动（全部限 `src/eval_matrix/`，未触碰禁区文件）

| 文件 | 改动 |
|---|---|
| `bare_baseline.py` | **fixed 判定收敛 R1-③ 单一口径**：删除三口径备选实现（`extract_pool`/`p_base_values`/旧 `fixed_hits`），新增 `s_feasible_of`（按 §1.2 契约读 case 字段，未落盘返 None，不得用近似值顶替）；`judge_fixed_case` 判据 = `pass@10 > 2/|S_feasible|`（严格大于），三道前置条件任一不满足不出结论：未落盘→「等待 E5 落盘」、有效采样 <10→「采样不完整」、命中口径依赖标注 `hit_judgment_active=false` |
| `bare_baseline.py` | **fixed 命中复用判分器**：`fixed_hit_tokens` —— 候选 token（实体 ID 形态 + 真名形态）经 `evaluate.normalize_id` 归一后与 `case['gt']['id']` 相等（即 `_score_fixed` cp6 的 best==gt_id 文义）。**evaluate.py 现无 cs/co 自由文本命中专用入口（PR-34 相关），该依赖已如实记录于判定结果 `hit_dependency` 字段，判定生效以判分器支持为前提；未自写任何字符串比对**。真名路径沿用判分器 normalize_id 既有前缀匹配行为，偏松（宁多判命中 → 偏严废弃，方向安全） |
| `bare_baseline.py` | **open 侧 R1-② 接线位**：`resolve_composition_to_ids(composition, case_id) -> set`，函数签名级占位，`raise NotImplementedError('等待 E3 …')`，交付前不实现；`judge_open_case` 未注册 case 的 verdict 改引裁定出处（R1-① 等 E5、R1-② 等 E3） |
| `bare_baseline_config.json` | `fixed_caliber` 收敛为单口径（含字段读取契约、严格大于规则、命中判分器依赖说明）；新增 `open_caliber` 段留痕 R1-② 接线位；discipline.judge_reuse 覆盖 fixed/open 两侧 |
| `test_bare_baseline.py` | 重写 fixed 段锁定 R1-③：边界用例 pass@10 恰等于 2·p_base 不废弃、严格大于才废弃、未落盘标「等待 E5 落盘」、采样不完整不出结论、命中经 normalize_id 且粘连 token 不误判、`s_feasible_of` 字段契约八例、R1-② 占位抛 NotImplementedError |
| `bare_baseline.py`（第二轮） | **open 判定补采样完整性标注**（上游 g_b 到货后重跑所需）：已注册且无命中但有效样本 <k → verdict「干净（采样不完整）」+ note，不冒充满 k 干净；已有样本中命中照常按「任一次命中即污染」判；结果增 `n_valid_samples` 字段，summary 增 `open_active_count`/`open_sampling_incomplete` 计数。注册检查本就逐 case 进行，无一刀切 bug，无需改；补 2 条单测 |

**禁区自查**：未改 `evaluate.py` / `query_spec.json` / `dag_poc/data/**`；未跑正式三层矩阵；代码与配置内无 API key 字面量（走 agent_harness 环境变量机制）；未修改任何 case 文件。

---

## 3. 上游交付检测（第二轮实测，2026-08-07 夜）

| # | 交付项 | 实测状态 |
|---|---|---|
| ① | g_b per-case 注册（R1-①，归 E5） | **已到：15/15 dh 全覆盖**（另有 3 道 spare）。`b_partition.cases` 共 20 条（含 2 fixture）；dh 项 g_b 成员均为真实匿名实体 ID（z 前缀，如 z064386/z062680…），case_dh_drug_09 为 3 成员（同组成多纳入 G_B，与 ADR-0018 §8 一致）；**逐项检查 key_space 与成员 token：无「过渡期/占位」声明、无 ed:/bm_placeholder 前缀，placeholder 告警已消失，是真实数据** |
| ② | fixed `\|S_feasible\|` 落盘（R1-③，归 E5 task-04 §4.2） | **未到：0/30**。按读取契约（s_feasible_size/s_feasible/feasible_set_size/含 feasible 正整数字段）逐一探测 30 道 cs/co case JSON，均无命中 |
| ③ | E3 resolve 映射（R1-②，组成串→实体 ID 集合） | **未到**。evaluate.py 仅有 composition_of/composition_equal 组成比对纯函数（grounding 用），无组成→ID 映射；E3 task-05-research-round2 已出实现路径调研（数据源、挂载点均已锁定），代码未落盘 |

## 4. 判定结果（第二轮重跑 `judge`，机器可读件 `bare_baseline/judgments/report.json`）

**总账：真实判定结论 0 条（无污染清单可交 E5）。** 分项：

### 4.1 open：判定已生效，但采样全缺

- 15 道 dh × 3 模型 = 45 条：`hit_judgment_active=true`（g_b 已注册，命中判定走 `evaluate.hit_gb` 同一套逻辑，规范化走 gateway.normalize_b_key，未自写字符串比对）。
- dh 采样实测 **0/450**（第一轮采样在 cs_material_05 处中断，未及 dh 段），45 条均如实标「干净（采样不完整）」——该「干净」是零样本下的临时标注，不是结论；采样补齐后重跑复判。已有样本中的命中仍按判据判（本轮无样本，命中自然为 0）。
- 判定层对「部分 case 已注册」场景无一刀切 bug（注册检查逐 case 进行）；本轮新增 open 侧采样完整性标注与两条单测（不足 k 无命中→标注；不足 k 有命中→照常判污染）。
- 残留口径边界：组成串形态作答需 R1-② resolve 映射（等待项①）；实体 ID 形态作答经 hit_gb 已可判。

### 4.2 fixed：维持「等待 E5 落盘」90/90

口径 R1-③ 不变（§1）；|S_feasible| 0/30，90 条（30 道 × 3 模型）全部「等待 E5 落盘」，不得用近似值顶替。落盘后判定入口：

```
python3 -m src.eval_matrix.bare_baseline judge
```

### 4.3 采样进度与续跑进程

- 实测：有效样本 **277/1350**（13/45 个 case×model，均在 cs 段），失败记录 1140 条（`failures.jsonl`，尾部为 kimi-k3 ReadTimeout）。
- 第一轮后台续跑进程于 **20:58 异常退出**（日志无堆栈，终端已关闭；不排除环境回收），本轮先 `ps` 确认**无重复进程**后重启续跑，确认唯一进程存活，留痕追加到 `resume_run.log`。断点续跑机制保证已有样本不重采。

## 5. 等待项（收敛为两条）

| # | 等待项 | 解除条件 | 解除后动作 |
|---|---|---|---|
| ① | **E3 resolve 映射**（组成串 → 实体 ID 集合，一对多；ADR-0018 R1-②） | E3 判分侧交付（调研已就绪，代码未落盘） | E6 将 `resolve_composition_to_ids` 接线位接入 open 命中判定，重跑 `judge` |
| ② | **E5 fixed `\|S_feasible\|` 落盘**（E5 task-04 §4.2，R1-③）；g_b 注册（§4.1，R1-①）已到货不再等 | E5 task-04 §4.2 交付 | E6 重跑 `judge`：fixed 90 条出结论（届时仍需采样补齐，见 §4.3） |

另：dh 15 道「待独立验证（任务 #9）」复检通过前，最终废弃清单挂「待复检」前缀（progress.md 阻塞节留痕）。

## 6. 单测与回归

`python3 -m pytest src/eval_matrix/ -q` → **61/61 通过**（新增 2 例：open 已注册但采样不完整——无命中标注/有命中照常判污染；原有 R1-③ 边界用例不变）。
