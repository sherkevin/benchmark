# SciAgentArena（Agent4Science·dd）— 采样说明（深度版）【~200 科学 agent 任务·分步验证·代码即答卷】

## 论文与来源
- 论文：*Benchmarking AI Agents for Addressing Scientific Challenges Across Scales*（Liu et al.，Yale/Harvard 等 33 人；arXiv:2606.12736，2026-06）
- 代码：https://github.com/HelloWorldLTY/SciAgentArena ｜ 主页：sciagentarena.github.io
- 本地采样：gh raw，2026-07（顶层 README + dd 域 README/evaluate/registry + 3 个真实参考 agent + ground truth 样例）

## 它是什么
跨域科学 agent 竞技场：约 200 个真实科研任务，**分步验证（stepwise verification）+ agent 无关的交互环境**。其中 `evaluations/dd`（drug discovery 域，又称 Agent4Science）是最贴我们的部分：**79 个分子/蛋白任务，agent 写并执行 Python 代码，框架在受控环境跑代码并给输出判分**。

## 一个完整 case（真实，`dd_examples/agent_c4_tox.py`，C4 安全评估任务 reg_01_toxicophore）
- **query（输入）**：agent 从环境变量 `AGENT4S_INPUT_JSON` 读到任务载荷（含 `query_molecules_file_path`——一批待筛分子的 CSV）；
- **agent 行动**（参考实现，真实代码）：用 RDKit SMARTS 逐分子匹配毒性警示结构（nitroaromatic `[a][N+](=O)[O-]`、michael_acceptor、epoxide、quinone 等 7 类），输出严格的逐分子 JSON（含 alert_type/SMARTS/匹配原子索引/风险等级）到 stdout；
- **判分**：由 `oracle_regulator.score_toxicophore` 对照 ground truth 打分。

## ★ 判分机制：四级链条 + 五种任务模式（真实 `dd_README.md`）
- **统一判分链**：`executability → validity → correctness → strategic_success`——先查代码能不能跑，再查输出合不合法，再查内容对不对，最后查策略优不优（**分步/分层的 hard 判分**，与论文"stepwise verification"对应）；
- **五类任务（79 个）**：
  - C1 化学数据预处理（18，batch：算 MW/LogP/TPSA/形式电荷/相似度……）；
  - C2 化学数据分析（26，notebook：SAR/assay 分析画图与命名变量）；
  - **C3 分子优化（10，design：预算制 oracle——QED/penalized logP/DRD2/MPO/scaffold hop/rediscovery，真实 `oracle_budget_*`，超预算抛 `OracleBudgetExceededError`）**；
  - C4 化学安全评估（6：毒性警示/hERG/PAINS/代谢软点/反应性基团）；
  - C5 化学声称验证（19：识别不可信数据，校准地回答"inconclusive"而非自信地错）；
- **判分器**：`oracle_chem/vis/interactive/regulator/validity` + ground truth（`scorers/data/*_ground_truth.csv`，本地已采 Formal_Charge 样例）；
- **判分性质**：hard（代码执行 + 数值/结构比对），离线判分（`AGENT4S_NO_NETWORK=1`），无 LLM judge；支持 Biomni/ChemCrow/ChemToolAgent/Cactus/DrugAgent 等框架 agent 接入。

## 与我们的关系
- **最接近"过程判分"的现存者之一**：四级链条把"能跑→合法→正确→策略"分层判，比纯终态进一步；C3 的**预算制 oracle** 与我们的预算约束同构；
- **差异**：它的任务多为**单轮代码生成-执行**（agent 写一段代码跑完即判），不是"长程多阶段 + 每阶段 checkpoint 的过程追踪"；四级链条判的是"代码产物的分层质量"，不是"执行轨迹的逐步正确性"——我们的 Mode B/工具锚定仍是空白；
- **可复用资产**：C4 的毒性 SMARTS 警示库、C3 的预算制 oracle 包装（pytdc）、ground truth 组织方式，可直接当我们的医药 oracle 与负例素材。

## 采样内容
- `README_REPO.md`、`dd_README.md`（79 任务五类全貌）、`dd_evaluate.py`（统一 CLI）、`dd_registry.py`（任务路由）；
- `dd_examples/agent_c4_tox.py`（毒性筛查真实 agent）、`agent_c3_qed.py`（预算制 QED oracle 真实用法）、`agent_sar.py`；
- `scorers_data/Formal_Charge_easy_001_ground_truth_head.csv`（ground truth 样例）。
