# 下一步：学习第二次调用的条件净价值

日期：2026-10-08。状态：候选设计和只读方法审查；未运行效果实验，未替换主方法或生效合同。

## 目的与衡量标准

目的不是再证明一个阈值可以拒答，而是补上“第一次工具输出 → 判断是否值得继续取证 → 接受/拒答 → 延迟反馈改善下一次决定”的整体机制。当前稿的一次调用结果和次级拒答分析分别保留，不能把二者拼成已完成的在线 agent 能力证据。

对应生效标准：故事 H4/S4/S6，方法 M5/M8/M11/M13，Benchmark + Baseline v006 的 B9/B12/B13/B14。通过这张候选卡的最小条件是：条件取证策略相对最强静态 cascade 有终局效用收益；其在线版本相对自己的 frozen 版本再有收益；所有调用都收费、所有更新都来自已获得并已释放的反馈。三者不能共用一个不能区分原因的指标。

## 为什么优先做这个

1. 四个 checkpoint-bound provider 的 development panel 有 147/722 个 mixed-correctness 分子，也有 104/722 个全错分子。更换工具只能利用前者；相互验证不能自动修好共同错误。证据：`headroom_feedback_v1/report.md`。
2. 历史 33-provider static confidence escalation 为 92.38% accuracy、1.206 calls；动态 marginal risk replacement 为 90.03%、1.49 calls。它们不是四 provider 菜单的结果，但说明再次更新边际准确率不足以解决条件互补问题。证据：`confidence_escalation_v1/report.md`、`dynamic_verifier_v1/report.md`。
3. 157 分子上的拒答分析复用了已检查的留出集，只提供 secondary observation；它没有验证 delayed update 或第二次调用。证据：`postcall_fresh_confirmation_v1/correction_amendment_20261008.md`。
4. 先前 context exploration 实验的有效样本数不足以支持逐 cell 的强置信门；不能靠降低界限使其强行开放。证据：`research/AAMAS2027_BBB_Context_Exploration_Only_Evaluation_Report_v2.md`。

## 从决策损失出发

原始对象尽量只保留四个：已有可见证据 $E$、候选下一工具 $b$、独立答案 $Y$ 和该工具将返回的 $O_b$。当前证据包含分子、端点、已调用工具 ID/返回及已释放状态，不含其他工具本题的输出或真值。停止规则 $d$ 在 fit 区固定，输出 0、1 或拒答。错误答案损失为 1，正确答案为 0，拒答为 $\rho$；追加调用代价为 $\kappa_b$。

Case：输入是 development CSV 的某个 BBB 分子；工具 A 已返回 `{"probability":0.58,"status":"ok"}`，这属于 $E$。工具 B 尚未调用，$O_b$ 不可见。$Y$ 是 evaluator 之后释放的 BBB 0/1 标签，不能输入当前决策。$\rho=.25$ 表示拒答损失，$\kappa_b=.01$ 表示额外一次调用损失；两者是实验效用设置，不是医学风险估价。

真正需要估计的是追加调用对终局损失的条件改善：

$$G_b(E)=\mathbb E[\ell(d(E),Y)-\ell(d(E,O_b),Y)\mid E]-\kappa_b.$$

Case：若原先会错误作答、第二次取证后正确，观测改善为 $1-0=1$；反过来为 $0-1=-1$；若原本拒答、追加后正确，为 $.25-0=.25$。只有已实际获得两个输出并且真值已释放，才有这条 paired target。没有调用 B 的 episode 不能从缓存补出 $O_b$ 或标签。

这一定义不要求先声称工具独立。两个工具各自 80% 准确，并不能决定追加调用是否有价值。一个可精确枚举的反例是：$Y$ 均匀二元，两个工具都是对称 80% 准确，停止规则使用真实后验，$\rho=.25,\kappa=.01$。

- 条件独立错误时：两个输出一致的总概率是 $.8^2+.2^2=.68$；这部分的无条件作答错误是 $.04$。分歧概率 $.32$，后验为 $.5$，因此拒答损失 $.32\times.25=.08$。第二次观测后总损失 $.12$，原先为 $.20$，净价值 $.20-.12-.01=.07$。
- 完全共享错误时：第二个输出总等于第一个；后验及损失不变，净价值为 $-.01$。

