# Paper 研究与写作 Pipeline 调研记录

> 日期：2026-09-29
> 范围：本地 paper/research skill、公开的一手论文与开源项目、Orca 的调度能力
> 用途：为 AAMAS 2027 项目确定一套从 idea 到投稿审查的工作标准，不把自动写作器当成科学验证器。

## 1. 结论先行

本项目不需要再造一个“大而全”的科研 skill。现有能力可以拼成一条完整链：

1. `paper-research-pipeline` 管 idea、最近邻、数学/机制审查、structured debate、proposal card 和 experiment card。
2. `paper-writer` 管 B/M/E/D/R 阶段门、pre-results review、RQ 实验结构和 claim–evidence reconciliation。
3. `scholar-search`、`med-paper-search`、`agent-reach` 管文献发现与一手来源回查；`check-citations` 做投稿前引用闸门。
4. `paper-reviewer` 管投稿前的五角色独立红队审查；不能直接套它的 AAAI 评分数字，AAMAS 规则由项目自己的三份验收文档定义。
5. Orca 负责 Run、task DAG、worktree、worker、消息和 decision gate；它是执行与留痕层，不替代研究判断。

因此采用“**阶段门 + 结构化辩论 + 原始证据回填**”作为标准流程。全自动 agentic tree search 只保留为 idea 阶段的备选，不进入当前论文主实验链。

## 2. 本地 skill 盘点

| 能力 | 本地来源 | 适合做什么 | 不能替代什么 |
|---|---|---|---|
| 研究主线 | `paper-research-pipeline` | B/M/E/D/R 阶段、候选 idea、角色分工、debate、kill criterion、artifact schema | 真实实验和人工大方向裁决 |
| 成稿与结果 | `paper-writer` | benchmark/method freeze、pre-results、RQ 组织、结果回填、submission guard | 结果尚未出现时的经验结论 |
| 论文红队 | `paper-reviewer` | EIC、methodology、domain、perspective、devil's advocate 独立审稿与编辑综合 | AAMAS 的正式评分依据；不能把模型分数当录用概率 |
| 文献漏斗 | `scholar-search`、`med-paper-search`、`agent-reach` | 找候选、查摘要、回到论文/代码/数据一手来源 | 只引用 tldr 或搜索摘要 |
| 证据调研 | `deep-research`、`storm-research` | 多来源 triangulation、矛盾地图、证据打包 | publication-grade 结论若没有 source/evidence ledger |
| 引用闸门 | `check-citations` | Crossref/Semantic Scholar/OpenAlex 交叉核验 | 论文本身的 novelty 判断 |
| 写作与图 | `academic-writing`、`paper-figure-design` | 段落结构、语气、图表设计 | 研究问题和实验是否成立 |
| Orca 控制面 | `orca-cli`、`orchestration` | worktree、Run、task、worker、消息、DAG、decision gate | 研究真值、创新性和实验结果 |

`orchestration` 是 Orca 专用控制面，但目前没有发现“paper + Orca”官方一体化 skill。Orca 官方 skill registry 主要提供 `orca-cli`、`orchestration`、浏览器和线性工单等通用能力；paper 逻辑需要由项目自己的 pipeline skill 负责。

## 3. 已核对的外部工作

