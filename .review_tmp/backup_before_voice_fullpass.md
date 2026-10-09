# 制药科学预测冲突诊断与恢复 Agent Benchmark：Proposal

## 1. 背景与动机

###  1.1 基本事实
1. 当前制药 Agent 已能调用大量分子性质、对接、亲和力、ADMET、蛋白结构和成药性预测工具。但这些工具多为统计学习或模拟计算，即使正常返回、格式正确、数值看似合理，也会因训练偏差、适用域变化、构象与参数假设等产生误差。多个工具可能对同一性质给出矛盾结论，甚至在错误方向上形成"伪共识"。

2. 化学信息学的工具生态极度碎片化，同一功能有一堆互不兼容的实现，而且差异大到有文献专门做过系统比较。最典型的是 logP。Mannhold 等在九万余个化合物上逐一比较过各家 logP 计算方法^[25]^；Plante 与 Werner 在同一个外部测试集上报出的均方根误差是 JPlogP 1.04、XLogP3-AA 1.16、ACD 1.39、KowWIN 1.42、MLogP 1.95 个 log 单位^[26]^。更直接的证据来自 SAMPL6 盲测：11 个分子、91 份提交，各方法的均方根误差从 0.74 一直排到 4.00 个 log 单位，其中一个分子上不同方法给出的 logP 相差 4.6 个 log 单位^[27]^。也就是说换一个实现，同一个分子的 logP 能差出一到数个 log 单位——这直接决定这分子被判成"成药性好"还是"成药性差"。

### 1.2 研究动机

本项目针对的是 agent 工具使用上的一个一般性问题，制药域给它提供了一个能测量的入口，下面按问题、行为、信号三层说明，文中所谓"外显"，指的就是一个内在问题在可观察行为上留下的痕迹

**A 本质问题（一般层）：训练后 agent 的工具偏好分布不等于任务最优分布** 本质上就是训练的时候某几个工具见得太多，到了真正干活的环境里就更偏好这几个，跟它们的契合度确实也更好，可外部工具是多种多样的，一个工具值不值得用取决于它对当前任务有多大帮助，跟 agent 训练时见过没见过没有关系，这中间的落差就是工具过拟合造成的损失，机制上我们的假设是训练时的工具面和部署时的工具面往往就是同一套，模型于是把训练环境里那几个工具当成了工具使用的默认形态，当然这一条目前还只是假设，本项目不拿它当前提

**B 外显（行为层）：工具过拟合表现为选工具偏心** 在 K 个功能重合的工具之间，agent 偏向更熟悉的那个，而不是在当前任务上更可能有效的那个，也就是说 A 说的是分布，一次调用里看不出来，能看见的只有 B 这一次次的选择

**C 信号（数据层）：偏心选出的工具一旦给出错误结果，就留下一个可计数的信号** 判错的依据是工具输出与实验测定值不符，规则算得出来，不需要人来判

那么三层连起来就是本项目的技术路线，A 是根本问题却没法直接优化，只能顺着它的外显往下走一层，把 C 这个信号写成一个损失项，把这个损失压下去来纠正 B，从而缓解 A

工具超时、报错、格式不合法属于另一类问题，不在本项目范围内，本项目关心的是：**当科学工具正常工作但结果不可靠时，Agent 能否利用同功能工具和其他相关性质发现异常、定位可疑属性、选择更可靠的结果，并恢复后续研发决策。** 这直接影响虚拟筛选、先导优化、蛋白/抗体设计和候选优先级判断的可信度

### 1.3 工具结果冲突实例

**表里的四个工具各是什么**

| 列名 | 具体是什么 | 出处 |
|---|---|---|
| ADMET-AI 1.4.0 | ADMET-AI 平台里的血脑屏障渗透模型，调用 `admet_ai.admet_model.ADMETModel` | Swanson 等，Bioinformatics 2024^[22]^ |
| DeepPurpose CNN | DeepPurpose 在 MoleculeNet 的 BBBP 数据集上的预训练模型，分子编码器是卷积网络 | 库见 Huang 等，Bioinformatics 2021^[23]^；数据集见 Wu 等，Chemical Science 2018^[24]^ |
| DeepPurpose Morgan | 同上，分子编码器是 Morgan 指纹 | 同上 |
| DeepPurpose MPNN | 同上，分子编码器是消息传递网络 | 同上 |

