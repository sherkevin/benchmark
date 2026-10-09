# M1/M2 理论闸门修正记录

**日期：** 2026-09-25  
**范围：** 只修正本地 M1/M2 合同；没有连接 Nebula，也没有启动远程实验。

## 为什么先修合同

上一版 runner 可以记录延迟反馈，但有三个地方会让代码里的量和理论目标不一致：

1. `bias + provider one-hot` 会让尚未观测的 provider 继承共享 bias；
2. `gain` 用 tool correctness 计算，遇到失败或拒答时不等于终止任务损失差；
3. M2 用点估计开门，没有把 provider 的样本量和估计不确定性放进 gate。

这三个问题必须在昂贵实验前修掉，否则远程结果无法判断是在测方法还是在测实现偏差。

## 修正后的量

对一个已经调用首工具的 episode，定义固定终止规则的任务损失：

$L(r)=0$，当工具返回且答案正确；

$L(r)=1$，当工具返回但答案错误；

$L(r)=\lambda_{\mathrm{abs}}$，当工具失败，终止动作是 abstain。

其中 $\lambda_{\mathrm{abs}}\in[0,1]$ 是 runner 的显式参数。第二次调用 provider $p$ 的观测增益为

$g_i(p)=L_i^0-L_i^p\in[-1,1]$。

因此 M1/M2 的 `baseline_loss`、`after_loss`、`gain` 使用同一个任务损失定义；调用成本不混入 gain，只在 M2 gate 中比较。

## 无共享 bias 的状态

每个 provider 使用一个 one-hot 坐标。对 propensity 为 $\pi_i$ 的已释放反馈，状态递推为

$G_p\leftarrow G_p+g_i(p)/\pi_i$，

$Z_p\leftarrow Z_p+1/\pi_i$，

$W^{(2)}_p\leftarrow W^{(2)}_p+1/\pi_i^2$。

provider 的 estimate 为 $\hat g_p=G_p/(Z_p+\varepsilon)$。如果该 provider 没有被观测过，$Z_p=0$，estimate 返回 `None`，不会从其他 provider 的反馈生成伪冷启动信号。

记录 $W^{(2)}$ 是为了保留 propensity 对有效样本量的影响：

$n_{\mathrm{eff},p}=Z_p^2/W^{(2)}_p$。

## M2 gate

第一版使用 bounded gain 的保守 Hoeffding 半径：

$r_p(\delta)=\min\left\{1,\sqrt{\log(2/\delta)/(2n_{\mathrm{eff},p})}\right\}$。

利用分支选择最大下置信界的 provider，并且只有在

$\hat g_p-r_p(\delta)>c_p$

时才调用。没有 provider-specific 支持时 radius 和 LCB 都是 unknown，利用分支 STOP；探索分支仍按预先记录的 propensity 随机 probe。

这个 radius 是第一版的安全合同，不声称已经是最终最紧的 IPW 置信界。后续若需要更高样本效率，可以替换 radius，但不需要改状态或 gate 接口。

## 代码与验证

- [m1_dynamic_jev.py](/Users/jingwu/work/benchmark/src/probe_headless/m1_dynamic_jev.py)：M1 v2；无共享 bias、任务损失、provider ESS。
- [m2_dynamic_jev.py](/Users/jingwu/work/benchmark/src/probe_headless/m2_dynamic_jev.py)：M2 v2；LCB gate。
- [test_dynamic_jev_m1.py](/Users/jingwu/work/benchmark/src/probe_headless/tests/test_dynamic_jev_m1.py)
- [test_dynamic_jev_m2.py](/Users/jingwu/work/benchmark/src/probe_headless/tests/test_dynamic_jev_m2.py)

本地合同与 Task36 回放回归：**157 passed**。测试没有调用新的模型或远程服务；没有生成新的预测结果。

## 还没有解决的事

这次修正只使 M1/M2 的 estimand、冷启动语义和 gate 输入保持一致。它没有证明 IPW 在 BBB 数据上的有限样本误差，也没有把 one-hot provider marginal gain 变成 context-conditioned JEV。下一道闸门应是合成潜在损失上的 IPW 收敛测试，再是冻结的本地 BBB train/confirmation split；两者通过后才考虑提交一条 Nebula 队列。
