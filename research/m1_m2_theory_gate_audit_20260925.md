# M1/M2 gain gate 理论闸门审查

**日期：** 2026-09-25  
**结论：** 先不要把当前 M2 直接提交到 Nebula 做昂贵实验。M1 的随机 probe 合同可以继续做本地小规模验证；M2 只有在损失定义、context、冷启动和置信门这四处修正后，才有资格进入单批远程实验。

## 1. 先固定真正要估计的量

第 $i$ 个 episode 在第二次调用前的决策信息记为 $\mathcal F_i$。它必须包含：当前分子和函数、已经返回的工具结果、候选菜单、当前 learner state/version 以及调用预算。令 $a=0$ 表示停止，$a=p$ 表示追加 provider $p$。

先固定两个潜在终止轨迹：

- $L_i^0$：在当前历史上立即停止，并按固定规则回答或拒答的终止损失；
- $L_i^p$：调用 $p$ 后按固定规则回答或拒答的任务损失，不含调用成本；调用成本单独记为 $c_p$。

真正的 target 是

$$
\Delta_i(p)=L_i^0-L_i^p,
\qquad
\tau_p(\mathcal F_i)=\mathbb E[\Delta_i(p)\mid\mathcal F_i].
$$

调用的净效用增益是 $\Delta_i(p)-c_p$，所以 gate 比较 $\widehat\tau_p-c_p$；也可以把成本并入 utility loss，但不能同时在 $L_i^p$ 和 gate 阈值中扣两次。

如果第二次调用失败后是拒答，那么 $L_i^p$ 必须使用拒答损失；不能把工具 failure 直接记成错误 1，同时把实际终止动作记成 abstain。否则代码里的 `gain` 与公式里的 verification gain 不是同一个量。

若 $A_i(p)$ 表示实际是否调用 $p$，并且随机化满足

$$
\Pr(A_i(p)=1\mid\mathcal F_i)=\eta_i(p\mid\mathcal F_i)\geq\eta_{\min}>0,
$$

且当前动作在给定 $\mathcal F_i$ 后不再依赖潜在结果，则有

$$
\mathbb E\left[\frac{A_i(p)\Delta_i(p)}{\eta_i(p\mid\mathcal F_i)}\middle|\mathcal F_i\right]
=\tau_p(\mathcal F_i).
$$

这是 IPW 的识别条件。有限样本下分子/分母比值一般不严格无偏，只能在 overlap、有限权重和稳定性条件下讨论一致性或误差界。

## 2. 当前实现的阻塞项

### 2.1 `gain` 与终止损失不一致

`m1_dynamic_jev.py` 当前用每个 tool feedback 的 correctness 计算 `baseline_loss` 和 `after_loss`。这在两个 provider 都成功时等同于“第二个输出替换第一个输出”的错误差；一旦出现失败或拒答，就不再等同于终止损失。实验前必须冻结：

$$
L^{\mathrm{task}}=\mathbf 1[\text{wrong answer}]
 +\lambda_{\mathrm{abs}}\mathbf 1[\text{abstain}],
\qquad
U=-L^{\mathrm{task}}-c_{\mathrm{call}}\,n_{\mathrm{calls}}.
$$

并由同一个终止规则计算 $L_i^0,L_i^p$；调用成本只在 utility 或 gate 阈值中扣一次。若第一版不研究拒答，必须把规则固定为“失败视为错误、第二个成功输出直接回答”，并在论文中收窄主张。

### 2.2 当前特征不是条件 gain

`VerificationGainState` 只使用 `bias + provider one-hot`，没有首 provider 的返回标签、状态、分子表示或历史内容。因此当前 M1/M2 学到的是带共享 bias 的 provider 边际平均 gain，不是 $\tau_p(\mathcal F_i)$，也不是动态 JEV。

这带来一个直接错误：某个 provider 有一条正 gain 后，共享 bias 会让尚未被观测的 provider 也得到非空 estimate；这和文档中“冷启动时未知 provider 只探索、利用分支 STOP”的合同不一致。进入实验前二选一：

