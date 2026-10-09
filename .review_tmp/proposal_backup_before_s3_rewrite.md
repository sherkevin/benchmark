# 制药科学预测冲突诊断与恢复 Agent Benchmark：Proposal

## 1. 背景与动机

###  1.1 基本事实
1. 当前制药 Agent 已能调用大量分子性质、对接、亲和力、ADMET、蛋白结构和成药性预测工具。但这些工具多为统计学习或模拟计算，即使正常返回、格式正确、数值看似合理，也会因训练偏差、适用域变化、构象与参数假设等产生误差。多个工具可能对同一性质给出矛盾结论，甚至在错误方向上形成“伪共识”。

2. 化学信息学的工具生态极度碎片化，同一功能有一堆互不兼容的实现，而且差异大到有文献专门讨论。最典型的是 logP：RDKit 的 Crippen 算法、XLogP3、ALOGPS、ChemAxon 对同一个分子能差出 1-2 个 log 单位——这直接决定这分子是"成药性好"还是"成药性差"。你不用编造冲突，去跑一遍就有。这是天然存在的，且在制药领域这个问题尤为突出

### 1.2 研究动机

本项目的出发点是一个关于 agent 工具使用的 **一般性观察** ，以及它在制药域的一个**可测量外显**。

**A（本质问题，一般层）**：训练后 agent 的工具偏好分布 ≠ 任务最优分布。根本原因：训练-推理 harness 不一致 + 注意力向熟悉 token 集中，导致 agent 对训练时见过的工具过度自信、对功能等价的新工具系统性回避。这其实是一个很 general 的问题，可以称之为 **工具过拟合**，本质上是在训练的时候见过一些工具过多，在后续真正的生产环境工作的时候会更偏好这种工具，和这些工具的契合度更好，但是外部工具是多种多样的，其价值不取决于 agent 在训练时候见过没见过，而是取决于对于任务本身的裨益，这就产生了一个 gap，也就是本身的模型任务指标和其训练的 gap

**B（外显，行为层）**：在 K 个功能重合的工具间，agent 偏向"更熟悉的"而不是"在这个任务上更可能有效的"。因为工具过拟合的一就是对工具使用过于自信，这就是问题 A 的外显

**C（信号，数据层）**：要捕捉这个外显的 B 行为，就要有一个可衡量的信号可量化这个行为的程度，当 Agent 根据自己的偏好选择对应的工具后若该工具出现了结果错误，即捕捉到了该信号，我们要做的就是对该信号进行建模


我们本质上是想要解决问题 A，问题 A 是一个根本问题，是一个 general 的问题，在这里就是工具过拟合问题，没办法直接来解决它而只能通过他的外显来间接缓解它，表现 B 就是问题 A 的一个外显，这个 B 就是选工具有偏好，这个 B 动作会产生一个信号就是 C，也就是工具选错，这个选错的判断来自于其输出结果不对，从而我们拿到了一个信号，通过建模这个信号并将其看作一个与 loss 关联的值，通过消掉 loss 去解决 B，从而缓解 A，这是一个比较清晰的故事线，而且是有价值的，因为其最终指向了一个 general 的问题。


在叙述上，我们说真正关键的问题不是 Agent 能否处理超时或异常，而是：**当科学工具正常工作但结果不可靠时，Agent 能否利用同功能工具和其他相关性质发现异常、定位可疑属性、选择更可靠的结果，并恢复后续研发决策。** 这直接影响虚拟筛选、先导优化、蛋白/抗体设计和候选优先级判断的可信度。

## 2. 相关工作及不足

已有工作分别覆盖了这一问题的部分环节：

- **Mind the Tool Failures**^[1]^：在医学影像中研究多个诊断工具的实例级互补与仲裁，但不涉及分子或蛋白计算。
- **Verify-or-Trust**^[2]^：测试 Agent 是否应验证有时会自信犯错的生物基础模型，但主要是“预测器—实验数据复核”，不是多个同功能工具间的科学仲裁。
- **Protein-Structure Trust-Routing**^[3]^：测试是否应相信 Boltz-2 的结构置信度，并以实验结构为真值，但主要围绕单一预测器。
- **BioDesignBench**^[4]^：证明蛋白设计 Agent 的主要短板之一是评价维度不足和过早停止，强制进行结构、界面、物理和亲和力多指标评价可以提升表现；但全部为 in silico，也没有控制性冲突标签。
- **MolBench/MolClaw**^[5]^：覆盖长程小分子多工具工作流和质量检查，但主要评价任务完成度，没有单独评价“发现冲突—定位错误—选择替代工具—恢复决策”。
- **TxBench-PP**^[6]^：使用真实的小分子临床前药理数据评测 Agent 的证据分析和研发判断，但不直接测试多个科学预测工具之间的冲突诊断与恢复。
- **ToolBench-X**^[7]^与 **MemToC**^[8]^：分别研究不可靠工具环境中的冲突恢复，以及模型记忆与工具返回冲突后的仲裁；但缺少制药知识、真实科学模型误差和实验真值。

