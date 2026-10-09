# MIRA 下一版 method card：实现风险审查与最小可证伪候选

日期：2026-09-29  
角色：方法/理论审计（独立补充，不改变 `docs/研究主线/生效/*`）  
状态：候选设计；未运行 confirmation，未声称方法成立。

## 0. 审计立场

**POSITION**  当前 MIRA 最可能的失败原因不是缺少一个更复杂的在线优化器，而是四类问题叠加：事件重放有审计盲区、selected-only 信息边界被越过或被误读、表示和目标标签单位不一致、以及在确定性 protocol slot 上使用了过强的统计结论。下一版应把贡献收窄为“信息安全的 incumbent-safe switch”，而不是再主张未选 provider 的在线质量估计。

**TARGET**  审查 `development_master_table_v1/report.md` 与已有 JEV、router、verifier 和 version-event 候选，给出可以在现有 BBB CPU replay 上运行的一个 MIRA 核心机制。该机制必须有明确输入、状态、更新、动作规则、可证伪预测和停止条件。

**EVIDENCE**  当前 Best Fixed 为 `0.9155`，Oracle 为 `0.9972`；Matched Online-SGD 在 delay `{0,1,4}` 为 `0.9024/0.8975/0.8975`，仍低于 Best Fixed。PILOT、BayesianRouter 和 Greedy 在大多数 delay 低于自己的 Frozen。AG-JEV 的支持审查显示，即使全量随机审计，33 个 provider 平均只有约 `21.9` 个样本，带四个 context cell 时每格约 `5.5`，置信门不能打开。dynamic verifier 的动态阈值三种 delay 都为 `0.900277`，平均调用 `1.490305`，低于静态 `0.923823` 且超过成本上限 `1.25`。相反，controlled version event positive control 在有预先注入的 provider degradation 时，fit-curvature residual 相对 Frozen 的事件后增益为约 `+3.0--3.7` pp，说明“已观测的选中 provider 退化”是当前唯一有清楚学习对象的动态信号。

**REASONING**  直接估计每个未选 provider 的当前 residual 需要额外的 propensity 或结构共享；当前短流无法同时支付 33-provider × context 的支持成本。一个更窄而可识别的规则可以只使用 selected-only feedback 来否决已经被观测为不可靠的 incumbent，并把未观测候选的质量仍锚定在 fit-only 先验。这样不会解决一般 P/Q 不可识别性，但不会伪装已经解决它；在无退化的固定池上应当保持接近 Frozen，在明确退化事件上应当恢复。

**DECISION TEST**  若下一版不能在独立事件回放中重现“收到目标 provider 的释放失败后，选择率下降且事件后准确率恢复”，或在无事件控制上不劣于 Frozen，则停止 direct-selector 动态主张，保留为负证据。

**CONFIDENCE**  0.86（对当前主要风险的诊断）；0.63（对下述最小候选在受控事件上的可行性）。

## 1. 最可能的问题（按类别排序）

### 1.1 代码/事件重放：`verified` 不等于策略复现

**POSITION**  目前最危险的工程错误是把字段、队列和状态 hash 校验当作完整的 policy replay。对于 MIRA，必须从冻结输入和原始事件重新计算 `q_a^0`、阻断状态、菜单 argmax 和每个 release/update 的顺序；仅核对日志中的 `action` 或 `state_hash` 不能排除实现把未来标签、全 panel 或未释放事件读入策略。

**TARGET**  影响方法论评价 H1/H2/H5/H8，以及 M5（信息边界）、M8（可恢复事件轨迹）、M13（可复现实验）和 M15（对照公平）。任何越界都会把收益归因全部作废。

**EVIDENCE**  structured CPU 旧 verifier 曾只检查字段；后续 recovery v2 才补充 score/menu/argmax 和 queue 检查，但仍不是从 checkpoint 重新运行模型。方法文档明确要求：决策前只可见 `H_t`；反馈必须在 release tick exactly-once 更新，unknown、provider failure、未释放事件不能静默转为 0/1。已有一个早期候选还出现过 feedback early-tick bug。

