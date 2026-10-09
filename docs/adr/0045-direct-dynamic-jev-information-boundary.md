# ADR-0045：收束 direct 动态 JEV，转向可观测的验证流程

状态：部分被 `docs/决议/0006-目标主张保持与方法新命名.md` 替代

日期：2026-09-28

## 背景

当前 direct task 在调用前只提供分子、性质、provider 菜单和已经释放的 selected-only 二值反馈；未选 provider 的 evaluation 标签不可见，round1/round2 也没有证明真实工具版本漂移。已有真实开发结果覆盖普通 Online/Replay-SGD、BayesianRouter-style、PILOT-adapted、provider transfer、context memory、propensity/confidence-bin residual 和 Laya 静态表示，均没有稳定超过 train-only Best Fixed。

## 决定

不再把“再换一个在线 head”作为主方法搜索方向。原决定中“暂停 direct pre-call 动态 JEV 主创新声明”的目标降级部分，已由 `docs/决议/0006` 替代：MIRA 的目标 claim 保持，后续继续修复和验证。这里的具体负结果、信息边界和“不靠调参挽救”的约束仍然有效；DG-SafeJEV 仍是一个受限 CPU 控制，而不是 MIRA 的最终实现。

研究主线转为两阶段工具流程：先用 fit-only Best Fixed 做一次调用，再根据调用后 numeric confidence 决定接受、拒答或追加第二工具。该阶段有额外可观测信号，且 confidence-gated second-tool replay 已在相同 fallback 数量的随机对照下提高 pooled accuracy。

## 证据

- Best Fixed pooled accuracy `0.915512`。
- fit-only confidence verifier accepted accuracy `0.961606`，实际接受数匹配的随机拒答 `0.916230`。
- confidence-gated second-tool replay pooled accuracy `0.923823`，平均调用次数 `1.206`；相同 fallback 数量随机追加 `0.904432`，4/5 folds 不低于 Best Fixed。
- 直接动态更新的完整负结果和信息边界见 `task/task49-pharma-benchmark-baseline-freeze/fresh_method_review_20260928.md`。

## 后果

论文仍保留 direct-selection benchmark 和现代 baseline 的完整比较，避免把验证结果冒充工具选择结果。下一步只在 CPU 上验证动态 verifier 的 selected-only 延迟风险状态；只有它在相同调用成本下超过静态 escalation，才提交一条 A800 smoke。该裁定可被后续强证据 supersede，不能静默改写。

## 后续命名与目标说明

2026-09-29 起，论文目标方法统一使用工作名 **MIRA（Menu-aware Incremental Reliability Adaptation for Scientific Tool Calls）**。本 ADR 中的 JEV 是历史功能参照，不是论文方法名。当前候选失败不会自动撤销 MIRA 的目标；每个后续候选仍需单独 method card、匹配基线、kill criterion 和恢复证据。