因此，目前仍缺少一套面向制药场景、以真实科学模型误差为冲突来源、同时评价跨性质诊断和同功能工具替换，并由实验数据提供隐藏真值的 Agent Benchmark。

## 3. 我们要做什么

### 3.1 核心任务

首版同时覆盖小分子与蛋白/抗体，设置四类递进任务：

1. **同性质冲突识别**：多个 docking、affinity、ADMET 或结构预测工具给出不同结果，Agent 判断是否存在实质冲突。
2. **跨性质异常诊断**：利用构象合理性、相互作用、置信度、溶解性、聚集、稳定性等互补性质，判断哪个属性或工具最可疑。
3. **预算约束复核与仲裁**：Agent 在有限成本下选择同功能替代工具或正交验证工具，并选出更可信的结果。
4. **下游决策恢复**：在修正错误后，重新完成 hit 排序、lead 选择、蛋白设计筛选或 go/no-go 决策。

超时、空返回和异常可作为次级鲁棒性测试，但不作为主要研究动机。

### 3.2 数据收集

**小分子数据**

- **Hit 发现与湿实验验证**：CACHE^[9]^ 提供面向真实靶点的候选提交、实验测定和公开结果，可构造虚拟筛选排序、错误共识和命中恢复任务。
- **物化性质与自由能**：SAMPL^[10]^ 提供 pKa、logD、溶剂化及结合自由能等盲测数据，适合比较多个性质预测或自由能工具的系统误差。
- **对接、构象与亲和力**：D3R^[11]^ 和 CASF-2016^[12]^ 提供蛋白—配体 pose、affinity ranking、scoring 与 screening 数据，可构造 docking/affinity 冲突。
- **活性和 ADMET 扩展数据**：ChEMBL^[16]^、BindingDB^[17]^ 和 Therapeutics Data Commons（TDC）^[18]^可补充大规模生物活性、结合常数、吸收、分布、代谢、毒性等 endpoint，用于训练、校准和外推测试。
- 对同一分子—靶点运行多个真实计算工具，保存原始输出、置信度、参数、软件版本、运行日期和失败日志，形成可复现的“工具—预测—实验真值”三元组。

**蛋白与抗体数据**

- **抗体亲和力与成药性**：AIntibody^[13]^ 提供盲测设计、实验 affinity 以及表达、热稳定性、聚集和多反应性等 developability 测量，适合构造跨性质冲突。
- **蛋白结构与复合物界面**：CASP^[19]^、CAPRI^[20]^ 和 FoldBench^[14]^可用于结构、蛋白—蛋白/抗体—抗原界面及 all-atom 预测冲突；RCSB PDB^[21]^提供实验结构真值。
- **突变效应与蛋白适应度**：ProteinGym^[15]^ 汇集深度突变扫描数据，可测试 Agent 是否能在多个 fitness/stability 预测器不一致时进行复核和恢复。
- 对相同序列或复合物运行 AlphaFold、Boltz、Chai、ESMFold、Rosetta 及界面、稳定性、聚集和成药性工具，并保存模型置信度、构象、输入模板与 MSA 条件。

**统一与切分原则**

- 统一实体、质子化状态、构象、单位、实验条件和 endpoint 定义，避免把“定义不同”误标为“工具错误”。
- 不以随机篡改数值为主，而从真实工具残差中抽取自然分歧、错误共识、OOD、活动悬崖和高置信度错误；必要的受控注入也应按真实误差分布生成。
- 按 scaffold、蛋白家族、靶点和时间进行开发集、校准集、冻结测试集和隐藏外推集划分，实验真值只保留在私有 grader 中。
- 对每个来源记录许可证、可再分发范围和下载日期；无法公开再分发的数据只发布构建脚本、样本标识及评测接口。

