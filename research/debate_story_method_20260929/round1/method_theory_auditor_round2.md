# Round 2：两个未决方法冲突的最终裁定

- 审查角色：method / theory auditor
- 日期：2026-09-29
- 范围：只裁定 A（selected-only P/Q 不可识别）与 B（feedback-adaptive fixed-pool 的动态性）
- 约束：不修改生效主线文档；结论区分已知证据、条件性预测和建议实验

## A. selected-only P/Q 不可识别：放弃 provider adaptation，还是声明探索/先验条件？

POSITION: revise（保留有条件的 provider adaptation；放弃无条件的强主张）

TARGET: 在只有已选 provider correctness、没有额外探索或结构假设时，是否仍可声称 updater 学会了未选 provider 的质量并改善后续选择。

EVIDENCE: 方法合同只释放 selected-only feedback，选择前禁止读取当前真值和未选 provider 输出（`docs/研究主线/生效/02-方法论.md:16-24`；`03-Benchmark与Baseline.md:35-39`）。构造两个环境：对任意输入，选 A 时 $Y(A)\sim\mathrm{Bernoulli}(0.9)$；环境 P 中 $Y(B)\sim\mathrm{Bernoulli}(0.95)$，环境 Q 中 $Y(B)\sim\mathrm{Bernoulli}(0.05)$。若策略不探索 B，两个环境产生相同可见历史 $(x,e,A,a,R)$，但最优 provider 相反。现有 development 的普通 selected-only 更新均未超过 Best Fixed（`development_master_table_v1/report.md:18-39`），provider transfer 也没有形成独立正证据（`current_acceptance_audit_v2/report.md:27-32`）。

REASONING: P/Q 反例不要求现在放弃所有 provider adaptation；它要求放弃**无条件、跨环境、无需 overlap 的 adaptation 叙事**。合法的三种收窄方式是：(1) 预登记固定概率探索并记录 propensity/overlap；(2) 预登记 provider 结构共享或 fit-only prior，并把可迁移条件写进 estimand；(3) 只声称 selected provider calibration、失败规避或已观测 event 下的策略更新，不声称识别未选 provider 的当前反事实。可形式化为：在 overlap/先验条件 $\mathcal A$ 下比较 $V(\pi\mid\mathcal A)$；没有 $\mathcal A$ 时，P/Q 只允许报告不可识别边界。更多 seed、更多 backbone 或更复杂 updater 不能消除该信息缺口。若采用探索，探索成本和 propensity 也会进入 utility；若采用共享结构，必须与 permutation/leave-provider-out 反事实区分真正 transfer 与 family artifact。

评价条款/分数影响：方法论 H2、H3、H9、H10；M3、M6、M10、M11、M15 在未声明 $mathcal A$ 时最高 2 分，声明后仍需独立恢复和 matched control 才可能达到 3。M5/M8/M13 不能用“加了 prior”替代必要性反例；故事线 H4/S4/S6 只能写成有条件机制，不能写成一般 provider adaptation。

DECISION TEST: 做两个顺序固定的最小实验：

1. **不可识别性单元测试（不看结果调参）**：在相同 A 历史下运行 P/Q 两个 B 环境；无探索/无先验的 updater 必须产生同一动作轨迹，恢复器应报告“不可区分”，而不是伪造 B 的估计。
2. **条件可识别回放**：在真实 BBB fixed pool 上预登记 $\epsilon$-探索（或固定 fit-only prior）以及 `Audit-Frozen`、Matched Online、Ours；记录每 provider exposure、propensity、pending/release、prefix/suffix accuracy、累计 regret、update cost 和 family/scaffold cluster CI。只有在至少两个 delay 中 Ours 相对同探索 `Audit-Frozen` 有预登记 utility 改善，且改善不依赖单一 family 或额外调用，才保留“在条件 $\mathcal A$ 下的 provider adaptation”。

若第 1 项失败（策略在无条件下区分 P/Q），直接判信息泄漏；若第 2 项无 overlap、transfer-vs-permutation 不成立，或 utility 不超过 `Audit-Frozen`/Matched Online，则停止 provider adaptation 方法主张，保留 selected-only 不可识别性和 benchmark 负结果。停止并不删除 provider menu 或静态 Best Fixed；它只删除超出可识别条件的在线适应结论。

CONFIDENCE: 0.99；P/Q 是构造性不可识别性结论，条件性保留范围由当前合同和 M3/M10/M11 直接决定。

## B. feedback-adaptive fixed-pool 是否足够支撑动态方法，还是必须有 arrival/retire/version event？

POSITION: revise（fixed-pool 足以支撑 `feedback-adaptive` scope；不足以支撑 `environment-nonstationary` scope）

TARGET: “动态方法”是否必须观察 provider arrival、retirement、version shift 或分布漂移，才能保留 dynamic 研究方向。

