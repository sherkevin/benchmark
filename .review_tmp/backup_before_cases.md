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

## 2. 研究现状

### 2.1 工具冲突诊断与仲裁的工作

| 工作 | 领域 | 冲突从哪来 | 判对错靠什么 | 多个功能<br>重合的工具 | 评"该不该信" | 弃答 | 能拿去训练 |
|---|---|---|---|:---:|:---:|:---:|:---:|
| Mind the Tool Failures | 医学影像 | 多个诊断工具天然分歧 | 标注真值 | 有 | 有，做实例级仲裁 | 未查 | 有 |
| Verify-or-Trust | 生物基础模型 | 预测器与实验数据不一致 | 实验数据 | 无 | 有 | 部分 | 无 |
| Protein-Structure Trust-Routing | 蛋白结构预测 | 自报置信度与实际不符 | 实验解析结构 | 无 | 有 | 部分 | 无 |
| BioDesignBench | 蛋白设计 | 不设冲突 | 全部为计算结果 | 无 | 无 | 无 | 无 |
| MolClaw | 小分子多工具工作流 | 不设冲突标签 | 任务完成度 | 有 | 无 | 无 | 无 |
| TxBench-PP | 小分子临床前药理 | 不设冲突标签 | 真实药理数据 | 未查 | 无 | 无 | 无 |
| ToolBench-X | 通用工具使用 | 人工注入 | 标准答案精确匹配 | 有 | 有 | 未查 | 无 |
| MemToC | 通用问答 | 记忆与工具返回冲突 | 标准答案 | 无 | 有 | 未查 | 无 |
| 本提案 | 制药，先做一个性质 | 天然，来自真实模型残差 | 实验测定值 | 有 | 有，是主任务 | 有 | 有，是目的 |


### 2.2 工具置信度

| 工作 | 做了什么 | 在什么域 | 涉及工具冲突 | 有决策代价 | 与本提案重叠在哪 |
|---|---|---|:---:|:---:|---|
| Rewarding Doubt | 严格评分规则加结果奖励训报把握，并展示跨任务迁移 | 事实问答 | 无 | 无 | 最直接的先例，"训校准能迁移"已被它说过 |
| Taming Overconfidence | 改造策略优化算法压过度自信 | 通用问答 | 无 | 无 | 奖励设计 |
| DCPO | 指出准确率目标与校准目标之间有梯度冲突 | 可验证奖励任务 | 无 | 无 | 两段式管线必须回应它 |
| RefGRPO | agent 当自己的验证器，只对判为正确的轨迹下结论 | agent 任务 | 部分 | 无 | 作答与弃答的动作设计 |
| Tool-R0 / Agentic-DPO | 遮蔽工具名与函数签名，提升对新工具的泛化 | 工具调用 | 无 | 无 | 遮蔽设计 |
| RLMF | 两段式解耦校准，也声称可泛化 | 通用 | 无 | 无 | 两段式训练 |
| Know Your Limits 弃答综述 | 系统梳理弃答问题 | 通用 | 无 | 部分 | 弃答动作 |

### 2.3 我们剩下什么

1. **数据从哪来**。要训"什么时候该信工具"，得同时有三样东西：一个不靠人判断的对错标准、工具之间本来就存在的分歧、以及答错要付的真实代价。化学三样都有——实验测定值就是对错标准；同功能的工具算同一个分子，结果能差到把结论翻过来；答错会浪费一次湿实验的钱和几个月时间。§2.2 那批工作所在的域没有实验标准，只能拿标准答案当裁判，也没有答错的代价。
2. **问法不一样**。已有工作问"哪个工具准"，我们问"手上这组结果够不够下结论"。差别在换一套工具之后：前者学到的偏好全部作废，因为"哪个准"绑在具体工具上；后者问的是这个分子本身难不难，那是分子的属性，换工具仍然成立。

**不声称的**：把模型的把握训准这件事本身。§2.2 那批工作已经做过，我们不在这上面宣称创新。

## 3. 我们要做什么

### 3.1 七个 Step 一览

| Step | 做什么 | 产出 |
|---|---|---|
| 1 | 取有实验测定值的分子，用几个原理不同的工具各预测一遍同一个性质 | 一张记录表 |
| 2 | 核查实验值本身有没有自相矛盾 | 可用分子名单 |
| 3 | 遮住一部分工具造题，并定下判分口径 | 一批带标签的题 + 一个总损失 |
| 4 | 排除"信号只是数字分歧"这个可能 | 通过或不通过 |
| 5 | 改提示，不动模型 | 一组对照成绩 |
| 6 | 改权重 | 一个会先估把握再决定的模型 |
| 7 | 换一套工具、换一个领域测 | 泛化与迁移两组成绩 |