| 来源 | 可复用结构 | 对本项目的限制 |
|---|---|---|
| [Multiagent Debate, ICML 2024](https://proceedings.mlr.press/v235/du24e.html) | 独立回答 → 多轮质询 → 综合；可作为 no-debate/single-agent 之外的流程对照 | 该结果不证明辩论天然正确 |
| [Encouraging Divergent Thinking, EMNLP 2024](https://aclanthology.org/2024.emnlp-main.992/) | tit-for-tat、适度轮数、自适应停止；提醒异构 judge 偏置 | 固定多轮会浪费成本并放大共识漂移 |
| [Debate or Vote, NeurIPS 2025](https://proceedings.neurips.cc/paper_files/paper/2025/hash/934252acd87f254d5d4672fbde283bd2-Abstract.html) | 将 majority vote 与 debate 拆开比较 | 任何声称“debate 有收益”的实验都要有 vote/no-debate 反事实 |
| [Tree-of-Debate, ACL 2025](https://aclanthology.org/2025.acl-long.1422/) | claim/subtopic 节点、未解决问题驱动的动态展开、迭代检索 | 只借 claim graph 和停止条件，不把树搜索当主方法 |
| [STORM, NAACL 2024](https://aclanthology.org/2024.naacl-long.347/) / [代码](https://github.com/stanford-oval/storm) | 多视角检索、角色化追问、证据整理 | source bias 和错误关联仍需独立核验 |
| [VIRSCI, ACL 2025](https://aclanthology.org/2025.acl-long.1368/) / [代码](https://github.com/InternScience/Virtual-Scientists) | team organization、idea discussion、refinement、novelty assessment | novelty vote 只能排优先级，不能证明新颖 |
| [The AI Scientist](https://arxiv.org/abs/2408.06292) / [代码](https://github.com/SakanaAI/AI-Scientist) | idea→代码→实验→写作→模拟审稿的端到端脚手架 | 官方代码也提示 reviewer positivity bias；不能替代 raw evidence |
| [AI Scientist-v2](https://github.com/SakanaAI/ai-scientist-v2) | progressive tree search、experiment manager、失败节点记录 | 只适合探索分支，不能事后挑 winner |
| [PaperOrchestra](https://arxiv.org/abs/2604.05018) / [代码](https://github.com/google-research/paper-orchestra) | outline、literature、plotting、section writing、content refinement 的成稿流水线 | 它从 pre-writing materials 开始，不能替代我们的 B/M/E 研究门 |
| [Co-Scientist, Nature 2026](https://www.nature.com/articles/s41586-026-10644-y) | 专门 agent、异步队列、ranking/evolution、tournament | 专家验证规模和成本仍需审计，不把 tournament 当真值 |

项目历史上已经有一份四方独立评审的真实先例：`docs/inherited/0028-calibration管线v1四方评审裁决.md` 及其四份原始报告。它证明“先独立审查、再综合裁决、把杀实验前置”在本项目内已经可执行，不需要把旧争议重新讨论一遍。

## 4. debate 的正确定位

debate 是**研究决策的压力测试器**，不是创新性或真值的投票器。每个候选方向必须先写成可检验的 candidate card，再进入辩论。每条意见都必须绑定一个 claim/assumption，并给出证据、最小决定性实验和 kill criterion。

采用以下固定消息格式：

```text
POSITION: support | challenge | clarify | revise | unresolved
TARGET: claim / assumption / design choice
EVIDENCE: source ID + locator，或 NONE
REASONING: 证据为什么影响该目标
DECISION TEST: 什么最小观察会改变立场
CONFIDENCE: 0.0–1.0 + 理由
```

默认角色：literature scout、domain specialist、methods/statistics auditor、feasibility/compute auditor、devil's advocate、theory–experiment auditor。Round 0 独立产出，Round 1 逐条攻击高影响假设，Round 2 回应/修订/让步，coordinator 输出 claim graph 和 decision matrix。连续一轮没有新证据或新决定测试，或最多三轮，停止；未解决的少数意见必须保留。

只有在检验“debate 过程本身有没有价值”时，才运行 single-agent、no-debate、majority-vote、structured-debate 四个反事实，并记录 token、时间和错误报警。多数票只能表示意见趋同，不能表示科学结论成立。

## 5. Orca 适配结论

Orca 适合把这套流程变成可追踪 DAG：

```text
charter
  ├─ literature scout
  ├─ domain / data audit
  ├─ method / theory audit
  ├─ feasibility audit
  └─ devil's advocate
        ↓
debate round 1 → debate round 2 → coordinator synthesis
        ↓ human decision gate
proposal card → experiment card → execution → evidence reconciliation
        ↓
five-reviewer red team → editorial synthesis → revision gate
```

每个角色有自己的 Orca task/worktree，只写自己的报告；不允许 worker 直接改三份生效主线文档、benchmark manifest 或原始结果。coordinator 合并结构化意见，只有通过 decision gate 后才更新 canonical 文档。实验 worker 只能写 `task/`、`experiments/logs/` 和自己的分支；raw JSONL、配置、哈希和失败现场先于任何结果 prose。

当前本机的 Orca runtime 正在运行，但 `/usr/local/bin/orca` 是不可解析的 root symlink；直接调用报 `Unable to determine Orca.app path from symlink`，直接启动 app 又返回已有实例锁。此记录只说明 CLI 接入尚未完成，不把本轮 collaboration subagent 结果冒称为 Orca Run。恢复 Orca CLI 后，按同一 DAG 建立 Run；流程和 artifact schema 不变。

## 6. 对当前项目的落地顺序

当前 Gate A/B/C 均为 HOLD，所以 pipeline 从 benchmark/baseline 和方法候选的审查门开始，而不是从全自动 idea search 或新 A800 开始：

1. 建立当前 charter，锁定 fixed-pool delayed selected-only feedback、非主张和资源边界。
2. 对动态 JEV 候选做三方向独立 candidate cards，并完成数学/可计算性/反例审查。
3. 补齐 Gate A 的 endpoint qualification、provider lineage 和 confirmation 隔离。
4. 补齐 Gate B 的强近邻 baseline fidelity、统一 raw-run contract 和 Laya/正式降级处置。
5. 冻结 Gate C experiment card，先 CPU + independent verifier，再单条 Nebula A800。
6. 返回结果后做 evidence reconciliation，再进入论文写作和五角色红队。

完整执行标准已落在 `docs/论文研究与写作流程.md`；本记录只保存调研证据和采用理由。