**REASONING**  MIRA 的状态是可序列化策略状态，而不是报告中的附加计数。如果动作由日志直接提供，任何未来信息泄漏都会被同一日志掩盖。delay=0 也必须先终止、再 release、再 update、最后影响下一次 action；不能在当前 action 内即时读取当前题反馈。

**DECISION TEST**  为每条 stream 写一个独立 NumPy replayer，只读 `(input_hash, menu, selected provider, terminal, release tick)` 和 fit-only `q_a^0`。逐事件重算 `blocked_a`、argmax、pending queue、状态 hash；与生产策略逐项比对。增加两条故障注入：把一个未选 provider 的标签随机翻转，以及把一个未到期事件提前一个 tick。正确实现必须在前者完全不改变动作，在后者仅从合法 release 之后改变后续动作；否则停止。

**CONFIDENCE** 0.91。

### 1.2 信息边界：selected-only 下不能宣称未选 provider 的当前质量

**POSITION** 任何把 `H_t` 中选中 provider 的标签直接泛化为所有 provider 的 residual、或把固定菜单当作已经有 off-policy 支持的策略，都没有可识别性。MIRA 的 pre-call selector 只能用分子、端点、菜单、fit-only 先验和已释放 selected feedback。

**TARGET**  影响 H1/H3/H4/H6/H9/H10，以及 M5（selected-only 信息边界）、M8（反馈到达时序）、M15（机制与 baseline 的可比性）。

**EVIDENCE**  可构造环境 `P,Q`：策略始终选择 A；A 的反馈分布完全相同，但未选 B 的正确率在 P 中为 `0.95`、Q 中为 `0.05`。两环境产生相同 `H_t`，任何只依赖 `H_t` 的更新函数状态相同，无法同时识别 B 的当前质量。AG-JEV 试图靠随机审计修复，但支持计算显示 gate 必然关闭；margin propensity residual 也没有超过同探索 Frozen。

**REASONING**  若目标是“provider adaptation”，必须声明额外条件：固定正探索/propensity、fit-only 结构共享、可重复局部上下文或可观测 provider/version event。没有这些条件时，安全可识别的动作只能是：继续按冻结先验选择，或在观测到已选 provider 的风险后把它列入黑名单。后者是 one-sided veto，不是对未选 provider 的 posterior 估计。

**DECISION TEST**  代码审计中禁止读取完整 evaluation panel、当前题真值和 post-call confidence；日志必须包含每次动作的可见历史摘要和 propensity。加一个 P/Q synthetic replay：保持 A 的 selected labels 与菜单完全不变，只反转 B 的隐藏结果。若 selector 在无 B 观测时改变动作或报告 B 的在线 residual，判定越界。

**CONFIDENCE** 0.97。

### 1.3 表示/目标错配：BT 排序、system success 和 chemical correctness 不是同一个量

**POSITION** 现有候选混用了 BT/logit 排序分数、provider numeric probability、`system_success` 和化学端点 correctness。它们的单位不同，不能直接相加、共享一个 Beta 风险状态或用一个 threshold 解释。

**TARGET**  影响 H2/H3/H5/H7/H10；M5（目标定义）、M8（状态更新的标签语义）、M13（指标完整性）。

**EVIDENCE**  `q_a^0(x)` 在 AG-JEV 中被明确定义为 fit-only 校准的端到端 `system_success` 概率；早期结构化候选却把 BT/CLS hidden 输出与在线 head 混合。方法合同还要求 unknown、provider failure 和未释放事件分别记账，不能静默填成错误。调用后 numeric confidence 只能在 provider 已返回后用于 verifier，不能喂给 pre-call selector。