**六个实例。** 数字是模型输出的"能透过"概率，不小于 0.5 判能透过；✓ 表示与实验真值一致，✗ 表示不一致。四个工具跑的是同一批输入，重复跑三轮结论都没变。

| 分子 | ADMET-AI 1.4.0 | DeepPurpose CNN | DeepPurpose Morgan | DeepPurpose MPNN | 实验真值 | 对几个 |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| 洛哌丁胺 | 0.781 透 ✗ | 0.789 透 ✗ | 0.853 透 ✗ | 0.900 透 ✗ | 不能透过 | 0/4 |
| 左旋多巴 | 0.495 不透 ✗ | 0.025 不透 ✗ | 0.803 透 ✓ | 0.187 不透 ✗ | 能透过 | 1/4 |
| 曲美沙特 | 0.700 透 ✗ | 0.041 不透 ✓ | 0.606 透 ✗ | 0.659 透 ✗ | 不能透过 | 1/4 |
| 沙利度胺 | 0.955 透 ✓ | 0.121 不透 ✗ | 1.000 透 ✓ | 0.382 不透 ✗ | 能透过 | 2/4 |
| 色氨酸 | 0.880 透 ✓ | 0.378 不透 ✗ | 0.995 透 ✓ | 0.303 不透 ✗ | 能透过 | 2/4 |
| 卡西酮 | 0.873 透 ✓ | 0.017 不透 ✗ | 0.991 透 ✓ | 0.370 不透 ✗ | 能透过 | 2/4 |

**实验真值的来源。** 这一列不取数据集标签，是逐条查文献核定的，每条都给出处：

- 洛哌丁胺不能透过。文献原话是它不穿过血脑屏障、直接注入脑内才有效^[28]^；它是 P-糖蛋白底物，敲除该泵的小鼠脑内浓度升七倍^[29]^。
- 左旋多巴能透过。经 LAT1 载体主动转运入脑^[30]^，这正是它能治帕金森的前提，因为多巴胺本身进不去。
- 曲美沙特不能透过。猴的药代研究明写它进入脑脊液很差^[31]^，据此判不能透过；人体数据待补。
- 沙利度胺能透过。文献把沙利度胺这一类药（IMiDs）的血脑屏障通透性写成它们相对同类抗炎药的优势^[32]^。这一条是文献的比较性陈述，不是通透性实测值，证据强度弱于上面几条。
- 色氨酸能透过。大中性氨基酸经 LAT1 入脑^[33]^，这也是脑内合成血清素要拿它当前体的前提。
- 卡西酮能透过。合成卡西酮类的血脑屏障通透性有专门测定^[34]^，但受测对象多为取代卡西酮；本体判能透过，主要依据是它自身的中枢兴奋作用。

**三条读表结论**

1. **四个工具会一起错，而且都给出高概率。** 洛哌丁胺那一行四个工具给出 0.781 到 0.900，全部判能透过，而它实际被外排泵挡在脑外。所以"几个工具都同意"本身不构成可靠信号。
2. **工具自报的概率不能当可信度用。** DeepPurpose CNN 在左旋多巴上给 0.025、在卡西酮上给 0.017、在沙利度胺上给 0.121，三次都极度自信，三次都错。卡西酮那一行 DeepPurpose Morgan 给 0.991、CNN 给 0.017，两个极端撞在一起。
3. **数据集标签自己也会错。** 曲美沙特在公开数据集里同一个分子既有"能透过"也有"不能透过"两条记录，入库时取到了错的那条；猴的药代研究结论是不能透过。所以拿标签当裁判之前必须先核标签。


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
2. **问法不一样**。已有工作问"哪个工具准"，我们问"手上这组结果够不够下结论"。差别在换一套工具之后：前者学到的偏好绑在具体工具上，换一套工具就不再适用；后者问的是这个分子本身难不难，那是分子的属性，预期换工具仍然成立。这条预期由 §3.8 Step 7 验证，目前还没有数。

