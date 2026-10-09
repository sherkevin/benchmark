# 外挂在线工具管理器：两次调用的可证伪候选

> 2026-09-23 作者侧候选 v1；2026-09-24 主线修订见 [ADR-0012](../../docs/adr/0012-three-layer-dynamic-jev-agent-loop.md)。回答“实时训练的多臂老虎机到底能解决什么”。这不是 Owner 已批准方法、已完成实验或已证明创新；Task38 的当前池训练/新预测暂停和 Task42 协议未冻结仍有效。项目目标见 [当前目标](../../docs/目标.md)与[阶段汇报 §2.6](/Users/jingwu/Desktop/科学工具可靠性_Agent阶段汇报_v7_20260918.md)。

## 结论先行

外挂在线模块是动态工具适应模块中的**具体解法候选**，而不是整篇系统的全部能力。普通单步 contextual bandit 能解决“当前分子先选哪个具体模型”；动态Jev还要把证据验证、是否复核、是否作答或拒答作为结构化决策；主Agent最终承担这些动作的执行。Agent看见首次输出再请求复核时，第二次模型的目标必须从单模型准确率改为**对当前证据的条件补益**。最多两次调用时，可用两个在线学习头实现；完整问题属于有隐藏真值的序列决策，不能把单步Bandit的保证直接套给整条Agent轨迹。

## 1. 三层职责与合法状态

- 主 Agent 的动作：`REQUEST(function_key)`、`INSPECT`、`VERIFY`、`ANSWER(0/1)`、`ABSTAIN`。它决定何时请求、是否复核和何时停止。动态Jev与管理器提供结构化风险和验证建议，主Agent仍要学习并执行这些动作；主实验只收到被选模型的身份、真实输出或失败及剩余预算。
- 动态工具适应模块在收到请求时选合格的具名 `provider/version`，并维护版本、反馈和预算；模型版本变化视为新 arm。同一题重复调用确定性同版本不算新增证据。BBB 试点只有一个功能 key，故暂不能据此声称跨功能能力。
- 首次状态为 `s0=(x, function_key, eligible_versions, budget, policy_version)`；二次状态再加 `(a1, z1, valid1)`。选择时不得看本题实验真值、未调用模型输出、全表排名或未来反馈。
- 一题是一个 episode：工具输出/失败立刻可见；正确性只在**整题终止且实验真值确实到达后**可见。在线更新发生在题与题之间，不在同一题第一次和第二次调用之间偷渡真值。真实部署标签迟到或缺失时，该题保持 pending；静态缓存逐题揭示真值只模拟这种反馈顺序，不证明真实部署能够即时学习。

## 2. 候选学习器：先选正确模型，再选有补益的模型

令 `a` 是具体模型/版本，`x` 是事先冻结的分子特征。第一个头估计

`q_a(x) = P(valid_a=1 且 z_a=Y | x)`。

资格合格后，用共同单调用历史 `H0` 初始化具名模型的正则化条件回归；在线训练期可用 LinUCB 或后验采样作探索并记录选择概率，冻结评价期按预登记规则执行。输出失败记为 `valid=0`，可以更新可用率；其 `chemical_correct` 仍为 `null`，不能伪造化学错误。只有取得真值的有效输出才更新条件正确率。实际实现可把 `q_a=P(valid|x,a)·P(correct|valid,x,a)` 分解，以便检查失败与化学错误。

若 Agent 看过第一次结果 `z1` 后再次请求同一功能，管理器用第二个头对每个未调用合格模型 `b` 估计

`Δ_b(s1) = E[ L(d0(s1),Y) − L(d0(s1⊕(b,Z_b)),Y) | s1 ] − λ_call`。