**REASONING**  若 `r=system_success` 把 provider 执行失败和化学预测错误合并，安全阻断可能惩罚错误对象；若 `r=chemical_correctness` 又把运行失败记成 0，会高估化学风险。下一候选应先固定一个目标：selector 只用合法、可释放、具有明确 0/1 语义的 `system_success`（并单独报告 chemical correctness/unknown）；fit-only 校准将 BT 分数映射到该目标的概率单位。post-call confidence 另做 verifier，不参与本候选的 pre-call action。

**DECISION TEST**  在 raw event 中强制保存 `terminal_status ∈ {success, chemical_incorrect, provider_failure, unknown}` 与 `reward_valid`。分别跑：(i) 仅 valid chemical correctness 更新；(ii) 仅 explicit provider failure 触发阻断；(iii) 把所有 `0` 混合更新的错误实现。若 (i)/(ii) 与 (iii) 的动作或指标无法区分，说明标签映射未被实现隔离，停止。

**CONFIDENCE** 0.93。

### 1.4 统计不足：确定性 protocol slot 被误当独立随机重复

**POSITION** 现有 `seed=0/1` 是确定性 protocol slot，不是 iid seeds；旧 bootstrap 若把分子行独立抽样会产生伪精度。所有动态比较必须以 molecule/scaffold 全局 cluster 做 paired resampling，并把 provider family 与曝光量作为敏感性分析。

**TARGET**  影响 H6/H7/H8/H9/H10；M5（统计设计）、M8（延迟比较）、M13（可复现）、M15（对照公平）。

**EVIDENCE**  development master 明确写出 seed 是 protocol slot，不能生成 seed 置信区间；structured JEV 的第一版 bootstrap 被审计为 pseudoreplication。33 个 provider 也含相关家族成员，不能把每个 provider 当独立证据。AG-JEV 的 gate 不是“调参太保守”，而是样本量下置信半径约 `1.05`（无 context）或被截到 `2`（四格 context）。

**REASONING**  微小的 pooled accuracy 差值不能独立支持 dynamic claim。必须同时报告全局 scaffold-cluster paired CI、每 fold/slot 差值、family macro、provider exposure、delay-specific recovery 和调用/更新时间。门槛应在读取 evaluation 结果前冻结；不能因为 gate 全程关闭而事后减小 `n_min` 或增大 context。

**DECISION TEST**  对同一 raw JSONL 做两种分析并预先固定：全局 scaffold cluster paired bootstrap（fold 先聚合、molecule 只在 cluster 内重采样）和 fold-level sign/permutation test。若两者对 MIRA-SafeSwitch 的事件后差异方向不一致，或收益只由一个 family/slot 驱动，停止主张。

**CONFIDENCE** 0.95。

## 2. 下一版 MIRA method card：MIRA-SafeSwitch

### 2.1 定位与信息权限

**POSITION** 采用“incumbent-safe one-sided veto”作为下一版最小核心。它不估计未选 provider 的在线残差，也不把 scalar Kalman、Beta 或 UCB 本身包装成新颖后验。它只回答一个窄而可识别的问题：在 fit-only 冻结先验保持不变时，能否用已释放的 selected feedback 阻断一个已经观察到退化的 provider，并在事件后恢复。

**TARGET**  这是 MIRA selector 的最小实现卡；verifier/escalation 不并入 selector 分数。它把动态 claim 限定为“observed incumbent degradation 下的安全切换”，固定池无事件时只要求不劣，而不要求超过 Best Fixed。

**EVIDENCE**  controlled version event positive control 已显示目标 Best Fixed provider 的 selected failures 可以使低维 residual 降低选择率并提高事件后 accuracy；普通固定池上 Online-SGD、PILOT、BayesianRouter 和 Greedy 没有稳定超过 Best Fixed。AG-JEV 的全 provider/context 估计在当前支持下不可行。

