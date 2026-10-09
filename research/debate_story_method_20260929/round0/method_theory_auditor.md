# Round 0：方法与理论审查（独立）

- 审查角色：method / theory auditor
- 日期：2026-09-29
- 审查对象：`docs/研究主线/生效/01-故事线与创新.md`、`02-方法论.md`、`03-Benchmark与Baseline.md`、`task/task49-pharma-benchmark-baseline-freeze/current_acceptance_audit_v2/report.md`、`task/task49-pharma-benchmark-baseline-freeze/development_master_table_v1/report.md`
- 结论级别：Round 0 独立风险清单；不改动生效主线，不把预测写成结果

## 总裁定

POSITION: challenge

TARGET: “动态 JEV 工具适应流程”是否已经是一个可识别、可实现、相对 baseline 可归因的算法主张。

EVIDENCE: 生效故事线把候选写成 selector → provider → verifier → delayed updater 的流程（`01-故事线与创新.md:34-41`）；方法论只给出 `a_t=π_sel(x_t,e_t,A_t,H_t)`、`v_t=π_ver(o_t,c_t,s_t)` 和 `S_{t+1}=U(S_t,x_t,e_t,a_t,r_t,τ_t)`（`02-方法论.md:16-44`）。当前 development 表中 Best Fixed=`0.9155`，Matched Online-SGD/Replay、PILOT、BayesianRouter、Greedy 均未超过强固定策略（`development_master_table_v1/report.md:18-69`）；dynamic verifier 的 pooled accuracy=`0.900277`、平均调用=`1.490305`，低于静态 confidence escalation 的 `0.923823`、`1.206` 调用（`dynamic_verifier_v1/report.md`）。

REASONING: 目前能审计的是一份信息与事件协议，以及一个尚未冻结状态参数、更新规则、探索规则和最终 backbone 的候选流程；不能从流程名称推出算法性增益。固定池、selected-only、延迟反馈下，selector 的未选 provider 反事实不可由当前历史直接识别；post-call confidence 又只在选择之后可见。现有正信号属于 post-call verifier，现有 direct selector 负结果不能被流程集成重新命名为正向 JEV。因而当前最安全的判定是：**问题有意义，协议大体可运行，pre-call 动态选择的因果识别与机制归因尚未成立；主方法应 HOLD。** 评价条款/分数影响：方法论 H2、H4、H9、H10；M5、M10、M11、M15 目前不能达到 3 分，故事线 H4/H5/S4/S6 也只能按未完成处理。

DECISION TEST: 在同一 5-fold、33-provider、selected-only 合同下，先冻结一个含明确状态和探索的候选，要求：(i) 独立恢复通过；(ii) 至少两个 delay 达到或超过 Frozen 且 pooled accuracy 不低于 fit-only Best Fixed；(iii) 相对同成本 `Audit-Frozen`、Matched Online-SGD 和最强现代近邻的 paired molecule/scaffold 差值在预登记 ROPE 外；(iv) 若声称环境动态，必须另有预登记 version/arrival/retire 事件与 Frozen-pool 对照。任一项失败，直接把主张收窄为 fixed-pool feedback-adaptive 或 protocol/benchmark 结果。

CONFIDENCE: 0.94；原因是结论由形式上的反事实不可识别性、已完成 development 主表和已报告动态 verifier 负结果共同支持，不依赖尚未运行的实验。

## P0：识别性与信息边界

### P0.1 selected-only 历史不足以识别未选 provider 的价值

POSITION: challenge

TARGET: 只用 `H_t` 更新 selector 就能普遍改善后续 provider 选择。

EVIDENCE: 任务合同只释放已选 provider 的 correctness；未选输出、当前真值和未来反馈在选择前不可见（`02-方法论.md:16-24`，`03-Benchmark与Baseline.md:35-39`）。当前普通在线更新都低于 Best Fixed（`development_master_table_v1/report.md:18-39`）。

