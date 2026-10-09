# 医药同功能工具选择：最近邻与方法审查

> Task40；2026-09-22开始。当前主线见 `docs/目标.md`，本报告不是新主线裁定。
> 结论级别：原文覆盖审查与数学/信息合同审查；没有新的医药训练结果，没有宣称文献已穷尽。

## 1. 本轮科学决定

Task39候选“相关错误结构→联合估计条件能力→Bandit选择”如果具体化为固定任务核、图正则或共享低秩参数，并不足以构成新算法：KMTL-UCB、GOB.Lin及后续多任务Bandit已覆盖相应机制。将该构造降为必须面对的参考基线，而不是先实现后命名Ours。

这里还混合了三个不同对象：未知条件均值之间的共享结构；同一输入上多个工具错误的联合分布；本题已经看见的工具输出。只有第一种直接进入跨题共享均值估计；后两种主要影响证据融合/复核，且在题后才能见真值。不能从“错误相关”直接推出三者可互换。

保留研究问题的重要性，但撤回仅靠这些通用机制的创新表述。下一版候选必须说清自己改进的是哪一种估计、靠什么合法观测来识别，以及比哪个具体已发表方法更好。

## 2. 已核最近邻（按数学对象，不按年份凑表）

| 方法/论文 | 已核覆盖 | 与本项目的关系/处置 |
|---|---|---|
| META-DES，Cruz等，Pattern Recognition 48(5):1925–1935，2015 [S1] | 学习每个分类器在输入局部的能力；综合特征邻域、输出轮廓、置信度 | 条件能力估计不新；标准META-DES需要当前全池输出/概率，不能直接当单调用同信息基线 |
| KMTL-UCB，Deshmukh/Dogan/Scott，NeurIPS 2017 [S2] | 工具/任务核乘上下文核，联合拟合、单臂奖励更新、UCB；讨论已知和估计任务核 | 最直接的“相关工具→共享估计”最近邻；必须保留。原任务正是arms，映射比GOB更直接 |
| GOB.Lin，Cesa-Bianchi/Gentile/Zappella，NeurIPS 2013 [S3] | 图拉普拉斯与Kronecker特征，通过单次观测共享节点参数 | 图共享已有；原节点是外生到达用户，不是本项目可选provider，移植必须标适配 |
| Lin/Moothedath/Vaswani，ICML 2024 [S4] | 低秩共同表示，交替投影梯度/最小化，多任务共同学习 | 最近的表示共享先例；原每轮为每个任务各选动作并获反馈，不能免费变成33工具33标签 |
| PAK-UCB / RFF-UCB，Hu/Leung/Farnia，ICML 2025 [S5] | 同一输入下每个模型独立核估计；只更新被选模型；RFF加速 | 非线性、现代model-selection baseline；就是Task30曾审过的RFF来源，不是另一个新baseline |
| Cascade Routing，Dekoninck/Baader/Vechev，ICML 2025 [S6] | 统一调用前routing与根据已见输出继续cascading，质量—成本优化 | 换成“先调一个，再决定复核”也不自动新；应在多调用协议单独比较 |
| Boda/Prashanth，ICML 2019 [S7] | 用成对观测估计相关性，最小化对全部臂的重建MSE | 相关性学习需要观测条件；目标不是本项目正确率，不作为主表默认行 |
| DAK-UCB，Jafari/Farnia，arXiv 2603.23140v1，2026 [S8] | prompt-aware生成质量与多样性联合目标，使用两样本核函数 | 本轮核得预印本；生成多样性不等于错误独立性，不应把它改名为相关错误基线 |

## 3. 最小比较矩阵：按信息权限分层，而非把所有方法塞一表

| 层 | 可见数据 | 最小对照 | 不允许的混用 |
|---|---|---|---|
| M1 单次调用、少反馈 | 同一获准初始历史；当前x；所选模型输出；整题后该模型正确性 | 固定工具控制、LinUCB、PAK/RFF-UCB、KMTL-UCB家族参考；候选只有明示额外机制后才加 | 当前全池输出、其他模型正确性、oracle补标签均不可见 |
| M2 有成对训练历史、当前单次调用 | 训练区合法完整/成对面板；推断仍只调一个工具 | 训练选最佳固定、OLA局部能力、独立与共享条件能力估计 | 与完全无成对历史的M1不混为同信息比较；OLA应只用历史局部准确率选模型后调用 |
| M3 同题预算化多调用 | x、已见输出、剩余预算；整题后所有被调用模型反馈 | 相同Agent下的静态排序、无依赖共享参考、Cascade Routing适配、候选 | 把每次调用后立即正确性更新当作真实运行；把全池META-DES的成本记为一次 |
| 辅助能力对照 | 相同已获得的二值题后标签预算 | x→真值直接预测器，以及主Agent无工具/固定工具 | 将直接预测器视为不存在，或让其多拿全表真值却不标权限差别 |

