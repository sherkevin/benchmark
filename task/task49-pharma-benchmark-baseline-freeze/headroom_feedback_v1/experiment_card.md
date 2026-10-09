# 剩余错误：选择空间、分数排序与反馈覆盖

日期：2026-10-07。执行前冻结；只做development事后诊断，不运行新策略，不改变方法或confirmation。

## 目的与复用

六格闭环中，分数尺度改善，但query中心化未到预登记1pp升级线。本轮回答：剩余错误中有多少可由菜单内其它工具救回；已有合法分数能否区分正确与错误工具；可救错误是否集中在正确候选完全没有已释放反馈的时刻。对应E1任务headroom、E3机制问题与E5错误分析。

最近方法仍是既定BayesianRouter-adapted；本轮不重跑其效果，也不凭反馈计数断言它或探索算法会胜出。33-provider指纹诊断、七家族票的旧数字不迁移到当前四checkpoint菜单。

复用四provider/722分子的封存真实缓存及上一轮完整状态。0训练、0推理、0API/GPU；CPU预算120秒。一个分析脚本，一次正式分析，定向合成数学测试在先。全部结果留档，不按结论换指标或子集。

## 输入与固定分母

- fit-score缓存 summary SHA：`ad5650d68e8e8d7f9ca497f2fa93c20abd6a4dfc41ed5711ebe83ba7391176c0`；依其manifest核验native probabilities、prior_cache、OOF scores、features和fit references。
- 六格summary SHA：`791a4cd25a0fe5215914aa9f7105b0bef68d7155da59767336edfa9461b8d7ae`；独立验证SHA：`ed43f414806f21258f5e035e032f2339d8a310f7def710cb4f5326a28e8afd44`。
- 六格raw events SHA：`2658cc7c01231263ce68fccd2ce1bfa55daf6fb4a790665e8ba02c86e39015c7`；原始257MB与可还原gzip都已保存，选一种读取并核raw hash。
- 原development输入SHA：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。保持原5fold fit/evaluation与provider次序。
- 静态分数审计每分子只计一次（722）；在线错误分析只取natural的6cells×3delay，共90流、12,996决策，每cell 2,166。分delay报告；不把重复分子当独立样本，不读取confirmation。
- 全面板正确性只在分析器中可见，不产生训练标签、不写回policy状态。当前四工具全部原生status=ok；没有unknown/refusal能力结论。

## 数学与实例

令$r_{i,a}$为缓存中工具$a$对分子$i$是否正确，$a_i$为已记录的实际选择。事后可救上界$h_i=\max_a r_{i,a}$完全由评价器计算。逐个错误满足

$$1-r_{i,a_i}=(1-h_i)+(h_i-r_{i,a_i}).$$

实例：四工具正确性为$(0,1,0,1)$，实际选择第一个，错误1=全菜单均错0+可救1；若四个均错，错误1=全菜单均错1+可救0。“全菜单均错”只指这四个冻结工具，不是不可约误差。该上界知道本题所有答案，不是可部署方法，也不是Retrospective Best Fixed。

记$C_i=\{a:r_{i,a}=1\}$，$W_i=\{a:r_{i,a}=0\}$，它们只是上述结果向量的派生集合。对两集合均非空的分子，用调用前冻结分数$q_{i,a}$计算

$$T_i=\frac{\sum_{a\in C_i,b\in W_i}\left(\mathbf1[q_{i,a}>q_{i,b}]+\tfrac12\mathbf1[q_{i,a}=q_{i,b}]\right)}{|C_i||W_i|}.$$

实例：正确工具分数0.8/0.4、错误工具0.6/0.4，则四对贡献1、1、0、0.5，$T_i=0.625$。每个mixed分子等权，另给全部pair加权值；不让有更多discordant pairs的分子默默获得更高权重。raw/corrected分数均从原OOF logits与fit正确率按上卡公式计算。

对每次实际可救错误，检查decision的state_before中已释放次数$n_{i,a}$。记录$\max_{a\in C_i} n_{i,a}=0$（所有正确候选零反馈）及其补集。实例：正确候选为第2、4工具，counts=(8,0,4,0)属零反馈；counts=(8,1,4,0)不属零反馈。正计数不等于当前分子有相似历史，零计数也不证明探索会带来收益。

零反馈再分为“全部正确候选此前从未选过”和“至少一个正确候选已选但反馈仍pending”；两类互斥且完备。只读pending的provider/事件身份，不读其reward。例：上述第2工具有一个pending选择，属于第二类；若第2、4均无pending，属于第一类。所有方法都有离线先验，零在线反馈不等于零信息。固定另报最初$D+1$次决策（尚不可能收到任何反馈）及其后的结果。

## 输出

1. 当前四provider的固定准确率、fit-only Best Fixed、evaluator-only Retrospective Best Fixed、逐分子Oracle；逐fold与全体，以及all-correct/mixed/all-wrong数量。fit-only每折从fit_rates选一个；Retrospective全体表为看过722条结果后选定的一个全局provider，分折表另标各折事后provider，二者不混称同一策略。重算锚点用于解释当前菜单，历史已有数值明确标复核。
2. fit-rate向量、raw、corrected三者的mixed-only top1、molecule-macro与pair-weighted排序一致率和tie频率；逐fold和全体。top1按生产实现argmax的首个最大值；fit-rate控制provider身份本身的排序能力。另报每provider每fold的OOF AUROC（分子正确性排序），单一类别为null。校正对同provider/fold只是常数logit平移，AUROC不变是计算校验，不是改善判别性的实验发现。它不等于工具之间的相对选择能力。
3. 六cell自然错误分解：全菜单均错/可救；可救中所有正确候选零反馈/至少一个有反馈；零反馈再按此前从未选择/至少一个pending细分。计数守恒，报告占所有决策和占可救错误的两个分母，零分母为null。逐fold、delay和pooled，另按前/后半固定tick、最初D+1次及其后拆分。记录具体entity、provider、正确候选、score、counts和时间，供后续定位。
4. 固定历史上正确候选的最少/最多反馈计数、其冻结分数与实际选择分数的差，只作描述，不计算未经运行的新策略准确率。
5. config先落盘：命令、源码/card/input hash、Python/NumPy、CPU环境、预算；逐样本/逐决策JSONL；summary与短报告分开。原始收据、旧summary与方法均不改。

## 判定边界与下一步

- 锚点或错误分解不守恒、raw hash不符、六cell准确率不能还原上轮summary：停止解释，保留失败，修复分析。
- Oracle headroom不等于可学习headroom；分数排序弱也不证明所有合法特征不可预测。当前AUROC/一致率是development诊断，无确认性显著性或新的CI。
- top1决定当前面板上是否观察到相对fit-rate的选择优势，concordance解释成对排序；二者不一致分别报告。concordance改进不能替代top1；concordance下降也不能否定已观察到的top1改进。
- “全部正确候选零反馈”的数量只定位既有轨迹直接暴露出的盲点。它不是探索收益的因果估计；探索会改变后续轨迹，不能用这个数给探索算法作全局上界。
- 若零反馈仅出现在最初D+1次或pending已存在，应优先解释延迟可用性；若该计数为0，则“全部正确候选无在线反馈”不能解释这些已发生错误。
- 若可救错误主要已有反馈，不能继续以“完全没见过该工具”解释缺口，应检查context条件下的辨别力与信息传递；若明显集中于零反馈，先设计固定预算的探索对照，不先假设探索有效。
- 根据结果只选择一个后续可证伪问题；不同时扩大backbone、探索率、先验计数和训练目标。新策略仍需同合同baseline、完整负结果和未用于选方案的确认数据。
