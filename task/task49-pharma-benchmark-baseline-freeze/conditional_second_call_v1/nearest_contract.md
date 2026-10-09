# 条件调用的最近邻与可复用合同

2026-10-08；状态：原文核对后的方法分析，实验尚未产生结论。正式文献、代码、版本与来源在 [literature/report.md](literature/report.md)。本文件不修改生效故事、方法或目标主张。

## 这次核对改变了什么

OCL（ICML 2024）已按需获取付费教师标签，并在线更新分类器与defer gate；Cascade Routing已经根据当前返回预测后续模型质量/成本，并选择继续或停止。因此本次小实验检验的是合法条件信号与paired标签，不把“多调用一次”“加在线更新”分别当作新贡献。

主论文的 source-aligned delayed update 仍是独立的待整合机制；它能否在调用后决策中带来增量，必须另作matched实验。目标不因近邻重叠而取消，方法区别也不能通过改名字取得。

## 一个可精确核对的等价关系

固定终局规则d，Cascade Routing的同任务utility适配把集合质量设为负的期望终局损失：

$$Q(E)=-\mathbb E[\ell(d(E),Y)\mid E],\qquad Q(E,b)=-\mathbb E[\ell(d(E,O_b),Y)\mid E].$$

Case：第一次工具输出是BBB阳性概率.58，当前证据E只含已见结果。若固定规则拒答，损失为.25，Q(E)=-.25；此时不读取真实Y或其他工具输出。

选择下一工具b的比较是：

$$Q(E,b)-\kappa_b>Q(E)\quad\Longleftrightarrow\quad G_b(E)>0.$$

Case：若当前预计损失.25，拿到b之后的预计终局损失.12，调用成本.01，二者都判断追加的净收益为.12，并选择调用。Q(E,b)已对尚未观察的O_b取条件期望，不允许提前读O_b。

所以，在同一验证器、估计器、特征、数据和updater下，两种命名产生相同决策，不能当两条独立baseline。最近邻原文的native质量是estimated-best-output质量；换成共同refusal-aware loss是明确的任务适配，不是假称原文自带本任务全部语义。

## 对照如何分工

| 对象 | 可以复用 | 必须明确的差异 | 当前用途 |
|---|---|---|---|
| CR-native | 输出条件质量估计、合法集合搜索、成本权衡 | 原生best-output质量与共同验证器的终局损失不同 | 后续强baseline保真实现 |
| CR-utility适配 | 同一d下的停止/继续比较 | 与本候选G决策等价；学习器相同时合并为一条 | 本轮gain诊断的解释基准 |
| OCL原生 | 付费标签获取、在线gate和分类器更新 | 教师伪标签、可训练小模型、固定顺序；无独立真值释放队列 | 最近邻合同参考 |
| OCL gate-only delayed适配 | 当前错误/分歧预测的小gate | 冻结provider、替换标签及增加delay均是适配 | 后续受限机制对照，不冒称完整OCL |
| conditional vs pair-mean | 同一离线配对标签、同验证器与预算 | 唯一改变是能否读取首次输出置信度 | 当前静态信号消融 |
| source-aligned vs matched updater | 同一value目标、合法pair与release时钟 | 必须明确新增状态究竟改变哪个更新，并验证额外收益 | 尚未实施，不预填结果 |

## 直接复用边界

已下载OCL正式20页PDF、固定源码commit和CR原文。OCL源码的部分路径预先计算所有小模型输出，实时数据wrapper也可能在gate选择前调用教师；不能直接拿逻辑选中计数当实际调用数。原源码没有发现LICENSE，当前只保留研究审计副本，不复制其实现作为本项目发布代码。

小实验先复用已核验的真实概率缓存、scaffold划分与A-only首次选择器。若静态gain模型不能利用额外信息，优先从oracle headroom、验证器能力、pair支持与样本量分析原因。若有效，下一步完成CR适配的保真与延迟更新归因。两种结果都必须保留，不更换指标追胜。