### 3.3 测评框架与实验设置

Agent 在可执行沙箱中获得初始预测和工具目录，可自主调用同功能或互补工具；每个调用有成本，且不能访问隐藏实验真值。系统记录完整工具回执、参数、证据引用和决策时间。

评价分为：

- 冲突发现率与误报率；
- 错误属性/错误工具定位准确率；
- 复核工具选择质量与验证预算效率；
- 最终仲裁准确率及相对“实例级理想选择器”的 regret；
- 相对最佳固定单工具、多数投票和简单 ensemble 的 recovery gain；
- 恢复后的排序、筛选或 go/no-go 决策正确率；
- 置信度校准、合理拒答和证据可追溯性。

基线包括：始终相信首个工具、最佳固定工具、多数投票、平均/排名融合、传统 ensemble、不确定性路由、规则化一致性检查、领域专家和实例级 oracle。实验需比较小分子/蛋白、同性质/跨性质、不同冲突强度、自然误差/受控误差及不同预算，并进行多次重复和分组 bootstrap。


### 3.4 从评测到能力提升：训练闭环

#### 3.4.1 四方评审的交叉结论与逐条处置

评审对象：训练管线 v1（工具遮蔽+性质轮换 → SFT 带置信度 → RL 离散 accept/verify/arbitrate 奖励表）。交叉结论：

| # | 发现 | 性质 | 处置 |
|---|---|---|---|
| F1 | 离散奖励表最优策略是**恒 verify**：E[accept]=p(1+λ)−λ、E[verify]=p−ε，接受阈值 p*>1−ε/λ（ε=0.1、λ=2 → 0.95）。Elkan (2001) cost-sensitive threshold 定理的直接推论，非训练风险 | 结构错误 | **校准与决策解耦**：训练只产连续 P(usable)，answer/abstain/escalate 阈值部署时按预注册成本比导出 |
| F2 | verbalized confidence + proper scoring rule + 跨任务迁移已由 **Rewarding Doubt（ICLR 2026）** 发表；Taming Overconfidence（ICLR 2025）、DCPO、RefGRPO、RLMF、Tool-R0、Agentic-DPO、PA-Tool 均直接重叠 | novelty 重定位 | 声称降级为**数据生态位 + 任务重构**（§3.4.6），不声称 calibration 方法创新 |
| F3 | 730 分子 × 4 工具（2920 点）量级下，最强可学信号可能是 **ensemble 方差**而非分子内容；方差 domain-general，迁移测试会假阳性 | 待验，可判死 | **杀实验前置**（§3.4.5）：scalar-only / content-shuffle 消融先于一切 GPU 训练 |

独立攻击（各 1 方，全部接受）：

| # | 攻击 | 处置 |
|---|---|---|
| A1 | 性质轮换是范畴错误：只有 BBB 有真值，重标为 logP/solubility 是喂假标签 | **作废 property rotation**，改 LOTO（leave-one-tool-out）做 identity vs content 消融 |
| A2 | 训练面剥掉工具身份会把 MDP 变 POMDP，policy 被迫 miscalibrate | **训练面保留工具身份**；遮蔽只作评测期压力测试 |
| A3 | GRPO 局部 advantage 归一化在 collapse 点给零梯度（biased estimator） | 换 **REINFORCE++** 或 PPO+critic（arXiv:2501.03262） |
| A4 | ε/λ 不是超参，是业务成本比 | 锚定真实部署经济学 + 敏感性扫描（沿用方向7 调研的机械锚与档位表） |
| A5 | meta-cognitive ceiling：让弱 agent 评判更专业的工具，逻辑循环 | 主任务换轨后消解（§3.4.2）：判"能不能用"是分子难度判断，非工具评判 |
| A6 | "迁移到无真值真实场景"结构性不可测 | 迁移证据改落**有金标准的异质域**（§3.4.6） |

#### 3.4.2 主任务：选择性作答（answer / abstain / escalate）

工具 k 在分子 i 上的误差可分解为 `e_ik = μ + α_i + β_k + γ_ik + ε_ik`（α_i 分子主效应 / β_k 工具主效应 / γ_ik 交互 / ε 噪声），由此得**不可绕的二难**：