**不声称的**：把模型的把握训准这件事本身。§2.2 那批工作已经做过，我们不在这上面宣称创新。

## 3. 我们要做什么

### 3.1 七个 Step 一览

| Step | 做什么 | 为什么要这一步 | 具体例子 | 产出 |
|---|---|---|---|---|
| 1 | 取有实验测定值的分子，用几个原理不同的工具各预测一遍同一个性质 | 没有逐行可核对的记录，"工具会出错"只是一句判断，说不出哪一行谁错了 | 卡西酮这一行，四个工具给 0.873、0.017、0.991、0.370，实验值是能透过 | 一张记录表 |
| 2 | 核查实验值本身有没有自相矛盾 | 谁对谁错全靠实验值裁判，而这个字段是从外部数据集直取的 | 曲美沙特在源文件里既标能透过又标不能透过，入库取到了错的那条 | 可用分子名单 |
| 3 | 遮住一部分工具造题，并定下判分口径 | 所有工具全错的分子太少，考不出"该不该认怂"；遮蔽能把这种情形摊开 | 卡西酮只给 DP-CNN 和 DP-MPNN，两个都错，该弃答；换成 ADMET-AI 加 DP-Morgan，两个都对，该作答 | 一批带标签的题 + 一个总损失 |
| 4 | 排除"信号只是数字分歧"这个可能 | agent 可能没读分子，只在看那几个数字散不散；这条规律与化学无关，会让迁移测试假阳性 | 只把那四个数喂给逻辑回归，若它和 agent 打平，说明分子内容没被用上 | 通过或不通过 |
| 5 | 改提示，不动模型 | 先确认不改权重能拿到多少，否则训练涨的分说不清是谁的功劳 | 要求 agent 在采信 DP-Morgan 的 0.991 之前，先写出自己对这个分子的把握 | 一组对照成绩 |
| 6 | 改权重 | 强化学习只能放大策略里已有的行为，得先把"先估把握"灌进去，再磨准 | 同一道题采样几条轨迹，只留把握报得准且答对的那几条做微调 | 一个会先估把握再决定的模型 |
| 7 | 换一套工具、换一个领域测 | 要区分学到的是通用判断力，还是对这几个工具的记忆 | 扣掉 DP-Morgan 完全不训、只在它上面测；再换成数学题，给同一问题几条解法的不同结果 | 泛化与迁移两组成绩 |

**Step 2 和 Step 4 是关卡**：任一不过就停下，不做后面的。

先只做一个性质，做通之后再按同样流程扩到 pKa、logD、对接打分等有实验或盲测数据的性质。

### 3.2 Step 1：建记录表

1. 性质选血脑屏障渗透。它有公开实验数据集，也有多个能在本地跑的预测模型。
2. 每个分子逐行记下：结构、各工具的预测值与判定、实验测定值，以及两个不同来源的工具是否给出相反结论。同一批输入重复跑几轮，只有几轮都一致的分歧才算。

### 3.3 Step 2：核查实验值

1. 查同一个分子在源文件里有没有相反的标签，逐行裁决取哪一条或整行剔除，裁决留依据。
2. 标出每个分子被哪几个工具在训练时见过。之后只在所有工具都没见过的分子上比对错。

### 3.4 Step 3：造题与判分

**造题**：每道题只给 agent 看工具的一个子集，每种非空组合各出一道。可见工具全部判错的题标为"该弃答"，由记录表按规则算出。这批题再分两类：所有工具本来就全错的，和有正确工具被遮掉的。

**答卷**：题面只给分子和一份工具目录，不给预测值，agent 自己决定调哪个、要不要再调一个复核，每次调用付代价，它要报两个把握值，事前那个在调工具之前给出，说的是这个工具在这个分子上答对的可能性，事后那个在看到输出之后给出，说的是这个结果值几分信任，最后按事后那个决定作答还是弃答。

**判分**：答错罚一分，弃答付代价 c，相加得总损失。c 有一个可以算出来的取值区间——比"从不弃答"的错误率还低，最优策略就成了每题都弃答；比这个错误率除以该弃答题的占比还高，最优就成了从不弃答；正式比较在区间内取三档。另算三条参照线（从不弃答、多数票、只在该弃答时才弃答）和两个诊断率（多余弃答、漏弃答），诊断率不进总损失，只用来看损失偏在哪一头。