`d0` 是**事前固定、只用合法训练区拟合**的“现在停止会答什么或弃答”的参考读出；需要先用独立的合格成对 warm-up 数据或一条事前固定的简单融合规则建立它，不能一边用同条成对反馈拟合 `d0` 一边给自身增益打分。`L` 包含答错与弃答损失。`Δ` 问的是模型 `b` 在**已见第一次证据条件下**能否改变并改善决策，不是它独自有多准。管理器只选 `b`，不替 Agent 作复核/停机决定。该价值思想已有 [Gao–Koller 2011](https://proceedings.neurips.cc/paper_files/paper/2011/hash/303ed4c69846ab36c2904d3ba8573050-Abstract.html)，公式不作为原创。

为了有可学的 `Δ`，训练期需要在获准分子上作少量**随机成对探测**，对研究的 `(a1,b,s1)` 区域保持正选择概率，并记下 `p(a1|x)`、`p(b|s1)`。这属于单独的数据收集政策；主实验仍由 Agent 自己决定是否请求第二次。一次实际成对调用在题后真值 `Y` 到达时产生

`g_b = L(d0(s1),Y) − L(d0(s1⊕(b,z_b)),Y) − λ_call`。

只把 `(s1,b,g_b,propensity,policy_version)` 加入实际选中 `b` 的历史；未调用模型没有奖励。用低维状态/具名模型特征加来源家族收缩拟合 `Δ`，交叉拟合 `d0`、报告权重截断和有效样本数。家族只帮助估计，不代替具体动作。若某模型对没有探测支持，标为“不可估”，不输出虚假高置信增益；是否探索须受预登记预算限制。按自适应日志比较策略时，用已记录概率作 IPS/DR 或其他合规离策略估计，并报告方差；这些统计工具亦非原创。

`d0` 的增益仅是**参考决策器的一步收益**，不等同于主 Agent 的最终收益。正式端到端指标仍是 Agent 的错误作答、覆盖、调用与逐题轨迹。先固定 Agent 比较动态模块，再固定模块比较 Prompt/真实 SFT/RL，最后报告动态Jev、模块与Agent的三层交互；不同时变动全部组件然后把收益归给某一个模块。

## 3. 在线事件顺序与失败边界

```text
before case t: freeze selector θ_t and Agent version; log eligible weights/versions
Agent REQUEST(f) -> selector samples a1 with logged p1 -> observe z1 or failure
Agent may REQUEST(f) -> selector samples a2≠a1 with logged p2 -> observe z2 or failure
Agent ANSWER or ABSTAIN -> episode ends; log answer, calls, versions and failures
if qualified experimental Y arrives: score only actually called outputs; update q and observed Δ; save θ_(t+1)
if Y absent: keep feedback pending; do not invent correctness or update Δ
```

若实验标签只在某些被实验室挑中的分子上出现，学习目标实际限于该选择机制下的分子；要外推到所有分子，必须记录标签取得机制或另采独立确认集。概念漂移也不能从无时间变化的 BBB 缓存推出。同版工具随时间变差须靠带时间戳的后续真值验证；版本更换直接建立新 arm。

## 4. 为什么普通 Bandit 不够，如何证伪

两个模型即使各自准确率相同，一个若几乎复制首模型错误，复核价值会很低；另一个单独准确率略低却能纠正首模型的典型错误，复核价值可能更高。单调用历史无法区分“错误独立”与“完全克隆”两种联合分布；[Task40 的反例](../task40-pharma-nearest-method-audit/report.md)已在项目内证明。已见输出一致也不能在所有模型共同犯错时自证为错。

实验拆成三项：

1. **首选模能力：**在未见合格分子上，在线 `q` 选择优于最强固定模型、独立 LinUCB/标准 contextual bandit，且与有相同标签预算的直接 `x→Y` 预测器比较。冻结泛化与先预测后更新的 prequential 在线适应分表报告；后者展示按到达顺序的累计错误和学习曲线，不能拿期末模型回填此前分子。
2. **复核补益：**在同样的已见输出、标签、平均调用和作答覆盖下，`Δ` 选择优于“挑次高 `q`”、随机复核、条件独立 VOI、同信息适配的 Gao–Koller、[Kelly 相关专家](https://proceedings.mlr.press/v267/kelly25a.html)和 [Cascade Routing](https://proceedings.mlr.press/v267/dekoninck25a.html)。合法全输出训练面板若存在，也须提供其监督上界对照。若只因多调用、多弃答或信息权限更多而获胜，主张失败。
3. **系统收益：**真实 Agent 在固定管理器下学会何时请求/复核/停止，且固定 Agent 在新管理器下获益；与规则控制器和真实 SFT/RL 对比，避免将外挂选择效果冒充 Agent 判断力。

当前 [Task42 单调用协议](../task42-p02-evaluation-contract/protocol_proposal.md)只支持第 1 项诊断；第 2 项须另冻成对探测合同。Task44 的“最多两次调用”是首轮可审 pilot 的协议边界，不是动态工具适应模块的总架构边界。试点最多两次调用、先用约 4–6 个有资格的具体模型、约 128 个合格外部分子只排查方向；独立确认所需分子数由盲化试点的不一致率决定，不能用 128 宣布效果。Task43 提议正式主读出为 90% 覆盖、平均最多 1.5 次调用，仍待协议裁定。

## 5. 论文贡献的诚实边界

“外挂模块”“在线 Bandit”“反馈延迟”和“见结果后继续调用”本身都已有文献：[LinUCB](https://arxiv.org/abs/1003.0146)、[延迟在线反馈](https://proceedings.mlr.press/v28/joulani13.html)、[延迟广义线性 Bandit](https://proceedings.mlr.press/v206/howson23b.html)、[Gao–Koller](https://proceedings.neurips.cc/paper_files/paper/2011/hash/303ed4c69846ab36c2904d3ba8573050-Abstract.html)及上述 2025 年近邻。可探索的方法增量是**在真实有限反馈下，用少量计入预算的共调用识别首结果条件下的减错收益，再作有不确定性约束的在线具体模型选择**。但“随机探测＋收缩回归＋IPS/下界”各部件也有先例；只有对同信息强近邻有可复现优势、并说明其成立条件，才能写成方法贡献。否则这是有价值的 benchmark/实证方案，不能宣称新算法。

当前不能执行学习：Task38 的 29 项外部资格 UNKNOWN、4 项未审，且 Owner 明确未批准当前池开训；P02 也未冻结。上述是可实施设计，不是已经得到的实证结论。
