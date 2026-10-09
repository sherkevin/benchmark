# 制药工具预测冲突与过拟合缓解 Agent Benchmark：Proposal

> **2026-09-22 范围说明：本提案为历史方案，不能单独代表当前方法。** Owner 已明确医药同功能具体模型选择采用 Contextual Bandit，主 Agent 的可靠性和参数训练属于另一层；新工具冷启动不是全文唯一主问题。当前目标见 [目标.md](目标.md)，实际进度、拟贡献与未完成项见 [论文主线与进度](AAMAS2027_论文主线与进度.md)。本页保留原文和已有ADR引用，不把旧训练条款或计划数字当成本轮已完成事实。

# 制药科学预测冲突诊断与恢复 Agent Benchmark：Proposal

## 1. 背景与动机

### 1.1 基本事实

1.  当前制药 Agent 已能调用大量分子性质、对接、亲和力、ADMET、蛋白结构和成药性预测工具。但这些工具多为统计学习或模拟计算，即使正常返回、格式正确、数值看似合理，也会因训练偏差、适用域变化、构象与参数假设等产生误差。多个工具可能对同一性质给出矛盾结论，甚至在错误方向上形成"伪共识"。
    
2.  化学信息学的工具生态极度碎片化，同一功能有一堆互不兼容的实现，而且差异大到有文献专门做过系统比较。最典型的是 logP。Mannhold 等在九万余个化合物上逐一比较过各家 logP 计算方法^\[25\]^；Plante 与 Werner 在同一个外部测试集上报出的均方根误差是 JPlogP 1.04、XLogP3-AA 1.16、ACD 1.39、KowWIN 1.42、MLogP 1.95 个 log 单位^\[26\]^。更直接的证据来自 SAMPL6 盲测：11 个分子、91 份提交，各方法的均方根误差从 0.74 一直排到 4.00 个 log 单位，其中一个分子上不同方法给出的 logP 相差 4.6 个 log 单位^\[27\]^。也就是说换一个实现，同一个分子的 logP 能差出一到数个 log 单位——这直接决定这分子被判成"成药性好"还是"成药性差"。
    

### 1.2 研究动机

本项目针对的是 agent 工具使用上的一个一般性问题，制药域给它提供了一个能测量的入口。下面按问题、行为、信号三层说明。文中"外显"指一个内在问题在可观察行为上留下的痕迹。

**A 本质问题（一般层）：训练后 agent 的工具偏好分布不等于任务最优分布。** agent 对训练时见过的工具过度自信，对功能等价的新工具系统性回避，我们把这个现象叫工具过拟合。一个工具的价值本应取决于它对当前任务有多大帮助，与 agent 训练时是否见过它无关；两者之间的落差就是工具过拟合造成的损失。机制上我们的假设是：训练时的工具面与部署时的工具面往往是同一套，模型因此把训练环境里的那几个工具当成了工具使用的默认形态。这一条只是假设，本项目不以它成立为前提。

**B 外显（行为层）：工具过拟合表现为选工具偏心。** 在 K 个功能重合的工具之间，agent 偏向更熟悉的那个，而不是在当前任务上更可能有效的那个。A 本身不可直接观察，B 是它在单次决策上留下的痕迹。

**C 信号（数据层）：偏心选出的工具一旦给出错误结果，就留下一个可计数的信号。** 判错的依据是工具输出与实验测定值不符，由记录表按规则算出，不需要人来判断。

三层连起来就是本项目的技术路线：A 是根本问题，但没法直接优化，只能顺着它的外显往下走一层——把 C 这个信号写成一个损失项，降低这个损失来纠正 B，从而缓解 A。

工具超时、报错、格式不合法属于另一类问题，不在本项目范围内。本项目关心的是：**当科学工具正常工作但结果不可靠时，Agent 能否利用同功能工具和其他相关性质发现异常、定位可疑属性、选择更可靠的结果，并恢复后续研发决策。** 这直接影响虚拟筛选、先导优化、蛋白/抗体设计和候选优先级判断的可信度。

### 1.3 工具结果冲突实例

