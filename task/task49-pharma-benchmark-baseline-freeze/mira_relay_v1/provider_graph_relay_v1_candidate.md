# Provider-Graph Relay v1：候选机制卡

- 状态：已实现并完成 fold0/seed0 CPU gate；按预注册停止，降为负机制诊断/对照，不升级为主方法
- 日期：2026-10-04
- 触发证据：`margin_family_diagnostic_v1.md/json`
- 目标：解决 fixed provider pool 中 80% 的 selected updates 落在 singleton family、family residual 没有可迁移 sibling 的问题。

## 1. 具体问题

v3 的 family-sharing 依赖人工 family。五折 transfer receipt 中 1,095/1,369 次 MIRA updates（80.0%）来自 singleton family，opportunity 为 0；non-singleton 的 274 次虽然 opportunity 为 100%，相对 Independent 的 action difference 只有 14 条，rescued 为 0。HRA fold0 有动作变化，但 delay4 包含一个 singleton 切换且净损失 1 条。

所以当前需要检验的不是“再加一个 local head”，而是：**能否只用 fit-only 信息构造覆盖更广、可审计的 provider 关系，使 selected feedback 有真实的可迁移目标，同时不越过 selected-only 信息边界。**

## 2. 候选机制

在 fit split 中，对每个 provider $p$ 记录正确性向量 $c_p\in\{0,1,\mathrm{NA}\}^{n_{fit}}$，其中 $c_{p,i}=1$ 表示 provider $p$ 在 molecule $i$ 上的 `status=ok` 输出与冻结 truth 一致，`status!=ok` 记为 NA；pair $(p,q)$ 只在两者都为 `ok` 的交集 $I_{pq}$ 上计算，并要求 $|I_{pq}|\ge m_{min}=128$。令 $\hat r_{pq}$ 是该交集上的 Pearson correlation；收缩相似度固定为 $s_{pq}=\frac{|I_{pq}|}{|I_{pq}|+\lambda}\hat r_{pq}$，其中 $\lambda=32$。若任一向量方差为 0、$s_{pq}\le0$ 或 $|I_{pq}|<128$，不建边。每个 provider 取 $k=2$ 个最大正相似邻居；相似度按固定 `tie_tol=1e-12` 量化后排序，量化相同的分数按 provider ID 字典序打破。fit correctness 只用于初始化关系图，不在 evaluation 期间更新图。图的版本、输入 hash、有效样本数、失败计数、$m_{min}$、$\lambda$、$k$、`tie_tol` 和邻居列表全部封存。

每个 provider 保留 local residual $u_p$。selected provider $q_t$ 的反馈残差为 $\delta_t$ 时，令 $N^+(q_t)=N(q_t)\setminus\{fallback\}$；若 $q_t\ne fallback$ 且 $N^+(q_t)\ne\varnothing$，固定 $\rho=0.5$，令 $a_{q_t,q_t}=1-\rho$，$a_{p,q_t}=\rho W_{p,q_t}$；若 selected provider 是 fallback 或没有可写邻居，退化为 v3 的 immutable/no-op 或 selected-only self update。候选更新为

$$u_p\leftarrow u_p+a_{p,q_t}x_t\delta_t,\qquad p\in\{q_t\}\cup N^+(q_t).$$

其中 $W_{p,q}=s_{pq}/\sum_{r\in N(q)}s_{rq}$；没有邻居时不建立传播边。

只有 selected provider 的 feedback 触发更新；$W$、provider menu、truth 和未选输出不会在 evaluation 中更新或读取。每次释放的计算量是 $O((k+1)d)$，状态量是 $O(|P|d+|E|)$。fallback、runtime hazard、failure ledger 和 delay queue 沿用 v3。`probe_degree_matched_random_graph` 使用同一 $k=2$、同一 $\rho=0.5$、同一 source writable out-degree、target in-degree 多重集和同一更新总质量，只随机置换 source-target 配对；随机边集、seed 和 hash 需要封存。

这不是已确认的新方法。它只提出一个可证伪机制：如果人工 family 的问题确实是关系覆盖不足，fit-only graph 应提高可用 neighbor opportunity，并在相同反馈预算下产生可重复 action consequence。

## 3. 最近邻与公平比较

同一 fold/seed、same frozen encoder、same fit rows、same provider menu、same probe schedule、same epsilon/hazard/fallback、same selected-only delay `{0,1,4}`。额外加入 degree-matched random graph control，区分“有邻居”与“邻居确实来自 fit-only error similarity”：

