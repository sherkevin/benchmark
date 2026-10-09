# task7 · 3 POC 综合判定

> **日期**: 2026-08-14 · **状态**: 基于 POC3+4 实测 + POC1+2 设计的综合判定

## 一句话结论

> **cp 设计 + 加权部分得分的 retrofit 模式在 2 个域实测通过 + 2 个域设计可行，跨域一致性成立；但过程量 vs 终答相关性尚未验证（需真 agent trajectory）。**

---

## 1. 四个验证目标的回答

### 目标 1：cp 设计能不能跑通？

| POC | 域 | cp 数 | Tier 1 占比 | 实测/设计 | 结论 |
|---|---|---|---|---|---|
| POC3 | 医疗 EHR | 6 | 50% | ✅ 实测 | ✅ 跑通（3/6 LLM-judge + FHIR cp 环境依赖） |
| POC4 | 药物 ADMET | 7 | 71% | ✅ 实测 | ✅ 跑通（5/7 T1 + 2/7 T2 全绿） |
| POC1 | 药物虚筛 | 7 | 71% | 🟡 设计 | ✅ 设计可行（5/7 T1 均可离线判分） |
| POC2 | 材料弛豫 | 7 | 71% | 🟡 设计 | ✅ 设计可行（物理守恒 = 免费 T1） |

**回答**：✅ **能跑通**。三档 cp（硬校验 / AI 判官 / 人工）在 4 个域均可设计。实测 2 域无阻塞，设计 2 域路径清晰。

### 目标 2：src 指标管线输出是否有意义？

| 指标 | POC3 | POC4 | POC1 | POC2 |
|---|---|---|---|---|
| 加权部分得分 | ✅ 0.500 vs binary 0 | ✅ 1.0/0.794/0.456 | 🟡 设计 | 🟡 设计 |
| autonomy@0 | ❌ 无真 agent | ❌ 无真 agent | ❌ | ❌ |
| 介入-成功率曲线 | ❌ | ❌ | ❌ | ❌ |
| pass^k | ❌ | ❌ | ❌ | ❌ |
| 介入守恒 | ❌ | ❌ | ❌ | ❌ |
| L0–L3 敏感性 | ❌ | ❌ | ❌ | ❌ |
| 许可校准 | ❌ | ❌ | ❌ | ❌ |

**回答**：🟡 **部分验证**。加权部分得分（核心指标之一）在 2 域实测有区分度；但 src 的 5 签名输出中其余 4 个需要真 agent trajectory 才能验证。

### 目标 3：过程量 vs 终答相关性

| POC | cp 通过率 | 终答正确率 | 相关系数 |
|---|---|---|---|
| POC3 | 3/6 (mock) | 无法计算 | — |
| POC4 | 7/7 vs 5/7 vs 1/7 (mock) | 完美 vs 部分 vs 几乎全错 | **强正相关（定性）** |
| POC1 | 设计 | — | — |
| POC2 | 设计 | — | — |

**回答**：❌ **未正式验证**。没有真 agent trajectory，无法计算 Pearson/Spearman 相关。POC4 的 3 档 mock 给出定性证据（cp 通过率与终答质量同向变化），但非统计意义的相关系数。

→ **这是 task7 最大的 gap**：过程量 vs 终答相关性是论文核心实验之一，必须在 MVP 阶段用真 agent 补上。

### 目标 4：借的方法学技术栈跨域是否一致？

| 方法学 | 来源 | POC3 | POC4 | POC1 | POC2 |
|---|---|---|---|---|---|
| 进度公式 `max(1/K·Σf)` | AgentBoard | ✅ 等价于 weighted partial | ✅ 等价 | ✅ 可套 | ✅ 可套 |
| 部分得分 | TheAgentCompany | ✅ `passed/total` | ✅ 加权版 | ✅ 设计 | ✅ 设计 |
| 负分 rubric | HealthBench | ❌ 未用 | ❌ 未用 | ❌ | ❌ |
| Tier 分级 | 自建 | ✅ 3 档 | ✅ 2 档 | ✅ 2 档 | ✅ 2 档 |

**回答**：✅ **跨域一致**。AgentBoard 进度公式 + TheAgentCompany 部分得分在 4 个域均可套用，无域特异性障碍。HealthBench 负分 rubric 暂未用（POC 阶段非必需）。

---

## 2. 跨 POC 核心发现

### 发现 1：binary Pass@1 在过程量评测中信息量严重不足

**证据**：
- POC3：binary=0（3/6 cp 通过的中等 agent）vs binary=0（0/6 通过的弱 agent）→ **不可区分**
- POC4：binary=0（medium 5/7 cp）vs binary=0（lazy 1/7 cp）→ **不可区分**

**结论**：任何只看终答对错的评测，都把"走了一半"和"几乎没走"打成同分。加权部分得分在两个独立域一致地解决了这个问题。

### 发现 2：Tier 1 ≥ 70% 铁律在主动设计时可达

**证据**：
- POC3（被动 retrofit）：50%（不满足，但 repo 自带，非我们设计）
- POC4（主动设计）：71% ✅
- POC1（设计）：71% ✅
- POC2（设计）：71% ✅

