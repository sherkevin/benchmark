# ADR 0028 · Calibration 训练管线 v1 四方评审裁决：主任务换轨、杀实验前置、多项设计作废

| 字段 | 值 |
|---|---|
| **状态** | Accepted |
| **日期** | 2026-09-02 |
| **决议人** | jingwu（综合 codex 四方评审：methodology / literature / rl-stability / devils-advocate） |
| **影响范围** | 提案 §3.4 训练闭环的全部后续修订；阶段 1 前置检验集扩充 |

## 1. 上下文

2026-09-02 下午，就 calibration 训练管线 v1（Stage1 mask+性质轮换 / Stage2 SFT 带 c / Stage3 RL accept/verify/arbitrate 奖励表）派出 4 个独立评审 agent（方法论、文献、RL 稳定性、devil's advocate）。评审原文入库：`governance/review_20260902_calibration_pipeline/`（4 份）。交叉综合后有 3 处共识级发现 + 6 条独立攻击，全部有出处。

## 2. 共识级发现（≥3 方独立命中）

| # | 发现 | 性质 | 出处 |
|---|---|---|---|
| F1 | **离散 accept/verify 奖励表的最优策略是常数动作**。E[accept]=p(1+λ)−λ，E[verify]=p−ε，接受阈值 p*>1−ε/λ（ε=0.1,λ=2 → 0.95）。RL 会"正确地"收敛到永远 verify。不是风险，是定理（Elkan 2001 cost-sensitive threshold） | 结构错误 | 4/4 方 |
| F2 | **Novelty 基本被前置工作吃光**。"verbalized confidence + outcome reward + proper scoring rule + 跨任务迁移"已由 Rewarding Doubt（ICLR 2026）完整发表并声称"general confidence awareness 涌现"；Taming Overconfidence（ICLR 2025）、DCPO、RefGRPO、RLMF、ReCalibrate 直接重叠；masking 泛化见 Tool-R0 / Agentic-DPO；schema misalignment 失败模式见 PA-Tool。"content-conditioned"一词本身已是 arXiv 已发表术语（2608.14641 / 2606.26130） | 框架作废 | literature |
| F3 | **732 分子量级下，最强可学信号大概率是 ensemble 方差，不是 content**。用随机标量替换全部化学内容、只保留方差结构，transfer 测试仍会"看起来成功"（方差 domain-general）——false positive 结构 | 杀实验对象 | devils + methodology |

## 3. 独立攻击（各 1 方，全部接受）

- **A1 性质轮换是范畴错误**（methodology）：只有 BBB 有实验真值，把 BBB 预测重标为 logP/solubility 是喂假标签。**作废 property rotation，改 LOTO**（leave-one-tool-out）。
- **A2 剥工具身份把 MDP 变 POMDP**（rl-stability）：observation 相同而可靠性不同的两个输入，policy 被迫 miscalibrate。⇒ **训练面保留工具身份；迁移压力测试只在评测时做遮蔽**。
- **A3 GRPO 在 collapse 点零梯度**（rl-stability）：局部 advantage 归一化是 biased estimator。⇒ 换 REINFORCE++ 或 PPO+critic。
- **A4 ε/λ 不是超参，是业务成本比**（rl-stability）：必须锚定真实部署经济学（一次湿实验 vs 一次计算调用），并做敏感性扫描；方向7 调研已给出机械锚推导（有效区间 (0.157, 1.59]）。
- **A5 meta-cognitive ceiling 循环**（devils）："不知道正确答案却知道预测何时错"就是 calibration 本身，训练循环假设了被训能力。⇒ 主任务必须改成不依赖"哪个工具对"的形式（见决策 D1）。
- **A6 transfer claim 结构性不可证伪**（devils）：化学域可测但没人在乎，通用域在乎但没真值。⇒ 迁移证据必须落在**有真值的异质域**（数学 QA / 代码执行 / 搜索冲突），不是无真值的"真实场景"。

## 4. 决策

