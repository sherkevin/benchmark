# MolClaw/MolBench 本地资产索引（按「对我们 goal 的可复用性」组织）

> 这是从 github.com/InternScience/MolClaw 完整拉到本地的可复用素材（2026-06-02，`git clone --depth 1`）。
> 评估结论见 `archive/task2/result.md`；事实核查见 `search/molbench_dataset_findings.md`。
> 下面按「能怎么推进我们的自主性度量 benchmark」标注每块资产。

## ⭐ 最高价值：过程脚手架（直接降低 PRD §5.1 专家标注成本）

| 本地路径 | 是什么 | 对我们的用法 |
|---|---|---|
| `skills/L2_workflows/` （14 个 .md）| **通用「金标准过程模板」**：阶段分解、显式 CHECKPOINT、成功判据、失败-恢复表、轨迹记录要求 | 当作**金标准轨迹 / 阶段 checkpoint / 介入卡的草稿骨架**，专家在其上「填空+审核」而非从零创作 |
| `skills/L3_methodology/` （2 个 .md，26 条原则/8 章）| 学科方法论原则（Count-Before-Report、Computation-first…）| 转成**纯过程评分 rubric**（方法学合规、是否引用工具数值而非编造）|
| `skills/L1_tools/` （60 个 SKILL.md）| 原子工具的 I/O 契约（docking/admet/boltz2/esmfold/reinvent/fpocket…）| 定义**合法工具调用动作空间**，用于轨迹级过程打分与中断分类 |
| `skills/LR_research/` （13 文件）| 文献检索/深研工作流（pubmed/wiki/multi-search + deep-research.md）| 「计算优先、文献辅助」层的参考；可借鉴我们的证据核验维度 |
| `skills/system_prompt_FULL.md` | MolClaw 完整系统提示 | 看它如何「把介入前置进 harness」——我们「介入守恒」论点的现成实证 |
| `skills/auto-generated-skills/skill-index.md` | 技能结晶索引（运行中自动沉淀）| 参考「成功轨迹→可复用技能」机制 |

## 数据与来源（PRD §2.2 seed；可绕过 MolBench 直接对接一手源）

| 本地路径 | 内容 | 备注 |
|---|---|---|
| `data_full/` | 释放的 190 条样本（MS-1/2/3 csv + MO json + manifest），目录结构与上游 `molbench/data/` 对齐 | 单轮 QA、仅最终答案；可抽 seed（靶点/ChEMBL ID/分子）|
| `config/*.yaml` | 跑测配置 | 标了数据源：`rdkit_bench`(CARA→ChEMBL) / `acnet_curated`(ACNet) / `molbench_vs` / `ChemCoTBench` |
| 一手源（外部，未拉取）| **CARA→ChEMBL、ACNet、ChemCoTBench** | 论文写明出处；我们做 seed 应直接对接这些，而非 MolBench 二次打包 |

## 评测与运行（参考实现，**非我们要的过程量评测**）

| 本地路径 | 内容 | 对我们的用法 |
|---|---|---|
| `eval/eval_runner.py` `run_eval_bench.py` `bench_loaders.py` | 纯最终答案评测器（抽 `<answer>` → hit@3/delta/scaffold）| 反面参考：我们要替换成「插桩-喂介入卡-续跑」的回路 |
| `templates/answer_format.md` `tool_routing.md` `variable_routing.md` `template.py` `outliners.py` | 输出契约 + 工具/变量路由 | 复用 `<finish>/<answer>` 契约与工具端点接法 |
| `environment.yaml` `env.template` | 运行环境 | 复现/容器化 harness（PRD §5.6）|

## ⚠️ 没拿到、需另想办法的（PRD 最相关的部分）

- **MolBench-E2E 数据 + 评分 rubric（Supplementary S5–S7）**：仓库无、论文附录有。E2E 是唯一长程、且已按「过程 rubric」打分的部分 → **建议向作者索取**（GitHub issue / 邮件）。
- **MO 的 drd/gsk/jnk（40 条靶点优化）**：被作者以「易被背诵、记忆污染」为由剔除，未释放。