| 若主任务是 | 答案由哪项决定 | 换一套工具后 |
|---|---|---|
| 从 K 个预测里选最准的（argmin_k e_ik） | **γ_ik**（绑定具体工具） | **失效** |
| 判断"这组预测能不能拿去做决策" | **α_i**（分子的属性） | **仍有效** |

若坚持"选工具"，则"能迁移"与"任务有信号"定义层互斥。故可训练主任务定为**选择性作答**：

| 项 | 内容 |
|---|---|
| 输入 | 分子 x（结构/指纹/基本性质）+ 可见工具子集 V 的预测值 + 任务句 |
| 输出 1 | **u ∈ [0,1]**：P(这组预测可用)——训练的唯一监督标量 |
| 输出 2 | **g ∈ {answer, abstain, escalate}**——部署时由 u 按预注册阈值导出，训练不直接监督 |
| 输出 3 | 若 g=answer：最终判定 + 简要依据 |

三个设计推论：

1. **u 是连续标量**——离散动作空间会把成本阈值烤进 policy（F1）；连续 u + 部署期阈值使同一模型适配不同成本环境。
2. **C 信号（选错且本可选对）降级为审计与反事实样本触发器**——不直接监督 u，用于生成"为什么这次判错"的反事实负例进拒绝采样池。
3. **§3.1 四类评测任务全部保留**——它们量失败模式；进训练环的只有选择性作答。

#### 3.4.3 训练信号与算法

| 组件 | 决定 | 对应评审攻击 |
|---|---|---|
| 监督目标 | 连续 u，Brier / 对数 scoring rule（严格 proper ⇒ 诚实报告是唯一全局最优） | F1 |
| RL 奖励 | R = 1[最终判定对] − β·Brier(u, 实际对错) − γ·(工具调用数 − 1) | F1、A4 |
| 冷启动 | **拒绝采样自举**（禁 teacher、禁蒸馏——沿用 08-31 决议两条硬排除） | — |
| RL 算法 | REINFORCE++ 或 PPO+critic；**不用 GRPO** | A3 |
| 抗锐化 | 误差表可无歧义标定"罕见但正确"的取证序列，做定向强化（arXiv:2506.02355 / 2510.11686 谱系，**待验**）；任何提升必须同报**纯采样基线**（pass@k，arXiv:2510.14901） | — |
| 成本比 c/λ | 部署期阈值；评测沿用方向7 预注册档位 {0.25, 0.5, 1.0} + 哨兵 {0.1, 2.0}，跨档 Kendall τ 报排名稳定性 | A4 |

**支撑文献（已核实）**：纯"答对给奖励"的 RL 抬不了上限（arXiv:2504.13837 NeurIPS 2025 oral；2507.14843、2510.15990 同向证否）；有正面证据的一类是修正优化偏差（2506.02355 诊断出常见算法只强化高概率轨迹）。本项目的特殊优势：有误差表，"罕见但正确"的轨迹可无歧义标定——数学题上做不到，这是方法层面的可能贡献点（**待验**）。

#### 3.4.4 阶段 1 前置检验五条（顺序不可颠倒，全过才进训练，零机时/近零）

| 序 | 验什么 | 不通过意味着 |
|---|---|---|
| 1 | 方差分解：α_i 占比；扰动 y_i 的分类翻转率 | α_i 占比低 ⇒ 主任务不成立；翻转率高 ⇒ 在拟合实验噪声 |
| 2 | 裸答检索天花板 + 恒答基线 | 能背 ⇒ 测的是记忆；恒答分高 ⇒ 必须同报弃答率 |
| 3 | 经典方法擂台：同表随机森林 / conformal | RF 打平 ⇒ 差异只能落在主动取证，静态判断不得宣称贡献 |
| **4** | **杀实验：scalar-only + content-shuffle 消融**（§3.4.5） | 打平全管线 ⇒ content 贡献为零，方向重定 |
| **5** | **纯采样基线（pass@k）** | 训练提升不超纯采样 ⇒ 是锐化不是增益 |

#### 3.4.5 杀实验与消融矩阵（先于一切 GPU 训练）

730 分子、scaffold split、报 bootstrap CI：