| 列名 | 具体是什么 | 出处 |
| --- | --- | --- |
| ADMET-AI 1.4.0 | ADMET-AI 平台里的血脑屏障渗透模型，调用 `admet_ai.admet_model.ADMETModel` | Swanson 等，Bioinformatics 2024^\[22\]^ |
| DeepPurpose CNN | DeepPurpose 在 MoleculeNet 的 BBBP 数据集上的预训练模型，分子编码器是卷积网络 | 库见 Huang 等，Bioinformatics 2021^\[23\]^；数据集见 Wu 等，Chemical Science 2018^\[24\]^ |
| DeepPurpose Morgan | 同上，分子编码器是 Morgan 指纹 | 同上 |
| DeepPurpose MPNN | 同上，分子编码器是消息传递网络 | 同上 |

| 分子 | ADMET-AI 1.4.0 | DeepPurpose CNN | DeepPurpose Morgan | DeepPurpose MPNN | 实验真值 | 对几个 |
| --- | --- | --- | --- | --- | --- | --- |
| 洛哌丁胺 | 0.781 透 ✗ | 0.789 透 ✗ | 0.853 透 ✗ | 0.900 透 ✗ | 不能透过 | 0/4 |
| 左旋多巴 | 0.495 不透 ✗ | 0.025 不透 ✗ | 0.803 透 ✓ | 0.187 不透 ✗ | 能透过 | 1/4 |
| 曲美沙特 | 0.700 透 ✗ | 0.041 不透 ✓ | 0.606 透 ✗ | 0.659 透 ✗ | 不能透过 | 1/4 |
| 沙利度胺 | 0.955 透 ✓ | 0.121 不透 ✗ | 1.000 透 ✓ | 0.382 不透 ✗ | 能透过 | 2/4 |
| 色氨酸 | 0.880 透 ✓ | 0.378 不透 ✗ | 0.995 透 ✓ | 0.303 不透 ✗ | 能透过 | 2/4 |
| 卡西酮 | 0.873 透 ✓ | 0.017 不透 ✗ | 0.991 透 ✓ | 0.370 不透 ✗ | 能透过 | 2/4 |

## 2. 研究现状

**局限**

1. 冲突来源：人为构造，与真实工具的天然结果分布不符合

2.  风险锚定：仲裁完全依赖工具输出本身，不与特定任务输入和任务难度挂钩，决策逻辑与工具高度绑定，容易出现二次过拟合
    

### 2.1 多工具冲突仲裁

| 工作 | 领域 | 冲突从哪来 | 判对错靠什么 | 多个功能<br>重合的工具 | 评"该不该信" | 弃答 | 能拿去训练 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Mind the Tool Failures | 医学影像 | 多个诊断工具天然分歧 | 标注真值 | 有 | 有，做实例级仲裁 | 未查 | 有 |
| Verify-or-Trust | 生物基础模型 | 预测器与实验数据不一致 | 实验数据 | 无 | 有 | 部分 | 无 |
| Protein-Structure Trust-Routing | 蛋白结构预测 | 自报置信度与实际不符 | 实验解析结构 | 无 | 有 | 部分 | 无 |
| BioDesignBench | 蛋白设计 | 不设冲突 | 全部为计算结果 | 无 | 无 | 无 | 无 |
| MolClaw | 小分子多工具工作流 | 不设冲突标签 | 任务完成度 | 有 | 无 | 无 | 无 |
| TxBench-PP | 小分子临床前药理 | 不设冲突标签 | 真实药理数据 | 未查 | 无 | 无 | 无 |
| ToolBench-X | 通用工具使用 | 人工注入 | 标准答案精确匹配 | 有 | 有 | 未查 | 无 |
| MemToC | 通用问答 | 记忆与工具返回冲突 | 标准答案 | 无 | 有 | 未查 | 无 |
| 本提案 | 制药，先做一个性质 | 天然，来自真实模型残差 | 实验测定值 | 有 | 有，是主任务 | 有 | 有，是目的 |

### 2.2 工具置信度

