# Action-supported transient/persistent replay：指标审查与 v2 预注册建议

日期：2026-09-30  
状态：只读审查；不改主方法，不重算已有结果，不启动新实验。

## 结论

`action_supported_report.md` 的事件注入和配对方式是可用的：每个
`(method, fold, seed, delay)` 都先由 event tick 前的 no-event prefix 冻结
target，再重放 transient 或 persistent failure。因此 event stream 与 control
stream 的 target 来源一致，跨方法的 raw post accuracy 不会被误读成同一干预
下的排名。

当前指标有一个需要在下一轮运行前修正的混淆：`post` 同时包含了 event tick
被强制置为 failure 的 terminal 和 failure 之后的策略行为。transient 中
Frozen、streak-only 和 relative 都下降约一个样本的准确率，这个下降是注入
本身造成的，不能用来判定恢复能力。现有 `post_recovery=tick+1:` 已排除了
注入样本，但仍把反馈释放前的盲区和释放后的可适应区混在一起；delay 变化时，
这个区间的含义不同。

因此下一轮只需要把一个 `post` 拆成三个固定量：注入 terminal 损失、反馈
释放前的恢复段、反馈释放后的恢复段，并用按 fold 聚类的 paired bootstrap
给出不确定性。已有 v1 的 raw receipt、v1 数字和“原条件会把必然注入损失
误判为失败”的记录全部保留，不回写成 v2 结果。

## 1. 固定的配对单位与符号

每个 stream 由 `(method, fold, seed, delay, kernel)` 唯一确定。`kernel` 为
`transient_failure` 或 `persistent_failure`。target manifest 已由 event
tick 前 prefix 生成；同一坐标的 event stream 与 no-event control 共用
`event_tick=T`、episode 顺序、feature snapshot、fit prior、menu 和
`delay=d`。

令一个 stream 的 evaluation 长度为 $N$，事件 tick 为 $T$，反馈释放 tick
为 $R=T+1+d$。这是当前 queue 的固定语义：在 tick $R$ 开始决策前，
`decision_tick=T` 的 failure feedback 已被消费。令 $Y_t=1$ 表示该 tick
的 selected terminal 在 evaluator 中正确，失败 terminal 的 $Y_t=0$。

对 event stream 与同坐标 control 分别记为 $Y_t^{E}$ 和 $Y_t^{C}$。所有
准确率都先在一个 stream 内求均值，再在 10 个 `(fold, seed)` slot 上等权
平均；另外可给出按 molecule 加权的 pooled 数字作为附录，不用它替换主
估计量。

## 2. 三段指标

### 2.1 注入 terminal 损失

事件 tick 的单次强制失败是一个独立的环境代价：

$$
L_{\mathrm{inject}}=Y_T^{C}-Y_T^{E}.
$$

在 action-supported 协议中，$Y_T^{E}=0$；$Y_T^{C}$ 从同坐标 no-event
control 读取。它回答“如果策略在事件时刻作出该动作，一次被强制失败损失了
多少”，不回答策略是否恢复。transient 的 `post` 结果必须同时报告
`L_inject`，否则读者会把一个必然的单点损失当成在线方法的副作用。

### 2.2 反馈释放前的盲区

事件后输出已经恢复（transient）或仍然退化（persistent），但事件反馈还
没有到达。定义

$$
W_{\mathrm{blind}}=\{T+1,\ldots,R-1\}.
$$

当 $d=0$ 时该集合为空，结果记为 `NA`，不能填零。对任一 kernel 和方法
报告

$$
\Delta_{\mathrm{blind}}=
\operatorname{mean}_{t\in W_{\mathrm{blind}}}Y_t^{E}
-\operatorname{mean}_{t\in W_{\mathrm{blind}}}Y_t^{C}.
$$

它只测延迟造成的不可适应窗口，不能归因给更新规则。

### 2.3 反馈释放后的恢复段

定义

$$
W_{\mathrm{adapt}}=\{R,\ldots,N-1\}.
$$

当该集合非空时，主恢复差值为

$$
\Delta_{\mathrm{adapt}}=
\operatorname{mean}_{t\in W_{\mathrm{adapt}}}Y_t^{E}
-\operatorname{mean}_{t\in W_{\mathrm{adapt}}}Y_t^{C}.
$$

transient 的 `\Delta_adapt` 衡量一次偶发失败是否造成额外的长期损失；
persistent 的 `\Delta_adapt` 衡量收到失败反馈后能否离开退化 target。若
窗口为空，记 `NA`。`post_recovery=T+1:` 可以保留作描述性旧指标，但不能
替代这个按 release tick 切分的主量。

## 3. 行为指标