REASONING: 令每个题有潜在结果 $Y_t(a)\in\{0,1\}$，策略只能观察 $R_t=Y_t(a_t)$。构造两个环境 $P,Q$：在两者中，若选 $A$，$Y_t(A)\sim\mathrm{Bernoulli}(0.9)$；在 $P$ 中 $Y_t(B)\sim\mathrm{Bernoulli}(0.95)$，在 $Q$ 中 $Y_t(B)\sim\mathrm{Bernoulli}(0.05)$。若策略初始和后续都选 $A$，所有可见 $(x_t,e_t,A_t,a_t,R_t)$ 历史分布相同，所以任何 $U(H_t)$ 产生相同状态和动作；但最优动作在 $P,Q$ 相反。没有探索重叠、provider 结构共享、可重复局部上下文或已登记漂移假设，不能从 selected-only 数据辨认未选 arm 的质量。`Best Fixed` 的 fit full-panel 只给训练区固定参照，不能为在线更新提供当前未选 arm 标签。评价条款/分数影响：方法论 H2、H3、H9；M3、M6、M10、M11、M15 至少一项低于 3 即 HOLD，故事线 H5/S6 不能通过。

DECISION TEST: 预登记固定探索率 $\epsilon>0$ 或其它可审计 propensity，并保存每条决策的菜单与 propensity；在 CPU 上加入两个上述不可区分环境的合成反例，检查恢复器是否给出同一轨迹并正确标出“不可识别”。在真实 BBB 流上再做 `Audit-Frozen`（同 $\epsilon$、永不更新）和 Ours 的 paired suffix accuracy。若无 overlap 或 Ours 与 Audit-Frozen 无可重复差异，停止“学习未选 provider 质量”的强主张；只能报告在给定探索/共享结构假设下的 policy value。

CONFIDENCE: 0.98；这是 selected-only bandit 的构造性反例，当前文档没有声明能排除它。

### P0.2 selector、verifier、updater 的 estimand 混在一起

POSITION: challenge

TARGET: 用一次流程级 pooled accuracy 支持“动态 JEV 改善工具选择和结果可靠性”。

EVIDENCE: 故事线同时把 pre-call 选择、post-call confidence/第二工具、延迟更新列为同一创新（`01-故事线与创新.md:34-41`）；方法论把 selector 与 verifier 写成不同函数（`02-方法论.md:26-30`），但当前 development 的正结果只来自调用后 confidence escalation（`confidence_escalation_v1/report.md`），direct selector 全部未超过 Best Fixed（`development_master_table_v1/report.md:18-69`）。

REASONING: selector 的可识别量是 $V_{sel}(\pi)=\mathbb E[Y_t(a_t)]$；verifier 的量是给定已选输出后，含接受/升级/拒答动作的最终 utility $V_{ver}=\mathbb E[Y_t(\hat a_t)]-\lambda\,\mathrm{calls}-\rho\,\mathrm{latency}-\kappa\,\mathrm{refusal}$。第二工具调用改变了动作集合和成本，不能把它的最终 correctness 回填为第一次 pre-call 选择准确率。若只报最终 accuracy，always-two-call、confidence gate、selector 改善三种原因不可区分。评价条款/分数影响：方法论 H4、H6、H10；M5、M8、M12、M15 最高只能给 2 分，故事线 S2/S3/S4/S6 不满足独立 claim–evidence 映射。

DECISION TEST: 固定同一个 pre-call selector，分别运行 `never-verifier`、fit-only static confidence gate、random same-cost gate、dynamic verifier；报告 first-call accuracy、final accepted accuracy、coverage/refusal、calls、latency 和 utility 五项。再固定 verifier 比较 Frozen/Matched Online/Ours selector。若 Ours 的所谓收益只在 verifier 打开时出现，或收益由额外调用成本抵消，删掉 pre-call dynamic claim。

CONFIDENCE: 0.97；现有文档已经明确两个模块时点不同，但结果与主张仍跨模块复用。

### P0.3 动态范围未识别：fixed-pool feedback-adaptive 与 environment-nonstationary 被混写

POSITION: revise

TARGET: “动态 JEV”可支撑 provider/version 漂移、arrival、retire 或一般环境非平稳的主张。

EVIDENCE: 当前主流回放是固定 33-provider pool，动态事件和 natural drift 仍是 Gate C 缺口（`current_acceptance_audit_v2/report.md:17-18,50-51`）。round2 差异没有版本/部署凭据，更多像 PaDEL 重复运行抖动；报告明确不能叫后来工具版本（`round_snapshot_audit_v1/report.md`）。已有 controlled version event 是把 Best Fixed provider 的终止改为 failure 的正控，且被标注为 development positive control、不能进入主 benchmark（`controlled_version_event_v1/report.md`）。

