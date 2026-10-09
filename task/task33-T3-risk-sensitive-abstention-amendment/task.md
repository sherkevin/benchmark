# task33 — T3 Risk-Sensitive Abstention Baseline Amendment

> 状态：✅ 已完成

## 背景

Task31 冻结后，Task32 close-neighbor review 发现 Liaw & Plaut (AISTATS 2026) 的 risk-sensitive contextual abstention 与 frozen onboarding setting 高度接近。

这不是偷偷修改 Task31 baseline freeze，而是显式 amendment：先判断该方法是否在当前 benchmark/context/feedback 下可忠实适配，再决定是否纳入 main/appendix comparator。

## 目标

1. 核对论文与官方代码的数学假设、算法、超参和反馈语义；
2. 给出 SCCO -> abstention-on-residuals 的严格映射与不等价项；
3. 只用 frozen protocol 已允许的信息实现 faithful/mechanical adapter；
4. 在三套 main benchmark 上跑 sensitivity，避免用 test set 选一个好看的单点；
5. 判定：mandatory main baseline / appendix diagnostic / assumption-mismatch related work。

## 验收标准

- 官方代码 commit/hash 有记录；
- adapter 不访问 unchosen counterfactual reward；
- incumbent prediction 只能来自 frozen history-only policy；
- L、sigma、trusted-region 等无法从 protocol 正当获得的量必须做敏感性或明确判 assumption mismatch，禁止 test-optimal 调参；
- 输出代码、data JSON、report.md；
- 结论写回 Task32 后才能进入 Ours。

## 固定证据

- official repo: .srcprobe/abstention-bandits-simulations
- audited HEAD: 69994acc6679bdc368a26ed9d2612aac67db4fee
- paper: Liaw & Plaut, AISTATS 2026, Learning When Not to Learn: Risk-Sensitive Abstention in Bandits with Unbounded Rewards.