| 模型 | 输入 | 判读 |
|---|---|---|
| A 全管线 | 分子结构 + 推理轨迹 + 4 工具预测 | 主方法 |
| **B scalar-only** | 仅 4 个预测标量（逻辑回归即可），无任何化学内容 | **B≈A ⇒ content 叙事作废，方向重定** |
| C identity-only | 工具名 + 4 预测，无内容 | C>A ⇒ identity 比 content 重要 |
| D random | — | 下界 |
| E conformal | conformal prediction 基线 | 经典擂台（检验 3 同族） |
| **F content-shuffled** | 预测与分子错配（断 content-校准链接、留预测值分布） | **性能不变 ⇒ 用的是方差不是内容** |

另加 **LOTO**：3/4 工具训、第 4 个测；换工具重测必须同报**残差相关系数**（防同家族假迁移——4 工具中 3 个同属 DeepPurpose，误差相关是已核实风险）。

#### 3.4.6 迁移证据与 novelty 重定位（对应 F2、A6）

- **迁移落点改为有公开金标准的异质域**：数学 QA（GSM8K/MATH，Rewarding Doubt 同源域，作负面对照）、代码执行（HumanEval/MBPP 类）、检索冲突。"迁移到无真值真实场景"按定义不可测，只作未来方向声明，不进贡献清单。
- **可声称的贡献（真实但增量）**：① 数据生态位——化学是目前唯一能大规模提供"客观实验金标准 × 真实多工具冲突 × 下游决策经济"三元组的域；② 任务重构——γ_ik→α_i 的选择性作答是定义层改进；③ 冲突来源真实——误差来自真实模型残差、逐条带实验真值、可重建。
- **与最直接先例的 delta**：Rewarding Doubt 在事实问答上做校准迁移（abstract 已核实），无工具冲突、无 abstain/escalate 决策经济、无真值稀缺域→稀缺域证据。本方案补的是这个角落。

#### 3.4.7 预期结果与止损点（全部待验，非预设结论）

| # | 假设 | 判据 |
|---|---|---|
| H1 | 选择性作答 AURC 优于 Ref-A/Ref-B；E 同时低于恒不升级（0.15717）与不高于恒升级（c） | 弱信号，仅证明选择性预测在化学冲突上有信号 |
| H2 | **杀实验 B≈A 不成立**（剥掉化学内容性能显著下降） | 整条故事线的命门 |
| H3 | LOTO 下迁到第 4 个工具仍优于 random，且同报残差相关 | 泛化证据 |
| H4 | 数学 QA / 代码执行域 zero-shot 优于基线 | 迁移证据 |
| H5 | H2 或 H4 失败 ⇒ 贡献退化为"首个带实验真值的选择性作答 benchmark + 校准不迁移的实证负结果" | 负结果亦可发表 |

**止损点**：阶段 1（含杀实验）结束时若 scalar-only 打平全管线，停止训练线，把已有设施整理为纯 benchmark 论文（10950 场景 + 五条强制基线 + 预注册代价表 + 杀实验结果即最小可发表单元）。

**数据底座现状（已核实，2026-09-02）**：`cases/_methodology/tool_conflict_census/bbb_authoritative_v2/` 已产出真实多工具误差表（730 分子 × 4 工具：admet_ai_1.4.0 + DeepPurpose cnn/morgan/mpnn；400 行跨体系冲突、330 行无冲突；每行带实验真值；复测漂移 0；12 份 run 回执 + 内容指纹密封）。评测协议（场景生成 730×15=10950、风险-覆盖曲线、E 指标、五条强制基线、c 预注册档）已在 `调研_20260902_方向7_工具遮蔽与风险覆盖曲线.md` 全部机械定义并实测。


***

## 4. 期望结论与分析

以下为待验证假设，而非预设结论：

1. 更强的通用 Agent 未必更谨慎，可能更稳定地盲从专业工具。
2. 单一置信度不足以判断预测是否正确；工具分歧、适用域和跨性质约束联合使用更有效。
3. 实例级工具选择有望超过最佳固定工具，但多数投票在工具误差高度相关时可能失效。
4. 强制多指标评价可提高恢复率，但验证越多不一定越好，存在明显的准确率—成本权衡。
5. 确定性校准门控、Agent 推理与二者组合可能分别适用于不同风险和预算场景。
6. Agent 即使得到正确最终答案，也可能经历错误诊断或幸运恢复，因此必须同时评价过程和结果。


### 4.1 资源预算：模型、token、算力（2026-09-02 补）

