# Baseline role roster v1

日期：2026-10-01  
状态：开发阶段角色合同；不改变 benchmark、不把 adapted 结果升格为严格主比较。

## 为什么要单独做这张表

当前 baseline 名单已经足够覆盖固定策略、静态路由、在线持续学习、现代 router 和
验证控制，但它们的输入表示、warm start、likelihood 和更新预算并不全相同。若把所有
accuracy 放进一张排名表，审稿人无法判断差异来自方法还是来自初始化和统计模型。

因此 roster 先固定**角色和允许的比较方式**，再决定是否补新的 strict receipt：

- `strict_main`：共同 hidden-64 query、fit-only prior、selected-only 时钟、delay 和预算；
- `adapted_separate`：确实运行过的现代方法，但适配/先验/likelihood 有明确差异，单独报
  自己的 paired effect；
- `audit_only`：没有完整 selected-only effect receipt 或任务 fidelity 合同，只保留源码/表示
  审计；
- `candidate`：MIRA 的机制候选，不冒充外部 baseline。

## 当前分层

| 层 | 方法 | 可以回答的问题 |
|---|---|---|
| strict main control | Frozen direct、Matched Online-SGD | 同一初始化下，静态控制与合法 logistic 在线更新是什么水平 |
| task anchors | Random、Best Fixed、Oracle | 任务是否有信号、强固定策略和 evaluator 上界在哪里 |
| candidate | MIRA-relative、MIRA-FamilyGuard | 结构化 reliability/family 状态是否比 controls 更好 |
| adapted modern | Greedy LinUCB、BayesianRouter、PILOT-adapted | 这些已运行的现代思路在明确适配合同下表现如何；不能混成 strict raw ranking |
| strict Gaussian | Greedy reward-ridge | fit-only binary reward prior + separately fixed Gaussian precision；已有 60-stream verified receipt，仍与 logistic stratum 分开 |
| separate static | MLP/BT+CLS static router | full-panel fit-only 静态表示/路由能力，不回答在线更新 |
| audit only | GraphRouter、Laya online | 输入语义、依赖或 selected-only update API 尚未闭合 |

机器 roster：`roster.json`。每行绑定唯一证据路径、`deployment_legal`、`comparison_allowed`、
`original_vs_adapted` 和排除原因；缺少 receipt 的方法没有被填成“已完成”。

## 对 Gate B 的影响

这张表关闭了“不同角色混排名”和“静态结果冒充在线 baseline”的口径问题，但不自动增加
Gate B 分数。当前仍保持 `11/16 HOLD`：

1. adapted modern baseline 的 Gaussian/logit likelihood 语义还需要一份合法合同；
2. Replay-SGD 与 strict common-prior 的字节、预算和恢复字段尚未重新核齐；
3. 所有主方法的统一成本、family macro、failure/unknown/refusal 和 paired uncertainty
   还没有一张完整主表；
4. GraphRouter/Laya audit-only 的范围被明确记录，但不能靠排除它们宣称“最近邻已经全部
   完成”。

下一步若补 Gaussian 强近邻，优先登记 fit-only reward-ridge：拟合正则和 posterior
precision 分开记录，Gaussian 预测均值不做隐式 clip；否则以 Matched Online-SGD 作为合法
logistic 强对照，保留 Greedy/Bayesian 现有 adapted 负结果。