- **D1 主任务换轨**：可训练主任务从"选/信哪个工具"（γ_ik 交互项驱动，绑定工具，不可迁移）改为**"这组预测能不能拿去做决策"（α_i 分子主效应驱动）+ 选择性作答（answer/abstain/escalate）**。此即为提案 §3.4.1 二难裁定的落地：选工具任务里"有信号"与"可迁移"定义层互斥；判断可用性任务里信号是分子属性，天然跨工具。C 信号（选错且本可选对）保留，但用途从监督目标降级为**取证触发与审计字段**。
- **D2 杀实验前置**：任何 GPU 训练之前，先过五条零机时/近零检验，顺序不可颠倒：①方差分解（α_i 占比+标签噪声翻转率）；②裸答检索天花板+恒答基线；③经典方法擂台（同表随机森林/conformal）；④**scalar-only/content-shuffle 消融**（ensemble 方差基线，若打平全管线则 content 贡献为零，方向重做）；⑤纯采样基线（pass@k，区分锐化与真实增益）。
- **D3 作废清单**：property rotation（A1）；训练面遮蔽工具身份（A2，遮蔽移到评测侧）；离散 accept/verify/arbitrate 三动作 RL 奖励表（F1）；GRPO（A3）。
- **D4 保留清单**：c 绑行动 + Brier proper scoring rule（但按 F1 解耦——训连续 P(usable)，阈值部署时按成本比定）；多 schema 共训/verbalized sampling（防 Simulator Collapse，arXiv:2608.12253）；抗锐化定向强化（罕见但正确的轨迹可被误差表无歧义标定，arXiv:2506.02355/2510.11686 谱系，**待验**）。
- **D5 叙事重定位**：贡献从"calibration 方法创新"降级为**数据生态位创新**——化学是唯一提供大规模客观金标准冲突训练场的域（Rewarding Doubt 在事实 QA 上做过校准迁移，但无工具冲突、无下游决策经济、无真值稀缺域到稀缺域的迁移证据）。

## 5. 不动的部分

- 提案 §1–§3.3 评测层（四类任务、数据源、测评框架）一字不动——评审打的是训练闭环，不是评测机。
  - **2026-09-02 晚解冻（owner 授权）**：为解决「跳步多、依赖前置知识、单文件读不懂」的问题，§3 整章重排为七站数据流（站 0 账本 → 站 1 真值体检 → 站 2 造题 → 站 3 答题与判分 → 站 4 前置检验与杀实验 → 站 5 训练 → 站 6 评测与迁移）。解冻范围**仅限结构与措辞**：四类任务、数据源清单、测评框架、全部数字与出处原样搬运，无新增事实。同时新增站 1（真值体检）作为造题前的阻塞闸门，理由见下条。
  - 新增站 1 的依据：2026-09-02 真值核查发现 TDC `BBB_Martins` 有 11 个分子标签自相矛盾，其中 8 个已进 730 账本、7 个落在 400 冲突集内，且入库取值由源文件行序决定。本 ADR §5 沿用的「AURC/E/五条基线/(0.157,1.59] 区间」全部以真值当裁判，故在清污前一律标记为待重算。
- 方向7 调研的风险-覆盖曲线指标集与 c 预注册档位表（AURC/E/诊断率/五条基线/(0.157,1.59] 区间）原样沿用。
- §3.4.1 方差分解二难、§3.4.3 真值只作验证器、§3.4.6 前置检验三条保留并扩充为五条。

**本 ADR 自身的一处更正**：§2 的 F3 写"732 分子"，账本实测为 **730** 行（`wc -l screening_ledger.jsonl` 与 `summary.json` 的 `eligible_screened_rows` 均为 730）。结论不受影响。

## 6. 触发动作

1. 写提案 v2（`提案_20260902_制药工具冲突选择性作答训练管线_v2.md`），吸收本决议。
2. 跑阶段 1 五条前置检验（零机时），全部通过前不申请 GPU。
3. 调研 Rewarding Doubt / DCPO / RefGRPO 的精确 delta，写进 v2 相关工作。

## 7. 后续影响

- 训练目标若连阶段 1 都过不了，方向降级为纯 benchmark 论文（评测层不受影响）。
- 杀实验 D2-④ 若显示 scalar-only 打平全管线，则"content-conditioned"叙事作废，剩余贡献 = 数据生态位 + 选择性作答协议。

## 8. 相关

- 提案：`../提案_20260901_制药科学预测冲突诊断与恢复Benchmark.md` §3.4
- 评审原文：`../review_20260902_calibration_pipeline/`（4 份）
- 方向7 调研：`../调研_20260902_方向7_工具遮蔽与风险覆盖曲线.md`
- 前置检验三条原案：`../讨论_20260901_工具冲突诊断题型的三条前置检验.md`
