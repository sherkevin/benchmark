# task32 — AAMAS2027 Problem Formulation Closure

> 状态：⚡️进行中

## 目标

在 Task31 已冻结 benchmark / baseline / protocol 后，确定论文的严格数学问题，不直接设计算法。

需要回答：

1. 当前问题与 contextual bandit / pure exploration / treatment-effect policy learning / selective prediction / active level-set estimation / safe policy improvement 的边界是什么；
2. 哪些部分已经被现有文献覆盖，不能作为 novelty；
3. 在本仓冻结 feedback model 下，最小但真实的 unresolved formulation 是什么；
4. 这个 formulation 是否与三套 main benchmark 的真实协议一致。

## 验收标准

- 邻近领域比较有一手文献依据；
- 写出 observation / action / feedback / target / risk 的统一数学定义；
- 明确不依赖“传统 bandit 只优化累计 reward”这一过强说法；
- 给出至少一个可证伪的研究假设；
- 本任务结束前不根据实验结果改 Task31 冻结协议。