> 本节回答一个问题：把 §3.4 的完整管线（五条前置检验 → 杀实验 → SFT → RL → 迁移验证）跑完，需要什么模型、多少 token、多少算力。所有数字均为**估算值**，逐条标出依据；依据来自已落盘实测（`cases/_methodology/模型时延边界实测报告.md`、`governance/讨论_20260901_工具冲突诊断题型的三条前置检验.md`）的会单独注明。

#### 4.1.1 各阶段模型需求与 token 估算

| 阶段 | 内容 | 用什么模型 | token 估算（含依据） | 算力 |
|---|---|---|---|---|
| **阶段 0** 误差表 | 已完成：730 分子 × 4 工具 × 3 轮复测 | admet_ai（本地）+ DeepPurpose cnn/morgan/mpnn（本地） | 不计 LLM token（全是本地模型推理） | **已完成**，零 LLM 成本 |
| **阶段 1** 五条前置检验 | 方差分解 / 裸答天花板 / 经典擂台 / 杀实验 / 纯采样基线 | 检验 1、3、4 全部**无需 LLM**（随机森林、逻辑回归、conformal）；检验 2、5 需要 LLM | 检验 2（裸答天花板）：730 分子 × ~1k token/次 ≈ **0.7M token**；检验 5（pass@k，k=8）：730 × 8 × ~1k ≈ **5.8M token** | 检验 1、3、4 零 GPU；检验 2、5 约 6.5M token，API 或本地小模型均可 |
| **杀实验**（阶段 1 第 4 条） | 六模型消融（A 全管线 / B scalar-only / C identity-only / D random / E conformal / F content-shuffled） | B、D、E、F **无需 LLM**（逻辑回归 / conformal / 随机）；A、C 需要 LLM | A、C 各 730 × ~2k token（含推理轨迹）× 2 配置 ≈ **2.9M token** | 约 2.9M token |
| **阶段 2** SFT 冷启动 | 拒绝采样自举：对每个 episode 采样 N 条轨迹、按结果过滤、留正确且校准的 | 开源模型（Qwen3-8B 级，本地推理，零 API 费） | 730 episode × 8 采样 × ~3k token/轨迹 ≈ **17.5M token**（生成）+ 过滤后 ~2–3M 进 SFT | 本地推理，~17.5M token；SFT 本身 ~1 张 A100 × 数小时 |
| **阶段 3** RL（REINFORCE++ / PPO） | 在 episode 分布上 rollout、按 R = 1[对] − β·Brier − γ·成本 更新 | 同上（Qwen3-8B 级）+ reward 验证器（本地查台账，零 LLM） | 730 episode × 16 rollout/episode × ~3k token × ~5 epoch ≈ **87.6M token**（rollout 生成） | 本地推理 ~87.6M token；RL 训练 ~4 张 A100 × 1–2 天 |
| **阶段 4** 迁移验证 | 数学 QA（GSM8K/MATH 子集）+ 代码执行（HumanEval/MBPP）+ 检索冲突 | 训后模型 | 3 域 × ~500 题 × ~1k token ≈ **1.5M token** | 约 1.5M token |
| **评测**（贯穿） | 10950 场景（730 × 15 遮蔽子集）跑训后模型 + 五条基线 | 训后模型 + 基线（基线全是规则，零 LLM） | 10950 × ~2k token ≈ **21.9M token** | 约 21.9M token |

**token 总量（估算）**：约 **138M token**（0.65 + 2.9 + 17.5 + 87.6 + 1.5 + 21.9 ≈ 132M，向上取整到 138M 留 buffer）。其中 ~63% 集中在阶段 3 的 RL rollout。

#### 4.1.2 成本锚点（已落盘实测）

- **API 路线**：本项目现行实测成本为"一轮 12 个 run 计 3.74 美元、26 分钟"（纯 API + 本地 RDKit，见 `讨论_20260901_工具冲突诊断题型的三条前置检验.md`）。按此外推，若全走 API（以 GPT-4o 级 ~$2.5/M input、$10/M output 计），138M token 量级约 **$500–1500**，视 input/output 比例与模型档位而定。
- **本地路线**：阶段 2/3/4 全部可用 Qwen3-8B 级开源模型在本地推理（实测 qwen3.7-max 在流式下可稳定跑完 758s、9 万字符的长思维链，见 `模型时延边界实测报告.md`），LLM token 成本归零，代价是 GPU 时间（RL 阶段约 4 张 A100 × 1–2 天）。
- **混合路线（推荐）**：阶段 1/2/评测用 API（省事、量小），阶段 3 RL 用本地开源模型（量大、需反复 rollout）。此路线总 API 成本压到 **$50 以内**，GPU 成本集中在 RL。

