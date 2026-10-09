# 独立三标准复核：方法 Gate 纠正（机制消融后更新）

- 日期：2026-10-07
- 复核对象：`research/experiment_section_style_20261006/three_standard_recheck_20261007.md`
- 主稿：`paper/aamas2027/main.tex`
- 复核者：独立只读审查；未修改主稿、未启动实验。

## 结论

原重检中的 Benchmark 分数仍可用于声明范围内的 routing gate，但方法侧的“46/60、H7 UNKNOWN”已经被后续独立回放和机制消融更新，应改为：

- **Benchmark + Baseline：** Gate A/B/C 仍分别为 15/16、13/16、14/16，限于四 fresh provider、一次调用、selected-only delay 和 controlled drift 的窄切片可以通过；
- **故事线：** 当前主稿的窄切片故事可以继续做盲读和文字复核，作者侧 85.5/100 只是 provisional；
- **方法论：** **H7 = PARTIAL/PASS_FOR_REPLAY**。完整 fresh-state replay 已经关闭“只能看到 opaque hash、不能独立恢复 state/action”的缺口；新机制消融把严格分数更新为 **52/60**。在当前四-provider、一次调用、controlled-drift 声明范围内，M5/M13 达到 3 分；论文主稿已回填机制表，paper-level gate 仍待第二位复核。

主稿已回填机制表，且 `paper_results_binding_audit_20261007.md` 返回 `verified`。因此当前剩余的是第二位审查者的独立重打分和 family/lineage 外推边界，不是数字来源缺口。

这不是“方法错误”的结论。现有证据足以证明事件合同、反馈时钟、同实现回放和 controlled drift 现象；它还不足以证明当前 MIRA-Relay 的 learned state 可以由独立实现恢复，也不足以证明 provider-local residual 是受控恢复的必要机制。

## 方法逐项纠正

| 项 | 原作者分 | 独立复核分 | 复核理由 | 最小关闭动作 |
|---|---:|---:|---|---|
| M1 | 4 | 4 | 对象、shape、case、schema 齐全 | 保持 |
| M2 | 4 | 4 | decision→termination→release→update、delay 和 exactly-once 有 receipt | 加入重启/乱序 mutation |
| M3 | 4 | 4 | selected-only、禁止字段和 event audit 齐全 | 保持 |
| M4 | 4 | 4 | chemical/runtime/unknown/refusal 分开 | fresh scored stream 的边界继续注明 |
| M5 | 3 | **3** | Local、Frozen、Shared-Credit、Relative 与现代 online controls 已在同一 fresh contract 对比；Local−Shared controlled drift 为 +7.05 pp，scaffold CI $[+3.11,+9.98]$ | 增加同族 provider 后再讨论 family sharing |
| M6 | 3 | 3 | 更新公式可计算，复杂度和残差边界有说明 | 增加适用条件/偏差边界 |
| M7 | 4 | **4** | v2 verifier 不导入 production runner/policy，独立重建 Morgan 特征、prior weight、full state、score/action，并逐事件核对可用 production hash | 后续代码扩展需维持同一 contract |
| M8 | 3 | **3** | v2 replay 逐事件重建 state→next-action，且 controlled action change 可复现；尚无自然 fresh panel 的净提升 | 在同一合同下补 mechanism ablation |
| M9 | 3 | 3 | update p95、state size、calls 已记录 | 补 K/d/pending 压力曲线 |
| M10 | 4 | 4 | feedback-adaptive scope 的 release→state→next-action 已验证；未声称自然非平稳 | 不扩大为 environment-nonstationary |
| M11 | 3 | 3 | $D\in\{0,1,4\}$、selected-only 和 paired comparison 齐全 | 补更完整的 propensity/偏差说明 |
| M12 | 3 | 3 | menu、feature、feedback、delay、budget 合同已写清 | 补 modern adapter fidelity 细节 |
| M13 | 3 | **3** | natural null、controlled rescue/harm、Local/Shared/Relative/Frozen paired CI 已完成；family/lineage 反事实仍不可识别 | 同族 menu 或显式 interface-null receipt |
| M14 | 4 | **4** | raw/config/hash/GPU/event audit 加上 v2 canonical full-state trace、逐事件 hash 核对和 reward/score mutation rejection 已齐全 | 未来 receipt 再补 terminal/final-state hash 字段 |
| M15 | 3 | 3 | 主张已限定到 endpoint/menu/delay/controlled event | 继续分开 selector/verifier/agent claim |

总分：**52/60**。方法标准要求 M1–M8、M10–M12、M15 至少 3 分；当前 declared scope 的方法证据达到门槛，但主稿尚未回填机制表，paper-level Gate 仍 HOLD，待整合后复核。H7 仍是“完整回放证据通过”；family sharing 与更广 agent claim 不在本次通过范围内。

## H7 的证据边界

`verify_fresh_context_replay_v2.py` 已经在不导入 production runner/policy 的前提下，从 sealed prior cache、confirmation manifest 和 receipt 逐事件重建 full state。natural/stress 各覆盖 30 streams、4,710 decisions、9,420 个可用 production state hashes，score 重算误差为 0；一条 reward 或 score mutation 都会被拒绝。历史 terminal 记录没有 state hash，summary 也没有 final-state hash，因此报告明确记录这两个字段缺失，不把缺失伪装成匹配。

这关闭的是当前 fresh receipt 的独立回放与证据链缺口，不等于机制必要性已证明。sealed fit cache、provider terminal output 和 evaluator reward 仍是 replay 输入，offline optimizer 与 provider inference 没有被重新运行。

## 关闭顺序

1. 把 `fresh_mechanism_ablation_v1/report.md` 的 Local/Shared/Frozen/Relative 结果回填主稿实验矩阵，并重新跑三份 paper-level 标准；
2. 在后续 fresh menu 中加入同族 provider，才讨论 family-sharing 或 independent-residual 的识别；
3. 未来 fresh receipt 直接保存 initial weight/feature bytes、terminal hash 和 final-state hash，缩短剩余证据边界。

在这四项完成前，论文可以保留当前 controlled drift 数字作为**同实现、合同审计通过的窄切片结果**，但不能把它写成已经完成的独立方法证明或通用实时学习证明。