REASONING: feedback-adaptive 只要求 selected-only delayed release 改变后续状态；environment-nonstationary 还要求 provider/version/menu 或数据分布有时间事件。固定池回放可以检验前者，不能识别后者。正控中 policy 未见 version flag，因而能证明“有真实退化事件时该规则可能恢复”，但不能证明自然部署中有同样事件率、方向和可预测性。若论文保留“provider/version event”语言，读者会把 controlled intervention 当自然适应证据。评价条款/分数影响：方法论 H9、M10、M15；dynamic_scope 未收窄前 M10/M15 最高 2 分，故事线 H8/S11 触发范围校准缺口。

DECISION TEST: 在主结果前声明 `dynamic_scope=feedback-adaptive`。若要升级到 `environment-nonstationary`，预登记至少一种 arrival/retire/version-shift，冻结事件时间和方法盲化的 event log，加入 Frozen-pool、Reset-on-event、Full-panel event oracle，并报告 event 后 recovery steps、选择率、成本和 paired CI。事件前后若 Ours 不能超过 Frozen-pool 与 Matched Online，或事件仅由离线注入 failure 产生，删去自然动态主张。

CONFIDENCE: 0.96；scope 结论已在验收审计中写明，正控只能支持更窄的机制 sanity check。

## P1：数学定义、模块归因与统计

### P1.1 延迟队列公式不是可执行的多事件状态递推

POSITION: challenge

TARGET: `S_{t+1}=U(S_t,x_t,e_t,a_t,r_t,τ_t)` 已足以定义 delayed updater。

EVIDENCE: 方法论只把一个事件传入 $U$，未定义同一 tick 多个 release、乱序完成、retry、duplicate、crash/restart 的 tie-break 和 exactly-once 语义（`02-方法论.md:32-38`）。合同要求事件日志含 `released_feedback_ids`、状态 hash 与更新时间，但这是日志字段，不是状态方程（`baseline_contract_v1.json`）。

REASONING: 延迟更新应写成事件流上的递推，例如 $E_k=\mathrm{sort}\{e_j:\mathrm{release\_tick}_j=k\}$，$S_{k+1}=U(S_k,E_k)$；每个 $e_j$ 至少带 `event_id, decision_tick, terminal_tick, release_tick, x_j,e_j,a_j,r_j,status`。若同 tick 的更新顺序改变动作，策略价值是队列实现细节的函数；若 release tick 与 $\tau_t$ 混用，可能把决策时钟当释放时钟，导致提前或重复更新。现有 Greedy verifier 通过说明某个 runner 可恢复，不能证明未冻结 JEV 候选也满足同一不变量。评价条款/分数影响：方法论 H1、H2、H3、H7；M2、M6、M7、M14 最高 2 分，任一 H1/H2 失败应先冻结效果结论。

DECISION TEST: 先冻结上述事件 schema 和排序键，在相同输入上对 release 事件做任意合法乱序、同 tick permutation、crash/restart、duplicate injection；要求恢复器逐事件 `state_hash_after`、动作和指标字节一致，或显式报告 order-sensitive estimand。任一合法重放改变结果而没有预登记的排序规则，方法实现 FAIL，停止效果比较。

CONFIDENCE: 0.92；现有 baseline 合同有足够日志方向，但 JEV 本身尚无确定的队列数学。

### P1.2 reward 标签语义混淆 execution success 与 chemical correctness

POSITION: challenge

TARGET: 公式中的 $r_t\in\{0,1\}$ 可同时代表 provider 正确性、系统成功和失败惩罚。

EVIDENCE: 方法论将 $r_t$ 定义为与冻结真值一致的 correctness（`02-方法论.md:16-18`）；在线合同与日志同时区分 `system_success`、`chemical_correct`、unknown 和 provider failure，failure 的 system reward 记 0 而 chemical correctness 可为 null（`online_adapter_v1/contract.md`、`online_adapter_v1/report.md`）。动态 verifier 报告把第二工具 failure 记为最终错误，但没有展示化学正确性与服务可用性的二维分母（`dynamic_verifier_v1/report.md`）。

REASONING: 若 failure 直接变成 $r=0$，更新器学习的是“不要选会失败的服务”与“不要选预测错误的服务”的混合目标；若 unknown 被当成 0，则缺失真值被误当成错误。两者会改变 Best Fixed、selector 和 verifier 的 estimand。需区分 $R^{chem}=\mathbf1[\hat y=y]$、$R^{sys}=\mathbf1[\text{terminated successfully}]$，以及 failure cost；否则一个方法通过降低失败率即可被误报为化学 correctness 提升。评价条款/分数影响：方法论 H1、H4；M1、M4、M6、M15 最高 2 分；benchmark H2/H9 不能按通过处理。