| 工作 | 做了什么 | 在什么域 | 涉及工具冲突 | 有决策代价 | 与本提案重叠在哪 |
| --- | --- | --- | --- | --- | --- |
| Rewarding Doubt | 严格评分规则加结果奖励训报把握，并展示跨任务迁移 | 事实问答 | 无 | 无 | 最直接的先例，"训校准能迁移"已被它说过 |
| Taming Overconfidence | 改造策略优化算法压过度自信 | 通用问答 | 无 | 无 | 奖励设计 |
| DCPO | 指出准确率目标与校准目标之间有梯度冲突 | 可验证奖励任务 | 无 | 无 | 两段式管线必须回应它 |
| RefGRPO | agent 当自己的验证器，只对判为正确的轨迹下结论 | agent 任务 | 部分 | 无 | 作答与弃答的动作设计 |
| Tool-R0 / Agentic-DPO | 遮蔽工具名与函数签名，提升对新工具的泛化 | 工具调用 | 无 | 无 | 遮蔽设计 |
| RLMF | 两段式解耦校准，也声称可泛化 | 通用 | 无 | 无 | 两段式训练 |
| Know Your Limits 弃答综述 | 系统梳理弃答问题 | 通用 | 无 | 部分 | 弃答动作 |

### 2.3 我们剩下什么

1. 场景基准空白：缺乏基于制药领域天然工具残差、带真实实验金标准、包含错误业务代价的多工具信任决策基准。 2. 决策机制空白：缺乏「事前置信度评估任务难度→多工具交叉核验→事后综合置信决策」的完整元思考流程，现有校准均为事后拟合，不具备跨工具泛化性。 3. 训练目标空白：缺乏针对「训练诱导的工具偏好过拟合」的专门矫正，以及换工具场景下证据判断能力的泛化验证。

## 3. 我们要做什么

### 3.1 Steps 一览 

| 步骤 | 名称 | 核心目标 | 主要操作 | 关键输出 |
| --- | --- | --- | --- | --- |
| 1 | 多工具基准数据集构建 | 搭建基于真实工具残差的场景基准 | 收集目标领域分子样本、多套同功能预测工具、实验测定金标准真值 | 原始多工具预测结果数据集 |
| 2 | 真值清洗与分子难度标注 | 确保基准可靠性，锚定任务难度属性 | 回溯文献核验真值、剔除不可靠样本、标注分子结构与难度特征 | 清洗后的带标注分子样本库 |
| 3 | 训练/评测任务集构造 | 构造标准化任务与评价指标 | 严格划分训练/测试集、枚举可用工具子集生成任务、定义坏轨迹评价指标 | 训练任务集、测试任务集、评价指标体系 |
| 4 | Prompt基线消融实验 | 分离提示工程可达到的效果边界 | 用完整思考流程Prompt跑测试集，对比基线指标变化 | Prompt基线模型、纯提示效果边界 |
| 5 | 双置信度轨迹训练 | 矫正工具偏好过拟合，固化元思考习惯 | SFT监督微调灌输双置信度流程；RL强化学习校准置信度与流程完整性 | 训练完成的校正Agent |
| 6 | 泛化与有效性验证 | 验证学到的是元思考能力而非工具特定拟合 | leave-one-tool-out工具泛化测试、跨领域零样本测试、捷径行为诊断子实验 | 工具泛化性结论、跨领域迁移性结论、捷径行为验证 |

### 3.2 Step 1：多工具基准数据集构建

1.  性质选有公开实验数据集，也有多个能在本地跑的预测模型。
    
2.  每个分子逐行记下：结构、各工具的预测值与判定、实验测定值，以及两个不同来源的工具是否给出相反结论。
    

### 3.3 Step 2：真值清洗与分子难度标注

1.  查同一个分子在源文件里有没有相反的标签，逐行裁决取哪一条或整行剔除，裁决留依据。
    
2.  标注分子结构与难度特征，反查文献真值
    

### 3.4 Step 3：训练/评测任务集构造

**造题**：对每一个分子样本，枚举全部工具的非空子集作为Agent可访问工具集合，每一个 (分子，工具子集) 构成一道独立任务。任务输入仅提供分子结构与可用工具目录，工具的预测结果对Agent不可见，需Agent主动在线调用获取。

