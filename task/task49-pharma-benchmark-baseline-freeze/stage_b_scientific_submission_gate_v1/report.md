# Stage-B scientific submission gate v1

日期：2026-10-06  
状态：**在声明的一次调用、fresh BBB provider、selected-only delayed feedback 和 controlled correctness-drift 范围内通过；不是整篇三层 Agent 论文的最终通过。**

## 这次真正闭合的门

1. `E6` 不再是空白：四个 checkpoint-bound provider 在 157 个 scaffold-disjoint candidate 上完成一次自然 fresh confirmation，240 条 stream 独立回放通过。
2. 动态主张有了可计算的压力场景：中点后翻转 fit-best provider 的 selected correctness，120 条 stream 独立回放通过。
3. 不再把旧的全局 reliability factor 说成动态效果：自然 confirmation 中 MIRA-Relative 与 Frozen 都是 89.81%，差值 0。
4. 新的 context-relative residual 有清楚的冷启动不变量：$\Delta_0=0$，先验和 Frozen 完全相同；只在 selected feedback release 后更新。

## 结果判断

- 自然 fresh menu：MIRA-Relative `89.81%`，Frozen `89.81%`，paired scaffold CI `[0.00, 0.00]` pp。这个结果通过稳定性边界，不通过“平稳环境自然增益”主张。
- controlled correctness drift：MIRA-Context `87.56%`，Frozen `73.38%`，差 `+14.18` pp，scaffold bootstrap 95% CI `[+9.61,+19.03]` pp；Matched Online-SGD 为 `+10.66` pp，Greedy LinUCB 为 `+7.77` pp。这个结果支持“selected-only delayed outcome 可以驱动 provider ranking 在已声明 drift 下恢复”的机制主张。

## 仍然不能写进当前主张的内容

- 受控翻转不是自然分布漂移，不能写成线上环境已验证；
- 157 条 natural holdout 没有证明 MIRA 在 stationary fresh menu 上胜过 Frozen；
- post-call verifier、拒答、两次调用和主 Agent 训练尚未进入这一 gate；
- hERG/AMES 和 33-provider full model-fresh menu 仍是明确边界。

因此，这一 gate 允许论文进入“可靠工具路由 + 延迟 selected-only adaptation + controlled drift recovery”的结果填充阶段。若正文继续声称“端到端 Agent 普遍提升”或“自然 drift 泛化”，仍然不通过。