DECISION TEST: 构建 2×2 分层结果（system success × chemical correctness），对每种方法分别报告 chemical accuracy、failure rate、unknown rate、最终 utility；更新器只消费预登记目标（例如 selected `system_success` 或有真值时的 chemical correctness）。在 synthetic label-swap 检查中交换 failure cost 与 chemical label，算法不得静默改变标签解释。若主结论依赖把 failure 当 prediction error，必须明确改写为 service-reliability claim。

CONFIDENCE: 0.95；冲突直接出现在生效方法定义与 baseline/日志合同之间。

### P1.3 机制归因未隔离：各方法 Frozen 表示不同

POSITION: challenge

TARGET: development master table 可直接证明“延迟更新/动态 JEV”优于静态或现代近邻。

EVIDENCE: master table 明确每个 online 方法的 Frozen 是各自初始化和表示的控制，不能互换（`development_master_table_v1/report.md:7-16,18-39`）。Greedy、PILOT、BayesianRouter 使用的表示和先验不同，Laya/GraphRouter 还未完成 fidelity（`current_acceptance_audit_v2/report.md:17`）。

REASONING: 观测到 $A(\text{Online})-A(\text{Frozen}_{\mathrm{same}})>0$ 只能说明该 online head 相对自身初始化有收益；跨方法与 Best Fixed 的差值同时含表示、离线先验、训练标签、更新规则和预算效应。若 proposed JEV 只换了 encoder 或 prior，不能把差值归因于 delayed state。必须定义 integration gain，例如
$$\Delta_{int}=U(\mathrm{full})-\max\{U(\mathrm{best\ single}),U(\mathrm{remove\ interface}),U(\mathrm{remove\ state})\}.$$
当前没有 full / selector-only / verifier-only / updater-only 的共同 backbone 消融。
评价条款/分数影响：方法论 H4、H6；M5、M8、M12、M13 至少有一项低于 3 即 HOLD，故事线 H4/S4 不能宣称整体组合必要性。

DECISION TEST: 固定同一 fit-only backbone、provider head、初始化、菜单、delay、预算与 feature hash，运行 `Frozen`、`selector-only`、`updater-only`、`verifier-only` 和 `full`；另加与 Ours 同探索率的 `Audit-Frozen`。按 molecule/scaffold 配对报告 Δ_int 与 95% cluster bootstrap CI。若 full 不超过最佳单组件或超过主要收益来自额外特征/标签/调用，不能写组合创新。

CONFIDENCE: 0.94；现有汇总已主动承认 Frozen 不可互换，这正说明当前尚未完成机制归因。

### P1.4 confidence gate 的跨 provider 可比性与第二调用收益未充分识别

POSITION: revise

TARGET: provider 返回的 numeric confidence 能作为通用风险状态，并且 confidence escalation 的收益属于 JEV 机制。

EVIDENCE: confidence escalation 在开发集上 pooled accuracy=`0.923823`、平均调用=`1.206`，相同 fallback 数量的随机追加=`0.904432`，这是 post-call static verifier 结果（`confidence_escalation_v1/report.md`）。dynamic verifier 更新 Beta 风险状态后 accuracy=`0.900277`、平均调用=`1.490305`，所有 stream 回退到最大阈值（`dynamic_verifier_v1/report.md`）。

REASONING: `c_t=2|p_t-0.5|` 是 provider 内部概率的变换，不保证跨 provider、端点或版本校准；同一 `c=0.8` 可对应不同错误率。selected-only 延迟反馈只估计实际调用分布下的条件风险，不能外推到未调用 provider。第二调用的收益还可能来自更强的第二 provider 或两次机会本身，而非 gate 学到的风险。动态 verifier 已显示风险门的置信上界在短流内无法通过，导致过度 escalation；这说明“可审计状态”不等于可用风险控制。评价条款/分数影响：方法论 H4、H10；M5、M6、M11、M13、M15 最高 2 分，benchmark B12/E5 需把 calls、coverage、refusal 纳入主指标。

DECISION TEST: 每个 provider/fold 先在 fit 区做 calibration curve、ECE、AUC 和 coverage/error 曲线；阈值按 provider-specific fit 规则固定。用相同 first selector 比较 static threshold、random same-cost、always-second、dynamic state；utility 必须含最终 correctness、calls、latency、refusal。若 calibration 跨 fold 不稳定，或 dynamic 在 ≥2/3 delay 低于 static、平均 calls>1.25，则停止动态 verifier 主张（当前报告已经满足负向停止条件）。