### 3.5 Step 4：排除假信号

1. 拿掉分子，只把几个工具的预测数字喂给一个逻辑回归。
2. 把预测值和分子故意配错，再跑一遍。

两条里任一显示成绩不掉，就说明 agent 读的是数字分歧而不是分子内容，到此停止。"打平"的阈值本轮未定，动手前必须先定下来。

另配三条零成本对照：直接问模型这个分子的答案，看它会不会背；一律回答"不可信"能得多少分；同一张表上训一个随机森林。

### 3.6 Step 5：改提示，不动模型

1. 各工具的名称、参数格式、返回结构统一成一套写法，让模型没法靠名字眼熟来选。
2. 要求 agent 在调工具之前先报事前把握，看到输出之后再报事后把握，事前把握低于阈值时就强制它再调一个工具。

### 3.7 Step 6：改权重

1. 让模型自己在题目上多采样几条轨迹，按结果过滤，只留事前把握报得准且答对的那几条拿来做监督微调，不引外部强模型当老师。
2. 再做强化，奖励里计入三项，判定对不对、事前把握报得准不准、多调了几次工具，这里只监督事前那个，本质上是因为事后把握看着输出就能报得挺准，梯度顺着数字模式走，思考过程一点没训到，而事前把握没有输出可读，要报准就只能把分子结构和工具声称的适用范围对起来看，那么训的自然就是这个对照的过程，事后把握只做一致性约束，事前报 0.9、看到输出改口 0.3，这种反复本身要罚，成绩必须与 Step 5 并列报出。

### 3.8 Step 7：换工具、换领域测

1. 扣掉一个工具完全不参与训练，只在它上面出题、测成绩，同时报被扣工具与其余工具的误差相关系数。
2. 换到非化学但有标准答案的领域，题面结构照搬：给同一个问题的几个互相不一致的答案，让模型同样报两个把握值、再决定答还是不答，按同一套总损失算分，数学题用多条解法给出的不同结果，代码题用多次执行得到的不同输出。

Step 4 没通过时，这一步的成绩不作解读。



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

- 走公开接口还是本地部署，成本结构完全不同。前面几站用量小，走接口最省事；强化阶段要反复采样，用量大，本地部署更划算。
- 用多大的模型还没定。冷启动之后看拒绝采样的保留率：保留率太低说明基座模型撑不住这个行为，要换更大的，显卡预算同比例上调。
- 强化阶段的开销随题目数量线性放大。Step 4 之后若需要扩量，这一项跟着涨；Step 2 若剔除受污染的行，题目数会变少。

## 5. 这件事对制药领域有什么用

本项目不做新的对接、性质预测或结构预测模型，做的是这些工具之上的一层可靠性评测。具体产出四样东西：

- 一批带实验真值的工具冲突数据，以及配套的评测协议。冲突来自真实模型残差，不是人工注入的。
- 一套判分口径，能量出 agent 在真实模型误差下"什么时候该相信计算结果"的判断质量，而不只是任务完成度。
- 一份关于"哪片化学空间不能信计算"的读数。这对研发团队的直接用处是决定哪些候选必须做实验验证。
- 一条从评测到改进的通路：先用不改权重的办法改外壳，再看改权重能不能再进一步，两者都对着同一批题和同一套参照线读。

## 6. 未来可继续推进的方向

§3 已经把"学习哪片区域不能信工具"、"预算约束下的主动复核"、"面向恢复过程的训练"、"换一套工具重测"这四条收进主线了，本节只列还没进主线的方向。

