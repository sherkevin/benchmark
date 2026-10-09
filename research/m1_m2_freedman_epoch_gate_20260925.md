# M1/M2 Freedman 半径与批次 gate

**日期：** 2026-09-25  
**状态：** Superseded by ADR-0017 and the v5 safe-overlap implementation. The numerical results below are retained as historical exploratory runs only.

## 后续复核结论（2026-09-25）

这份记录里的结果不能作为方法确认。复核发现三处问题：利用分支命中当前最优 provider 时旧代码记录了探索分支 propensity；Freedman 反解少了平方根内的线性项；运行中观察到的 `V/B` 被直接当成确定界使用。旧 coverage、BBB gate 和 accuracy 数字因此全部降级为 exploratory。

当前代码改为：记录完整 mixture propensity；用策略声明的确定性 `pi_min` 计算 `V_bar`、`B_bar`；使用正确的 Freedman 反解，并把 alpha spending 分到 look 和 provider。可复核的设计决议见 `docs/adr/0017-deterministic-overlap-bound-for-online-gain-gate.md`。Nebula 任务若提交，只能验证冻结回放的代码和环境，不作效果结论。

## 为什么继续改

HT 估计解决了 self-normalized IPW 的随机分母问题，但上一版每个 episode 都消耗一次 alpha，722 个分子上 gate 全程不打开。这个实现虽然保守，却没有形成可用的实时决策。

## 半径

令 $D_i=I_i g_i/\pi_i-g_i$。在 $g_i\in[-1,1]$ 下，条件方差满足

$\operatorname{Var}(D_i\mid\mathcal F_i)\le v_i=\max(0,1/\pi_i-1)$，

并且 $|D_i|\le b_i=\max(1,1/\pi_i-1)$。令 $V=\sum_i v_i$、$B=\max_i b_i$，代码使用两侧 Freedman 界

$r(\delta)=\frac{\sqrt{2V\log(2/\delta)}+B\log(2/\delta)/3}{N}$。

相较于把 $b_i^2$ 直接相加的 Hoeffding 界，这个式子利用了 Bernoulli inclusion 的条件方差，propensity 较小但仍有正支持时更紧。

## 批次 gate

gate 每 $K=64$ 个 episode 才重新计算一次；批次内冻结上一个 gate 的 provider 和开关，但仍按 $0<\epsilon<1$ 的 mixture 做探索。第 $k$ 个批次使用

$\delta_k=\delta\frac{6}{\pi^2 k^2}$。

因为 gate 只在确定的批次边界重新查看一次，$\sum_k\delta_k\le\delta$，这个 alpha spending 不再按每个 episode 过度消耗。批次内的随机 probe 继续给每个候选正的 inclusion propensity。

## 合成验证

1000 个 seed、真实均值 $0.6/-0.1$、$\pi=0.5$ 的有界流上，Freedman 版本的覆盖率为：

| episodes | good coverage | bad coverage | good MAE | bad MAE |
|---:|---:|---:|---:|---:|
| 100 | 0.976 | 0.959 | 0.1033 | 0.1125 |
| 500 | 0.970 | 0.951 | 0.0461 | 0.0510 |
| 2000 | 0.971 | 0.951 | 0.0222 | 0.0254 |

日志：[m1_m2_theory_synthetic_20260925_v6](/Users/jingwu/work/benchmark/experiments/logs/m1_m2_theory_synthetic_20260925_v6)。

## BBB 冻结确认

在 722 个可评分分子上，首 provider 为 CNN，候选 Morgan/MPNN，反馈延迟 1，10 个 seed，$\epsilon=0.8$、$K=64$、调用成本 0：

- Morgan oracle 平均增益：$0.2742$；HT MAE：$0.0553$；
- MPNN oracle 平均增益：$0.1731$；HT MAE：$0.0225$；
- gate 平均打开 $376.4/722$ 个 episode；平均调用 $1375.0/722=1.904$；终止准确率 $0.8083$；
- 10 个 run 全部可评分、propensity 全正、队列清空。

静态冻结参考为：never 的准确率 $0.6039$、平均调用 1；always Morgan 的准确率 $0.8781$、平均调用 2；always MPNN 的准确率 $0.7770$、平均调用 2。这个确认只证明状态、估计和 gate 在本地回放上能工作；它不是独立测试集上的方法效果结论。

日志：[m2_bbb_ht_confirmation_20260925_v7](/Users/jingwu/work/benchmark/experiments/logs/m2_bbb_ht_confirmation_20260925_v7)，静态参考：[bbb_static_baselines_20260925_v1](/Users/jingwu/work/benchmark/experiments/logs/bbb_static_baselines_20260925_v1)。

## 进入远程前还剩的闸门

当前版本已经有一条可以尝试的可运行方案：固定 provider-marginal HT 状态、Freedman 置信半径、64-episode 批次 gate、严格探索。还没有 context encoder，因此不能把它写成最终动态 JEV。下一步才是提交**一条** Nebula 复现实验，目的限定为检查远程环境和规模化日志，不在远程调参或同时开多条线。