CONFIDENCE: 0.93；静态正结果与动态负结果都已观测，剩余是跨 provider 外推是否成立。

### P1.5 统计单位和不确定性不能把 deterministic slots 当独立重复

POSITION: challenge

TARGET: 60 streams、2 seed slots、5 folds 足以给 pooled accuracy 和“稳定收益”做常规显著性结论。

EVIDENCE: master table 明确 `seed=0/1` 是 deterministic protocol slot，不是独立随机重复，表中不生成 seed 置信区间（`development_master_table_v1/report.md:11-16`）。大多数结果只有 5 outer folds × 2 slots × 3 delays；33 个 provider 还有 family correlation（`03-Benchmark与Baseline.md:29-33`）。

REASONING: 把 stream 或 provider 事件当 iid 会低估方差。pooled accuracy 还可能被分子重复、provider family 变体和 fold 大小加权。delay 0/1/4 是同一流的配对条件，不是三个独立样本。应以 molecule 或 scaffold 为基本重采样单位，以 fold 作为高层 cluster；family macro 和 provider exposure 需要单独报告。没有 CI、paired permutation 或预登记 ROPE，0.5–1 pp 的差异不能支持机制性结论。评价条款/分数影响：方法论 H10；M13、M14、M15 最高 2 分；benchmark B9/B12/E5 不能以 pooled accuracy 单独判定。

DECISION TEST: 预登记 paired molecule/scaffold bootstrap、5-fold cluster permutation 和 family-balanced macro accuracy；主 claim 只在差值下界超过 ROPE（建议先沿用任务现有 `+0.005` 量级，或在实验卡冻结其它阈值）且至少 4/5 folds 同向时成立。若 pooled 提升但 cluster CI 跨零，报告为不确定；若 family-balanced 方向反转，停止“跨 provider”主张。

CONFIDENCE: 0.91；当前结果已明确不把 slots 当独立 seed，但尚未提供正式配对不确定性。

### P1.6 多候选筛选使“当前最佳方向”容易后验选择

POSITION: challenge

TARGET: 在已看过大量 negative/positive development 结果后，再选择一个 JEV 候选进行 confirmation，仍可视为单一预注册方法检验。

EVIDENCE: 当前已有 Online/Replay SGD、Greedy、PILOT、BayesianRouter、provider transfer、context memory、risk gate、dynamic verifier、controlled event 等多条实验线；主线明确 confirmation 仍关闭、方法状态未冻结（`current_acceptance_audit_v2/report.md:5-18`）。

REASONING: 方向、表示、学习率、先验、阈值和 scope 若在 development 结果后反复挑选，confirmation 的名义错误率不再对应一个预登记假设。把 negative 结果保留并不能自动消除“只确认看起来最好的一条”选择偏差。必须把 exploratory candidate 与 confirmatory method 分开，冻结源码、配置、菜单、阈值和 baseline。评价条款/分数影响：方法论 H8、H10；M13、M14、M15 最高 2 分；故事线 H6/H8 触发事后改叙事风险。

DECISION TEST: 生成 candidate ledger：每个候选的首次登记时间、最近邻、预算、唯一主指标和 kill criterion；从 ledger 中选一条后锁定 commit/hash，再开一次 confirmation。确认前不得改变 provider/menu/epoch/threshold；若任何 confirmation 标签回流，整轮只能标 exploratory。若候选无法在不看 confirmation 的情况下冻结，停止方法 claim，保留 benchmark/negative evidence。

CONFIDENCE: 0.90；项目已有冻结规则，但候选数量和迭代历史较多，审稿风险真实存在。

## P2：可行性、数据边界与替代解释

### P2.1 数据与 provider lineage 不足以承载广泛“科学工具”或 model-fresh 泛化

POSITION: revise

TARGET: 722 条 BBB development、33 个 provider 足以支持跨端点、跨模型版本和普遍工具适应。

EVIDENCE: Gate A 仍缺 hERG/AMES endpoint equivalence、provider training membership、model-fresh lineage 和旧 panel/confirmation overlap 收口（`current_acceptance_audit_v2/report.md:14-18`）。33 个 provider 中 24 个 B3clf 变体不是 24 个独立模型家族，lineage 等级为 G1–G3（`03-Benchmark与Baseline.md:29-33`）。旧 development panel 与官方 BBB test 有 147 条 canonical、235 条 scaffold overlap，不能直接当 untouched confirmation（`03-Benchmark与Baseline.md:23-27`）。