#### 4.1.3 三条不确定性与止损

1. **RL token 估算方差最大**：87.6M 是按"730 episode × 16 rollout × 5 epoch"估的；若杀实验后 episode 需扩量（§3.4 数据底座风险：730 偏小），这一项线性放大。止损：杀实验（阶段 1 第 4 条）不过，阶段 2–4 不启动，token 消耗冻结在 ~10M 以内。
2. **模型选择待验**：Qwen3-8B 是否足以承载"选择性作答"能力，需阶段 2 冷启动后看拒绝采样的保留率；若保留率 <5%，换更大模型（14B/32B）并同比例上调 GPU 预算。
3. **评测 token 与遮蔽场景数挂钩**：10950 是 730 × 15 的全组合；若阶段 1 发现分子池需扩量，这一项同步放大。


## 5. 对制药领域的贡献

本项目不是再做一个新的 docking、ADMET 或蛋白结构模型，而是建立这些工具之上的**可靠性评测层**：

- 提供首套系统化的制药科学预测冲突诊断与恢复任务；
- 量化不同 Agent 在真实模型误差下的可信工具使用能力；
- 为小分子和蛋白药提供可复用的自然冲突数据、实验真值和评测协议；
- 帮助研发团队判断何时可以相信计算结果、何时必须复核、应调用什么工具；
- 推动制药 Agent 从“会调用工具”走向“会质疑、会验证、会恢复科学决策”。

## 6. 未来可继续推进的方向

> ⚠️ **2026-09-02 更新**：下列八条中的**第 1、2、5、6 条已上收为主线**（第 3 条降为判分侧配套），  
见 §3.4。本节原文保留不动，仅作方向清单与出处。第 4、7、8 条仍为未来方向，  
其中第 4 条与本项目「单 agent」硬边界冲突、第 8 条与 08-28「收敛到制药」裁定冲突，**本轮不启动**。

1. **学习实例级工具可靠性**：为每个工具建立随分子 scaffold、蛋白家族、任务 endpoint 和适用域变化的正确率模型，而不是使用固定工具排名；重点优化高置信度错误和错误共识的识别。^[1–3]^
2. **预算感知的主动复核**：将工具调用视为有成本的信息获取问题，让 Agent 学会选择预期信息增益最高的同功能或正交工具，并研究准确率、时延和算力之间的 Pareto 前沿。
3. **跨性质约束推理**：建立制药知识图谱或可执行约束层，把结构、亲和力、物化性质、ADMET、稳定性和成药性联系起来，帮助 Agent 区分真实矛盾、单位/定义差异和合理的条件依赖。
4. **规划—执行—审计分工**：探索 planner、tool executor 和 scientific auditor 的多角色架构；审计角色专门检查证据来源、参数一致性、未验证假设和替代解释，降低过早停止和工具盲从。^[4–5]^
5. **面向恢复过程的训练**：利用自然冲突轨迹和受控反事实样本进行监督微调、偏好优化或强化学习；奖励应同时覆盖正确诊断、合理复核、成本控制、校准和最终决策，而不只奖励最终答案。^[7–8]^
6. **动态工具生态与漂移监控**：允许工具新增、升级或版本变化，评测 Agent 能否重新校准可靠性并发现性能漂移，避免把历史经验永久固化为错误先验。
7. **人机协同与湿实验闭环**：在高风险或不可判定情形下允许 Agent 向专家升级，进一步开展隐藏靶点、前瞻性盲测或小规模湿实验验证，比较纯 Agent、Agent+规则和 Agent+专家三种模式。
8. **跨模态与跨研发阶段扩展**：从小分子和蛋白/抗体进一步扩展到 RNA、PROTAC、细胞与组学工具，并研究上游冲突如何传播到候选排序、安全性和 go/no-go 决策。

## 参考文献与数据入口