Case：上述是二元合成分布的数学反例，不是 BBB 实验；它证明边际可靠性相同仍可产生相反的取证决策。真实任务还必须证明条件互补规律在合法证据中可学习。

## 分两步做，避免直接堆模型

**第一步：条件价值能否学会。** 仅使用已有 722 development、五个 scaffold folds 和四个 checkpoint-bound provider。固定首次选择为 Corrected Frozen，固定一次最多追加一个工具。训练和阈值选择仅使用该 fold 的 fit/validation；157 留出集不参与本候选开发。先定义并冻结停止规则，再用 fit-only 实际 paired outputs 构造 rescue/harm/refusal targets。每个模型使用相同特征、warm-start 和调参预算。

对照必须包含：fit-selected static confidence cascade/refusal、依据原文适配的 Cascade Routing、frozen conditional-value policy，以及去除条件互补信息的 matched control。Cascade Routing 已涉及输出条件级联及成本优化，所以“把 routing 和 cascade 放在一起”不足以构成这张候选的区别。参考本地一手论文审查：`research/AAMAS2027_医药工具选择最近邻与方法审查.md`。在实现前补完其原公式与本合同的逐项映射。

**第二步：延迟反馈能否再改善。** 第一阶段有可学习信号后，才比较同一条件价值模型 Frozen / Online / Replay，delays $\{0,1,4\}$。任何随机 paired audit 都必须预先固定 propensity、有正支持、计入调用成本；两输出未齐或反馈未释放不得更新 paired target。两次调用的具体预算怎样分配要在执行卡中固定；不能把探索费用记在系统外。

## 拟定门槛和停止规则

以下是待实现前冻结的候选门槛，不是已获得结果，也不替换全项目验收标准：

- 最多两次调用，平均不超过 1.25；终局 utility 按每个输入计，不能只比较 accepted accuracy。
- 第一阶段相对最强 frozen comparator 的 utility 改善至少 .01/input，paired scaffold 95% CI 下界大于 0，且预算满足。
- 去除条件互补信息后若不降效用，就不能把结果归因于该机制；保留结果并重新分析，不换名字宣称机制成功。
- 第二阶段 Online 必须相对自身 Frozen 在同初始化、同审计预算下有额外效用。只得到静态收益时，不报告动态收益。
- 一折约 145 个 evaluation 分子，10% paired audits 只有约 14 条，不能同时估计很多 provider/context cells。区间过宽应记为 inconclusive，并估算需多少新观测；不能挑顺序或改阈值追胜。
- 策略 replay 通过后，再以固定策略运行真实 agent 对照，报告终局 utility、违规动作和 verify/refuse 决策执行情况；不能把 replay policy 收益写成 agent 能力已经提升。

## 独立只读审查

本卡吸收 `/root/gate_next_scientific_step` 的审查：优先级 1 是条件第二次调用净价值，优先级 2 是有真实输出的 provider/version 或输入分布变化，优先级 3 才是继续一次调用 backbone 试错。审查明确指出 online paired feedback 稀疏、静态 cascade 可能解释全部收益、policy gain 不等于 agent gain。这些中间意见和反例保留，不把审查意见视为实验事实。

## 最近的可执行工作

1. 对照已有 provider/native cache 和 split，写无泄漏 paired-label 生成合同；
2. 从 Cascade Routing 原文逐公式映射到两次调用/拒答合同，列同信息比较；
3. 在 fit/validation 内固定停止规则、低维条件价值模型、预算和统计门；
4. 先做单 fold 真实缓存小测试，检查能否产生非退化 paired targets 和可行动价值；通过后才全量，失败保留。

## 二轮审查：固定 label 的含义与最近邻对照

第二阶段也必须冻结终局 verifier 的参数、校准、拒答阈值、输入变换和它读取的 reliability state；本阶段只更新条件调用价值模型。若只是冻结代码而状态仍变，paired gain 的标签仍会移动。

给停止规则加版本 $v$，已观测 paired target 是

$$g_t^v=\ell(d_v(E_t),Y_t)-\ell(d_v(E_t,O_b),Y_t)-\kappa_b.$$

