# POC 1 · DO Challenge · cp 链设计 + 离线分析 — Results

> **日期**: 2026-08-14 · **状态**: 🟡 设计完成 + 离线分析（server 未启动，阻塞于 Docker+MongoDB）

## 验证目标（来自 task7/goal.md）

> 在 DO Challenge 1M 分子虚筛任务上，验证"自定义预算约束 cp + 集合交集 grader + budget oracle 接口"的 retrofit 模式。

## 数据集概况

- **来源**: Deep Origin DO Events Challenge 2025（CC-BY-4.0）
- **规模**: ~1M 候选分子，真值 top 1000 标签
- **数据文件**:
  - `data/labels.pkl`（14MB）: 全量分子标签
  - `data/top1000_df.pkl`: top 1000 DataFrame
  - `DeepThought{1,2}_mappings.pkl`: ID 映射表
- **挑战机制**:
  - 每队有 token 预算（调 `lab_experiment(ids)` 获取标签花费 token）
  - 最终提交 3000–5000 个分子
  - 得分 = `|提交 ∩ 真前 1000| / 1000`
  - API: `remained_budget()` / `requested_ids()` / `lab_experiment([ids])` / `submit([ids])`

## 环境现状

| 组件 | 状态 | 说明 |
|---|---|---|
| Server 代码 | ✅ 已 clone | `do-events-challenge2025/`（Flask + MongoDB） |
| 数据 | ✅ 已下载 | `data/labels.pkl` + `top1000_df.pkl` |
| Docker | ❌ 未启动 | 需 `docker-compose up` 起 MongoDB + Flask |
| Client | ✅ 可用 | Python client `src/client.py` |
| 真 LLM agent | ❌ 未接 | POC 阶段用 mock agent |

## cp 链设计（7 cp，Tier 1 占 5/7 = 71% ✓）

| # | 阶段 | checkpoint | 档 | 类型 | 判定方法 | 离线可跑 |
|---|---|---|---|---|---|---|
| cp1 | 读题 | 靶点 JSON 字段齐全（UniProt/序列/位点） | T1 | 结构校验 | 字段存在性检查 | ✅ |
| cp2 | 策略 | 50K 预算花完时，已筛集前 1000 命中率 ≥ 20% | T1 | 集合校验 | `|已筛 ∩ top1000| / 1000` | ✅ mock trajectory |
| cp3 | 探索 | 采样 SMILES 全 RDKit 可解析 | T1 | 语法校验 | `Chem.MolFromSmiles()` | ✅ |
| cp4 | 探索 | 打分曲线随预算单调上升 | T1 | 排序校验 | 单调性检验 | ✅ mock budget trace |
| cp5 | 决策 | 换策略理由站得住 | T2 | 细则判官 | LLM rubric | ✅ 接真 LLM |
| cp6 | 提交 | `|最终 3000 ∩ 真 1000| / 1000` ≥ 阈值 | T1 | 集合校验 | 集合交集 | ✅ 读 pkl |
| cp7 | 报告 | 每条结论被前步骤数据支撑 | T2 | 细则判官 | LLM rubric | ✅ 接真 LLM |

### cp 链 trace 结构

```
读题(cp1) → 策略(cp2) → 探索(cp3, cp4) → 决策(cp5) → 提交(cp6) → 报告(cp7)
```

agent 的活动：
1. 解析靶点信息（JSON 输入）
2. 用预算调 `lab_experiment` 批量查分子标签（模拟实验）
3. 根据已知标签训练/调整打分模型
4. 中途可能换策略（如从随机采样切换到 scaffold-based）
5. 提交最终 3000 个分子
6. 撰写决策报告

## Mock Agent 3 档设计（沿用 POC4 模式）

| 档位 | 策略 | 预期 cp6 得分 | 预期整体 weighted |
|---|---|---|---|
| **perfect** | scaffold 多样性采样 → 命中率导向 → 提交全命中 | ~0.8+ | ~0.9+ |
| **medium** | 随机采样 + 简单过滤 → 部分命中 | ~0.3 | ~0.6 |
| **lazy** | 瞎选 3000 个 → 命中率 ≈ 0.3%（随机） | ~0.01 | ~0.2 |