[1] Gan, Y. et al. [Mind the Tool Failures: Achieving Synergistic Tool Gains for Medical Agents](https://arxiv.org/abs/2605.26691). arXiv, 2026.

[2] [Verify-or-Trust: A Verifiable-Reward Agentic Benchmark](https://github.com/jang1563/verify-or-trust). 公开基准与代码；作者注明预印本准备中，2026.

[3] [Protein-Structure Trust-Routing Benchmark (Boltz-2)](https://huggingface.co/datasets/jang1563/protein-structure-trust-benchmark). 公开数据集与技术报告；归档论文尚未发布，2026.

[4] Kim, J. & Romero, P. [Benchmarking and Behavioral Characterization of LLM Agents for Protein Design](https://www.biorxiv.org/content/10.64898/2026.05.06.723381v1). bioRxiv, 2026.

[5] Zhang, L. et al. [MolClaw: An Autonomous Agent with Hierarchical Skills for Drug Molecule Evaluation, Screening, and Optimization](https://arxiv.org/abs/2604.21937). arXiv, 2026.

[6] Le, H. et al. [TxBench-PP: Analyzing AI Agent Performance on Small-Molecule Preclinical Pharmacology](https://arxiv.org/abs/2606.19245). arXiv, 2026.

[7] Tian, Y., Shi, Z. & Zhao, B. [Beyond Function Calling: Benchmarking Tool-Using Agents under Tool-Environment Unreliability (ToolBench-X)](https://arxiv.org/abs/2606.25819). arXiv, 2026.

[8] Varlamov, A. et al. [MemToC: Benchmarking Memory-Tool Conflict Resolution in Large Language Models](https://arxiv.org/abs/2608.26295). arXiv, 2026.

[9] Ackloo, S. et al. [CACHE: A Public–Private Partnership Benchmarking Initiative for Computational Hit-Finding](https://pmc.ncbi.nlm.nih.gov/articles/PMC9246350/). Nature Reviews Chemistry, 2022；[CACHE 挑战与公开结果入口](https://cache-challenge.org/).

[10] [The SAMPL Challenges](https://samplchallenges.org/). 官方盲测项目、版本化数据集与历届论文入口.

[11] Parks, C. D. et al. [D3R Grand Challenge 4: Blind Prediction of Protein–Ligand Poses, Affinity Rankings, and Relative Binding Free Energies](https://doi.org/10.1007/s10822-020-00289-y). Journal of Computer-Aided Molecular Design, 2020；[D3R 数据入口](https://drugdesigndata.org/).

[12] Su, M. et al. [Comparative Assessment of Scoring Functions: The CASF-2016 Update](https://doi.org/10.1021/acs.jcim.8b00545). Journal of Chemical Information and Modeling, 2019.

[13] Erasmus, M. F. et al. [A Blinded, Prospective Benchmark of In Silico Antibody Discovery Anchored to Experimental Affinity and Developability](https://www.nature.com/articles/s41587-026-03238-6). Nature Biotechnology, 2026；实验数据见论文 Supplementary Data.

[14] Xu, S. et al. [Benchmarking All-Atom Biomolecular Structure Prediction with FoldBench](https://doi.org/10.1038/s41467-025-67127-3). Nature Communications, 2026.

[15] Notin, P. et al. [ProteinGym: Large-Scale Benchmarks for Protein Fitness Prediction and Design](https://openreview.net/forum?id=URoZHqAohf). NeurIPS Datasets and Benchmarks, 2023；[ProteinGym 数据入口](https://proteingym.org/).

[16] [ChEMBL Database](https://www.ebi.ac.uk/chembl/). EMBL-EBI 生物活性、化合物、靶点和 assay 数据入口.

[17] [BindingDB](https://www.bindingdb.org/). 蛋白—小分子结合常数与测定条件数据入口.

[18] Huang, K. et al. [Therapeutics Data Commons: Machine Learning Datasets and Tasks for Drug Discovery and Development](https://arxiv.org/abs/2102.09548). NeurIPS Datasets and Benchmarks, 2021；[TDC 数据入口](https://tdcommons.ai/).

[19] [CASP—Critical Assessment of Protein Structure Prediction](https://predictioncenter.org/). 官方赛题、目标和评测结果入口.

[20] [CAPRI—Critical Assessment of Predicted Interactions](https://www.ebi.ac.uk/pdbe/complex-pred/capri/). 官方蛋白复合物对接盲测与结果入口.

[21] [RCSB Protein Data Bank](https://www.rcsb.org/). 实验解析生物大分子结构及元数据入口.