OLA是Woods/Kegelmeyer/Bowyer的局部准确率选择[S9]，不是自造方法；库接口可能预计算全池测试输出，预算化实现应明确改为先估局部能力再仅调用被选模型，单独审实现等价。META-DES原版保留在全输出/全成本参考层，不假装与单调用完全等价。

这里只收敛第一次诊断需要的类别，不强行增加全量主表。LinUCB、PAK/RFF与KMTL的参数、初始历史、训练权重和宽度系数需统一预登记。Task30已有RFF官方等价代码可以复用，但旧二动作incumbent初始化不能直接冒充当前33工具协议。

## 4. P02信息语义纠正会影响故事可信度

实际Task36判分器给每个已调用工具题后正确性，因此多调用是延迟半带反馈；正确性不会在本题复核过程中到达。给定二值输出与正确性，终止后可重构真值。其他模型的预测仍未知，不能因此获得其正确性。

所以“观测一个工具结果后，在同题更新它的真实正确性再选下一个”会作弊；“没有显式真值字段，因此永远没有标签”也不成立。完整推导、方法方程及实际fixture检查见Task40 `method_audit.md` 和 `verify_math_contracts.py`。

## 5. 近期核查与新颖性结论边界

2026-08-06预印本BC-ICL [S10]使用冻结表格基础模型、bootstrap历史与乘性交互特征做单奖励Contextual Bandit；它再次说明“少数据、模型共享、复杂表示”不能单独宣称创新。本轮只核读方法/协议与来源，不核实其表格数值或复现结果；未确认同行评审录用，不默认加入首批主表。

反馈图Bandit [S11]中的边代表环境真实揭示其他动作奖励，而本项目的工具依赖边只是推断关系；不能把概率推断伪装成side-observation。该文在本轮是信息模型相关工作，不替代KMTL这一最接近机制。

本轮覆盖了精确命题对应的一手近邻，并非系统综述穷尽。宽泛年份/缩写检索曾返回无关结果，不能拿“没搜到”证明原创；2026论文的最新发表状态和当前方法的完整实证仍需逐项补齐。

## 6. 一手来源与核读深度

[S1] Cruz, Sabourin, Cavalcanti, Ren. *META-DES: A dynamic ensemble selection framework using meta-learning*. Pattern Recognition 48(5), 1925–1935, 2015。核读作者全文§2–3及Algorithm1与官方输入接口；2018是arXiv存档日期，不是期刊年。https://www.sciencedirect.com/science/article/pii/S0031320314004919 ; https://arxiv.org/pdf/1810.01270 ; https://deslib.readthedocs.io/en/latest/modules/des/meta_des.html

[S2] Deshmukh, Dogan, Scott. *Multi-Task Learning for Contextual Bandits*. NeurIPS 2017。核读官方正文和作者长版§3–4的任务核、加权目标、核解与理论假设。https://proceedings.neurips.cc/paper_files/paper/2017/hash/b06f50d1f89bd8b2a0fb771c1a69c2b0-Abstract.html ; https://arxiv.org/html/1705.08618v1

[S3] Cesa-Bianchi, Gentile, Zappella. *A Gang of Bandits*. NeurIPS 2013。核读问题与图正则、原文Algorithm1的Kronecker特征。https://papers.neurips.cc/paper/5006-a-gang-of-bandits.pdf

[S4] Lin, Moothedath, Vaswani. *Fast and Sample Efficient Multi-Task Representation Learning in Stochastic Contextual Bandits*. ICML 2024，PMLR235:30227–30251。核读发表页及作者长版§2.1每任务反馈、低秩结构与特征/噪声假设；未复现实验。https://proceedings.mlr.press/v235/lin24o.html ; https://arxiv.org/html/2410.02068v1

[S5] Hu, Leung, Farnia. *PAK-UCB Contextual Bandit: An Online Learning Approach to Prompt-Aware Selection of Generative Models and LLMs*. ICML 2025，PMLR267:24447–24481。核读模型选择、独立核估计与RFF；作者代码dgm-online-select，本仓Task30已有该RFF适配。https://proceedings.mlr.press/v267/hu25m.html ; https://arxiv.org/html/2410.13287v5 ; https://github.com/yannxiaoyanhu/dgm-online-select

