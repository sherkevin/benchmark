# 调用后拒答与追加调用：效用审计 v1

日期：2026-10-01。状态：分析方案，执行前固定；已有 accuracy 已知，因此属于事后开发审计，不是新的确认实验。对应生效标准 B12、E5、C3/C4，不修改六份主线合同。

## 问题与本轮取舍

旧静态 confidence verifier 提高了已回答题目的正确率，confidence escalation 提高了全量正确率。我们还不知道这些改善能承受多少拒答代价和调用代价。先把这个问题算清，再决定是否值得训练动态 verifier。

三个方向：直接训练动态风险门；新跑 accept/escalate/refuse 联合策略；先重建旧策略的效用边界。本轮选择第三个：它能用已有真实 cache 区分“错误减少”与“少答/多调用”的贡献，无需 GPU、API 或重新调阈值。前两个仍是后续候选，目标没有撤回。

最近对照是本仓已执行的 fit-only confidence threshold 与 confidence escalation，以及 always-accept / always-refuse / 同动作数随机控制。本轮不提新算法贡献，不声称完成 PILOT/BayesianRouter 或动态 MIRA 的比较。

## 数据与可见性

- 固定 `fingerprint_diagnostic_v1/inputs.json`，SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`；722 个 BBB development 分子、33-provider 菜单、原五折。8 个 unresolved 不进效果 cohort；不读取 confirmation。
- 每折仍只用 fit correctness 排第一、第二 provider；fit confidence 的目标覆盖率仍为 0.8，按原 tie 规则冻结阈值。validation 不重新选策略。
- evaluation 决策只读取已调用 provider 的返回，`confidence = 2*abs(p-0.5)`，仅 kind=prob 时有意义。动作固定后 evaluator 才评分。
- 旧 results/recovered 文件仅有逐折汇总，**没有完整逐分子轨迹**。本轮从冻结 inputs/cache 独立重建每条记录，注明 `reconstructed_static_audit`；不伪称找回历史执行 trace。
- 首先逐行核对 status/label/correctness 与 input，逐折核对旧 summary 和 recovery hash。第一 provider 如果出现 missing confidence 或 failure，停止本轮效用推论并报告旧分母风险，不静默排除。当前效用分析限于第一调用均可用的已核 cohort。
- 第二 provider 失败保留：failure 终止类别与 wrong answer 分开。label 型第二 provider 返回合法标签时仍可回答，不能因为没有 numeric confidence 就算 runtime failure。旧 threshold 汇总已核对第一 provider 的五折 `n_evaluation_numeric == n_evaluation_rows`；新脚本仍必须逐 entity assert 第一调用 `status=ok`，不继承汇总中的隐式排除。

## 冻结动作

| policy | 动作 | 调用数 |
|---|---|---:|
| always_accept | 采用第一 provider 的有效输出 | 1 |
| confidence_refuse | 第一 confidence ≥ fit 阈值时采用，否则主动拒答 | 1 |
| confidence_escalate | 第一 confidence ≥ 阈值时采用，否则调用已冻结的第二 provider，采用其有效输出；第二调用失败则记 failure | 1 或 2 |
| postcall_always_refuse | 完成第一调用后总是拒答，作为零回答覆盖边界 | 1 |

零调用就拒答是另一任务阶段，本轮不把它与调用后的动作混在一起。四个策略均无在线更新，故不新增或改变 refusal 后 learner 的反馈权限。unknown/failure/refusal 的流语义沿用 semantic smoke，不把本轮 evaluator 的反事实对错释放给策略。

同动作数随机控制只作 **evaluator-side conditional null**：在每折固定与 confidence 策略相同的拒答/追加数量，对所有随机位置求期望。实际 evaluation 动作数来自完整 cohort，因此它不能作为可部署在线 baseline。未来完整实验中的可部署 random 必须使用 fit-only 冻结概率，实际动作数可以浮动；两者分表。

## 最小数学形式

对已核且有独立真值的 $N$ 道题（unknown 不进 utility 分母，另报 censoring），$W$ 是有效返回但答案错误数，$F$ 是最终没有可用返回数，$R$ 是主动拒答数，$C$ 是实际 provider 调用数。正确、错误、最终失败、拒答四个终止类别互斥，合计为 $N$。输入里的 `system_success=0` 不能直接当 $W+F$：它把 provider runtime failure 也编码为 0。本轮从 `output_statuses` 和 `output_labels` 重建这两个类别。一个失败调用之后若仍得到最终答案，则失败调用只进事件账，不再给终止损失重复加罚。

$$U(\rho,\kappa)=-\frac{W+F+\rho R+\kappa C}{N}.$$

Case：三道题分别答对、答错、主动拒答；每题调用一次，则 $N=3,W=1,F=0,R=1,C=3$。若拒答代价 $\rho=0.25$、每次调用代价 $\kappa=0.01$，效用为 $-(1+0.25+0.03)/3$。这里把一次错答和最终失败归一成损失 1，只是明确的评测偏好，不代表临床代价；原始四类计数始终同时报告。

固定策略不随成本变化。报告完整 $\rho,\kappa\in[0,1]$ 上的线性表达式和两两胜出条件，不从该区域挑一个能赢的成本作为论文主指标。

$$\Delta U_{\mathrm{refuse,accept}}=\frac{B-\rho R}{N},\qquad \rho^*=\frac{B}{R}.$$

Case：一组 10 题中拒答 2 题，其中 1 题原本会答错，则 $B=1,R=2$；拒答比全答更好当且仅当 $\rho<0.5$。$B$ 是原本会错且被拒答的计数，可由同一输入的已选第一输出在 evaluator 端核对；它不是 decision 输入。若 $R=0$，阈值记 null。

$$\Delta U_{\mathrm{escalate,accept}}=\frac{G-H-\kappa E}{N},\qquad \kappa^*=\frac{G-H}{E}.$$

Case：追加 4 次调用，救回 2 个第一工具的最终坏结果，又把 1 个第一工具的好结果改成坏结果，则 $G=2,H=1,E=4$；每次追加的成本须小于 $0.25$ 才有正效用。这里的“坏结果”统一指 `wrong` 或最终 `failure`，所以不会把一次终止失败漏算或与中途 runtime failure 双算。$G/H$ 分别是减少/增加的终止坏结果，不能只报净准确率。$E=0$ 时阈值记 null；若 $G-H\le0$，正成本下无胜出区。

## 统计与记录

- 逐 entity/fold 保存第一/第二 provider、fit 阈值、已调用原始返回、动作、计数、counterfactual evaluator 字段及 rescue/harm；逐折和 pooled 都报告。
- pooled empirical break-even 是描述性结果。固定 $5{,}000$ 次 scaffold-cluster paired bootstrap、seed `20261001`；同一 scaffold 全部 entity 一起抽，空 scaffold 仍为 `ACYCLIC`。禁止逐 provider 输出格或 online 日志行抽样。
- 报告两个主要 paired contrast（refuse−accept、escalate−accept）的系数及 break-even 的 percentile 区间；区间仅在固定五折拟合策略下描述样本不确定性，不包含重新拟合的变异，也不证明未见 scaffold 泛化。
- 固定展示网格 $\rho\in\{0,0.1,0.25,0.5,1\}$、$\kappa\in\{0,0.01,0.05,0.1,1\}$；网格 CI 是 pointwise，不作同时显著或选点主张。网格不用于改动作。
- 原始 JSONL、config（开始/结束、命令、Python/numpy/硬件、git commit、输入/源码/旧结果 hash）、fold 汇总、paired bootstrap 分开保存。失败保留，不覆盖原目录。

## 否定条件与下一步

1. 旧汇总无法重建、source hash 不符、scaffold 泄漏、分母缺失：停止，先修 provenance/语义；不产出有效效用结论。
2. 拒答/追加仅在极低代价下有优势：不能把 accepted accuracy 当可靠性的完整增益。后续候选必须明确降低错拒和无效追加，而不是只优化同一 accuracy。
3. 成本为零时 escalation 的 paired 区间仍跨 0：保留“点估计有救回但证据不足”，不能写稳定收益。
4. 不因该审计给 Gate C 自动加分。它补 E5 的旧策略效用和错误归因；MIRA 在线效用、modern/matched baseline 全矩阵、多端点、冻结 confirmation 仍需各自证据。

本轮产物可复用为未来联合 verifier 的评价器与错误分解。只有形成新的机制假设和合法信息来源后，才另登记动态策略；不因写完效用合同立即开 GPU。

## 旧汇总给出的待核预测（不是本轮结果）

把已保存的五折计数先作算术 sanity check，得到待独立逐行重建核验的预测：threshold policy 预计 $N=722$、拒答 $R=149$、接受错误 $22$、拒答集合中原本会错 $B=39$；因此相对 always-accept 的拒答 break-even 约为 $\rho^*=39/149=0.2617$。相对 post-call always-refuse 的边界约为 $22/573=0.0384$。confidence escalation 预计额外调用 $E=149$、净救回减损坏 $G-H=6$，对应 $\kappa^*=6/149=0.0403$。这些数字只来自已保存的 fold summary，不能写入结果表；如果逐行重建或 hash 校验不一致，全部作废。