**REASONING**  该规则将识别对象从“所有 provider 的当前质量”改为“已被选中且有 released evidence 的 provider 是否应暂时禁用”。未观测候选仍使用 `q_a^0`，所以不会制造 P/Q 区分能力。它若在无事件数据上有显著优势，反而需要怀疑实现或静态先验泄漏；它的主要证伪场景是受控 provider/version degradation。

**DECISION TEST**  先跑无事件对照，再跑已有 `controlled_version_event_v1` 的同一 event roster；不新增 A800，不读 confirmation。两类结果都必须由独立 replayer 重算。

**CONFIDENCE** 0.78（作为最小可证伪工程候选；不等于论文创新成立）。

### 2.2 输入

每个 decision tick `t` 的合法输入：

1. 当前分子和端点 `x_t,e_t`；
2. 当前菜单 `A_t` 及 menu hash；
3. fit-only 训练得到的每 provider 冻结校准概率 `q_a^0(x_t)∈[0,1]`；
4. 已释放历史 `H_t` 中的 selected terminal/reward 和 release tick；
5. 可序列化状态快照 `S_t`。

`q_a^0` 的校准目标先固定为 valid `system_success`。provider failure、unknown、未释放事件不能隐式转换为 chemical error；chemical correctness 另表报告。provider 返回的 numeric confidence、当前题真值和未选 provider output 不属于 selector 输入。

### 2.3 状态

每个 provider `a` 维护：

\[
S_t(a)=(N_a,F_a,blocked_a,blocked\_until_a,last\_release_a),
\]

其中 `N_a` 是已释放且 valid 的 selected observations，`F_a` 是 valid failure 数；`blocked_a` 是是否暂时禁用；`blocked_until_a` 是预先固定的 batch 结束点或 version reset 点。pending queue 只保存 `(event_id, a, terminal_status, release_tick)`，不能保存未选 provider 标签。

对 valid `system_success` 使用一个只用于阻断的 Beta 风险摘要：

\[
\theta_a\mid H_t\sim\mathrm{Beta}(1+F_a,1+N_a-F_a),
\qquad L_a=Q_{\delta}(\theta_a),
\]

这里 `theta_a` 是 provider 在当前被选样本上的 error rate；不宣称它是未选 provider 的全局 causal error rate。推荐首版固定 `δ=0.05`、`n_min=20`、`τ_fail=0.20`，均在读取 evaluation 前冻结。参数只用于一个安全 veto，不能调到 gate 必开。

### 2.4 决策

先定义显式 provider runtime failure 的即时规则：若 terminal status 是已审计的 `provider_failure`，则该 provider 在下一个 batch 暂时 blocked；若只是 valid chemical/system correctness `r=0`，只更新 Beta 摘要，不因单条标签即时封禁。

对非 runtime failure 的普通决策：

\[
a_t=\arg\max_{a\in A_t:\ blocked_a=0} q_a^0(x_t).
\]

若所有 provider 都被阻断，回退到 fit-only `argmax_{a\in A_t}q_a^0(x_t)` 并记录 `all_blocked_fallback=1`，不能静默跳过动作。每个 batch 内 `blocked_a` 冻结；release/update 只影响下一个 batch。这样延迟只改变阻断生效时间，不改变当前 action 的因果顺序。

### 2.5 更新

在 release tick 对每个 event exactly-once 执行：

1. 若 `terminal_status=unknown` 或真值未释放：不更新 `N_a,F_a`，单独计数；
2. 若 `terminal_status=provider_failure`：写入 runtime failure counter，并在下一 batch 设置 `blocked_a=1`；
3. 若 `reward_valid=1`：`N_a←N_a+1`，若 `r=0` 则 `F_a←F_a+1`；
4. 当 `N_a≥n_min` 且 `L_a≥τ_fail` 时，在下一 batch 设置 `blocked_a=1`；否则保持；
5. 记录旧/新 state hash、event id、release tick、block reason。

不做：未选 provider residual transfer、post-call confidence 回灌、完整模型重训、利用分支标签的 inverse propensity 修正、按 evaluation 结果修改 `n_min/τ_fail`。若后续要研究恢复，另设预先固定的 `blocked_until`；首版不在固定池上自动 decay，以免把已知退化重新放回。