Case：$Y_t=1$，原 verifier 只看第一次返回时答 0，看过 B 后答 1，因此标签为 $1-\kappa_b$。若 verifier 自己学习后，仅凭第一次返回就答 1，调用 B 后也答 1，则新标签是 $-\kappa_b$。provider 和环境完全没变，标签仍差了 1。不能把这种变化直接叫做工具漂移。

因此决策时保存证据和两种终止动作，真值释放后才填 loss difference；不先用本样本标签更新 verifier 再重算该样本的 target。未来允许 verifier 同步学习时，要另行定义 versioned targets 或分阶段冻结，不把旧 verifier 下的收益自动当作当前 verifier 的收益。

最近邻原文重读也明确了不能弱化的比较对象。`task/task40-pharma-nearest-method-audit/evidence/cascade_full.html` 的 §3、§4 Algorithm 3 和 Appendix D.3 已经允许输出条件质量/成本估计、按价值继续调用、并使用 confidence、entropy、agreement、JS divergence。不能把 Cascade Routing 缩成只会按置信度固定调用一个次优模型。

需要比较的额外约束是：跨 episode 仅从付过调用成本且被选中的 pair 学习，终局后才按延迟释放。登记的选择概率 $\pi_{t,b}>0$ 和是否调用的指示量 $I_{t,b}$ 给出

$$\mathbb E[I_{t,b}g_{t,b}^v/\pi_{t,b}\mid E_t,H_t]=\mathbb E[g_{t,b}^v\mid E_t,H_t].$$

Case：若合法可见状态下以 $.1$ 概率审计 B，观测 pair 才用 $1/.1=10$ 加权；另外 $.9$ 的 episode 不访问 B 输出。这个等式要求选择随机性与未见结果在给定合法历史后满足观测条件；它说明可识别性，不保证小样本方差、模型正确或创新性。

下一轮必须包含 **Cascade Routing + 相同 delayed updater**，从而排除“只是加了普通在线更新”的解释。该论文引用的 Nie 等 *Online Cascade Learning for Efficient Inference over Streams*（ICML 2024）也必须先读其反馈和训练合同。当前不宣称 delayed paired feedback 是文献中空白；先完成最近邻检查再冻结实验卡。

## 三轮审查与最小实践（2026-10-08，运行前）

独立方法审查建议把标签几何和可学习性放进同一个小实验，不再单独堆一次 headroom 审计。具体设置已冻结在 `experiment_card.md`：原fit区拟合selector与验证器，原validation区拟合gain，原evaluation区评估。这样不需要先做复杂cross-fitting，也避免验证器已经看过gain训练样本的标签。

共同斜率的初始建议被修正：若 $\widehat g_{a,b}(c)=\alpha_{a,b}+\beta c$，对于固定a，改变c不会改变b排序。最终诊断采用12个pair截距+4个candidate斜率；未见pair禁止追加。这个修改来自计算关系本身，没有按结果选模型。

审查还指出，预算控制器把相邻决策连在一起，不能仅bootstrap已经发生的逐行loss后声称评估了完整策略。全量分析将按scaffold block重采样，并逐方法重跑token控制器。区间条件于当前拟合模型，不包含重新训练的波动；block重排本身也要披露。

保留的反对意见：9参数共享验证器可能学不到pair的相关误差；value-fit只有94–133个分子，16参数模型也可能不稳；当paired oracle空间存在但策略抓不到时，无法仅凭这一结果区分表示与样本不足。无收益只能拒绝本次构造的效果结论，不能推翻完整verification目标。正收益也不能跳过强cascade、delayed updater与真实agent实验。

此次最小实践得到已有持续实验授权支持，只是候选诊断，不将候选升为双方确认的新主方法。

协调者补充了一个运行前反例：若第4题gain=.1、第5题gain=1，token初值0且每题+.25，贪心特权策略第4题花掉唯一token，反而不如第5题才调用。因此它不是预算上界。实验卡改为按前缀约束求exact offline DP，并保存其动作；该上界只在evaluator里计算。

运行前原始split核对补充：value-fit的B实际为133/94/86/95/98个分子，前文审查中的“94–133”估计遗漏fold2的86，后续以实验卡和原始indices为准。provider自身使用独立数据的validation选过epoch；A-only只描述本轮selector/verifier训练，不能扩成“整个链条都没有validation”。