REASONING: 当前数据可支持“固定已部署 BBB 菜单上的反馈自适应回放”，不能支持“所有科学工具”“model-fresh”或自然跨端点泛化。provider family 相关会放大可救错误和有效样本数；训练成员重叠还可能让 Best Fixed 看起来异常强。A800 只能增加运行量，不能修复 endpoint 语义、lineage 或 overlap。评价条款/分数影响：方法论 H8、H10、M15；benchmark H1–H4、H8、B1–B4 若未补齐只能保持 Gate A HOLD，故事线 H8/S11 不得给 3 分以上。

DECISION TEST: 在 Gate A 通过前，把 claim scope 固定为 BBB、固定 deployed menu、development/stress；若要写 model-fresh，重新在公开 train split 拟合并绑定 checkpoint hash。对 confirmation 只使用 molecule-disjoint，若声称结构独立则使用 scaffold-disjoint 且重新生成 provider outputs。hERG/AMES 任一资格失败时，不把其结果并入主结论。

CONFIDENCE: 0.97；这些是验收审计中已列明的硬缺口。

### P2.2 计算可行性与方法可得性不足以支撑 Laya/GraphRouter 等候选

POSITION: challenge

TARGET: 可以用现有 compute/代码把动态 JEV 与 Laya/JEV、GraphRouter 一起做成公平强 baseline。

EVIDENCE: Gate B 仍缺 GraphRouter fidelity 和 Laya selected-only online 适配（`current_acceptance_audit_v2/report.md:17`）。GraphRouter 依赖的 provider description/graph edge 语义在 BBB 尚未冻结；Laya runtime 没有 optimizer、loss 或在线 checkpoint 写回，且本地缺 `torch_geometric`（对应 `graphrouter_static_adapter_v1/blocking_report.md`、`laya_online_adapter_v1/blocking_report.md`）。online adapter 的本地 CPU 只验证递推和日志，不代表 backbone/API/GPU 端到端实时性（`online_adapter_v1/report.md`）。

REASONING: 低维 head 的 $O(Kd^2)$ 更新可以在 CPU 完成，但这只说明接口可行；它不能替代 Laya encoder、GraphRouter 图语义和动态 provider 事件的实际预算。若为了赶进度重写上游方法，适配版会变成不同模型；若只做静态 forward，又不能称 online JEV。提交更多 A800 不能解决输入合同和 fidelity 缺口。评价条款/分数影响：方法论 H1、H3、H5、H7；M9、M12、M14 最高 2 分；benchmark H6/H7/B6/B10 仍不能按通过处理。

DECISION TEST: 先在 CPU 闭合 common feature、初始化、日志、恢复器和 event stress；每个上游 baseline 要么完成 commit/依赖/输入语义一致的 fidelity，要么正式降为“论文行为审计/静态负诊断”。只有 CPU 方法闸门通过且主 claim 已冻结，才开一条 A800 smoke；A800 smoke 若只改善吞吐，不能升格为科学效果。

CONFIDENCE: 0.94；可执行性缺口已有独立 blocking reports。

### P2.3 现有正/负结果的替代解释需要显式反事实

POSITION: challenge

TARGET: development 结果中的差异可以归因于“延迟在线学习”“confidence gate”或“动态 JEV”本身。

EVIDENCE: Matched Online-SGD 相对自身 Frozen 约 `+0.78 pp`，仍低于 Best Fixed；PILOT、BayesianRouter、Greedy 多数 delay 不如自身 Frozen；confidence escalation 只有在调用后第二工具控制中为正；controlled version event 的正结果来自人为注入 Best Fixed provider failure（`development_master_table_v1/report.md:18-69`、`confidence_escalation_v1/report.md`、`controlled_version_event_v1/report.md`）。

REASONING: 至少有五个可替代原因：(a) Online-SGD 的小增益来自较弱 Frozen 初始化，不是 delayed feedback；(b) confidence gate 的增益来自额外第二 provider 或第二 provider 本身，而不是 confidence 的因果作用；(c) controlled event 的恢复来自已知“目标 provider 会退化”的构造和 failure 信号，不代表自然 drift；(d) round2 差异是执行抖动，不是版本更新；(e) provider family 相关、训练成员重叠和静态 Best Fixed 的强先验改变了 headroom。若不做反事实，整体流程只能说明“若给它额外信息/额外调用，回放分数可能变”。评价条款/分数影响：方法论 H4、H10；M5、M8、M13、M15 最高 2 分，故事线 H4/H5/S6 的独立反事实要求未满足。