EVIDENCE: 生效方法评价标准明确划分：`feedback-adaptive` 只要求 selected-only correctness 在延迟释放后改变状态和后续动作；`environment-nonstationary` 另需 arrival、retire、version/shift 证据（`docs/研究主线/生效/评价标准/02-方法论评价标准.md:73-81,95-101`）。当前主流数据是固定 33-provider BBB pool、delay `{0,1,4}`，自然版本证据不足；Gate C 把动态 event/recovery 列为缺口（`current_acceptance_audit_v2/report.md:17-18,50-53`）。已有 controlled version event 是人为注入 Best Fixed provider failure 的 positive control，报告明确不能替代自然动态 benchmark（`controlled_version_event_v1/report.md`）。

REASONING: fixed-pool feedback-adaptive 是一个有定义的动态 scope：策略在 tick $t$ 只看已释放事件，状态在 release 后改变，后续 action 可因状态改变而不同。它检验的是“反馈到达造成的在线适应”，不检验“环境自身随时间演化”。因此不能因为没有 arrival/retire/version 就把所有 dynamic 研究判死；应禁止未限定的 `dynamic JEV` 术语，改用 `feedback-adaptive provider selection under a fixed menu`。若方法在固定池中从未改变状态或所有延迟都与 Frozen 相同，则它没有证明动态机制，只给出负结果。arrival/retire/version 是 environment-nonstationary 扩展的硬条件，不是 feedback-adaptive 主线的硬条件。

评价条款/分数影响：方法论 H1、H2、H5、H9、H10；在 fixed-pool scope 下，M2、M7、M10、M13、M15 可按 release→state→next-action 证据评分；在 environment-nonstationary scope 下没有事件时 M10/M15 最高 2 分。M8 只有在状态真正改变决策计算对象并有删状态反事实时才可过 3；M5 需要说明该延迟/selected-only 组合相对 nearest baseline 的必要性。

DECISION TEST: 先做 fixed-pool 主实验：同一 menu、feature、fit-only initialization、budget 和 label semantics，运行 Frozen、`Audit-Frozen`、Matched Online/Replay、唯一 Proposed，delay `{0,1,4}`。逐事件验证 `decision → terminal → release → update → next-action`、pending/duplicate/restart、状态 hash、update p50/p95 和内存；报告 prefix/suffix accuracy、累积 regret、switch rate、反馈到 recovery 的步数、calls/failure/unknown 和 molecule/scaffold paired CI。只有当 Proposed 在至少两个 delay 中相对同合同 `Audit-Frozen`/Matched baseline 达到预登记 utility（direct correctness claim 还需不低于 Best Fixed）且删掉状态后效果消失，才保留 `feedback-adaptive` 方法主张。

若要声称 environment-nonstationary，再单独运行 event extension：预登记一种 arrival、retire 或 version shift，加入 Frozen-pool、Reset-on-event、Matched Online 和 Full-panel event oracle；事件时间和方法不可见，报告 event-after recovery、遗忘/切换延迟和成本。事件实验未通过时，不得写自然 drift 或 provider evolution，但仍可保留通过的 fixed-pool feedback-adaptive 结论。fixed-pool 主实验也未通过时，删除 dynamic 方法主张并保留 protocol/negative boundary。

CONFIDENCE: 0.98；scope 区分直接来自生效评价标准；fixed-pool 是否有真实收益仍是待实验预测，不能由术语判断。

## 最终合并意见

POSITION: revise

TARGET: 两个冲突的统一决策。

EVIDENCE: A 的 P/Q 反例和 B 的 dynamic_scope 定义均已有项目内合同、审查和评价标准依据；当前 development 只证明强 Best Fixed、普通在线 head 负结果以及 post-call confidence 的独立信号，尚未证明 Proposed 的 matched utility gain。

REASONING: 最终不放弃 provider menu 或所有在线研究；放弃的是两个越界主张：没有 overlap/先验条件却声称识别未选 provider，以及没有环境事件却声称 environment-nonstationary。下一轮应把唯一候选声明为“fixed-menu, selected-only, delayed, feedback-adaptive”，并在方法卡中写出 $\mathcal A$（探索/先验/共享结构）、reward 语义、utility、状态不变量和 kill criterion。若 fixed-pool matched 实验失败，结论收窄为不可识别性 + benchmark/negative result；若通过，再以独立 event extension 检验更强的 environment-nonstationary 主张。

DECISION TEST: coordinator 只需做两个冻结决定：

1. 是否允许在 $mathcal A$ 明确登记后运行 provider adaptation；若否，停止 adaptation，只做 selected calibration/benchmark。
2. 是否把论文 scope 写成 `feedback-adaptive`；若要写 environment-nonstationary，必须先批准独立 event protocol 和 Frozen-pool 对照。

CONFIDENCE: 0.98；这两个决定保留了可证伪研究路径，同时严格遵守信息边界和动态范围。
