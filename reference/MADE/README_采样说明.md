# MADE — 采样说明（深度版）【闭环材料发现环境·oracle 预算下的序贯决策】

## 论文与来源
- 论文：*MADE: Benchmark Environments for Closed-Loop Materials Discovery*（Malik et al., Oxford；arXiv:2601.20996，ICML 2026）
- 代码：https://github.com/diffractivelabs/MADE ｜ 本地采样：gh raw，2026-07（README + LLM orchestrator + agent 基类 + 环境/agent 配置）

## 它是什么
**闭环材料发现的 benchmark 环境**：agent 在**受限 oracle 预算**下迭代"提议结构→拿能量评估→改进"，形式化为"在给定相图凸包（convex hull）上寻找热力学稳定化合物"。它是目前与我们的材料发现领域**最直接同域**的长程 benchmark。

## 一个完整 case（真实，`README_REPO.md` Quick Start）
> **Li-O 体系，random agent，ORB oracle，3 个 episode，每次 5 次查询预算。**
- **query（任务）**：在 Li-O 相图上，用 ≤5 次 oracle 查询，找到落在凸包上（热力学稳定）的化合物；
- **agent 行动**：每轮提议候选结构 → 环境调 oracle（ORB/MACE/解析式）算形成能 → 反馈 → 再提议；
- **产出**：`trajectories/episode_000.json`（完整轨迹）+ `summary/summary.json`（指标）。

## ★ 架构与判分机制（真实配置）
- **环境** `ConvexHullEnvironment`（`configs/environment/convex_hull.yaml`）：定义相图上的发现任务、预算、稳定判据（meV）；
- **Oracle**：形成能评估（ORB=MACE 类 ML 势，或解析式）——确定性物理量，hard；
- **Agent 组件化**（Hydra 配置拼装，真实文件）：
  - generator（`chemeleon` 生成模型 / `random` / `cached`）提议结构；
  - filter（`smact` 电荷中性 / `min_distance` 原子间距 / `uniqueness` 去重）；
  - planner（`llm.yaml` / `ucb` / `grid_search` / `diversity`）决定下一步探哪；
  - scorer（`oracle.yaml` / `llm.yaml` / `chain`）给候选排队；
  - **全 agentic 模式**：`llm_react_orchestrator.py`（56KB，LLM ReAct 编排器，把组件当工具自主调用）；
- **指标**：AF（加速因子）、EF（增强因子）、AUDC（发现曲线下面积）、mSUN——评的是**多快多省地找到稳定材料（发现效率）**，跨 episode 汇总均值/方差。

## 判分性质
- **hard**：能量 oracle + 凸包稳定性判定全为物理/数值计算；
- **评效率+终态发现质量**：闭环过程被记录（episode 轨迹），但判分锚在"找到了几个稳定材料、用了几次查询"，**不在中间 checkpoint 上判"每步做得对不对"**；
- agent 形态灵活：从固定 pipeline 到 LLM ReAct 全自主。

## 与我们的关系（最重要的参照系）
- **同域同闭环**：材料发现 + 迭代 + oracle 预算——它证明这个设定是 2026 年顶会关注点；
- **关键差异**：MADE 评"**发现效率**"（多快找到稳定材料），我们评"**过程正确性**"（每步 checkpoint 是否被正确完成、证据是否落地、顺序是否合规）——两者互补不冲突，我们的过程级 hard checkpoint 正是它没有的维度；
- **可复用资产**：凸包环境/稳定判据（energy above hull）与我们的稳定性 oracle 同概念；它的 smact/min_distance 过滤器可作我们候选池过滤的 oracle。

## 采样内容
- `README_REPO.md`（含 Quick Start 完整 case）；
- `src/llm_react_orchestrator.py`（56KB LLM ReAct 编排器）、`src/agents_base.py`；
- `configs/`：environment_convex_hull.yaml、dataset_phase_diagram.yaml、agent_llm_react_orchestrator.yaml、scorer_oracle.yaml、planner_llm.yaml。
