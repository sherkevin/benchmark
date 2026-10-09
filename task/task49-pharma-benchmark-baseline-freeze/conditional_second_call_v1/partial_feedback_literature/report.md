# 提前反馈候选：五篇一手文献审计

2026-10-08。范围：核查 [候选](../early_feedback_candidate.md) 的提前收益、informative delay、临时目标和延迟校正；这是近邻审计与基线建议，未运行实验、未修改论文、未冻结新方法或长期决议。全部结论基于下载的正式论文正文；搜索摘要只用于定位。

## 当前可以下的结论

**“先临时反馈，随后改正”以及“先写 Gram，结果回来再补响应”已有直接先例，不宜单独申报为新算法贡献。** ALT 2025 的 Evolving FTRL 允许对过去反馈不断改写；ICML 2020 的 OTFLinUCB 用全部已选动作的特征矩阵和当前可见的响应估计；GP-BUCB 在 2014 年已使用当前均值作为尚未返回结果的替代值。下文逐项区分其合同与本候选，不能把这些论文的保证直接搬到医药上下文。

本任务仍有一个明确、可检验的应用问题：**付费得到第二输出后，有些配对损失差在真值前就被确定；怎样利用这部分精确信息，同时避免先到负标签改变后续探索。** 这是尚待验证的任务结构，当前检索不足以证明其独有性，更没有证明它产生在线效用收益。

第一原则是信息是否已经确定目标，而不是反馈被叫作“即时”“代理”还是“延迟”：对固定损失与固定验证器，每个已取得 pair 都可计算 `g(0)` 和 `g(1)`。两者相同才是精确早标签；两者不同只获得一个可行区间和可能有用的中间状态。选择第二个工具之前不能查看它的输出。

构造案例（非实测）：取额外调用成本 $\kappa=0.01$、拒答损失 $\rho=0.25$，答错/答对损失为 1/0。若追加前后都答 1，则 $g(0)=g(1)=-0.01$；若由拒答改答 1，则 $g(0)=-0.76$、$g(1)=0.24$。后一个 pair 已知动作变化，却还不知道净收益的正负。

## 核读的五篇原文

原始 PDF、提取文本和出版社页面在 [sources](sources/)；URL、获取时间、每个文件 SHA256 见 [source_manifest.json](source_manifest.json)。以下页码是本地 PDF 页序（从 1 开始），并给出正文节/算法定位。