DECISION TEST: 对每个机制预登记唯一 intervention：延迟只改变 release tick；verifier 只改变 gate，保持 first selector、second provider 和调用预算；event 只改变 provider failure/version，加入 no-event 与 random-event 对照；family-balanced menu 与 provider-shuffled null 检查相关性。删掉状态、接口或额外调用后，若主指标仍不变或收益保留在单个控制中，停止该机制 claim。

CONFIDENCE: 0.92；替代解释与已观察的负结果相符，但需要配对消融才能最终排除。

### P2.4 “可审计流程”本身不是算法性收益

POSITION: revise

TARGET: 把信息边界、日志、selector/verifier/updater 三模块连在一起即可构成创新。

EVIDENCE: 故事线把创新表述为“把信息边界、实时更新和调用后可信度连成一个可审计的流程”（`01-故事线与创新.md:41`）；方法论同时声明标准 LinUCB、SGD、Bayesian posterior、PILOT、Laya/JEV 组件本身不算创新（`02-方法论.md:64-68`）。当前尚未冻结能改变计算对象的统一状态、不变量或正向 integration gain。

REASONING: 日志、状态 hash、事件合同主要贡献可复现性和测量有效性；它们可以是 protocol/interface contribution，但不会自动提高 policy value。若完整流程与最佳单组件使用相同动作和状态，$\Delta_{int}$ 为 0，则“组合”没有可检验机制收益。论文可以选择把贡献定位为 benchmark/protocol/negative evidence，但不能同时暗示一个已证明的 JEV 算法。评价条款/分数影响：方法论 H4；M5、M8、M15 最高 2 分，故事线 S4/S6 只能支持 protocol/infrastructure 贡献，不能支持 algorithmic novelty。

DECISION TEST: 在相同动作策略上做“去掉日志/事件状态但保留计算”和“保留日志但去掉状态/接口”的反事实；分别报告 accuracy、recoverability、latency、memory。若唯一可重复收益是 recoverability/auditability，贡献类型写成 interface/protocol；若要保留 algorithmic claim，必须满足 matched integration ablation 和预登记 utility 改善。

CONFIDENCE: 0.95；这是从当前创新表述与实验证据边界直接推出的贡献类型约束。

## 必须补的最小实验包

POSITION: recommend

TARGET: 在不继续堆普通 selector/head 的前提下，决定动态 JEV 方向是否值得进入 confirmation。

EVIDENCE: Gate C 当前缺 E1–E5 全矩阵、动态事件、paired uncertainty 和 confirmation（`current_acceptance_audit_v2/report.md:17-18,50-53`）；baseline 合同已经定义 C0–C8、事件对照和指标角色，但主表仍缺 Random 收据、GraphRouter fidelity、Laya online 或正式降级（`baseline_contract_v1.md:2-8`）。

REASONING: 最小闭环应把识别性、模块归因、统计与可行性一次性隔离，实验规模应留在 CPU 可审计范围；A800 只在这些条件通过后用于最终确认。评价条款/分数影响：方法论 H1–H10、M1–M15 和 benchmark E1–E6；在缺任一反事实、成本、恢复或 scope 证据时，方法最高只能算 development，不得进入 confirmation。

DECISION TEST: 按以下顺序执行并保存 raw JSONL、配置、hash、独立恢复和失败现场：

1. **反例与信息注入测试**：两个不可区分环境 $P,Q$、未选 output/full-panel/future reward 注入、非法事件顺序和 duplicate；任何泄漏或恢复不一致即停止。
2. **共同表示 matched matrix**：同一 fit-only head 运行 Frozen、Audit-Frozen、Matched Online-SGD/Replay、Ours selector-only、Ours verifier-only、full；预先固定 exploration、delay `{0,1,4}`、calls 与 label 语义。
3. **机制与事件测试**：无事件、预登记 version/retire/arrival、controlled failure positive control；Frozen-pool、Reset-on-event、Matched Online、Full-panel event oracle 同表报告前缀/后缀 accuracy、regret、recovery steps、switch latency。
4. **统计与成本测试**：molecule/scaffold paired bootstrap、fold-level permutation、family macro、ROPE、accepted coverage/error、calls、latency p50/p95、memory、failure/unknown/refusal；不得把 deterministic slots 当 iid seeds。
5. **冻结确认**：完成 Gate A provider lineage、端点资格、overlap 和 menu manifest 后，冻结唯一 candidate commit/config，只开一次 molecule/scaffold-disjoint confirmation；标签不回流。