**Step 2 和 Step 4 是关卡**：任一不过就停下，不做 Step 5 及之后。

先只做一个性质，做通之后再按同样流程扩到 pKa、logD、对接打分等有盲测或实验数据的性质。

### 3.2 Step 1：建记录表

1. 选一个性质：血脑屏障渗透。它有公开实验数据集，也有多个能在本地跑的预测模型。
2. 取有实验测定值的分子，用几个原理不同的工具各跑一遍，同一批输入重复跑几轮。
3. 逐行记下：分子结构、每个工具的预测值与判定、实验测定值、几轮结论是否一致、是否出现两个不同来源的工具给出相反结论。

**产出**：一张记录表，逐行可核对谁对谁错。

### 3.3 Step 2：核查实验值

1. 在源文件里查同一个分子有没有出现相反的标签，以及入库时取的是哪一条。
2. 对查出的每一行裁决取哪个标签，或者整行剔除，裁决写明依据。
3. 标出每个分子被哪几个工具在训练时见过。

**产出**：可用分子名单，以及每个分子的工具训练归属标注。之后只在所有工具都没见过的分子上比对错。

### 3.4 Step 3：造题并定判分口径

**造题**

1. 每道题只让 agent 看到工具的一个子集，工具的每一种非空组合各出一道题。
2. 可见工具全部判错的题标为"该弃答"，由记录表按规则算出，不需要人工判断。
3. 该弃答的题分两桶：所有工具本来就全错的，和有正确工具被遮掉的。

**题面与答卷**

4. 题面给分子和一份工具目录，不预先给出预测值。agent 自己决定调哪个、要不要再多调一个，每次调用付代价。
5. agent 输出一个把握值，再据此决定作答还是把问题交出去。

**判分**

6. 答错罚一分，把问题交出去付一个较小的固定代价，两者相加是总损失。
7. 同时算三条参照线：从不交出去、按多数票、只在该交时才交。

**产出**：一批带标签的题，和一个可机械计算的总损失。

### 3.5 Step 4：排除假信号

1. 拿掉分子，只把几个工具的预测数字喂给一个逻辑回归，看它能做到多好。
2. 把预测值和分子故意配错，再跑一遍完整流程。
3. 另跑三条对照：直接问模型这个分子的答案，看它会不会背；一律回答"不可信"能拿多少分；在同一张表上训一个随机森林。

**判读**：逻辑回归打平 agent，或者配错之后成绩不变，都说明 agent 用的是数字分歧而不是分子内容。此时停止，不进 Step 5。

**产出**：通过或不通过。

### 3.6 Step 5：改提示，不动模型

1. 把各工具的名称、参数格式、返回结构统一成一套写法。
2. 要求 agent 采信任何预测之前，先写出这个工具在这类分子上是否可靠、自己的把握是多少。
3. 把握低于阈值时，提示强制它再调一个工具。

**产出**：一组不改权重就能拿到的成绩，作为 Step 6 的对照。

### 3.7 Step 6：改权重

1. 让模型在题目上自己多采样几条轨迹，按最终结果过滤，只留把握报得准且答对的。不引外部强模型当老师。
2. 用留下来的轨迹做监督微调，让"先报一个把握值"变成默认动作。
3. 再做强化，奖励里同时计入三项：判定对不对、把握值报得准不准、多调了几次工具。

**产出**：一个会先估把握再决定的模型。成绩必须与 Step 5 并列报出。

### 3.8 Step 7：换工具、换领域测

1. 扣掉一个工具完全不参与训练，只在它上面出题、测成绩；同时报被扣工具与其余工具的误差相关系数。
2. 换到非化学但有标准答案的领域，把题面结构原样搬过去：给同一个问题的几个互相不一致的答案，让模型报一个把握值、再决定答还是不答，按同一套总损失算分。数学题用多条解法给出的不同结果，代码题用多次执行得到的不同输出。

**判读**：Step 4 没通过时，这一步的成绩不作解读。

**产出**：换工具和换领域两组成绩。


***

## 4. 期望结论与分析

以下六条是待验证的预期，不是预设结论。它们说的是我们预计会观察到什么行为；方案本身成不成立由 §3.1 里 Step 2、Step 4 两道关卡决定，是另一回事。