| 原文 | 实际核读位置 | 反馈/算法合同 | 与候选的精确关系及限制 |
|---|---|---|---|
| Lancewicki, Segal, Koren, Mansour, ICML 2021, [Stochastic Multi-Armed Bandits with Unrestricted Delay Distributions](https://proceedings.mlr.press/v139/lancewicki21a.html) | PDF pp.3,6–7；Protocol 1、§4、Algorithm 6 OPSE | 每轮 reward/delay 向量从固定联合分布独立抽取；允许 reward-dependent delay。每臂分母是所有 pulls；未到奖励分别填最小/最大可能值构造上下界，然后才淘汰。 | 正中“早到标签偏负”的问题。不能把已到样本均值当总体均值。原文是固定有限臂、平稳随机奖励，没有分子上下文、固定验证器或配对收益；这里只复用 pending 计数和上下界思路，不沿用其 regret。 |
| Esposito et al., ICML 2023, [Delayed Bandits: When Do Intermediate Observations Help?](https://proceedings.mlr.press/v202/esposito23a.html) | PDF pp.2–5；§2–4、Algorithms 1–2 | 动作后立即见有限状态，稍后见该状态损失；stochastic-loss 分析要求跨轮 i.i.d. 的状态损失。MetaBIO 有足够同状态历史才向 base bandit 提前提交状态平均损失的乐观估计，否则等待；收到真值会更新状态数据库，已提前提交那轮不会再重复反馈给 base。 | “动作→已见中间状态→延迟损失”已有完整框架，且不保证中间观察一定有用。`C=动作是否改变` 的 C=0 状态有确定收益，但 C=1 的收益还依赖分子、动作方向和 provider；直接池化 C=1 不满足其状态充分性。它不是本候选的逐样本残差校正。 |
| Bar-On & Mansour, ALT 2025, [Non-stochastic Bandits With Evolving Observations](https://proceedings.mlr.press/v272/bar-on25a.html) | PDF pp.2,5–6,8–10；§3、Algorithm 2、§4.1 | oblivious adversary 预定真实损失及每轮对过去损失的修订；bandit 只见自己当时所选动作的修订。Evolving FTRL 使用最新版本的累计 importance-weighted loss，分母固定为当时采样概率；理论显式支付反馈误差。§4.1 明确包括先收到 hint、之后替换为真值。 | 最直接覆盖“临时值→后续覆盖→始终优化真实损失”的抽象创新。它不是上下文 ridge，且学习器自己历史训练的 hint 是自适应的，不能直接满足其预定反馈假设。其 unbiased 指对当前反馈值的 action-IPW，无意保证 hint 对最终真值无偏。 |
| Vernade et al., ICML 2020, [Linear Bandits with Stochastic Delayed Feedback](https://proceedings.mlr.press/v119/vernade20a.html) | PDF pp.2–5；§3–5、Eq.(2)、Theorem 1、Algorithm 1 | Bernoulli 转化奖励；观察到的响应在转化到来前为 0，未转化也永远为 0。所有所选特征进入 V，B 随转化更新；设窗口 m，过晚转化被截断。估计对象是共同转化概率因子 τ_m 乘真实参数，置信半径另含 pending 项。 | 已有“单次 Gram 插入、后来修正 response”的线性先例。其零不是精确负标签；原文的独立/common-delay 转化合同和被截断的缩放目标，不是本任务 signed pair gain。不能照搬零填充或半径，更不能因为 final drain 相等就声称中途无偏。 |
| Desautels, Krause, Burdick, JMLR 2014, [Parallelizing Exploration-Exploitation Tradeoffs in Gaussian Process Bandit Optimization](https://jmlr.org/papers/v15/desautels14a.html) | PDF pp.6–7,10–12；§3.1、§4.1、Algorithm 2、Eq.(7)–(9) | GP-BUCB 的均值只用已到真实观测，方差使用所有已选位置；等价于给 pending 填最新已观测 posterior mean。作者专门放大置信系数以防凭空缩小不确定性。 | 非零模型预测代填已有直接先例。线性核对应 ridge 结构，但原文反馈映射/噪声/函数假设与 outcome-dependent 提前释放不同。可复用“刷新代填值保持均值不变”作匹配对照，不能把 pending 特征当成真实标签数量。 |

注意：这五篇是本次有界审计的全部深读论文。没有以找到这些论文为由宣称已穷尽 2026 年相关工作。2020 非平稳中间观察等其他结果只作检索线索，没有纳入证据结论。

## 对本候选的逐项判断

| 候选表述 | 审计判断 | 允许保留的范围 |
|---|---|---|
| 同动作的 `g=-κ` 在第二次返回后精确成立 | 推导正确；这是损失差相消，不是“两个工具一致所以正确” | 必须实际取得 pair，并冻结动作/损失/版本；不是免费未选标签 |
| 先到负标签会造成偏差 | 已有明确理论先例 | 描述为 reward/information-dependent release；应记录已取得数量和未到标签，不能只记录已到样本 |
| 用预测目标先写 A,b，真值后只补 b | 递推正确，抽象和线性结构均有先例 | 实现/适配贡献；不能单凭该账目提出原创机制 |
| drain 后等于同一 acquired set 的 batch ridge | 代数恒等式 | 固定 φ、w、λ、初始化；浮点实现是容差内一致。排空不会恢复不同采样轨迹错失的 pair |
| 预测代填减少偏差 | 尚未成立 | 需检测 pending 的条件误差；IPW 只校正 acquisition selection，无法自动校正错误代填或 reward-dependent release |
| 比全等待好即证明残差校正有效 | 不成立 | exact early signal、旧预测惯性、方差缩小、探索改变都可能贡献，须拆开对照 |

### 一个应直接纳入匹配基线的恒等式

把已精确知道标签的 paid pairs 记为 $R$（含动作不变的早标签），其余为 $P$。$\phi_i$ 是调用前冻结的特征，$w_i$ 是冻结权重，$\lambda$ 是 ridge 正则；$A_0,b_0$ 是同一初始化。当前候选使用 16 维特征，故 $A_R$ 为 $16\times16$，$b_R,\theta_R$ 为 16 维。沿用这些量：

$$
A_R= A_0+\sum_{i\in R}w_i\phi_i\phi_i^\top,\quad
b_R=b_0+\sum_{i\in R}w_i\phi_i g_i,\quad
\theta_R=A_R^{-1}b_R.
$$

构造案例（用一维特征演示账目，非实测）：取 $A_0=\lambda=1$、$b_0=0$，$R$ 只有一条 $\phi_1=w_1=1$、$g_1=-0.01$ 的已知 pair，则 $A_R=2$、$b_R=-0.01$、$\theta_R=-0.005$。另一条已取得、真值未到的 pair 属于 $P$。

若每次真实信息变化后，把所有 pending 的临时目标刷新为 $\phi_i^\top\theta_R$，则

$$
(A_R+\sum_{i\in P}w_i\phi_i\phi_i^\top)^{-1}
(b_R+\sum_{i\in P}w_i\phi_i\phi_i^\top\theta_R)=\theta_R.
$$

续上面的构造案例：设 pending pair 的 $\phi_2=2,w_2=1$，刷新目标为 $-0.01$，则总 $A=6$、总 $b=-0.03$，解仍为 $-0.005$。若缓存的旧目标是 $0.02$，总 $b=0.03$，解变为 $0.005$；差异来自缓存值。

这是本审计针对 ridge 的推导，由 GP-BUCB 的均值代填启发；不是引述论文里的上下文定理。它说明**刷新均值代填在点预测上等于只拟合精确标签**。插入时预测一直缓存、只在该条真值到达时修正，会在别的真值先到时保留旧信念的影响。因此其对比必须有这个刷新对照，才能知道收益来自新信息还是陈旧代填造成的平滑。两者都不自动消除早标签的选择偏差。

### 还能免费得到的是区间，不是额外真值

对已取得 pair 计算下界 $L_i=\min(g_i(0),g_i(1))$、上界 $U_i=\max(g_i(0),g_i(1))$。$A_{all}=A_R+\sum_{i\in P}w_i\phi_i\phi_i^\top$ 含全部已取得 pair 的特征。若 $P$ 中标签尚未确定，取一个合法 query 特征 $\phi$，令 $c_i=w_i\phi^\top A_{all}^{-1}\phi_i$，它表示该缺失目标对 query 预测的线性影响。同一 acquired set 的完整 ridge 点预测位于：

$$
\phi^\top A_{all}^{-1}b_R+
\left[\sum_{i\in P}\min(c_iL_i,c_iU_i),\;
      \sum_{i\in P}\max(c_iL_i,c_iU_i)\right].
$$

构造案例（沿用上述一维账目，非实测）：pending pair 若由拒答改答 1，则 $[L_2,U_2]=[-0.76,0.24]$；query 取 $\phi=1$，有 $c_2=1/3$、已知项为 $-0.01/6$，预测区间约为 $[-0.255,0.07833]$。若 query 改为 $-1$，$c_2$ 变为 $-1/3$，下端点应配 $U_2$；这就是不能忽略系数符号的原因。

这是对缺失目标的确定性包络，不是对总体期望收益的统计置信区间。使用 min/max 时必须考虑 `c_i` 的正负，不能只拟合“全低标签”和“全高标签”便假设得到所有 query 的上下界。可借 OPSE 的思路把它用作过早关停/过度自信诊断。

## 最小可复用交付与下一步

[baseline_contract.md](baseline_contract.md) 给出无需新依赖的直接实现合同：全等待 ridge、精确标签 ridge/刷新均值代填、候选缓存代填，以及一个使用全部 immediate 状态的分解基线。前两者是必要匹配对照；分解基线针对“早到全负”问题，不把已有通用 delayed learning 换名。

同一 paid stream 先检验：D=0 三种 ridge 的最终与中间状态应一致；D>0 要同时测 pending error、正收益 pair 预测和精确标签条件下的偏移；drain 检查只能证明账目。闭环另测 utility、预算、正收益 pair 获取率，并保留强制付费探索。候选若只赢“直接抢先训练负样本”，却不能赢精确标签/刷新或分解对照，不能主张新机制。

可复用资产已实际落盘：五篇正式 PDF/文本/页面及哈希；算法/假设定位；可实现的 baseline 合同；刷新均值恒等式与 pending 区间诊断。未启动 provider/LLM/GPU 调用，未读或重用 confirmation 真值，未改生效文档或论文。

## 检索留痕与范围限制

遵循 agent-reach/platform-search 的检索路由，并优先仓库配置的 idealab。idealab scholar 返回限流并自动退到 OpenAlex；general search 的 Bing 结果出现无关页面，未作为证据；unlimited DuckDuckGo 四个精确查询无结果。随后 built-in web 成功定位官方 PMLR/JMLR 原文，再用直接 HTTPS 获取。检索式和执行摘要见 [search_audit.json](search_audit.json)。正文细读只涉及这五篇，不包含第三方综述作为事实来源。