**答卷**：题面只给分子和一份工具目录，不给预测值。agent 自己决定调哪个、要不要再调一个复核，每次调用付代价；最后输出一个把握值，据此决定作答还是弃答。

**完整流程：**

1. 输入：分子SMILES + 当前任务允许调用的工具目录；

2. Agent自主决策工具调用序列，每调用一次工具产生固定调用成本；支持迭代多次调用工具做交叉复核； 3. 完成证据收集后输出事后置信度，基于置信度做出最终动作：输出预测结果，或执行弃答（Yield）。

**判分**：答错罚一分，弃答付代价 c，相加得总损失。

$\mathcal L = \mathbb I(\text{answer wrong}) + c\cdot \mathbb I(\text{yield})$

*   预测错误：罚1；执行弃答：付出代价系数c。
    

代价系数c存在理论有效区间：

*   若c低于下界，最优策略退化为全部样本直接弃答；
    
*   若c高于上界，最优策略退化为永远不弃答。 正式实验在有效区间内选取低、中、高三档c开展对比，观察模型行为随业务风险代价的变化。
    

**基准参照线：**

设置三组规则式基线作为性能参照：

1. 从不弃答：永远输出预测，不执行弃答； 2. 多数投票：在可用工具中做多数投票输出结果，不弃答； 3. Oracle-yield：仅在标记为「该弃答」的样本上执行弃答（理论上界）。

**评价指标：**

*   多余弃答率：证据充分可以作答却错误弃答的比例；
    
*   工具调用偏好度：各工具的调用频次分布，衡量工具偏好过拟合程度；
    
*   检验阶段触发率：主动进入多工具交叉核验的样本占比，重点统计伪共识样本上的触发率；
    
*   置信度校准误差：事前置信度、事后置信度与实际准确率的偏差。
    

### 3.6 Step 4：Prompt基线消融实验

量化纯提示工程的行为矫正边界，区分「思考格式对齐可解决的问题」与「必须通过轨迹损失训练矫正的工具偏好偏差」，为后续训练提供性能基准。

1.  通过系统提示注入完整思考范式；使用Step 3划分的独立测试集，以原生无引导Agent为空白基线；交互规则、工具调用代价、损失函数与Step 3完全一致。
    

### 3.7 Step 5：双置信度轨迹训练

> **2026-09-13 裁定（[ADR-0006](adr/0006-training-goal-decision-usability.md)）。** 本节的训练目标表述「矫正训练诱导的工具偏好过拟合」已被覆盖。可训练主任务改为**「判断这组预测能不能拿去做决策」+ 选择性作答（answer/abstain/escalate）**，即维持 [ADR-0028](inherited/0028-calibration管线v1四方评审裁决.md) D1；「选哪个工具 / 纠正选工具偏心」降为**评测**目标，不进入训练环。理由是方差分解 `e_ik = μ + α_i + β_k + γ_ik + ε_ik`：选最准工具由交互项 γ_ik 决定、绑定具体工具、换工具即失效，判断可用性由分子主效应 α_i 决定、换工具仍有效，「能迁移」与「有信号」对选工具任务在定义层互斥。本节余下原文（双置信度流程、SFT 工具选择随机化、RL 校准）保留作历史与流程参考。**GPU 闸门保持关闭，须先过 ADR-0028 D2 五条零机时检验。** 本节是本文件唯一被 ADR-0006 覆盖之处；§1–§3.3 评测层、§2 文献表、§3.4 判分损失式不受影响。

在纯 prompt 工程效果不好的前提下考虑。矫正训练诱导的工具偏好过拟合，固化「先评估任务难度→交叉核验→置信决策」的元思考习惯。

1.   SFT监督微调
    

*   目标：向模型灌输双置信度完整思考流程
    
*   做法：脚本自动生成示范轨迹，固定链路为：事前置信度→工具调用→事后置信度→最终决策
    
*   关键控制：工具选择随机化，不标注任何工具优劣，避免模型学习特定工具偏好
    

1.   RL强化学习
    

*   目标：校准置信度，矫正工具偏好，强化完整流程执行
    
*   reward 定义同 step3
    

### 3.8 Step 6：泛化与有效性验证

