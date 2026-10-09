# 已释放反馈的条件化利用：最小闭环干预

日期：2026-10-07。状态：执行前冻结；development机制诊断。主方法、论文题目、旧confirmation及三份验收标准不变。

## 问题与范围

四工具诊断中，Corrected Frozen尚有39/722题可救；现有Q在启动前缀后的112次可救错误中109次已有正确候选反馈。下一问题：把每工具一个统一残差改为根据当前分子的残差，能否利用已有反馈改善选择？provider级计数不证明相似context样本充足，也不否定探索价值。

本轮只改变在线残差的特征。沿用同一corrected frozen分数、四checkpoint菜单、722 development、五折、D∈{0,1,4}、n0=20、原顺序和selected-only时钟。三cell×五fold×三delay×两环境，共90流、12,996次真实缓存决策/释放。无新provider推理、离线训练、API或GPU；实际运行在线ridge训练。CPU预算180秒，先定向测试，一次正式运行。

最近方法比较沿用BayesianRouter-adapted的先验中心ridge/秩一更新；本轮是同一分数下的matched mechanism control，不能代替完整现代baseline主表。标准ridge不是新贡献，正结果只决定是否值得继续研究。

## 数学、输入与实例

原始对象只有当前分子$x$、候选工具$a$、已选工具释放的正确性$r\in\{0,1\}$。$q_a(x)$是已封存corrected OOF分数，$u(x)$是现有Morgan64归一化向量；二者都在调用前可用。缓存最后一维是截距，构造本轮特征时移到第一维。

三cell仅定义不同的$\phi(x)$：

$$\phi_{Q}(x)=(1,0_{64}),\qquad \phi_{N}(x)=(1,v_0),\qquad \phi_{C}(x)=(1,u(x)).$$

实例：$v_0=(1,0,\ldots,0)$为固定64维单位向量，与分子无关；$u(x)$由缓存中这条分子的64维指纹给定，L2范数为1。三个$\phi$均65维；Q的平方范数1，N与C为2。若缓存出现零向量或非单位范数，停止并保留资格失败，不静默改定义。

对工具$a$，只累加当前决策前已释放且实际选择该工具的历史。历史$j$的$q_j,\phi_j$固定为调用时值。参数精确解为

$$b_a=\arg\min_b\left[\sum_{j\in H_a}(r_j-q_j-\phi_j^\top b)^2+20\|b\|^2\right],\qquad s_a(x)=q_a(x)+\phi(x)^\top b_a.$$

实例：某条已释放记录选ECFP logistic，调用时$q_j=0.8$，参考结果$r_j=0$，残差目标为−0.8；只进入该工具的$H_a$，不会为另外三个工具生成训练标签。历史集合是释放日志的派生对象。所有坐标包括截距均正则化；$s$用于排序，不裁剪也不声称已校准概率。

令$P_a=(20I+\sum_{j\in H_a}\phi_j\phi_j^\top)^{-1}$；初始$P_a=I/20,b_a=0$。一条反馈释放时，用旧$P,b$更新：

$$v=P_a\phi_j,\quad d=1+\phi_j^\top v,\quad b_a^+=b_a+\frac{v}{d}(r_j-q_j-\phi_j^\top b_a),\quad P_a^+=P_a-\frac{vv^\top}{d}.$$

实例：初始Q收到上例反馈时，$v=(0.05,0,\ldots)$，$d=1.05$，截距变为$-0.8/21$。必须减旧预测$\phi_j^\top b_a$，不能把最后括号直接替换成$r_j-q_j$。每次只改已选provider；pending未释放时不改参数。

两个常数特征有解析退化：

$$s_{Q,a}(x)=q_a(x)+\frac{\sum_{j\in H_a}(r_j-q_j)}{20+|H_a|},\qquad s_{N,a}(x)=q_a(x)+\frac{\sum_{j\in H_a}(r_j-q_j)}{10+|H_a|}.$$

实例：累计残差为−2、已释放20次，Q加−0.05，N加−0.0667。N的有效先验10由固定特征平方范数2推导，未做超参搜索。第一条同context反馈的Q修正为$e/21$，N/C为$2e/22$；因此C必须超过N才能把收益归因于分子区别，而非更强更新。

## 合同、数据与计算

- 使用`fit_score_reference_20261007/v1`的原722×65特征、prior_cache和OOF；依manifest及summary SHA `ad5650d68e8e8d7f9ca497f2fa93c20abd6a4dfc41ed5711ebe83ba7391176c0`验输入；原输入SHA `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- corrected分数严格复用六格公式。natural真实标签不改；controlled stress沿用每折fit最优工具为target，从tick floor(N/2)起若被选就翻转返回标签。stress是评价器控制，不是自然漂移。
- 决策first argmax、一次调用；仅所选reward在tick+1+D释放。不同策略各跑真实闭环，不强行共用反馈历史，不计算shadow策略accuracy。
- Q20必须逐事件还原上一轮corrected/query的30条natural/stress流，分数容差1e-10。Frozen数字只引用同分数已核结果，不重复训练；N必须与独立标量Q10影子状态在同一实际历史上数值一致，该影子不单独计效果。
- 每provider维护65×65矩阵与65向量；release为O(65²)，decision为O(4×65)。保存实际decision/update计时及数值状态字节；不把CPU操作延迟当端到端服务延迟。
- 记录完整可重建的初始状态、decision-time特征hash/q、release target/创新量与更新输入、状态承诺、尾部释放与最终状态。避免逐事件输出所有矩阵；独立核验必须从初始状态及转换记录重建参数，不能只核opaque hash连续。
- config先记录card/code/cache hashes、Python/NumPy/CPU、命令/种子（本协议确定性无随机动作）、原始事件与失败；逐行JSONL、结果、独立核验和报告分开。不得覆盖或删除任何失败尝试。

## 预先确定的指标与去留

主终点为natural成对准确率C−Q、C−N，并列对应Frozen。报告全部fold/delay/pooled、rescued/harmed、前后半、时间/状态成本；stress作为原动态目标的保留检查。每cell每环境2166决策、722重复分子，不作确认性显著性。

**仅在同时满足下列资源规则时进入后续候选研究**（这是本轮预冻结规则，不是旧Q门槛的事后移用）：

1. natural C−Q≥1pp，C−N>0，且C不低于对应Frozen；
2. natural C−Q与C−N的五个leave-one-fold-out差均>0；
3. controlled stress C不低于Q，保留原反馈适应目标；
4. 退化、直接ridge解、延迟合同、全部结果/状态独立重构通过，全部负折/负delay保留。

任一未满足，则不把此固定构造升级为方法，也不换lambda、特征或delay挑正数。C若只胜Q而不胜N，不能归因为context。负结果不证明合法context不可学；正结果不自动证明创新或confirmation。先报告这一干预的结论，再决定是否检查表示/离线排序；不并行扫多个结构。