**结论**：主动设计 benchmark 题时，通过"三句话筛"（能脚本判→T1 / 能让 LLM 判→T2 / 其余→T3），可在不同域稳定达到 ≥70% Tier 1。被动 retrofit 现有 benchmark 时不一定满足。

### 发现 3：LLM judge 单 judge 可用但不够

**证据**：
- POC3：Qwen 单 judge，JSON 解析 100% 成功
- POC4：Qwen 单 judge，rubric 区分度高（perfect 1.0 vs medium 0.167）

**未做**：jury（多 judge ICC）+ κ 人工校准

**结论**：单 judge 在 POC 阶段够用，但论文必须补 jury + κ 校准（否则 reviewer 会质疑 LLM judge 可靠性）。

### 发现 4：lazy baseline 反向测试是 cp 设计的必要步骤

**证据**（POC4 坑5）：
- `filter_value_set` cp 用 [0.8, 3.2] 容差带 → lazy 随便选也能拿 0.667
- 容差太宽 = cp 被钻空子

**教训**：每个 Tier 1 cp 设计后，必须先用 lazy/random baseline 反向测一遍。若 lazy ≥ 0.5，cp 需要收紧。这是 task6 调研没料到的实践发现。

### 发现 5：src 框架可跨域复用但需扩展 kind

**证据**：
- POC4 直接复用 `_judge_value_set` + `_judge_selection`
- POC1 设计可直接用 `_judge_selection`（集合交集）
- POC2 需要新的 kind（物理校验：力收敛 / 对称性）

**结论**：`src/core/judge.py` 的 dispatch 机制可跨域复用，但需扩展 `_DISPATCH` 表（加 `physics_convergence` / `symmetry_conservation` 等 kind）。

---

## 3. 对 task6 调研结论的实证收紧/纠错

| task6 调研说法 | POC 实证 | 收紧/纠错 |
|---|---|---|
| "PhysicianBench 670 cp 可直接 retrofit" | POC3: ✅ 4/6 cp 可独立跑（不需 FHIR） | **收紧**：FHIR cp 需 Docker，LLM-judge cp 可单独验证 |
| "DO Challenge ~3-4 天改造" | POC1: 数据 clone + 分析 done，server 未起 | **部分验证**：离线可做，起 server 加 2-3 小时 |
| "MPtrj 零标注物理真值" | POC2: 设计确认真值可从帧数据直接算 | **确认**：力/应力/能量都是帧自带，不需人工标注 |
| "部分得分升为主分 ~1-2 行" | POC3: 确实 `passed/total` 现成 | **确认**：PhysicianBench `score_jobs.py` 已有，改 1 行 |
| "加权 vs binary 有信息量差" | POC3+4: ✅ 两域一致证实 | **强化**：不仅"有差"，而是 binary **完全不可区分** |
| "6 步方法学可复用" | POC4: ✅ 走通 | **确认 + 补坑**：6 坑实际跑了才知道（见 POC4 results.md） |

---

## 4. 论文实验的种子结果

### 已可用的种子

1. **加权部分得分 vs binary 信息量对比**（2 域 × 定量数据）
2. **Tier 1 铁律跨域可达性**（4 域 × 定性证据）
3. **6 步方法学走穿**（PharmaBench 实例 + 6 坑教训）
4. **src 框架复用度**（judge dispatch 跨域可用）

### 尚缺的种子（MVP 阶段必须补）

1. **真 agent trajectory**（POC3/4 的 mock → 真 LLM agent）
2. **过程量 vs 终答相关性**（Pearson/Spearman，需 ≥10 个 trajectory/域）
3. **jury ICC + κ 校准**（多 judge 一致性）
4. **L0-L3 介入敏感性**（4 档 prompt × 真 agent 的 cp 通过率变化）

---

## 5. 交付物状态

| # | 文件 | 状态 | 说明 |
|---|---|---|---|
| 1 | `poc1_do_challenge/results.md` | ✅ 设计+分析 | cp 链 7 cp，离线可行性分析 |
| 2 | `poc2_matbench_mptrj/results.md` | ✅ 设计 | cp 链 7 cp，物理真值零标注 |
| 3 | `poc3_physicianbench/results.md` | ✅ 实测 | 6 cp × 真 LLM，binary vs weighted |
| 4 | `poc4_pharmabench/results.md` | ✅ 实测 | 7 cp × 3 档 agent × 真 LLM |
| 5 | `evaluator/` | ✅ 骨架 | grader.py（聚合+LLM judge 封装） |
| 6 | `result.md` | ✅ 本文 | 综合判定 |

---

## 6. 下一步建议

| 优先级 | 动作 | 工作量 | 预期产出 |
|---|---|---|---|
| 1 | POC1 离线跑通（`run_poc1.py` + mock 3 档） | 半天 | 第 3 个域的 weighted vs binary 数据 |
| 2 | POC3/4 接真 agent loop | 2-3 天 | 真 agent trajectory → 过程量 vs 终答相关性 |
| 3 | POC2 装环境 + 跑 1 材料 | 1 周 | 材料域的实测数据 |
| 4 | jury + κ 校准 | 1-2 天 | LLM judge 可靠性证据 |
| 5 | 推进 χ-Bench / TheAgentCompany POC | 2-3 天/个 | 更多跨域验证 |
| 6 | 写 MVP 30 题构建计划 | 1 天 | 正式论文实验的路线图 |