验证模型学到的是通用元思考习惯，而非训练集工具特定拟合；同时确认是否根除「仅以工具输出分歧触发检验」的廉价捷径。

1.  工具泛化测试
    

*   做法：测试阶段开放该从未见过的新工具加入可用集合
    
*   观测：工具调用均衡度、伪共识样本检验触发率、置信度校准稳定性
    
*   判定：指标维持稳定则证明能力不绑定特定工具
    

1.   跨领域零样本测试
    

*   做法：选取完全不同领域的同功能多工具场景，模型零样本直接运行
    
*   观测：事前置信度评估、多工具交叉核验、证据不足弃答等完整行为模式是否保留
    
*   判定：行为模式一致则证明元思考能力具备跨领域迁移性
    

1.  捷径行为诊断
    

*   做法：筛选两组样本，控制工具输出离散程度一致，分别为简单正确样本、工具全错的伪共识样本
    
*   观测：两组样本的检验阶段触发率
    
*   判定：伪共识组触发率显著更高，则触发开关锚定任务本身难度，而非工具输出分歧
    

## 4. 期望结论与分析

1.  通用能力更强的 agent 不一定更谨慎。它可能更稳定地盲从专业工具，因为工具在它看来比自己专业。
    
2.  只看一个工具自报的置信度不足以判断它这次对不对。把工具之间的分歧、分子是否落在工具擅长的范围内、以及其他相关性质放在一起看，应该更有效。
    

3.  工具功能难度会对整体结果有较大影响，若一个工具简单或被训进了模型参数里，则进入校验的比率会下降
    
4.  复核不是越多越好。多调一个工具能提升准确率，但存在一个拐点，过了拐点只增加成本。
    
5.  规则型的门控、agent 自己推理、以及两者结合，可能各自适合不同的风险与预算场景，而不是有一个通用最优。
    

### 4.1 模型消耗与耗时

token消耗主要集中在评测环节，其中Prompt基线消融实验测试消耗约30M token，耗时数天；双置信度轨迹训练为核心消耗环节，监督微调约200M token，强化学习轨迹采样与迭代更新约1.7B token，合计约1.9B token，耗时1–2周；泛化与有效性验证测试消耗约70M token，耗时数天。全流程API调用token合计约2.0B，估算参考同类工具Agent实验通用量级，按单条测试任务多轮交互3–4k token、万级训练样本与多轮迭代规模测算。

## 5. 这件事对制药领域有什么用

*   产出一套评测 agent 调用制药相关工具的泛化能力的 benchmark
    

*   提出一个新的衡量工具过拟合现象的新视角和缓解策略
    
*   提升 agent 本身的工具调用能力，改善其工具使用的训推不一致和偏好性的更 general 的问题
    

## 6. 未来可继续推进的方向

1.  跨性质约束推理。建立一个可执行的约束层，把结构、亲和力、物化性质、代谢与毒性、稳定性、成药性连起来，帮 agent 区分三种情况：真的互相矛盾、单位或定义不同、以及本来就该随条件变化。本轮只作为判分侧的配套，用来校验 agent 给出的理由是否成立，不作为提示喂给 agent——否则测的就变成指令遵循了。
    
2.  规划、执行、审计三个角色分工。让一个角色专门检查证据来源、参数一致性、未验证的假设和替代解释。与本项目"单个 agent"的硬边界冲突，本轮不启动。
    
3.  人机协同与湿实验闭环。在高风险或者确实判不了的情形下允许 agent 向专家求助，再进一步做前瞻性盲测或小规模湿实验。专家介入会破坏可重复性，湿实验目前没有条件，所以本轮只保留弃答作为 agent 的一个输出选项，不真的接人。
    
4.  扩到其他模态与研发阶段。从小分子和蛋白抗体扩到核酸、蛋白降解剂、细胞与组学工具，研究上游的冲突如何传播到候选排序、安全性判断和继续或停止的决定。与"本轮收敛到制药单一性质"的裁定冲突，本轮不启动。
    

## 参考文献与数据入口