1. `probe_mira_relay`：人工 family shared residual；
2. `probe_independent_residual`：provider-local residual；
3. `probe_hierarchical_residual`：fold0 已核验并按预登记停止，只作历史诊断，不再扩展；
4. `probe_provider_graph_relay`：本候选。
5. `probe_degree_matched_random_graph`：使用同一 source writable out-degree、target in-degree 多重集和同一更新预算，但随机置换 source-target 配对；只作机制 control，不作为主方法。

图构造只能在 fit split 完成；不能用 validation、evaluation、confirmation 标签选择 $k$、相似度阈值或 epoch。第一批只做 fold0/seed0 CPU gate，先核验 graph manifest 和初始 score 是否能由独立 NumPy 实现重算。

## 4. 预先固定的指标和停止条件

### 实现和信息边界

- graph manifest、fit-row hash、provider roster、family/graph source hash、邻居列表和 code commit 可独立复核；provider lineage 和 fit/evaluation overlap 仍按现有审计分层，不把相关 provider 变体当独立证据；
- selected-only delayed event contract、pending queue、failure/unknown 语义和 state hash 全部沿用 v3；
- 未选 provider 的 state exact invariance；每次更新最多写 selected provider 和其 $k$ 个邻居；
- update p50/p95、全菜单 score 时间和 state bytes 不超过 v3 预登记预算的 2 倍。

### 主要效果指标

- `graph_opportunity_rate`：selected provider 的图邻居中至少一个同时 available 的比例；实现 gate 固定为至少 `0.400`，即超过当前人工 family 的 0.200 基线两倍；这个阈值只是资源/可识别性 gate，不是效果成功线，也不在 evaluation 结果上调节；
- paired action changes、rescued、harmed、net utility vs MIRA 和 Independent，按 delay 分开；
- 按 graph degree、similarity、provider family、failure status 分层；三个 delay 不是独立重复；
- family-size diagnostic 作为 context，不把 raw update 行当独立样本。

### 停止条件

停止并保留负结果，如果出现任一情况：

1. graph manifest 或 independent replay 不通过；
2. graph opportunity 低于 `0.400`，或虽然达到 `0.400` 但相对 MIRA 在至少两个 delay 没有真实 action change；
3. 相对 MIRA 的 rescued/harmed 只有一个 distinct molecule、一个 delay 或一个 provider，或所有 action changes 的 net 都为 0；
4. 需要 evaluation/confirmation 标签、额外调用、额外特征或超过预算；
5. delay4 出现系统性 harmed，且不能由预先登记的 failure slice 解释。

通过 fold0 gate 也只允许继续五折 development；不自动打开 seed1、A800 或 confirmation。

## 5. 先做的零机时检查

1. 从 fit rows 生成图并保存 `graph_manifest.json`，统计 degree、相似度分布、effective overlap 和 singleton coverage；
2. 用独立实现重算图和初始 logits，检查与 candidate runner 完全一致；
3. 只做 synthetic one-step update，验证 sparse graph update 的 $O((k+1)d)$ 复杂度、未选 provider exact invariance 和 $W$ 行归一；
4. 通过后再预登记 fold0 CPU receipt；任何一个检查失败都不写效果表、不申请 Nebula。

## 6. 当前决定

这张卡只把 family opportunity bottleneck 变成可检验候选，不改变生效主线、目标 claim、benchmark 或 baseline 选择。它能否成为论文方法，取决于上述 gate 是否在真实冻结 BBB provider pool 上通过，并且能否相对 MIRA 产生跨 molecule、跨 delay 的 paired evidence。

## 7. 新颖性风险和进入条件

图 bandit、relational bandit、label propagation 和 algorithm selection 都可能已经包含“相似臂共享反馈”或“从历史性能构造关系”的做法。因此本候选不能把稀疏图、邻居传播或 delayed update 单独写成创新。进入 CPU gate 前必须完成最近邻矩阵，至少逐项核对：图如何获得、是否使用 selected-only feedback、是否支持 delayed release、是否有安全/failure ledger、是否在 provider menu 上做 action selection，以及本候选的不可替代约束是什么。

如果最近邻已经覆盖相同的 fit-only graph + selected delayed diffusion 机制，则本候选降为 baseline/control，不投入实验；如果差异只剩医药 provider 应用，也必须把贡献写成任务/协议与可复现实证，不能写成通用 graph bandit 新算法。

## 8. 2026-10-04 先验核验后的决定

