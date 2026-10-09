# 条件第二次调用：开发集静态小实验 v1

2026-10-08；执行前冻结。本轮是可行性诊断，不替换主方法，不打开 confirmation，不声称完整 Cascade Routing/OCL 复现或在线效果。

## 唯一问题与判据

固定第一次选择和终止验证器后，合法可见的第一次返回是否含有能预测第二次调用净收益的信息？完成的标志是可独立重算的 label、动作、终局损失和对照；方法升级仍须 design_review 的强最近邻与 online 实验。

对照近邻：Cascade Routing 的输出条件质量/成本优化；Online Cascade Learning 的付费选择性在线更新。原文与代码合同由本目录 literature 单独核对。这里使用小 ridge 回归诊断可学习性，不把标准回归或 VOI 声称为创新。

## 数据与三段划分

- 仅使用已反复检查的 722 BBB development molecules、四个 checkpoint-bound provider。禁止读取 157 confirmation。
- 输入：`fingerprint_diagnostic_v1/inputs.json` 的 IDs/truth/scaffolds/folds，忽略其中旧33工具的预测列；真实四工具概率、65维分子特征和 fit-only prior 来自 `experiments/logs/task49_fit_score_reference_20261007/v1/`。
- A=原 fold fit：固定首次 Corrected Frozen selector；拟合终止验证器。
- B=原 fold validation：固定 A 的全部模型，仅训练追加调用价值。B 不再用于阈值、epoch、正则或模型选择。
- C=原 fold evaluation：本候选的外层评估。所有区按 scaffold 隔离；C 是 development 评估，不是首次未读的最终测试。
- 五fold的A/B/C规模依次为444/133/145、483/94/145、492/86/144、483/95/144、480/98/144；B实际范围86–133。
- 首次选择器沿用 A-only 的 prior_weights 和 fit_rates；分数为 sigmoid(raw_logit+logit(fit_rate))。缓存恢复源码表明固定 fit_prior 只接收 A indices；执行前再检查。

## 冻结验证器与标签

对每个可见工具子集 S（四个 singleton 和六个 pair），构造8维输入：四个可见标记，以及四个 masked clipped-logit native probability；未调用项置0。

$$q_S=\sigma(\beta_0+\beta^\top z_S).$$

Case：仅 ecfp_logreg 返回 .8 时，标记为 `[1,0,0,0]`，数值为 `[logit(.8),0,0,0]`；pair 获得后才打开第二项。logit 将概率裁到 `[1e-6,1-1e-6]`。使用 sklearn LogisticRegression，C=1、lbfgs、max_iter=1000、tol=1e-8、random_state=20261008。A 每分子10个子集，总权重1（每行.1）；保持截距不正则化。

终止规则：q≥.75答1、q≤.25答0，其余拒答。它是估计分数驱动的规则，不声称真实风险保证。错误损失1、正确0、拒答ρ=.25；调用成本κ=.01，每题至少一次、最多两次。

$$g_b=\ell(d_{\{a\}},Y)-\ell(d_{\{a,b\}},Y)-.01.$$

Case：第一次答错、第二次后答对，g=.99；第一次答对、第二次后答错，g=-1.01；原拒答、后答对，g=.24。B 的 full-panel 配对只用于双方同权限的离线冷启动；C 决策前不读取未调用输出或真值。验证器全程冻结，不用本样本标签更新后重算标签。

## 价值模型与对照

每个 B 分子按首次冻结选择 a，为三个未调用 b 构造配对目标，每条权重1/3，总权重1。

- **conditional_gain**：16维 φ，12个 ordered-pair one-hot 截距 + 4个 candidate-specific confidence slope。c=abs(2q_single-1)；对应 b 的 slope 项填c，其余0。
- **pair_mean_gain**：同12个 pair 截距，删除全部 confidence slope，是 matched information ablation。
- 两者用 $(\Phi^TW\Phi+I)^{-1}\Phi^TWg$，λ=1，所有系数同样正则，固定设置不搜索。
- B 中从未出现的 ordered pair 不允许追加，避免仅靠共享 slope 在无支持pair上外推。
- **stop_only**：一次调用后直接使用冻结终止规则。
- **confidence_cascade**：若首次终止规则拒答，追加首次 selector 分数最高的未调用 provider，再用同一冻结验证器终止。
- **evaluator upper bound**：真值与完整面板均可见，在同一次调用后比较三种 realized gain。用offline dynamic programming在相同prefix预算下最大化总gain，回溯动作；它是 evaluator-only headroom，不进入可部署排名或主CI对比。

Case：candidate-specific slope 允许随首次置信度改变第二 provider 的排序；若所有 slope 共享一个值，固定首次 a 时排序不变，不能检验这种条件选择。16维是固定的小模型诊断，不表示最终 backbone 已选定。

## 因果预算与顺序

每fold evaluation顺序为 NumPy default_rng(20261008+fold) 对其原 indices 的一次排列；所有策略相同。token初值0，每episode增加.25；有≥1 token才可追加，追加扣1。gain策略只在最高预测g>0时申请追加，tie按固定provider顺序。confidence策略也用同一预算。

因此任意前缀 t 的追加次数≤floor(t/4)，平均调用≤1.25。不按整批得分选 top25%，不补看未调用结果，未用预算可顺延。upper bound 用DP比较每题stop/call：状态是已用追加次数，只有不超过当前floor(t/4)才允许call，奖励为当题最高realized gain。它可以看未来，在有限序列中计算真正的预算上界；另报告各分子max(0,g)作为无预算逐题headroom。贪心oracle不保证预算上界，不能使用这个名称。

## 指标、日志与停止规则

- 主指标 U=-(wrong+.25refusal+.01calls)/N；报告相对stop、cascade及pair-mean的配对差、覆盖率、错误/拒答/调用数、rescue/harm，以及B/C各pair支持与目标分布。
- C 每个分子仅属于一个fold，五fold pooled共722；不把B的三条pair当独立分子。全量才作10,000次fold内scaffold block bootstrap（seed20261008）：每fold有放回抽取相同数量的scaffold block，block内保留原episode顺序，按抽到的block顺序连接，逐方法重跑token控制器后再pooled。拟合模型固定，不重新训练；区间包含这种block重采样/重排的影响，不包含训练不确定性，也不等同于真实时间漂移。
- 开始前写config、源码/输入/card SHA、环境、argv与git parent；流式写fit/B-target、决策与evaluator证据。policy日志不含truth/未选输出，evaluator诊断另存；同进程分离不是安全沙箱。
- 先fold0 smoke，验证split/状态/调用数/标签/复算；工程检查通过后跑全部5fold，无论smoke效果正负，不以首折效果决定是否报告其他折。
- 全量相对最强已实现合法控制若差<.01或CI跨0，记当前模型证据不足；即使过线，也须完整强近邻和Online/Replay才能升级。若pair缺支持或验证器不能使用第二输出，解释限于此固定构造。
- 不调参追胜；所有失败原样保留。预算：CPU本地≤5min，零外部API、零provider forward、零GPU，使用真实封存概率缓存。

## 范围

本轮先证实 paired label 合同与静态信号。后续必须在真实付费选中pair和延迟反馈上验证更新增量，再验证agent使用效果；本轮不能开启完整科学投稿gate。