\[1\] Gan, Y. et al. [Mind the Tool Failures: Achieving Synergistic Tool Gains for Medical Agents](https://arxiv.org/abs/2605.26691). arXiv, 2026.

\[2\] [Verify-or-Trust: A Verifiable-Reward Agentic Benchmark](https://github.com/jang1563/verify-or-trust). 公开基准与代码；作者注明预印本准备中，2026.

\[3\] [Protein-Structure Trust-Routing Benchmark (Boltz-2)](https://huggingface.co/datasets/jang1563/protein-structure-trust-benchmark). 公开数据集与技术报告；归档论文尚未发布，2026.

\[4\] Kim, J. & Romero, P. [Benchmarking and Behavioral Characterization of LLM Agents for Protein Design](https://www.biorxiv.org/content/10.64898/2026.05.06.723381v1). bioRxiv, 2026.

\[5\] Zhang, L. et al. [MolClaw: An Autonomous Agent with Hierarchical Skills for Drug Molecule Evaluation, Screening, and Optimization](https://arxiv.org/abs/2604.21937). arXiv, 2026.

\[6\] Le, H. et al. [TxBench-PP: Analyzing AI Agent Performance on Small-Molecule Preclinical Pharmacology](https://arxiv.org/abs/2606.19245). arXiv, 2026.

\[7\] Tian, Y., Shi, Z. & Zhao, B. [Beyond Function Calling: Benchmarking Tool-Using Agents under Tool-Environment Unreliability (ToolBench-X)](https://arxiv.org/abs/2606.25819). arXiv, 2026.

\[8\] Varlamov, A. et al. [MemToC: Benchmarking Memory-Tool Conflict Resolution in Large Language Models](https://arxiv.org/abs/2608.26295). arXiv, 2026.

\[9\] Ackloo, S. et al. [CACHE: A Public–Private Partnership Benchmarking Initiative for Computational Hit-Finding](https://pmc.ncbi.nlm.nih.gov/articles/PMC9246350/). Nature Reviews Chemistry, 2022；[CACHE 挑战与公开结果入口](https://cache-challenge.org/).

\[10\] [The SAMPL Challenges](https://samplchallenges.org/). 官方盲测项目、版本化数据集与历届论文入口.

\[11\] Parks, C. D. et al. [D3R Grand Challenge 4: Blind Prediction of Protein–Ligand Poses, Affinity Rankings, and Relative Binding Free Energies](https://doi.org/10.1007/s10822-020-00289-y). Journal of Computer-Aided Molecular Design, 2020；[D3R 数据入口](https://drugdesigndata.org/).

\[12\] Su, M. et al. [Comparative Assessment of Scoring Functions: The CASF-2016 Update](https://doi.org/10.1021/acs.jcim.8b00545). Journal of Chemical Information and Modeling, 2019.

\[13\] Erasmus, M. F. et al. [A Blinded, Prospective Benchmark of In Silico Antibody Discovery Anchored to Experimental Affinity and Developability](https://www.nature.com/articles/s41587-026-03238-6). Nature Biotechnology, 2026；实验数据见论文 Supplementary Data.

\[14\] Xu, S. et al. [Benchmarking All-Atom Biomolecular Structure Prediction with FoldBench](https://doi.org/10.1038/s41467-025-67127-3). Nature Communications, 2026.

\[15\] Notin, P. et al. [ProteinGym: Large-Scale Benchmarks for Protein Fitness Prediction and Design](https://openreview.net/forum?id=URoZHqAohf). NeurIPS Datasets and Benchmarks, 2023；[ProteinGym 数据入口](https://proteingym.org/).

\[16\] [ChEMBL Database](https://www.ebi.ac.uk/chembl/). EMBL-EBI 生物活性、化合物、靶点和 assay 数据入口.

\[17\] [BindingDB](https://www.bindingdb.org/). 蛋白—小分子结合常数与测定条件数据入口.

\[18\] Huang, K. et al. [Therapeutics Data Commons: Machine Learning Datasets and Tasks for Drug Discovery and Development](https://arxiv.org/abs/2102.09548). NeurIPS Datasets and Benchmarks, 2021；[TDC 数据入口](https://tdcommons.ai/).

\[19\] [CASP—Critical Assessment of Protein Structure Prediction](https://predictioncenter.org/). 官方赛题、目标和评测结果入口.

\[20\] [CAPRI—Critical Assessment of Predicted Interactions](https://www.ebi.ac.uk/pdbe/complex-pred/capri/). 官方蛋白复合物对接盲测与结果入口.

\[21\] [RCSB Protein Data Bank](https://www.rcsb.org/). 实验解析生物大分子结构及元数据入口.

\[22\] Swanson, K. et al. [ADMET-AI: a machine learning ADMET platform for evaluation of large-scale chemical libraries](https://academic.oup.com/bioinformatics/article/40/7/btae416/7698030). Bioinformatics, 2024.

\[23\] Huang, K. et al. [DeepPurpose: a deep learning library for drug–target interaction prediction](https://pmc.ncbi.nlm.nih.gov/articles/PMC8016467/). Bioinformatics, 2021.

\[24\] Wu, Z. et al. [MoleculeNet: a benchmark for molecular machine learning](https://pmc.ncbi.nlm.nih.gov/articles/PMC5868307/). Chemical Science, 2018.

\[25\] Mannhold, R., Poda, G. I., Ostermann, C. & Tetko, I. V. [Calculation of Molecular Lipophilicity: State-of-the-Art and Comparison of LogP Methods on more than 96,000 Compounds](https://doi.org/10.1002/jps.21494). Journal of Pharmaceutical Sciences, 2009, 98(3):861-893.

\[26\] Plante, J. & Werner, S. [JPlogP: an improved logP predictor trained using predicted data](https://doi.org/10.1186/s13321-018-0316-5). Journal of Cheminformatics, 2018, 10:18.

\[27\] Işık, M., Bergazin, T. D., Fox, T., Rizzi, A., Chodera, J. D. & Mobley, D. L. [Assessing the accuracy of octanol–water partition coefficient predictions in the SAMPL6 Part II log P Challenge](https://doi.org/10.1007/s10822-020-00295-0). Journal of Computer-Aided Molecular Design, 2020, 34:335-370.

\[28\] Chen, Y.-C., Chiang, C.-F., Chen, L.-F., Liang, P.-C., Hsieh, W.-Y. & Lin, W.-L. [Effects of surface modification of PLGA-PEG-PLGA nanoparticles on loperamide delivery efficiency across the blood-brain barrier](https://doi.org/10.1177/0885328211429495). Journal of Biomaterials Applications, 2013, 27(7):909-922.

\[29\] Schinkel, A. H., Wagenaar, E., Mol, C. A. & van Deemter, L. [P-glycoprotein in the blood-brain barrier of mice influences the brain penetration and pharmacological activity of many drugs](https://doi.org/10.1172/jci118699). Journal of Clinical Investigation, 1996, 97(11):2517-2524.

\[30\] Kageyama, T. et al. [The 4F2hc/LAT1 complex transports L-DOPA across the blood-brain barrier](https://doi.org/10.1016/s0006-8993\(00\)02758-x). Brain Research, 2000, 879:115-121.

\[31\] Balis, F. M., Lester, C. M. & Poplack, D. G. Pharmacokinetics of trimetrexate (NSC 352122) in monkeys. Cancer Research, 1986, 46(1):169-174. PMID 2933145（该刊此卷无 DOI，用 PMID 作可追溯标识）.

\[32\] Jung, Y. J. et al. [Neuroinflammation as a Factor of Neurodegenerative Disease: Thalidomide Analogs as Treatments](https://doi.org/10.3389/fcell.2019.00313). Frontiers in Cell and Developmental Biology, 2019, 7:313.

\[33\] Mans, A. M., Biebuyck, J. F., Shelly, K. & Hawkins, R. A. [Tryptophan transport across the blood-brain barrier during acute hepatic failure](https://doi.org/10.1111/j.1471-4159.1979.tb05170.x). Journal of Neurochemistry, 1979, 33(2):409-418.

\[34\] Fabregat-Safont, D. et al. [Understanding the pharmacokinetics of synthetic cathinones: evaluation of the blood-brain barrier permeability](https://doi.org/10.1111/adb.12979). Addiction Biology, 2021, 26(3).