## 离线评测可行性

### 可直接离线跑的 cp（无需 Docker）

- **cp1**（结构校验）：mock 一个靶点 JSON → 检查字段
- **cp2**（集合校验）：mock trajectory 记录每步查了哪些 ID → 与 top1000 求交集
- **cp3**（SMILES 合法性）：mock SMILES 列表 → RDKit 检查
- **cp4**（单调性）：mock budget-score 序列 → 检查单调
- **cp6**（集合交集）：mock 提交 3000 ID → 与 labels.pkl 求交集

### 需要 Docker 的

- 真 `lab_experiment` API 调用（token 预算机制）
- 真 `submit` API（得分计算）

### 需要真 LLM 的

- cp5 / cp7 的 LLM judge rubric 评测

## src 框架复用分析

| src 模块 | 用途 | 复用方式 |
|---|---|---|
| `core/judge._judge_selection` | cp6 集合交集 | 直接 dispatch（gold.correct = top1000 IDs, out.selected = 提交 IDs） |
| `core/judge._judge_ranking` | cp4 排序校验 | 适配：把 budget-score 序列编码成 ranking |
| `core/tasks.Stage` | 统一 stage 对象 | 直接用 |
| `core/conservation` | cp2 预算守恒 | 查 API 是否已有守恒检查 |

## 与 POC3/POC4 的对比

| 维度 | POC3 PhysicianBench | POC4 PharmaBench | POC1 DO Challenge（设计） |
|---|---|---|---|
| **域** | 医疗 EHR | 药物 ADMET | 药物虚筛 |
| **任务类型** | R（Retrofit） | B（Borrow & Build） | R（Retrofit） |
| **cp 数** | 6 | 7 | 7 |
| **Tier 1 占比** | 50% | 71% | 71%（设计） |
| **原生 cp 链** | ✅ 670 cp 自带 | ❌ 自建 | ❌ 无原生 cp 链（只有终态评分） |
| **数据 license** | Stanford DUA-gated | CC0 1.0 | CC-BY-4.0 |
| **环境复杂度** | 需 FHIR Docker | csv 即可 | 需 MongoDB Docker |
| **预算机制** | 无 | 30 次预测预算 | token 预算（核心特色） |

## 关键发现（离线分析）

1. **DO Challenge 的"预算"天然适配我们的"介入守恒"**：token 预算 = agent 可用资源，cp2 在 50K 预算时检查命中率 = 过程量而非终态量
2. **labels.pkl 可离线消费**：不需要起 server 就能做集合交集评测（cp6），这对 POC 验证足够了
3. **checkpoint_palette.md 的 cp 链设计可一字不改地落地**：从 task6 调研到 task7 设计，cp 链保持一致
4. **跨域复用 src judge 路径清晰**：`_judge_selection`（集合交集）直接可用

## 不验证什么（诚实边界）

- ❌ **没起真 server**：token 预算机制未实测
- ❌ **没跑真 agent**：mock 3 档，不验证 LLM 在长程 discovery 上的真实能力
- ❌ **没做 1M 规模测试**：mock 可能用小规模子集
- ❌ **没接真 LLM judge**：cp5/cp7 仅设计了 rubric，未实测

## 下一步

| 选项 | 工作量 | 收益 |
|---|---|---|
| **A** 写 `run_poc1.py` 离线评测脚本 | 半天 | 用 mock agent 跑通 cp1/cp2/cp3/cp4/cp6 |
| **B** 起 Docker + MongoDB | 2-3 小时 | 真 budget API 可用 |
| **C** 接真 LLM agent | 1-2 天 | 看真 LLM 在虚筛 discovery 上的 cp 分布 |
| **D** 跳到下一个 POC（χ-Bench / TheAgentCompany） | — | 更多跨域验证 |

**建议**：先做 A（离线跑通核心 cp），证明 retrofit 模式可行，再考虑 B/C。