已完成一轮只使用一手/作者来源的核验，记录见
`research/provider_graph_relay_prior_art_20261004.md`。最近邻至少包括 feedback-graph bandits、GOB.Lin/CoLin、correlated-arm bandits、label propagation 和 online algorithm portfolios。它们已经覆盖了“图关系上的反馈共享”“相关臂之间的迁移”和“在线算法选择”这些一般性叙述。

因此，`W` 加权邻居更新本身不再作为新颖点。候选只有在以下条件同时满足时才值得实现：

1. graph 明确定义为 fit-derived provider transfer graph，而不是可观测的 environment feedback graph；未选 provider 的结果、奖励和反事实标签仍不可见；
2. 与 `W=I`、人工 family、degree-matched shuffled graph，以及至少一个可适配的 relational/graph-sharing comparator 使用同一菜单、反馈、更新质量和计算预算；
3. 结果在 paired molecule/scaffold 单位上产生跨 delay 的 action-level consequence，而不只是提高 graph opportunity 或改变内部 state；
4. 论文把贡献表述为部署工具的 outcome graph、selected-only delayed relay 和可审计回放合同的整体方法，不能声称提出通用 graph bandit 或通用 online algorithm-selection 原理。

在上述矩阵和零机时检查完成前，Provider-Graph Relay 仍是候选机制，不进入主方法，不申请 Nebula，也不写入论文效果表。若图机制与最近邻无法通过接口和信息边界区分，则降为 baseline/control。

## 9. 更新公式的数学边界（进入实现前固定）

候选更新是一个**迁移启发式**，不宣称是 selected provider Bernoulli 后验的精确更新。令 selected provider 为 $q$，释放的 correctness residual 为 $delta_q=y_q-hat p_q$，则候选写入

$$u_pleftarrow u_p+eta a_{p,q}x_tdelta_q.$$

若把同一查询在 recipient $p$ 上的 correctness loss 写成 $ell_p$，其一阶变化为

$$Deltaell_p^{(1)}=-eta a_{p,q}lVert x_t
Vert^2delta_pdelta_q.$$

所以只有在 recipient 与 selected provider 的**当前 residual 同号**时，这次 relay 才有局部降损含义。fit split 上的 correctness Pearson correlation 只是这个条件的代理，不能直接当作保证；validation/evaluation 只用于检验该代理是否失效，不能回流建图或调参。

同理，top-$k$ Pearson 图一般是有向且不对称的。不能把该更新写成对称 PSD 预条件器或精确 Bayesian posterior；若未来需要这种解释，必须先额外证明 $A=[a_{p,q}]$ 的对称 PSD 条件，否则论文只使用上面的可计算迁移更新和实证边界。

### fallback 不变量

`fallback_provider` 永远不接收 graph relay 写入。定义 $N^+(q)=N(q)\setminus\{fallback\}$：若 $q=fallback$ 或 $N^+(q)=\varnothing$，更新退化为 v3 的 no-op 或 selected-only self update；否则只在 $\{q\}\cup N^+(q)$ 上按同一总质量归一化。这样 fallback score 与 hazard ledger 的冻结语义不被图边意外改变，且这一规则必须出现在 graph manifest、随机图对照和独立 verifier 中。

## 10. fold0 CPU gate 结果与停止决定（2026-10-04）

fold0/seed0 已按本卡合同完成：145 个 evaluation molecules、4 methods、3 delays、12 streams、5,220 条 raw events。独立验收器 `verification_v2.json` 从冻结 provider cell 重算 selected output、reward、反馈时钟、state hash、fallback 不变量和 paired metrics，状态为 `verified`。旧验收器的误拒绝已保留在 `verification_rejected_v1.json`，不影响正式回放结果；紧凑证据与事件哈希见 `provider_graph_relay_fold0_v1_report.md` 和同目录 `raw_manifest_v1.json`。

Graph Relay 相对 MIRA 的 paired `(rescued/harmed/net)` 为：delay0 `1/3/-2`，delay1 `1/3/-2`，delay4 `4/4/0`；相对 degree-matched random graph 为 `0/1/-1`、`0/1/-1`、`1/2/-1`。它确实改变了动作，但没有在任何 delay 产生正净差，并触发了本卡的负净差/无稳定收益停止条件。

因此本候选停止 fold1--4、seed1、Nebula 和 confirmation 扩展，保留为已核验的负机制诊断/对照实现。该结果不改变 benchmark、baseline 或目标 claim；它说明 fit-only correctness graph 的静态 relay 在当前菜单与反馈合同下不能可靠地把 action change 转成 utility gain。任何后续候选都必须先解释并检验如何避开这个失败模式。
