# ADR-0017：在线增益门使用确定性 overlap 半径

日期：2026-09-25

状态：Accepted

## 背景

M1/M2 用延迟到达的终止反馈估计候选 provider 的 verification gain。动作由当前状态决定，因而 inclusion propensity 也是自适应的。上一版把运行中观察到的方差项直接代入 Freedman 半径，并把半径裁到 1；这两点都不能作为严格的在线置信保证。M2 还有一个实现错误：探索抽到当前最优 provider 时，事件记录了探索分支概率，没有加上利用分支概率。

## 决策

1. 每个候选 provider 的事件 propensity 必须记录完整的 mixture probability：
   
   `pi = exploration_probability / K + (1 - exploration_probability) * I[gate selects provider]`。

2. 正式 gate 只使用声明在策略配置中的确定性下界 `pi_min`。对 `g∈[-1,1]`，令
   
   `v_bar = 1 / pi_min - 1`，`b_bar = max(1, 1 / pi_min - 1)`，`V_bar = N * v_bar`，`l = log(2 / delta)`，则使用
   
   `r = (b_bar*l/3 + sqrt(2*V_bar*l + (b_bar*l/3)^2)) / N`。

   这是把 Freedman 不等式反解后得到的保守半径。点估计投影到 `[-1,1]` 后，半径最多可裁到 2，不能裁到 1。

3. `delta` 按 look 和固定 provider 数量做 alpha spending。菜单改变会产生新的 look，不能复用旧 look 的预算。

4. `epsilon=0` 或 probe probability 为 0 的策略不进入 HT gate；所有注册菜单必须有正的、至多为 1 的 propensity。未解析样本先固定排除，不进入分析分母。

## 后果

半径比观察方差的经验版本更保守，早期 gate 可能不开；这是当前可证明性优先的取舍。后续若要恢复更紧的实时半径，需要单独实现 predictable-variance confidence sequence 或 peeling，并通过独立覆盖实验后再替换此合同。

Nebula 单任务只用于冻结回放的代码/环境 smoke test，不把它当作方法效果或 coverage 证据。