1. 通用能力更强的 agent 不一定更谨慎。它可能更稳定地盲从专业工具，因为工具在它看来比自己专业。
2. 只看一个工具自报的置信度不足以判断它这次对不对。把工具之间的分歧、分子是否落在工具擅长的范围内、以及其他相关性质放在一起看，应该更有效。
3. 多数投票在工具误差高度相关时会失效。我们四个工具里有三个同源，这种情况尤其可能出现。
4. 复核不是越多越好。多调一个工具能提升准确率，但存在一个拐点，过了拐点只增加成本。
5. 规则型的门控、agent 自己推理、以及两者结合，可能各自适合不同的风险与预算场景，而不是有一个通用最优。
6. agent 即使最后答对，过程也可能是错误诊断加上运气。所以过程和结果必须同时评。

### 4.1 模型消耗与耗时

| Step | 要不要大模型 | 要不要显卡 | token 估算 | 时间量级 |
|---|:---:|:---:|---|---|
| Step 1 建记录表 | 只用本地预测模型 | 不用 | 不计，本地模型推理 | 几天 |
| Step 2 核查实验值 | 不用 | 不用 | 0 | 一两天，主要花在人工裁决 |
| Step 3 造题与判分 | 不用，规则脚本 | 不用 | 0 | 分钟级 |
| Step 4 排除假信号 | 部分要 | 不用 | 约一千万 | 几天 |
| Step 5 改提示 | 要 | 不用 | 约两千万 | 几天 |
| Step 6 改权重，前半段监督微调 | 要 | 要，一张卡 | 约两千万 | 几天 |
| Step 6 改权重，后半段强化 | 要 | 要，数张卡。主要开销在这里 | 约九千万 | 一到两周 |
| Step 7 换工具换领域测 | 要 | 要 | 约两千万 | 几天 |

**合计约一亿五千万 token，其中六成落在 Step 6 的强化阶段。** 这个数按"一千道量级的题、每条轨迹两三千 token、强化阶段每题采十几条轨迹跑几轮"估的，题量或采样量变化时按比例放大。

三条说明：

- 走公开接口还是本地部署，成本结构完全不同。前面几站用量小，走接口最省事；强化阶段要反复采样，用量大，本地部署更划算。我们此前的实测参照是一轮十几次运行花几美元、耗时半小时量级。
- 用多大的模型还没定。冷启动之后看拒绝采样的保留率：保留率太低说明基座模型撑不住这个行为，要换更大的，显卡预算同比例上调。
- 强化阶段的开销随题目数量线性放大。Step 4 之后若需要扩量，这一项跟着涨；Step 2 若剔除受污染的行，题目数会变少。

## 5. 这件事对制药领域有什么用

本项目不做新的对接、性质预测或结构预测模型，做的是这些工具之上的一层可靠性评测。具体产出四样东西：

- 一批带实验真值的工具冲突数据，以及配套的评测协议。冲突来自真实模型残差，不是人工注入的。
- 一套判分口径，能量出 agent 在真实模型误差下"什么时候该相信计算结果"的判断质量，而不只是任务完成度。
- 一份关于"哪片化学空间不能信计算"的读数。这对研发团队的直接用处是决定哪些候选必须做实验验证。
- 一条从评测到改进的通路：先用不改权重的办法改外壳，再看改权重能不能再进一步，两者都对着同一批题和同一套参照线读。

## 6. 未来可继续推进的方向

§3 已经把"学习哪片区域不能信工具""预算约束下的主动复核""面向恢复过程的训练""换一套工具重测"这四条收进主线了，本节只列还没进主线的方向。

1. 跨性质约束推理。建立一个可执行的约束层，把结构、亲和力、物化性质、代谢与毒性、稳定性、成药性连起来，帮 agent 区分三种情况：真的互相矛盾、单位或定义不同、以及本来就该随条件变化。本轮只作为判分侧的配套，用来校验 agent 给出的理由是否成立，不作为提示喂给 agent——否则测的就变成指令遵循了。
2. 规划、执行、审计三个角色分工。让一个角色专门检查证据来源、参数一致性、未验证的假设和替代解释。与本项目"单个 agent"的硬边界冲突，本轮不启动。
3. 人机协同与湿实验闭环。在高风险或者确实判不了的情形下允许 agent 向专家求助，再进一步做前瞻性盲测或小规模湿实验。专家介入会破坏可重复性，湿实验目前没有条件，所以本轮只保留"把问题交出去"作为 agent 的一个输出选项，不真的接人。
4. 扩到其他模态与研发阶段。从小分子和蛋白抗体扩到核酸、蛋白降解剂、细胞与组学工具，研究上游的冲突如何传播到候选排序、安全性判断和继续或停止的决定。与"本轮收敛到制药单一性质"的裁定冲突，本轮不启动。


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