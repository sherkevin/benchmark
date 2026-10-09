# task27 — G3 Baseline-only New-Arm Cold-Start Replay

> 日期：2026-09-21  
> 对应：TODO T-283 / 实验矩阵 v2 的 G3、T3 前置  
> 状态：⚡️ 进行中  
> 权威矩阵：`docs/AAMAS2027_完整实验矩阵_DynamicToolRouting_v2.md`

## 1. 本任务只回答一个问题

在 **Ours 尚未设计** 的前提下，先验证：

> 当一个零历史的新 provider/tool 加入已有 capability pool 时，标准 bandit / contextual-bandit baseline 是否会出现可测量的 cold-start regret，并且随着真实 behavioral feedback 累积逐渐恢复？

如果这一现象在 LLMRouterBench / ASlib 上都不可测，就不值得继续设计“快速新工具 onboarding”方法。

## 2. 信息条件冻结

主协议不提供新工具的：
- 历史 reward；
- tool-side competence metadata；
- 人工写的“这个工具擅长什么”描述；
- tool ID 对应的先验性能。

系统只知道：它是当前 capability 下新加入的一个 arm。

因此 B=0 时，新工具的个体 specialization 在统计上不可识别；主比较关注收到 behavioral outcomes 后的 adaptation speed。

## 3. 统一 reward

所有主 benchmark 都转为 [0,1]、越大越好：

- LLMRouterBench：原始 per-instance score；
- BBB：correctness ∈ {0,1}；
- ASlib：`u = 1 - PAR10 / 12000`。

评测始终保留原生指标作解释，但在线 regret 用统一 utility。

## 4. Split / history / stream

### LLMRouterBench
- 每个 dataset 内 70/30 prompt split；
- incumbent arms 可见 70% history outcomes；
- held-out new arm 的 70% outcomes完全隐藏；
- 30% prompts 合并为 post-arrival stream；
- 每 seed 重新做 split + stream shuffle；
- 不输入 dataset ID 给 contextual baseline。

### ASlib SAT12-ALL
- 使用官方 `cv.arff` repetition=1；
- 每次一个 official fold 为 stream/test，其余 folds 为 history；
- incumbent solvers 可见 history outcomes；
- new solver 的 history outcomes隐藏；
- test fold 内做多个 stream-order seeds。

### BBB
- 5-fold Bemis–Murcko scaffold GroupKFold；
- train scaffolds = incumbent history；
- test scaffolds = post-arrival stream；
- new tool train outcomes隐藏；
- test fold 内做多个 stream-order seeds。

## 5. Context representation 冻结

为了不让“表示模型”成为新变量，本任务只用轻量、train-only representation：

- LLMRouterBench：prompt TF-IDF → TruncatedSVD(32) → L2 norm → bias；
- ASlib：官方 features → train median impute → StandardScaler → PCA(32) → bias；
- BBB：Morgan r=2 / 1024-bit → train TruncatedSVD(32) → L2 norm → bias。

所有 fit 只发生在 history/train contexts，不看 post-arrival reward。

## 6. Baseline-only pilot

本任务不实现 Ours。

先跑：
1. **Best Fixed**：只按 incumbent history 选全局均值最高 arm，新工具永不被主动采用；
2. **UCB1**：non-contextual；incumbent 由 history 初始化，新 arm n=0；
3. **Beta-TS**：bounded reward，Beta(1,1) prior；连续 reward 用 fractional pseudo-count update；
4. **LinUCB**：independent per-arm linear model，lambda=1, alpha=1；
5. **LinTS**：independent per-arm Bayesian linear model，lambda=1, v=1。

线性方法统一学习 centered reward `y=r-0.5`，使 zero-history prior 均值对应 0.5，而不是 0。

P-LinUCB/P-TS、ContextRoute、UCB-AA 在确认 cold-start 现象可测后再进入正式 T3 baseline matrix；本 pilot 不提前实现。

## 7. 评测指标

每个 held-out arm 都轮流作为 new arm，禁止 cherry-pick。

主指标：
- cumulative post-arrival regret；
- normalized post-arrival regret；
- average reward；
- new-arm selection rate；
- beneficial-adoption precision：`P(r_new > max_incumbent r | selected new)`。

诊断指标：
- Full Oracle；
- Incumbent Oracle（排除 new arm）；
- new-arm marginal value。

新工具冗余（marginal value=0）不是坏样本；正确 onboarding 行为应当是“快速拒绝/低使用”。

## 8. 重复次数

Pilot：
- LLMRouterBench：5 split/stream seeds；
- ASlib：全部 official folds × 3 stream-order seeds；
- BBB：5 scaffold folds × 10 stream-order seeds。

正式 T3 再统一为足够 seed 数 + 95% paired bootstrap CI。

## 9. Go / No-Go

G3 通过不要求某个现有 baseline已经表现很好，而要求：
1. new-arm arrival 确实产生可测的 early regret；
2. 不同 baseline 的 adaptation curve 可区分；
3. new-arm marginal value 既有“值得采用”也有“应快速拒绝”的 case；
4. 随 feedback 增加，至少一种 online baseline 对 new arm 的使用发生系统性变化。

若 1–4 不成立，则“快速 onboarding”不是当前 benchmark 上真实可测问题，停止设计 Ours。
