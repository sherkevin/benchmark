# MIRA 与 JEV 判别式模型、上下文老虎机、实时轻量更新的融合

日期：2026-10-05  
状态：候选实现说明；不是生效决议，不把 JEV 作为论文方法名。

## 结论

MIRA 可以干净地融合三类能力，但前提是职责分层：

1. JEV 判别式模型负责估计“当前任务调用某个 provider 后，结果是否可靠，以及已有证据下它是否有信息价值”；
2. 上下文多臂老虎机负责在候选 provider 之间做带探索和调用成本的选择；
3. 实时轻量训练负责在 correctness release 后更新低维 adapter、provider residual 和 runtime hazard；
4. stop/accept/refuse/continue 是上层有限视界 planner 的动作，不把拒答逻辑强行塞进 bandit arm。

这样 JEV 是预测器，bandit 是选择器，online adapter 是学习器，VOI planner 是证据决策器。论文内部继续称整体方法为 MIRA；JEV 只作为功能参考或组件说明。

## 数学接口

### Label contract

JEV 的监督标签来自独立 truth 和工具返回，不来自 bandit 本身：

- $y_i$：分子/端点的独立 truth；
- $o_{i,a}$：provider $a$ 的 0/1 output；
- $s_{i,a}$：`ok/error` runtime status；
- $r^{\mathrm{chem}}_{i,a}=\mathbf{1}[o_{i,a}=y_i]$，仅在 `ok` 时存在；
- $r^{\mathrm{run}}_{i,a}=\mathbf{1}[s_{i,a}=\mathrm{ok}]$。
- 端到端 bandit/task-success 标签是 $r^{\mathrm{sys}}_{i,a}=r^{\mathrm{run}}_{i,a}r^{\mathrm{chem}}_{i,a}$；在 runtime error 时它为 0，但 $r^{\mathrm{chem}}$ 仍是 null。

逐格来源是 `task/task49-pharma-benchmark-baseline-freeze/pharma_label_contract_v1/labels.jsonl`。冻结输入中的 legacy `system_success` 只作为 $r^{\mathrm{sys}}$ 的原始核对字段，不能代替 runtime status，也不能在 error cell 上解释成 chemical correctness。

JEV correctness head 学 $P(r^{\mathrm{chem}}=1\mid h,E,a)$，runtime head 学 $P(r^{\mathrm{run}}=1\mid h,E,a)$；answer head 学 $P(y\mid h,E)$。错误化学结果、runtime failure、拒答和 bandit reward 是不同层级的标签，不能用一个 0/1 混合。

冻结 JEV backbone $f_{\theta_0}$，对当前任务和当前已观测证据输出 provider score：

$$m_{a,t}=f_{\theta_0,\delta_t}(h_t,E_t,a),\qquad u_{a,t}=\mathrm{uncertainty}(h_t,E_t,a,L_t).$$

$m_{a,t}$ 是可靠性或 expected utility 的预测均值，$u_{a,t}$ 是由 bandit adapter 的 posterior/precision 得到的不确定度。bandit 只在 `CALL(a)` 候选中选择：

$$a_t=\arg\max_{a\in A_t}\left[m_{a,t}+\beta_tu_{a,t}-\kappa_a\right].$$

如果当前已有结果，JEV 的输入包括 $E_t$；因此第二次调用可以自然地区分“主要用于确认当前答案”和“主要用于提供互补证据”。不需要另外训练两个 action classifier。

真实 selected outcome 在 release tick 到达后，才更新低维参数。化学错误与 runtime error 分开进入 utility：

$$R_t=r^{\mathrm{sys}}_t-\lambda_{\mathrm{chem}}\mathbf{1}[r^{\mathrm{run}}_t=1\land r^{\mathrm{chem}}_t=0]-\lambda_{\mathrm{run}}\mathbf{1}[r^{\mathrm{run}}_t=0]-\kappa_{a_t}.$$

其中 error cell 的 $r^{\mathrm{chem}}$ 是 null，不能作为化学错误标签更新 correctness head。收到 release 后才更新低维参数：

$$\delta_{t+1}=\delta_t-\eta\nabla_{\delta}\ell\left(f_{\theta_0,\delta_t}(h_t,E_t,a_t),r_t\right).$$

实际实现可以使用 local-MAP、diagonal-precision 或 rank-one online Newton；冻结 backbone，不做全量反向传播。单个事件的更新复杂度为 $O(d)$。

## 冷启动与实时更新

### 冷启动

- 用 fit-only provider panel 和 independent truth 训练/校准 JEV；
- 用 JEV 的初始均值初始化 bandit prior；
- 用 fit-only 的样本数、残差方差或 Hessian 初始化 uncertainty；
- 使用固定的结果无关探索率和 family-balanced audit schedule。

### 实时阶段

- 当前工具返回后：只更新当前任务的 evidence posterior，不更新跨任务参数；
- correctness release 后：更新被选 provider 的 adapter 和 runtime hazard；
- 只有 paired audit 同时得到两个 provider 的结果时，才更新 provider relation/JEV joint component；
- 未选 provider 没有 feedback 时，不能伪造它的 online update。

这解决了普通老虎机的两个问题：奖励延迟，以及 provider 之间并非独立。JEV/JointEvidence 表达相关结构，bandit 只负责在这个预测面上探索和选择。

## 与上层证据决策的关系

planner 先比较三个动作：

- `ACCEPT`：直接使用当前 posterior 的答案；
- `REFUSE`：支付拒答成本 $\rho$；
- `CALL(a)`：支付调用成本 $\kappa_a$，并计算下一次证据的预期终止损失。

bandit 只在 `CALL(a)` 内部选择 provider。这样拒答不会被当成一个普通 provider，也不会出现“bandit 为了降低 regret 永远调用工具”的问题。

## 必须保留的控制组

- Frozen JEV + Frozen bandit：无在线更新；
- Independent Evidence + 同一 bandit/planner：去掉 provider 相关结构；
- JEV + no exploration：检查收益是否来自探索；
- JEV + ProbeNoUpdate：同探索、无更新；
- fixed cascade：同平均调用预算；
- Best Fixed、Random、direct-property control。

所有比较必须共享 backbone、fit panel、menu、delay、call cost、calibration 和 exploration schedule。

## 能否“解决问题”的判断

**从方法表达能力看，可以。** 三者组合后覆盖了：

- 首次选择最合适工具；
- 解释当前结果；
- 决定验证、互补、拒答或继续；
- 根据 delayed outcome 实时修正工作范围。

**从科学证据看，目前还不能说已经解决。** 当前真实 BBB 结果只证明联合 likelihood 有信号，posterior calibration 仍需修复；因此下一道闸门必须是 matched H=2 utility，而不是直接上 A800 或把 JEV score 当成方法提升。

## 实现顺序

1. JEV adapter 输出均值、uncertainty、failure risk；
2. delayed contextual bandit kernel，先做 H=0；
3. H=1 exact VOI planner，加入 accept/refuse/call；
4. selected-only release 更新和 paired audit 更新；
5. Independent/Frozen/fixed cascade controls；
6. CPU replay 通过后，再做 Nebula A800 和 agent wrapper。
