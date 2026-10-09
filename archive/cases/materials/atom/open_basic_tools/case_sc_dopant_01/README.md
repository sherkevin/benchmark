# case_sc_dopant_01 · open · 基本工具（不发清单、agent 自查发现，**分页且发现要付预算**）

**与 fixed 版的唯一差别**：fixed 把 Gd 目标清单 (`to_predict.txt`) 直接发给 agent；open 版**不发**，只点名"预测未合成的 Gd 系"，agent 必须用 `search_db` 自己从库里查出候选组成。真值、判分（cp1–4）、题目内核全不变——这样才能把"候选可见性（fixed↔open）"这一根轴单独隔离出来看。

> ## ⚠️ 2026-08-20 配置变更：旧「方案 A」的免费全量发现已废止
>
> **旧缺陷（实证两次：d 批 30 run + smoke0819 冒烟）**：`search_db` 免费且不分页，一次查询
> 返回**全库 30 个**候选 → 对目标 Gd 系发现覆盖率**中位 1.000、29/30 个 run 恒为 1.0**，
> "自己发现候选"这个 open 的唯一考点白送。后果两条：
> ① 规范 §11.1 判据 ④「发现有后果」要求 open 覆盖率**非恒 1.0** 且低覆盖组显著更低——
> 恒 1.0 时连"低覆盖组"都不存在，**该判据永远无法通过**；
> ② fixed/open 唯一合法差是候选可见性，发现免费时 open 信息量 ≈ fixed，**尺子② 那个 0.012
> 的差值失去解释力**（不能解读为"这题 fixed 不优于 open"，只能说这题的 open 还没做成立）。
> 横向看 dopant 是 4 个 open server 里唯一没分页的（`chain_open` 早已分页），属遗留实现。
>
> **现行语义**（owner 批，已按 §8 预登记）：`PAGE_SIZE=4` 分页 + **每翻一页扣 1 个预算**，
> `search_db` 与 `measure_tc` **共用** `measurement_budget = 23`。取值规则只看题目结构
> （同 chain_01 已固化公式）：可测 19 + ⌈拿全 30 个需 8 页 / 2⌉ = 19 + 4 = 23。
> 它迫使 agent 在"发现完整性"与"测量完整性"之间做真实取舍。
>
> **旧配置数据已归档**：`runs_freeDiscovery_deprecated/`（37 个 run = d1–d5 共 30 +
> cprobe 3 + dprobe 3 + smoke0819 1）。产自免费发现配置，**与新配置不可比**：
> 不得与新数据混算、不得作为达标证据引用。保留是作为"发现白给会怎样"的证据。
> 下文「实测」各节的数字均属旧配置，同样受此限定。

## 工具（open·基本工具档，比 fixed 多一个"读 A"原语）

- `search_db(query, page)` — 查库目录，按元素/子串**分页**返回组成标识列表（每页最多 4 个）、
  **不返 Tc**，带 `has_more` / `total_matches`。**每翻一页消耗 1 个预算单位。**
- `measure_tc(comp)` — 已合成（Er/Nd）测真值；未合成（Gd）返 `sample_available:false`。**与 search_db 共用预算。**
- 发现闭环：`search_db` 分页枚举家族 → `measure_tc` 试测分出已合成(Er/Nd 边界)/未合成(Gd 目标) → 从边界曲线外推预测 Gd。

判分器 `judge_dopant.py` 与 fixed 版**逐字节相同**（读 `answer.json` 的 predictions 比离线真值）。

## 实测（4 模型 × 2 轮，8/8 有效、解析 100%）

| 模型 | open 均值 | 轮次 | fixed 均值 | Δ(open−fixed) |
|---|---|---|---|---|
| qwen3.7-max | 0.659 | [0.376, **0.942**] | 0.478 | **+0.181** |
| qwen3-coder-plus | 0.570 | [0.559, 0.581] | 0.407 | **+0.163** |
| glm-5.2 | 0.281 | [0.281, 0.281] | 0.440 | −0.159 |
| deepseek-v3.2 | 0.285 | [0.280, 0.290] | 0.334 | −0.049 |
| **总均 / 极差** | **0.449 / 0.378** | | 0.414 / 0.144 | 均值持平、区分度翻倍 |

- **标准1 区分度**：open 极差 **0.378 ≥0.2 ✓**（fixed 是 0.144 ✗）——open 把强弱拉得更开。
- **目标 最强<0.9**：模型均值最高 0.659 ✓；但 qwen3.7-max **单轮 r2 冲到 0.942**（cp1 9/9 + cp4 命中，破了"Gd 比 Er 先死"的陷阱），说明这题**能被强模型偶尔近解**。
- **标准2 均值 0.5–0.9**：0.449 仍偏低（同 fixed，根因仍是 cp4 多数为 0）。

## 关键发现：方案 A 的 open **没能让题目更难**（重要，反直觉）

预期是 open>fixed（更难），但实测**总均基本持平（0.449 vs 0.414）**，且**强模型反而更高**（qwen/coder +0.16~0.18），只有弱模型（glm/deepseek）掉了一点。原因：

- 被判分的量（预测 Gd 曲线）在 fixed/open **完全一样**；open 只多了"发现候选"这一步。
- 而 `search_db("Gd")` 一次就枚举出全部 11 个 Gd——**发现对强模型是白给**，还顺带让它们多测了几个点、曲线拟得更好（分数不降反升）。弱模型没利用好这份自由，反而被发现开销分散。

**结论**：方案 A（点名目标 + 平凡的查库工具）这种"轻 open"不足以制造范式难度差；要验证"open 真的比 fixed 难"，得做**方案 B**（连目标都不点名，要求真探索——自己发现该研究哪个未合成家族、锚定哪两条边界）。这正印证了 owner"A/B 都要试"的判断。

## 复现

```bash
python src/probe_headless/sweep_dopant_open.py                       # 4 模型 × 2 轮
python cases/materials/atom/open_basic_tools/case_sc_dopant_01/judge_dopant.py
```

工具：`src/probe_headless/ws_mcp_server_dopant_open.py`（+search_db）、`ws_runner_dopant_open.py`（open 题面）。runs/ 与 traces 不入库。