1. 第一批只声称 provider-marginal contract，把 bias 去掉，并对没有 provider-specific 支持的候选返回 unknown；
2. 真正加入 context feature，并显式定义共享先验和不确定性，不能再把它当作未知 provider 的零成本估计。

推荐先做 1，再做 2。这样理论 target 与 runner 是一致的。

### 2.3 M2 只有点估计，没有安全门

当前 gate 使用

$$
\widehat g(p)>c_{\mathrm{call}},
$$

没有置信半径；默认 `call_cost=0` 还使任何单条正反馈都可能打开利用分支。要测试“可靠复核”，至少改成

$$
\operatorname{CALL}(p)
\Longleftrightarrow
\widehat g(p)-r_p(\delta)>c_{\mathrm{call}},
$$

其中 $r_p$ 来自记录的 IPW 权重和有效样本量。第一版可以用保守的 self-normalized empirical-Bernstein 半径；不能把当前点估计称为 risk gate。

### 2.4 overlap 没有被配置强制保证

代码允许 `exploration_probability=0` 或 `1`。这样某些 provider 或 STOP 动作的 propensity 为零，IPW/OPE 没有支持。要估计 provider gain，至少固定 $0<\epsilon<1$，并记录

$$
\eta_{\min}=\min_{i,p:\,p\in C_i}\eta_i(p\mid\mathcal F_i).
$$

如果还要估计完整 STOP/调用策略的 utility，STOP 也必须有正支持；否则只报告被调用 provider 的 gain，不报告无偏 policy value。

### 2.5 延迟与遗忘不能混在第一批

当 $\rho=1$ 时，延迟反馈的更新是加法且可交换，先后顺序只影响何时可用；这是当前最干净的合同。若设 $\rho<1$，必须明确按决策时间还是反馈到达时间衰减，并保证同一时间步的事件有确定顺序。第一批实验固定 $\rho=1$，不要同时声称 drift tracking。

## 3. 进入 Nebula 前的最小验证顺序

### A. 本地数学/合同测试

用合成有限总体构造每个 provider 的潜在 $L^0,L^p$，检查：

1. 经验 IPW 估计在已知随机策略下收敛到全量 oracle gain；
2. 未观测 provider 不会因为共享 bias 被错误地标成已知；
3. failure/abstain 的 $L$ 与代码中的 `gain` 一致；
4. 每个被使用的 action 都有正 propensity，重复释放不会重复更新；
5. gate 的 LCB 在样本不足时保持 STOP。

### B. 冻结的本地 BBB 回放卡

- 固定首 provider、候选菜单、终止规则和损失系数；
- 随机 probe 使用固定 $\epsilon\in(0,1)$，至少 10 个 seeds；
- 训练流与确认流按 entity 划分，确认流不参与方法选择；
- 用完整缓存只在 evaluator 侧计算 counterfactual oracle $L^0,L^p$，不能泄露给 learner；
- 主指标：prequential terminal loss、calls/episode、gain MAE、gate regret、有效样本量和 propensity 最大权重；
- 失败条件：IPW 误差不收敛、未知 provider 被利用、LCB gate 的调用伤害高于 always/never 对照。

### C. 远程只提交一条 Nebula 队列

只有 A、B 通过后，才在 Nebula 建一条任务：一个冻结配置、一个 train/evaluation split、一个结果目录和完整 JSONL 日志。远程任务用于确认规模与真实运行时，不用于在未修正的 M2 上盲目调参。

## 4. 审查结论

当前 M1/M2 已证明的是：延迟队列、exactly-once feedback、propensity 记录和小状态更新可以运行。它们还没有证明动态条件 gain、可靠 gate 或实时 JEV。最小可取的下一步是先把损失合同和冷启动支持修正，再做本地 oracle/IPW 对照；在这之前提交 Nebula 会把实现问题和方法效果混在一起，实验费用很难解释。