1. 跨性质约束推理。建立一个可执行的约束层，把结构、亲和力、物化性质、代谢与毒性、稳定性、成药性连起来，帮 agent 区分三种情况：真的互相矛盾、单位或定义不同、以及本来就该随条件变化。本轮只作为判分侧的配套，用来校验 agent 给出的理由是否成立，不作为提示喂给 agent——否则测的就变成指令遵循了。
2. 规划、执行、审计三个角色分工。让一个角色专门检查证据来源、参数一致性、未验证的假设和替代解释。与本项目"单个 agent"的硬边界冲突，本轮不启动。
3. 人机协同与湿实验闭环。在高风险或者确实判不了的情形下允许 agent 向专家求助，再进一步做前瞻性盲测或小规模湿实验。专家介入会破坏可重复性，湿实验目前没有条件，所以本轮只保留弃答作为 agent 的一个输出选项，不真的接人。
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

[22] Swanson, K. et al. [ADMET-AI: a machine learning ADMET platform for evaluation of large-scale chemical libraries](https://academic.oup.com/bioinformatics/article/40/7/btae416/7698030). Bioinformatics, 2024.

[23] Huang, K. et al. [DeepPurpose: a deep learning library for drug–target interaction prediction](https://pmc.ncbi.nlm.nih.gov/articles/PMC8016467/). Bioinformatics, 2021.

[24] Wu, Z. et al. [MoleculeNet: a benchmark for molecular machine learning](https://pmc.ncbi.nlm.nih.gov/articles/PMC5868307/). Chemical Science, 2018.

[25] Mannhold, R., Poda, G. I., Ostermann, C. & Tetko, I. V. [Calculation of Molecular Lipophilicity: State-of-the-Art and Comparison of LogP Methods on more than 96,000 Compounds](https://doi.org/10.1002/jps.21494). Journal of Pharmaceutical Sciences, 2009, 98(3):861-893.

[26] Plante, J. & Werner, S. [JPlogP: an improved logP predictor trained using predicted data](https://doi.org/10.1186/s13321-018-0316-5). Journal of Cheminformatics, 2018, 10:18.

[27] Işık, M., Bergazin, T. D., Fox, T., Rizzi, A., Chodera, J. D. & Mobley, D. L. [Assessing the accuracy of octanol–water partition coefficient predictions in the SAMPL6 Part II log P Challenge](https://doi.org/10.1007/s10822-020-00295-0). Journal of Computer-Aided Molecular Design, 2020, 34:335-370.

[28] Chen, Y.-C., Chiang, C.-F., Chen, L.-F., Liang, P.-C., Hsieh, W.-Y. & Lin, W.-L. [Effects of surface modification of PLGA-PEG-PLGA nanoparticles on loperamide delivery efficiency across the blood-brain barrier](https://doi.org/10.1177/0885328211429495). Journal of Biomaterials Applications, 2013, 27(7):909-922.

[29] Schinkel, A. H., Wagenaar, E., Mol, C. A. & van Deemter, L. [P-glycoprotein in the blood-brain barrier of mice influences the brain penetration and pharmacological activity of many drugs](https://doi.org/10.1172/jci118699). Journal of Clinical Investigation, 1996, 97(11):2517-2524.

[30] Kageyama, T. et al. [The 4F2hc/LAT1 complex transports L-DOPA across the blood-brain barrier](https://doi.org/10.1016/s0006-8993(00)02758-x). Brain Research, 2000, 879:115-121.

[31] Balis, F. M., Lester, C. M. & Poplack, D. G. Pharmacokinetics of trimetrexate (NSC 352122) in monkeys. Cancer Research, 1986, 46(1):169-174. PMID 2933145（该刊此卷无 DOI，用 PMID 作可追溯标识）.

[32] Jung, Y. J. et al. [Neuroinflammation as a Factor of Neurodegenerative Disease: Thalidomide Analogs as Treatments](https://doi.org/10.3389/fcell.2019.00313). Frontiers in Cell and Developmental Biology, 2019, 7:313.

[33] Mans, A. M., Biebuyck, J. F., Shelly, K. & Hawkins, R. A. [Tryptophan transport across the blood-brain barrier during acute hepatic failure](https://doi.org/10.1111/j.1471-4159.1979.tb05170.x). Journal of Neurochemistry, 1979, 33(2):409-418.

[34] Fabregat-Safont, D. et al. [Understanding the pharmacokinetics of synthetic cathinones: evaluation of the blood-brain barrier permeability](https://doi.org/10.1111/adb.12979). Addiction Biology, 2021, 26(3).
