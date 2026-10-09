# task7 POC — 工作笔记

> 2026-08-14 · 逐 POC 分析 + 综合判定

---

## 1. 项目当前状态总览

### 已完成

| POC | 状态 | 验证了什么 | 核心发现 |
|---|---|---|---|
| **POC3** PhysicianBench | ✅ 机制层 done (2026-06-10) | 加权部分得分 vs binary Pass@1 | weighted=0.500 vs binary=0，信息量差明确 |
| **POC4** PharmaBench | ✅ B 类样板 done (2026-06-11) | 6 步方法学走穿 + 7cp×3档 agent | weighted_by_tier: 1.0/0.794/0.456 vs binary: 1/0/0 |

### 未完成（需分析+设计）

| POC | 状态 | 阻塞原因 | 本次能做什么 |
|---|---|---|---|
| **POC1** DO Challenge | 代码 clone + 数据下载 | 需 Docker+MongoDB 起服务 | cp 链设计 + 数据结构分析 + 离线评测脚本 |
| **POC2** Matbench MPtrj | 目录未建 | 需 CHGNet/fairchem 环境 | cp 链设计 + 方法学文档 |
| **evaluator/** | 目录未建 | — | 共享 grader 模块设计 |
| **result.md** | 未写 | 需 POC 结果 | 基于 POC3+4 实测 + POC1+2 设计的综合判定 |

---

## 2. POC3 (PhysicianBench) 复盘

### 核心数据
- 6 个 cp：3 通过（cp2 LLM-judge, cp3 LLM-judge, cp6 documentation）+ 3 失败（cp1 trajectory 格式不符, cp4/cp5 FHIR 未接）
- **binary Pass@1 = 0**（需 6/6 全过）
- **weighted partial = 0.500**（3/6）
- 信息量差：binary 把"3/6 通过的中等 agent"和"0/6 通过的弱 agent"打同分 → 完全不可区分

### 工程坑
1. `_llm_client()` 硬编码不传 base_url → monkeypatch 一行解决
2. Hybrid cp 的"串行短路"：trajectory 缺则 LLM 不被调用
3. 4/6 cp 完全独立于 FHIR → 可单独验证

### 对论文的实证价值
- ✅ 部分得分公式（`passed/total`）可无缝嫁接到现成 benchmark
- ✅ LLM-judge cp 接真 LLM 单次响应 <3s，JSON 解析 100% 成功
- ❌ 没跑真 agent → 不验证 src 的 5 签名输出

---

## 3. POC4 (PharmaBench) 复盘

### 核心数据
- 7 个 cp（5 Tier 1 + 2 Tier 2 LLM judge），Tier 1 占比 71%（≥70% 铁律 ✓）
- 3 档 agent × 7 cp：

| agent | binary | weighted_equal | weighted_by_tier | n_pass |
|---|---|---|---|---|
| perfect | 1 | 1.000 | 1.000 | 7/7 |
| medium | 0 | 0.734 | 0.794 | 5/7 |
| lazy | 0 | 0.413 | 0.456 | 1/7 |

- **关键**：binary 把 medium/lazy 都压成 0 → weighted_by_tier 给出 0.34 的鲜明差距

### 6 步方法学验证
1. 解析数据 → CC0 csv 完全结构化（最容易档）
2. 定 e2e 任务 → virtual screen 自然出 5 阶段
3. 拆阶段 → 策略/探索/决策/提交/报告
4. 挖 cp → 7 个（守恒+结构+数值+集合+rubric）
5. 锁 GT → CC0 1.0 ✓，无污染风险
6. 派 L0-L3 → 借 drug_admet.json 模板

### 6 个坑（实际跑了才知道）
1. env 命名分裂（OPENAI_API_BASE vs OPENAI_BASE_URL）
2. src judge 输入 schema 不对齐真值 → 编码适配
3. LLM judge 输出不可控（围栏/json 前缀）→ 鲁棒解析器
4. `_judge_selection` 不是经典 F1 而是 Jaccard-flavor
5. **容差太宽 → lazy agent 蒙混 0.667**（最重要教训）
6. src 不原生支持 LLM judge kind → 内联更干净

### B 类成立性判断
- 🟢 结构化 csv 类（PharmaBench / LLM4Mat-Bench）：6 步直接套
- 🟡 半结构化（Syntheseus / MolBench seed）：Step 1+2 需扩展
- 🔴 散在论文文本：MVP 阶段不建议

---

## 4. POC1 (DO Challenge) 分析

### 数据结构（从 server 代码推断）
- `labels.pkl`：14MB，包含 ~1M 分子的标签（真前 1000 名）
- `top1000_df.pkl`：top 1000 分子 DataFrame
- `DeepThought1_mappings.pkl` / `DeepThought2_mappings.pkl`：ID 映射表
- 挑战机制：
  - 1M 候选分子，每队有 token 预算（调 `lab_experiment` 获取标签要花费 token）
  - 最终提交 3000~5000 个分子
  - 得分 = `|提交 ∩ 真前 1000| / 1000`（集合交集）

### cp 链设计（基于 checkpoint_palette.md 样板）

| # | 阶段 | checkpoint | 档 | 类型 | 判定方法 |
|---|---|---|---|---|---|
| 1 | 读题 | 靶点 JSON 字段齐全 | T1 | 结构校验 | 字段存在性检查 |
| 2 | 策略 | 50K 预算花完时，已筛集前 1000 命中率 ≥ 20% | T1 | 集合校验 | 集合交集 |
| 3 | 探索 | 采样 SMILES 全部 RDKit 可解析 | T1 | 语法校验 | `Chem.MolFromSmiles()` |
| 4 | 探索 | 候选打分曲线随预算单调上升 | T1 | 排序校验 | 单调性检验 |
| 5 | 决策 | 中途换策略理由站得住 | T2 | 细则判官 | LLM rubric |
| 6 | 提交 | 最终 3000 ∩ 真 1000 / 1000 | T1 | 集合校验 | 集合交集 |
| 7 | 报告 | 每条结论被前步骤数据支撑 | T2 | 细则判官 | LLM rubric |

- Tier 1 占比 5/7 = 71% ✓

### 阻塞与解决路径
- **阻塞**：需 Docker + MongoDB 启动 challenge server
- **离线可做**：
  - 读取 labels.pkl 获取真值
  - 用 mock agent（类似 POC4 的 3 档设计）模拟 trajectory
  - 跑 cp 链判分离线版
- **解决路径**：写 `run_poc1.py`，直接读 pkl 文件做离线评测

### 离线 cp 链可跑性评估
- cp2（预算命中率）：需 trajectory 记录 → mock 可模拟
- cp3（SMILES 合法性）：需 RDKit → 离线可跑
- cp4（打分单调性）：需预算-分数序列 → mock 可模拟
- cp6（最终集合交集）：需 labels.pkl → 已有
- ✅ 核心 cp（除 cp5/cp7 LLM judge）均可离线跑

---

## 5. POC2 (Matbench MPtrj) 分析

### 数据特征（来自 task6 调研）
- MPtrj = Matbench Discovery 附带的 158 万个 DFT 弛豫中间帧
- 每帧带：能量 (float) / 力 (N×3) / 应力 (3×3)
- 每材料 50–200 帧弛豫轨迹
- **零人工标注**的物理真值

### cp 链设计

| # | 阶段 | checkpoint | 档 | 类型 | 判定方法 |
|---|---|---|---|---|---|
| 1 | 弛豫 | 力收敛：mean|F| < 0.01 eV/Å | T1 | 数值校验 | 帧级力范数 |
| 2 | 弛豫 | 对称性保持：空间群不变 | T1 | 结构校验 | spglib 比对 |
| 3 | 弛豫 | 能量单调下降（允许小波动） | T1 | 排序校验 | 序列单调性 |
| 4 | 基态 | 最终能量 vs DFT 基态能量差 < tol | T1 | 数值校验 | 数值比对 |
| 5 | 策略 | 弛豫参数选择合理性 | T2 | 细则判官 | LLM rubric |
| 6 | 报告 | 收敛分析报告完整性 | T2 | 细则判官 | LLM rubric |

- Tier 1 占比 4/6 = 67%（略低于 70% 铁律，需再挖一个 T1）
- 补充 cp7：应力收敛（max|σ| < tol）→ T1，提升到 5/7 = 71% ✓

### 阻塞
- 需装 CHGNet 或 fairchem（ML 势函数）做弛豫
- 工作量大（~1 周），POC 阶段可先设计不跑

---

## 6. 跨 POC 综合发现

### 6.1 部分得分 vs binary — 跨域一致性 ✅

| POC | 域 | binary 区分度 | weighted 区分度 | 结论 |
|---|---|---|---|---|
| POC3 | 医疗 EHR | 0 vs 0（无区分） | 0.500 vs ~0.17 | ✅ 加权透出 PARTIAL |
| POC4 | 药物虚筛 | 1/0/0（中/弱压平） | 1.0/0.794/0.456（差距 ≥ 0.2） | ✅ 加权有鲜明区分 |

→ **两个独立域一致证明**：binary Pass@1 在过程量评测中信息量不足，加权部分得分跨域有效。

### 6.2 Tier 1 铁律跨域可执行性

| POC | Tier 1 占比 | 铁律满足 | 备注 |
|---|---|---|---|
| POC3 | 50%（3/6） | ❌ 但 repo 自带，非我们设计 | FHIR cp 算 Tier 1 但环境依赖 |
| POC4 | 71%（5/7） | ✅ | 6 步方法学主动挖到 |
| POC1 设计 | 71%（5/7） | ✅ | checkpoint_palette 设计 |
| POC2 设计 | 71%（5/7） | ✅ | 物理真值免费 T1 |

→ Tier 1 ≥ 70% 在 **主动设计** 的 POC 中均可达标；被动 retrofit 现有 benchmark 时不一定满足（如 POC3）。

### 6.3 src 框架复用度

- POC3：未复用 src（PhysicianBench 自带 pytest 框架）
- POC4：复用 `core/judge._judge_value_set` + `_judge_selection` + `core/tasks.Stage` → ✅ 直接 dispatch
- POC1 设计：可复用 `_judge_selection`（集合交集 cp6）+ `_judge_ranking`（排序 cp4）
- POC2 设计：需新写物理校验器（力收敛 / 对称性），但可复用 Stage 框架

→ **src 的 judge dispatch 机制跨域可用**，但需扩展 kind（物理类 / 守恒类）。

### 6.4 LLM judge 一致性

- POC3：Qwen 单 judge，cp2/3/6 通过，JSON 解析 100%
- POC4：Qwen 单 judge，strategy/report rubric 区分度高（perfect 1.0 vs medium 0.167 vs lazy 0.0）
- **未做**：jury（多 judge 一致性 ICC）、κ 校准

→ LLM judge 单 judge 可用，但 jury/κ 校准是论文必须补的。

### 6.5 关键教训（从坑里学到的）

1. **lazy/random baseline 反向测试是必须的**（POC4 坑5）：cp 设计后先跑 lazy，若 lazy ≥ 0.5 说明 cp 被钻空子
2. **LLM 输出格式不可控**：必须配鲁棒解析器（剥围栏 + json 前缀）
3. **env 命名兼容**：OPENAI_API_BASE vs OPENAI_BASE_URL 要做映射 fallback
4. **src judge 语义要确认**：`_judge_selection` 是 Jaccard-flavor F1，不是经典 F1

---

## 7. 对论文实验的预期贡献

| 论文实验 | 本 task7 提供了什么 | 还缺什么 |
|---|---|---|
| cp 设计跨域一致性 | ✅ 2 域实测 + 2 域设计 | 更多域实测（材料 POC2） |
| 部分得分 vs binary 信息量 | ✅ 2 域量化对比 | 统计显著性（N > 1 题/域） |
| 过程量 vs 终答相关性 | ❌ 未验证（没跑真 agent） | 真 agent trajectory |
| 介入守恒 | ❌ 未验证 | L0-L3 真跑 |
| LLM judge 可靠性 | ✅ 单 judge 可用 | jury ICC + κ 校准 |

---

## 8. 下一步优先级

1. **POC1 离线跑通**：写 `run_poc1.py`，读 labels.pkl + mock agent 3 档，跑 cp2/cp3/cp4/cp6
2. **result.md 初稿**：基于 POC3+4 实测 + POC1+2 设计，写综合判定
3. **POC2 环境准备**：装 CHGNet/fairchem → 跑 1 个材料的弛豫
4. **真 agent loop**：把 mock 换成 LLM agent（POC3/4 优先）
5. **jury + κ 校准**：多 judge 一致性测试