CONFIDENCE: 0.93；这是满足当前项目评价标准且能最小化新增 compute 的必要包，不是对结果方向的预测。

## 备选方向的停止线

POSITION: recommend

TARGET: 把已观察信号转成明确的 go/no-go，而不是继续调参追分。

EVIDENCE: 现有 fresh method review 已给出 DG-SafeJEV 与 dynamic verifier 的 CPU 停止条件；dynamic verifier 已发生成本和准确率双失败（`dynamic_verifier_v1/report.md`）。

REASONING: 预先写死停止线能防止用新阈值、prior、batch 或事件位置解释掉负结果。下面的阈值分别对应 direct selector、post-call verifier 和 environment event 三种不同主张。评价条款/分数影响：方法论 H10、M13、M15；任何停止线触发都应把对应 claim 标为 FAIL/HOLD，而不是用其它模块分数抵消。

DECISION TEST: 

- **Direct pre-call JEV**：若任意 decision 前读取未选输出、truth 或未来 reward，整流作废；若至少两个 delay 低于 Audit-Frozen，或 pooled accuracy 仍低于 Best Fixed，停止 direct dynamic 主线；若 gate 全程不开放，报告“当前数据没有可识别 drift”，不把零切换写成成功。
- **Post-call dynamic verifier**：若 ≥2/3 delay 低于 fit-only static verifier，或平均调用超过 `1.25`，停止动态 verifier；当前 `0.900277` 与 `1.490305` 已触发两条负信号，除非信息合同或 estimand 改变，不应再调参追分。
- **Environment-nonstationary claim**：若没有可信 version/arrival/retire event receipt，或 event 后 recovery 不超过 Frozen-pool/Reset-on-event，主张收窄为 feedback-adaptive fixed pool。
- **Algorithmic novelty**：若 full 的 Δ_int 不超过最佳单组件、Matched Online/modern nearest neighbor，或只在额外调用/额外标签下改善，停止“动态 JEV 算法”用语，保留 benchmark/protocol 与负结果。

CONFIDENCE: 0.96；每条停止线都能在已有合同、报告或形式反例上独立复核。

## 最终建议

POSITION: revise

TARGET: 下一轮论文主线和实验资源分配。

EVIDENCE: 生效故事线已经承诺只有同一信息、预算、强对照和 confirmation 同时通过才主张方法有效（`01-故事线与创新.md:24-30,53-64`）；当前三门 Gate A/B/C 均 HOLD（`current_acceptance_audit_v2/report.md:10-18`）。

REASONING: 建议把论文工作标题和实验计划暂时拆成两条可证伪分支：

1. **主分支（可立即审计）**：fixed-menu, selected-only, delayed feedback 的工具选择 benchmark + matched baseline + negative/feasibility evidence，严格称 `feedback-adaptive`。
2. **条件分支（通过最小实验才开启）**：有明确新观测的 incumbent-safe selector 或 post-call verifier；每条只保留一个状态、一个 utility、一个最近邻和一个 kill criterion。controlled version event 可以作为机制正控，但不能代替自然动态证据。

在 Gate A/B/C 未收口、标签语义未分离、common matched ablation 未完成前，不申请新的效果型 A800，不把 confidence escalation 正结果回填 selector，也不把流程审计性当算法增益。若所有候选均失败，论文仍有清楚可发表的系统性负结果：强固定 incumbent、selected-only 延迟反馈的识别边界、provider family/lineage 限制，以及 post-call confidence 作为独立 verifier 信号。
评价条款/分数影响：方法论 H8–H10、M5、M10、M12、M13、M15；故事线 H6/H8/S3/S4/S6/S11；当前适合 `REWORK/HOLD`，不能标 `STORY READY` 或 `SUBMISSION REVIEW READY`。

DECISION TEST: 由 owner 在下一轮冻结 `dynamic_scope`（推荐 `feedback-adaptive`）、reward 语义（推荐显式分开 `system_success`/`chemical_correct`）、唯一 candidate 和 confirmation 闸门；冻结后仅按上述最小实验包执行，任何新候选都回到 Round 0 重新登记。

CONFIDENCE: 0.95；建议与当前生效文档和已有开发证据一致，保留正信号同时避免过度主张。
