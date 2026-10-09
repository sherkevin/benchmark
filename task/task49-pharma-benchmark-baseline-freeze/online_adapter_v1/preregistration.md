# 在线 baseline 接口验证卡 v1

日期：2026-09-26。用途：开发数据上的数值及反馈合同验证；不是方法效果实验，不打开 confirmation。

## 要排除的疑问

若在线算法的反馈时序或计算式不对，后续“实时更新有收益”的结论没有意义。本轮只检验：三个 online kernel 是否实现对应公式，并且每次只用已选工具、已终止题目的到期反馈更新。

最近邻分别为 AAAI 2026 Online Multi-LLM Selection 的 Algorithm 1（此处 H=1）、PILOT §2.2.2 和 BayesianRouter 式 4–6。PILOT / BayesianRouter 的离线学习未完成，本轮名称明确带 online kernel；随机非零 prior 是数值覆盖 fixture，不作为已训练先验，不比较准确率排名。

## 数值验证

- 双精度 rank-one 更新逐步对照批量求逆解，包含非零 prior、不同 arm precision、非单位观测方差和乱序反馈。
- Greedy LinUCB 对照本地固定 commit 官方 notebook 的数值类；仅通过 AST 提取计算函数，不执行 notebook 的 API 初始化或调用。
- PILOT 用一个余弦排序和点积排序相反的例子，确认 query 归一化、cosine scoring 和原 ridge 更新；零 query 拒绝，零 arm 向量分数延拓为 0 并明确披露。
- Thompson scalar draw 对照投影后高斯分布；不把观测噪声加入采样，不将未知 action propensity 写成 1。
- 拒绝题前反馈、跨 query/arm、重复反馈；冻结决策特征，延迟后按真实到达顺序更新。失败为 system_success=0，chemical_correct/recovered_label 保持 null。

失败条件：任一步结果与独立解不一致、非法反馈被接收、有效样本被丢弃，立即停止真实缓存验证并修实现，保留失败日志。

## 真实缓存验证

- BBB development，Task36 33 个具体 provider、round 1。使用已排除 8 个 unresolved 的参考标签池；不把它称为 provider 未见数据。
- 固定 64 个分子：6 个已知 B3clf 失败分子，加按 entity_id 排序的其余前 58 个；最后按 entity_id 排序。这个构成只为了覆盖失败路径，不是代表性抽样。
- 特征：只读 canonical SMILES，RDKit Morgan radius=2、64 bits，L2 归一化；写入明确的 feature manifest 和原始向量。维度是数值检查选择，不是性能调参。
- 3 个 kernel × delay=0/1/4，seed=0。Greedy λ=1、α=0.5；PILOT λ=1、α=0.5，seed=0 的 N(0,0.01) fixture prior；BayesianRouter 同 fixture prior、prior variance=0.02、noise variance=1。没有离线 fit，也不声称 paper default 的完整复现。
- 另用冻结 fixed-provider 控制直接读取 B3clf 某一具体 provider 的 6 条实际失败；保留每条终局输出和收益，确认不丢分母。不能依赖随机策略恰好选择失败路径。
- 本地 CPU 完成接口验证，无新科学工具预测、无 LLM/API、无 GPU 训练；后续表示提取及科学训练仍使用 Nebula A800。
- 总计 576 次自适应缓存选择和 6 次固定失败控制；每次仅一个实际缓存读取。只读现有结果，不重新运行科学工具。

## 记录与完成条件

运行前保存命令、UTC 时间、Python/numpy/RDKit 版本、CPU/系统信息、seed、所有参数、git HEAD、修改代码 SHA-256、缓存输入 hash 和参考真值 hash。测试输出、JUnit 单例结果、按时间写入的 events.jsonl、每条决策/终局/反馈、最终状态及摘要分别保存。

通过标准：每个决策恰有一个终局、一个到期 feedback；policy 在决策时的 version 等于此前已释放更新数；队列清空；selected output 与真实缓存逐条一致；最终在线参数与用真实已释放记录批量重解的参数一致（float64 absolute/relative tolerance 1e-9）。固定失败控制 6 条全保留，正确性与可恢复真值为 null。

准确率总数只作为审计校验项，不作效应、优劣、显著性、实时吞吐结论；本轮不计算用于论文效应推断的置信区间。记录的本地选择/更新时间仅用于排查异常，不能冒充 A800 或完整服务时延。

完成后回看：这一步只让强对照的在线部分可核验，尚不证明动态 JEV 有创新或收益。下一步须补离线初始化，检验冻结表示的适应瓶颈，并取得 A800 的真实表示与训练证据。
