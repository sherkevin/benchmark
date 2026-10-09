# 调用后拒答与追加调用效用审计 v1：结果

日期：2026-10-01  
状态：development replay verified；不是 MIRA direct-selection 或 online-learning 结果；confirmation 仍关闭。

## 这轮回答什么

已有的 post-call confidence 结果只给了 accepted accuracy 和 average calls。这轮把错误答案、最终失败、主动拒答和调用数放进同一个预登记效用账，检查少答或追加调用到底在什么成本范围内值得。

这是调用后 verifier extension，沿用已冻结的 fit-only provider、阈值和五折 scaffold-heldout evaluation。它不改 pre-call selector，也不让拒答事件产生 chemical correctness 或 learner update。

## 协议与证据

- 输入：`fingerprint_diagnostic_v1/inputs.json`，722 个 BBB development 分子、33 个 provider、原五折；SHA-256 `a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`。
- 每折第一 provider、第二 provider、confidence 阈值和 action 都只由 fit 区确定；evaluation 只读取实际调用的 cached output。
- `system_success=0` 没有直接拿来当 wrong。runtime status、有效 output label、最终 grade 分开重建；中途 failure 只记事件，最终没有可用回答才进入 `failure`。
- refusal 的 policy receipt 不含 truth、`answer_correct`、`tool_feedback` 或 `grade`。这些字段只写到单独的 `evaluator_outcomes.jsonl`，并标记 `evaluator_only=true`；拒答本身的 `answer_correct` 保持 null。
- 同动作数 random 是 evaluator-side conditional null：它使用完整 evaluation 中 confidence policy 实际产生的动作数，只用于 matched-rate 诊断，不能当可部署 random baseline。

运行与恢复：

- policy receipts：`experiments/logs/task49_postcall_utility_audit_20261001/v3/policy_receipts.jsonl`（4,332 行）
- evaluator outcomes：`experiments/logs/task49_postcall_utility_audit_20261001/v3/evaluator_outcomes.jsonl`（4,332 行）
- 配置与汇总：`experiments/logs/task49_postcall_utility_audit_20261001/v3/config.json`、`summary.json`
- 独立恢复：`experiments/logs/task49_postcall_utility_audit_20261001/recovered_v3.json`，状态 `verified`
- 先行分母审计：`experiments/logs/task49_postcall_utility_contract_audit_20261001/v1/denominator_audit.json`，状态 `verified`

恢复器不导入 runner 的结果对象；它从相同 input/cache 重新构造 action、transcript、evaluator outcome、计数和 utility，并检查 policy read-set 没有 truth-derived 字段。

## 效用定义

在有独立真值的 $N$ 个 episode 上，unknown 不进入 utility 分母，另报 censoring。$W$ 是最终答错，$F$ 是最终没有可用返回，$R$ 是主动拒答，$C$ 是实际 provider 调用数：

$$U(\rho,\kappa)=-\frac{W+F+\rho R+\kappa C}{N}.$$

本轮展示完整预登记网格 $\rho,\kappa\in\{0,0.1,0.25,0.5,1\}$ 与 $\{0,0.01,0.05,0.1,1\}$，不看结果挑成本。中途 runtime failure 单独报告；若后续仍有最终答案，不重复计入 $F$。

## 结果

| policy | correct | wrong | final failure | active refusal | calls | runtime failure events |
|---|---:|---:|---:|---:|---:|---:|
| always-accept | 661 | 61 | 0 | 0 | 722 | 0 |
| confidence refusal | 551 | 22 | 0 | 149 | 722 | 0 |
| matched random refusal | 523 | 50 | 0 | 149 | 722 | 0 |
| always-refuse | 0 | 0 | 0 | 722 | 722 | 0 |
| confidence escalation | 667 | 52 | 3 | 0 | 871 | 3 |
| matched random escalation | 661 | 60 | 1 | 0 | 868 | 1 |

静态拒答相对 always-accept 避免了 39 个错误、拒答 149 个 episode；点估计 break-even 为 $\rho=39/149=0.2617$。相对 zero-coverage always-refuse，静态拒答的点估计边界为 $\rho=22/573=0.0384$。这只是固定 cohort 的描述性边界。

追加调用相对 always-accept 减少了 6 个最终坏结果（`wrong` 或最终 `failure`），额外调用 149 次，点估计 break-even 为 $\kappa=6/149=0.0403$。其中 wrong-to-correct 的救回为 28，wrong-harm 为 19，另有终止 failure 的变化；不能只看 accuracy 差。

在示例网格 $(\rho,\kappa)=(0.25,0.01)$ 下，utility 为：always-accept `-0.09449`，confidence refusal `-0.09206`，confidence escalation `-0.08824`，matched random refusal `-0.13084`，matched random escalation `-0.09651`。这些数字只描述本开发 cohort，不是最终业务成本结论。

scaffold-cluster paired bootstrap 5,000 次、seed `20261001`：

| 对比 | 固定审计点 | 均值差 | 95% percentile CI |
|---|---|---:|---:|
| confidence refusal − always-accept | $\rho=0.25,\kappa=0$ | `+0.00293` | `[-0.01456,+0.02218]` |
| confidence escalation − always-accept | $\rho=0,\kappa=0.01$ | `+0.00622` | `[-0.01149,+0.02466]` |

break-even bootstrap：拒答 $\rho^*$ 均值 `0.26669`，95% CI `[0.18502,0.37079]`；追加调用 $\kappa^*$ 均值 `0.04057`，95% CI `[-0.05000,0.13542]`。区间跨越 0 的地方不称稳定收益。

## 结论边界

1. confidence signal 确实能把错误集中到拒答集合，且在预设成本区域内存在 utility headroom；这补的是 post-call verifier 的 E5 错误/调用代价子项。
2. 两个主要 paired interval 都跨 0，不能写成稳定 utility superiority。尤其是追加调用的 break-even 区间很宽，不能据此申请 A800。
3. 这轮没有记录真实 provider wall-clock、更新耗时、内存或在线反馈，因此不关闭 E5 的完整实时成本要求；它不能替代统一 cost/budget 主表。
4. 这轮没有 selected-only online update，不能证明 MIRA 动态学习有效；也不能外推到 hERG、AMES、model-fresh 或 confirmation。

## 对验收标准的动作

这轮把 unknown/refusal 语义、拒答 utility、追加调用的错误归因和独立恢复补进 development evidence。Gate C 仍保持 `10/16 HOLD`：统一 E1–E5 主表、全部 baseline 的严格同协议闭合、多端点资格、完整动态事件 scope 和冻结 confirmation 仍未完成。不能用本轮 post-call development 结果替代这些缺口。

后续优先级：先把本轮 utility receipt 接入 E5 统一表；除非出现新的可观测反馈和清晰机制假设，不再调 confidence 阈值或重复做相同的 static verifier。GPU 与 confirmation 继续关闭。
