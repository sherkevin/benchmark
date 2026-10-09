# 材料发现长程任务 Checkpoint Benchmark · 专项调研报告

调研日期：2026-07-24 ｜ 方法：Deep Research（多渠道检索 + 官方仓库实地核验 + 真实样例落盘）
定位：回答"现有数据集够不够支撑材料发现长程 checkpoint benchmark"，并补齐材料方向缺口。
需求边界不变：**单 Agent + 一次性输入 + hard 中间 checkpoint**。

---

## 一、结论先行

**此前的 31 个数据集不够支撑材料方向——"材料 × 长程 × 过程量"三者同时满足的此前为零。** 本轮新找到并核验 4 个直接可用/可借鉴的候选（均已在 `reference/` 落盘采样），其中 **MADE 是靶心**（材料发现闭环环境，每步硬信号），**DiscoveryWorld 是 checkpoint 工程范本**（scorecard 原生过程分）。缺口从"空白"变为"有底座、待改造"。

## 二、缺口诊断（为什么此前不够）

| 维度 | 此前覆盖 | 缺口 |
|---|---|---|
| 材料领域 | 仅 MatBench/MPtrj（task7 POC，性质预测）、Tartarus/分子系（偏药物） | 无材料"发现流程"型任务 |
| 长程性 | SWE-bench/AgentBoard 等通用长程 | 无材料域多阶段 pipeline |
| 过程 checkpoint | SciCode/SUPER 等有 hard 卡点 | 无一个在材料任务上 |

## 三、本轮新增候选（全部实地核验，按匹配度排序）

| # | 数据集 | 一句话 | 单agent | 一次输入 | hard checkpoint | 数据可得性 |
|---|---|---|:---:|:---:|:---:|---|
| 1 | **MADE** ★靶心 | 闭环材料发现环境：oracle 预算内序贯提晶体→MLIP 算生成能→调策略，主指标=发现加速度 | ✓ | ✓ | ✓ 强（每步 oracle 生成能+稳定性阈值，可执行） | GitHub 全开源，已采样 |
| 2 | **MatTools** | 材料工具使用：每题 question + **properties 断言清单** + 单元测试，Docker 判分 | ✓ | ✓ | ✓ 强（性质=卡点，49题/138任务） | GitHub 全量，已采样真实三件套 |
| 3 | **DiscoveryWorld** | 科学发现全循环虚拟环境（120 任务含材料主题），**scorecard**：子目标逐项判 + `scoreNormalized` 过程分 | ✓ | ✓ | ✓ 强（子目标=环境状态判定；知识层除外） | GitHub 全开源，已采样官方 scorecard |
| 4 | **MatSciBench** | 1340 道材料推理题，带**分步参考解**，数值题规则判分 | ✓ | ✓ | ◐ 部分（数值 hard/公式 LLM；步级需自铸） | HF（本地因网络暂未落样例，schema 已核） |

各自详情、判分机制原文与真实 case 见 `reference/{MADE,MatTools,DiscoveryWorld,MatSciBench}/README_采样说明.md`。

## 四、考察过但不入选/次级参考

- **Matbench Discovery**（Nat. Mach. Intell. 2025）：晶体稳定性**筛选**基准，固定候选池、单步预测，无闭环——留作 oracle/数据源（figshare 全量可下）。
- **LLM4Mat-Bench**（npj 2025）：材料性质预测 QA，非长程。
- **HoneyComb / MatAgent / MARS**：是 agent **框架**而非 benchmark，无标准判分集。
- **PaRoutes**（45 万条多步逆合成路线，GitHub 可下）：化学邻域，多步路线+参考路线比对 hard——若扩展到"合成可行性"阶段可复用。
- **NIST 闭环 BO 平台 / A-Lab**：真实自主实验室，无法离线复现，仅方法学参考。

## 五、对我们构建的三条直接启示

1. **环境底座**：MADE 的「oracle 预算 + 每步硬信号 + 发现加速度」可直接作为材料长程题的执行环境或对照系——oracle 预算 ≈ 我们的介入预算，加速度 ≈ 过程量主指标。
2. **卡点范式**：MatTools 证明"**性质断言清单 = checkpoint**"在材料域可行；DiscoveryWorld 证明"**scorecard（子目标状态判定 + 归一化进展分 + 对 agent 隐藏卡点）**"是成熟工程形态——两者拼起来就是我们材料 pipeline 桩点的实现模板。
3. **组题原料**：MatSciBench 的分步参考解 + Matbench Discovery 的稳定性数据 + PaRoutes 的多步路线，可按「GT-first 反向建题」组装成材料发现多阶段长程题。

## 六、待办

- MatSciBench：经镜像补落 3-5 条真实样例（含 solution 分步解）。
- MADE：本地跑通一个最小 episode（random generator + oracle scorer），验证发现加速度指标可复算。
- DiscoveryWorld 材料主题任务：抽取其材料任务的 scorecard 定义，评估改造成本。

## 来源（全部经实际访问核验，2026-07）

- MADE: arXiv:2601.20996（前身 2506.13131）; github.com/diffractivelabs/MADE
- MatTools: arXiv:2505.10852; github.com/Grenzlinie/MatTools
- DiscoveryWorld: NeurIPS 2024 D&B Spotlight; github.com/allenai/discoveryworld
- MatSciBench: arXiv:2510.12171; github.com/Jun-Kai-Zhang/MatSciBench; HF JunkaiZ/MatSciBench
- Matbench Discovery: arXiv:2308.14920, s42256-025-01055-1; github.com/janosh/matbench-discovery
- LLM4Mat-Bench: arXiv:2411.00177 ｜ PaRoutes: github.com/MolecularAI/paroutes ｜ HoneyComb: EMNLP 2024 Findings
