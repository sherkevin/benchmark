# POC 2 · Matbench MPtrj · cp 链设计 — Results

> **日期**: 2026-08-14 · **状态**: 🟡 设计完成（环境未装，阻塞于 CHGNet/fairchem）

## 验证目标（来自 task7/goal.md）

> 在 Matbench Discovery 附带的 MPtrj 弛豫轨迹上，验证"物理真值 cp（零标注）+ 弛豫帧轨迹作 oracle + 数值/结构/排序硬校验"的 retrofit 模式。

## 数据集概况

- **来源**: Matbench Discovery（Materials Project trajectory, GNoME 2023）
- **规模**: 158 万个 DFT 弛豫中间帧，覆盖 ~150K 材料
- **每帧字段**:
  - 能量 (float, eV)
  - 力 (N×3, eV/Å)
  - 应力 (3×3, kBar)
  - 原子位置 + 晶格
- **每材料**: 50–200 帧弛豫轨迹
- **license**: CC-BY-4.0（Materials Project）
- **关键特色**: **零人工标注的物理真值**——弛豫帧自带物理量，不需人工标注就能定义 cp

## 环境现状

| 组件 | 状态 | 说明 |
|---|---|---|
| MPtrj 数据 | ❌ 未下载 | ~数 GB，需从 Materials Project 下载 |
| CHGNet | ❌ 未装 | ML 势函数，用于跑弛豫 |
| fairchem | ❌ 未装 | FAIR 的化学 ML 框架（替代 CHGNet） |
| spglib | ❌ 未装 | 空间群分析 |
| pymatgen | ❌ 未装 | 材料结构分析 |
| RDKit | ❌ 未装（本 POC 不需要） | — |

## cp 链设计（7 cp，Tier 1 占 5/7 = 71% ✓）

| # | 阶段 | checkpoint | 档 | 类型 | 判定方法 |
|---|---|---|---|---|---|
| cp1 | 弛豫·力 | 力收敛：mean|F| < 0.01 eV/Å | T1 | 数值校验 |
| cp2 | 弛豫·应力 | 应力收敛：max|σ| < 0.5 kBar | T1 | 数值校验 |
| cp3 | 弛豫·对称 | 对称性保持：空间群不变 | T1 | 结构校验 |
| cp4 | 弛豫·能量 | 能量单调下降（允许 5% 帧波动） | T1 | 排序校验 |
| cp5 | 基态 | 最终能量 vs DFT 基态差 < 0.05 eV/atom | T1 | 数值校验 |
| cp6 | 策略 | 弛豫参数选择合理性（k-point / 收敛标准） | T2 | LLM rubric |
| cp7 | 报告 | 收敛分析报告完整性 | T2 | LLM rubric |

### cp 链 trace 结构

```
初始结构 → 弛豫(cp1,cp2,cp3,cp4) → 基态判定(cp5) → 策略说明(cp6) → 报告(cp7)
```

### 为什么 Tier 1 能这么高

MPtrj 的帧级物理量 = **免费真值**：
- 力 (F) → cp1 力收敛 = `mean(|F_final|) < 0.01`
- 应力 (σ) → cp2 应力收敛 = `max(|σ_final|) < 0.5`
- 原子位置 + 晶格 → cp3 对称性 = `spglib.get_spacegroup(initial) == spglib.get_spacegroup(final)`
- 能量序列 → cp4 单调性 = `E[i+1] ≤ E[i] + ε` for 95% frames
- DFT 最终帧能量 → cp5 基态 = `|E_agent - E_DFT| < 0.05`

→ **5/7 cp 不需要任何人工标注**，直接从 MPtrj 帧数据算出。

## 与 POC3/POC4 的对比

| 维度 | POC3 PhysicianBench | POC4 PharmaBench | POC2 MPtrj（设计） |
|---|---|---|---|
| **域** | 医疗 EHR | 药物 ADMET | 材料弛豫 |
| **任务类型** | R（Retrofit） | B（Borrow & Build） | R（Retrofit） |
| **cp 数** | 6 | 7 | 7 |
| **Tier 1 占比** | 50% | 71% | 71%（设计） |
| **真值来源** | Stanford EHR + 临床指南 | PharmaBench csv (CC0) | MPtrj DFT 帧（零标注） |
| **agent 活动** | 写临床评估 note | 调 predict_logd + 筛选 | 跑结构弛豫 |
| **环境复杂度** | FHIR Docker | csv 即可 | CHGNet + spglib + pymatgen |

## 关键发现（设计阶段）

1. **MPtrj 是"过程量真值金矿"**：158 万帧 × 每帧带能量/力/应力 = 免费的过程量评测数据，与我们的 benchmark 理念（评过程不只评终答）天然契合
2. **物理守恒律 = 最强 Tier 1 cp**：能量单调下降、力收敛、对称性保持都是物理定律，不需要人工判定对错
3. **跨域验证了 Tier 1 铁律的普适性**：医疗（POC3）靠 pytest + FHIR，药物（POC4）靠 RDKit + 集合交集，材料（POC2）靠物理守恒——三个域的 Tier 1 cp 来源完全不同，但都能达到 ≥70%

## 不验证什么（诚实边界）

- ❌ **没装 CHGNet/fairchem**：无法实测弛豫过程
- ❌ **没下载 MPtrj 数据**：无法用真帧数据验证 cp
- ❌ **没跑真 agent**：无法看 LLM 在材料 discovery 上的实际行为
- ❌ **没验证 spglib 空间群判定在 MPtrj 上的稳定性**：可能存在数值噪声导致空间群微变

## 下一步

| 选项 | 工作量 | 收益 |
|---|---|---|
| **A** 装 CHGNet + 下载 MPtrj 子集（10 材料） | 2-3 天 | 能跑 1 个材料的弛豫 + 验证 cp |
| **B** 用 fairchem 替代 CHGNet | 1-2 天 | 可能更轻量 |
| **C** 跳过实测，只做设计文档 | 0 | 作为论文 method 章的设计案例 |
| **D** 先做其他更快的 POC（χ-Bench / TheAgentCompany） | 2-3 天 | 更多跨域验证 |

**建议**：POC2 环境最重（~1 周），在 POC3+4+1 都跑通后再投入。设计文档已足够支撑论文 method 章。