### 2.6 复杂度与边界

一次决策为 `O(|A_t|)`，一次 release 为 `O(1)`，状态为 `O(K)` 加 pending queue。它不需要探索 coin，也不声称识别未选 provider；因此没有 AG-JEV 的 `K×C` 支持爆炸。代价是：如果退化 provider 从未被选中，MIRA-SafeSwitch 无法发现；如果 global selected risk 只是分布偏移而非 provider event，可能错误阻断。这两个限制必须成为报告的一部分。

## 3. 可证伪预测

**POSITION** 下列预测是机制的硬测试，不是预期结果。

| 编号 | 预测 | 证伪方式 |
|---|---|---|
| P1 | 无事件固定池上，MIRA-SafeSwitch 与 Frozen 的 pooled accuracy 差值落在预登记 ROPE 内；block rate 很低。 | 任何两个 delay 超出 ROPE，或收益只来自调用数/菜单改变，停止。 |
| P2 | 受控 provider degradation 后，收到目标 provider 的首个合法 release failure 后，目标选择率在至多 `delay+1` 个 decision/batch 内明显下降。 | 目标 rate 不下降，或只在读取 future/full-panel 标签时下降，停止。 |
| P3 | 事件后 suffix accuracy 在 delay `{0,1,4}` 中至少两个 delay 高于 Frozen；recovery latency 随 delay 单调增加但不完全崩溃。 | 少于两个 delay 改善，或 recovery 与 delay 无关且 replayer 不一致，停止。 |
| P4 | 去掉 block state（等价 Frozen）会丢失 P2/P3；仅改变 Beta 先验而不改变 veto 不应创造收益。 | 若无 block 的对照同样收益，机制归因不成立。 |
| P5 | 在任何 decision 前注入未选 provider 标签或 post-call confidence 不会改变 MIRA 动作；改变合法 release 顺序只从相应 tick 后影响动作。 | 任意越界或提前动作改变，实验作废。 |
| P6 | 若没有真实/注入 event，MIRA 不应稳定超过 fit-only Best Fixed；若显著超过，优先查静态 `q^0`、menu hash 或标签泄漏。 | 在无事件上取得大幅优势而审计通过前，不能写成方法收益。 |

**TARGET**  P1/P5 是实现与边界测试；P2/P3 是动态机制测试；P4 是必要消融；P6 防止把 event-specific control 过写成自然动态结论。

**CONFIDENCE** P1/P5：0.9；P2/P3：0.7（基于已有 controlled event）；P6：0.85。

## 4. 最小 CPU 实验

**POSITION** 只做一个两臂 replay：同一 authoritative BBB manifest、同一 `q_a^0`、同一菜单、同一 delay 和同一 event roster，比较 Frozen 与 MIRA-SafeSwitch；保留已有 Matched Online-SGD 作为强 matched reference，不重新调参。

**TARGET** 证明上述机制是否有独立的 event recovery，而不是再排名一个普通 online optimizer。

**EVIDENCE** 现成资源：722 BBB development molecules、5 scaffold-disjoint folds、2 deterministic protocol slots、delay `{0,1,4}`、`controlled_version_event_v1` 的 50/90 条注入 event streams、fit-only Best Fixed target roster，以及已有 raw event/verification 文件。confirmation 关闭。

**REASONING** 采用现成 event roster 可复用已审计的 target-provider 选择与事件时间；无事件 stream 提供负控制。固定 `δ=0.05,n_min=20,τ_fail=0.20,batch=32`，不在 evaluation 上搜索。每条 stream 同时保存生产日志和独立重放日志，避免把候选实现当作自己的 verifier。

**DECISION TEST**  实验矩阵：