准确率之外，每条 stream 记录两项可解释的动作量：

1. **release 后 target exposure**

   $$
   E_{\mathrm{target}}=\frac{1}{|W_{\mathrm{adapt}}|}
   \sum_{t\in W_{\mathrm{adapt}}}{\bf 1}[a_t=\mathrm{target}].
   $$

   分母为空时为 `NA`。transient 主要看 exposure 是否在恢复后回到自身
   no-event 水平；persistent 主要看 exposure 是否在 failure release 后
   下降。

2. **release-to-switch**

   $T_{\mathrm{switch}}$ 是 $t\ge R$ 时第一次满足
   $a_t\ne\mathrm{target}$ 的 tick，若一直没有切换则右删失在 $N$。报告
   `N-R` 的删失值和删失比例；不把“未切换”当成任意一个更大的有限数。

这两项只描述 selected action 和反馈边界，不读取未选 provider 的 output 或
truth。persistent 的 failure count、首次 block tick/reason 作为机制诊断，
不替代 accuracy 主指标。

## 4. 配对不确定性

对每个 `(kernel, method, delay)`，先计算 10 个 paired slot 的
`L_inject`、`Delta_blind`、`Delta_adapt` 和 `E_target`。主区间采用
**按 scaffold fold 聚类的 paired bootstrap**：每次重采样 5 个 fold，保留
被抽中 fold 的两个 seed，再对 10 个 slot 等权求均值；重复 `B=20,000` 次，
使用 NumPy `PCG64`。随机种子固定为

$$
20261000+1000\,\mathbf{1}[kernel=\mathrm{persistent}]
+100\,method\_id+10\,delay.
$$

报告 percentile 95% 区间。跨方法比较时，先在同一 `(fold, seed, delay)`
上做差，再使用同样的 fold-cluster bootstrap；不把 10 个 slot 当作 10 个
独立分子，也不对两个 seed 做独立 cluster。只有 5 个 scaffold cluster，
区间仅作配对不确定性描述，不据此声称正式显著性。

## 5. 最小 v2 主表

每个 kernel、method、delay 只需要以下列：

`L_inject`、`Delta_blind`、`Delta_adapt`、`E_target`、`T_switch` 的点估计
和 paired 95% 区间；另列 `n_exposed`、`n_adapt_window_empty`、failure
count 和 block reason。`post` 与 `post_recovery` 保留在原始结果表，标明
它们是历史描述性量。

矩阵和方法不变：四个方法、5 folds、2 seeds、delay `{0,1,4}`、两种 event
kernel，共 240 条 event stream；no-event control 继续复用已冻结的 unified
receipt。只需扩展分析和独立 verifier 读取 `due_tick/release_tick`，不重训、
不调参、不改 MIRA 更新式。

## 6. v2 证伪条件

以下条件在运行前固定，且只针对新的估计量：

1. 若 relative 在 transient 的 `Delta_adapt` 在至少两个 delay 低于
   $-1$ pp，说明一次偶发失败在反馈到达后仍造成可见的额外损失；当前
   “偶发失败不引起长期切换”的说法不成立。单独的 `L_inject` 触发不能
   算作该条件。
2. 若 relative 在 persistent 的 `Delta_adapt` 在至少两个 delay 比
   streak-only 低超过 1 pp，说明 relative correctness factor 没有保留
   原有 availability hard-switch 的恢复能力。
3. 若 persistent 中 relative 的 `E_target` 比 matched 高至少 5 pp，且
   matched 的 `Delta_adapt` 在同一 delay 比 relative 高至少 1 pp，则把它
   解释为独立 runtime-failure response channel 的缺口；不能用 chemical
   correctness ledger 声称已经解决。
4. 任一 manifest target、event terminal、release clock、selected-only
   boundary、state/version 或 immutable source/snapshot hash 复核失败，整批
   只作为 engineering failure，不解释 accuracy。

若前两条均未触发，只能说明该候选通过这一个 CPU 机制门；仍不打开
confirmation。无论通过与否，v1 原始收据和 v2 分段结果都必须保留。

## 7. 与已有 v1 结果的关系

v1 的 `post` 数字和 `post_recovery` 数字不删除、不改名。它们已经说明：
transient 的非学习控制下降约一个注入样本，persistent 下 relative 明显好
于 Frozen 但落后 matched。v2 只是把必然的注入 terminal、反馈盲区和反馈
后的策略行为拆开，使下一轮结论可被审查；不能事后用 v2 估计量把 v1 写成
原先预注册的“通过”。

本审查不改变主方法、benchmark、baseline 或最终 claim；它只固定 recovery
指标的定义和不确定性计算，避免在下一轮看到结果后再选择分母。