[S6] Dekoninck, Baader, Vechev. *A Unified Approach to Routing and Cascading for LLMs*. ICML 2025。核读官方发表页、作者全文§2–4的先验/已见输出条件质量与调用成本；未将估计质量当真实正确性。https://proceedings.mlr.press/v267/dekoninck25a.html ; https://arxiv.org/html/2410.10347v3 ; https://github.com/eth-sri/cascade-routing

[S7] Boda, Prashanth L.A. *Correlated bandits or: How to minimize mean-squared error online*. ICML 2019，PMLR97:686–694。核读目标与成对观察说明；未复现方法。https://proceedings.mlr.press/v97/boda19a.html

[S8] Jafari, Farnia. *DAK-UCB: Diversity-Aware Prompt Routing for LLMs and Generative Models*. arXiv2603.23140v1，2026-03-24。本轮核读目标与两样本核定义；相关OpenReview入口有浏览挑战，未据此确认录用。https://arxiv.org/html/2603.23140v1

[S9] Woods, Kegelmeyer, Bowyer. *Combination of Multiple Classifiers Using Local Accuracy Estimates*. IEEE TPAMI19(4):405–410，1997，DOI10.1109/34.588027。核对出版记录与官方OLA实现说明；不是IEEE网页添加日期2002。https://ieeexplore.ieee.org/document/588027/ ; https://deslib.readthedocs.io/en/latest/modules/dcs/ola.html

[S10] Gupta, Tavker, Efimov, Sathyanarayana, Bhutani, Oreshkin. *Bootstrap-Conditioned Action Selection with Tabular Foundation Models*. arXiv2608.06559v1，2026-08-06。核读摘要、§2与实验信息合同；本轮不把预印本写成已发表基线。https://arxiv.org/html/2608.06559v1

[S11] Zhang, Zhang, Vrousgou, Luo, Mineiro. *Practical Contextual Bandits with Feedback Graphs*. NeurIPS2023。核对原文导论中的真实side-observation定义与正式发表PDF；本轮不作完整算法fidelity主张。https://proceedings.neurips.cc/paper_files/paper/2023/file/618c95f4557c15b253fb0e6f548ea0c0-Paper-Conference.pdf

取得的原始公开文献存于Task40 `evidence/`；`source_manifest.json`记录实际成功获取的URL与文件哈希。表述的阅读深度不等于运行作者代码，更不等于本项目方法已有效。

## 7. 2026-09-23继续核读与实现边界

Task40原数学检查已存在但交接未收口。本轮核对72项PASS、两份结果逐字节相同及全部三个源文件哈希；补齐报告、最小批次卡和来源索引，没有冒称重新运行那72项。Task41新建了固定工具核参考及Task36单调用终止反馈适配，独立人工矩阵/实际回放类fixture验证，不读医药缓存拟合。

再次核读官方KMTL正文第3–5页及第4页截图后，显式处理每工具次数归一化、全局1/N正则尺度及未观察臂的宽度文字差异。实现细节与不能宣称官方fidelity的原因见 `../task/task41-kmtl-reference-contract/fidelity_notes.md`。一般“联合估计+UCB”依然是参考基线，不因现在有代码就升级为新算法。

另核到Qi/Ban/He的 *Neural Bandit with Arm Group Graph*（arXiv2206.03644；作者页注明SIGKDD2022录用，DOI10.1145/3534678.3539312）：图关系、GNN估计和神经探索的组合也已有先例。因此不能简单将固定B替换为GNN就宣称新颖。本轮仅核摘要和发表信息，未审全算法，不追加首批主表行。https://arxiv.org/abs/2206.03644

**来源索引更正：** 上节曾写`source_manifest.json`记录实际成功获取URL；本轮接手发现该文件尚未存在。现在补建的是19个既有文件的哈希与书目来源对照，不是当时的HTTP下载回执。无法复原的原下载URL/时刻明确未知，作者PDF不凭文件名认定字节来源。正文引用可核与旧下载过程可核是两个结论。

当前P03已否定泛化候选的新颖性表述，但并未证明所有相关结构方法无效，也没有完成医药性能比较。后续应先收窄真正需额外机制的条件，再过资格/协议和最小真实数据检验；不继续通过增加抽象名词制造Ours。