1. **No-event control**：Frozen、MIRA-SafeSwitch、Matched Online-SGD；5 folds × 2 slots × 3 delays；目标是不劣与边界审计。
2. **Controlled-event**：同一三种方法，目标 provider 在 midpoint 后按既有协议退化；事件前/后分段统计。
3. **Ablations**：MIRA 去掉 block（Frozen 等价）、只用 runtime failure、只用 Beta risk block；每个 ablation 只改变一个机制。
4. **独立 replayer**：从 raw terminal/release 和冻结 `q_a^0` 重算 action、block reason、state hash；与生产日志逐事件相等。

逐条记录 JSONL：配置（源码 commit、输入 hash、fold/slot、delay、阈值、seed slot、硬件/软件）、decision、terminal、release、update、`N_a/F_a/L_a/blocked_a`、runtime p50/p95、错误与 unknown。聚合报告：总体和 event suffix accuracy、目标 provider rate（事件前/后/首个 failure 后）、首次 block latency、switch/block rate、all-blocked fallback、调用次数、invalid/unknown 数量、scaffold-cluster paired bootstrap CI、每 fold/slot 明细和 provider-family macro。

**通过条件**（必须在读取 evaluation 前写入 config）：

- 独立 replayer 逐事件 action/state 一致，且没有任何决策前的未选标签、当前真值或 confidence；
- no-event：MIRA 在三个 delay 的 pooled accuracy 均不低于 Frozen 的 ROPE 下界，且平均 block/fallback 成本低于预登记上限；
- controlled-event：至少两个 delay 的 event-suffix accuracy 高于 Frozen，目标 provider rate 在首个合法 failure release 后下降；paired scaffold CI 与 fold-level 方向至少不冲突；
- MIRA 不得只依靠增加调用次数、读取完整面板或改变菜单；更新 p50/p95 与内存符合 MIRA 计算约束。

**停止条件**：任一信息泄漏或 release 顺序错误；no-event 两个 delay 低于 Frozen；event 两个 delay 不恢复；收益只出现在一个 deterministic slot/family；所有 block 都由 unknown/invalid 标签触发；replayer 不能逐事件重算；或必须通过调小 `n_min`、调大 `τ_fail`、改变 event roster 才能过门。

**CONFIDENCE** 0.84（实验可在 CPU 上完成并清楚证伪）；0.58（在自然、无注入漂移上支撑更强 dynamic claim）。

## 5. 结论与贡献边界

**POSITION** MIRA-SafeSwitch 是下一轮最小实现卡，不是已验证的 AAMAS 主方法。它的可检验机制是“selected-only 延迟反馈下，对已观测 incumbent 退化执行安全 veto”，而不是 provider-wide adaptation、未选质量估计或全新后验。

**TARGET**  若该卡通过，只能支持：在明确 provider/version event 的受控条件下，信息边界自洽的安全切换有用；固定池无 event 下只能报告 non-inferiority/负结果。若失败，direct pre-call MIRA 应停止继续堆在线公式，转向已通过的静态 confidence verifier/escalation 和 benchmark/负结果贡献。

**EVIDENCE**  当前 development master 与 AG-JEV、dynamic verifier、controlled event 结果共同指向：强 incumbent、短流和 selected-only 使“估计所有候选的在线质量”不现实；但已有正控提供了一个可重放、可证伪的学习对象。

**REASONING**  该收窄保持信息合同诚实，避免把标准 Beta/Kalman/blacklist 重新命名成创新，同时为 MIRA 保留一次最小、可复现、可停止的机制实验。任何扩展到 provider arrival/retire、自然 version drift 或 chemical correctness transfer，都应另写 method card 和新的可观测性论证。

**DECISION TEST**  先完成本卡的 CPU 两臂 replay 与独立恢复，再决定是否进入 confirmation；不因单次 pooled accuracy 正点差放宽 Gate A/B/C 或申请 A800。

**CONFIDENCE** 0.9（关于贡献边界）；0.55（关于论文新颖性，需外部文献审查与实证后再判断）。
