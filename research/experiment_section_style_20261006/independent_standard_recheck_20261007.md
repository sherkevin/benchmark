# 独立三标准复核：方法 Gate 纠正

- 日期：2026-10-08（v2 follow-up 已绑定主稿）
- 复核对象：`research/experiment_section_style_20261006/three_standard_recheck_20261007.md`
- 主稿：`paper/aamas2027/main.tex`
- 复核者：独立只读审查；未修改主稿、未启动实验。

## 结论

原重检中的 Benchmark 分数仍可用于声明范围内的 routing gate。以下三条是 v2 之前的历史初审结论；当前有效的 follow-up 分数见文末，不能把历史分数读成当前 v2 结论。

- **Benchmark + Baseline：** Gate A/B/C 仍分别为 15/16、13/16、14/16，限于四 fresh provider、一次调用、selected-only delay 和 controlled drift 的窄切片可以通过；
- **故事线：** 当前主稿的窄切片故事可以继续做盲读和文字复核，作者侧 85.5/100 只是 provisional；
- **方法论（v2 之前的历史初审）：** **H7 = UNKNOWN/HOLD**。严格分数为 **46/60**，M5/M7/M8/M13 低于 3；该分数已由文末 v2 follow-up 补充，不能用于替代当前状态。

这不是“方法错误”的结论。现有证据足以证明事件合同、反馈时钟、同实现回放和 controlled drift 现象；它还不足以证明当前 MIRA-Relay 的 learned state 可以由独立实现恢复，也不足以证明 provider-local residual 是受控恢复的必要机制。

## 方法逐项纠正

| 项 | 原作者分 | 独立复核分 | 复核理由 | 最小关闭动作 |
|---|---:|---:|---|---|
| M1 | 4 | 4 | 对象、shape、case、schema 齐全 | 保持 |
| M2 | 4 | 4 | decision→termination→release→update、delay 和 exactly-once 有 receipt | 加入重启/乱序 mutation |
| M3 | 4 | 4 | selected-only、禁止字段和 event audit 齐全 | 保持 |
| M4 | 4 | 4 | chemical/runtime/unknown/refusal 分开 | fresh scored stream 的边界继续注明 |
| M5 | 3 | **2** | 当前四-provider 机制没有 interface-null、independent-residual 或 family-permutation 消融；旧 MIRA-v2 ablation 不能转移 | 同一 fresh menu/prior/clock 预注册机制消融 |
| M6 | 3 | 3 | 更新公式可计算，复杂度和残差边界有说明 | 增加适用条件/偏差边界 |
| M7 | 4 | **2** | 当前 verifier 重用 production runner/policy；opaque hash 不能独立重算 state/action | 不导入 production runner 的 NumPy replayer |
| M8 | 3 | **2** | state 写入和 controlled action change 可见，但没有当前契约的 remove-state/interface-null 反事实 | 保存 state bytes 并重放删状态对照 |
| M9 | 3 | 3 | update p95、state size、calls 已记录 | 补 K/d/pending 压力曲线 |
| M10 | 4 | 4 | feedback-adaptive scope 的 release→state→next-action 已验证；未声称自然非平稳 | 不扩大为 environment-nonstationary |
| M11 | 3 | 3 | $D\in\{0,1,4\}$、selected-only 和 paired comparison 齐全 | 补更完整的 propensity/偏差说明 |
| M12 | 3 | 3 | menu、feature、feedback、delay、budget 合同已写清 | 补 modern adapter fidelity 细节 |
| M13 | 3 | **2** | 当前 fresh 方法缺 remove-state/interface/residual 轴；旧 bundle 不能直接移作当前机制证据 | full / Frozen / no-update / independent-residual paired CI |
| M14 | 4 | **3** | raw/config/hash/GPU/event audit 齐全，但旧 receipt 缺 canonical state bytes，也没有 crash/restart/乱序/版本 mutation | canonical transition trace + import-free replay + mutation tests |
| M15 | 3 | 3 | 主张已限定到 endpoint/menu/delay/controlled event | 继续分开 selector/verifier/agent claim |

总分：**46/60**。方法标准要求 M1–M8、M10–M12、M15 至少 3 分；当前 M5、M7、M8 未达标，且 H7 UNKNOWN，因此方法 Gate 必须 HOLD。

## H7 的证据边界

`independent_receipt_checker_20261007_v2` 能验证 event identity、顺序、时钟、read-set、摘要和 opaque state-hash continuity；它明确不能从 hash 推出缺失的 learned-state bytes。正文也已承认“旧 receipt 不足以独立重 hash learned policy”。development 六单元 replay 有完整 counter/pending state，但它不能补齐旧 confirmation receipt，也不能转移成当前 fresh MIRA-Relay 的独立重算证据。

## 关闭顺序

1. 在当前四-provider fresh contract 上保存每个 decision/release 的 canonical state bytes 或完整 transition inputs；
2. 写一个不 import production runner/policy 的 NumPy replayer，至少完整重算一条 stream，最好覆盖全部 confirmation，输出 action/state/metric 与 mutation checks；
3. 同一 menu、feature、prior、delay 和 selected-only clock 下预注册 full MIRA-Relay、Frozen/no-update、provider-independent residual、interface-null/no-release-gate，并报告 paired scaffold CI、rescued/harmed 和 no-event suffix；
4. 复核通过后再更新 M5/M7/M8/M13/M14 和三份标准总状态。

在这四项完成前，论文可以保留当前 controlled drift 数字作为**同实现、合同审计通过的窄切片结果**，但不能把它写成已经完成的独立方法证明或通用实时学习证明。

## Author-side evidence update (not an independent re-score)

Added after the independent review above. This section records later author
work and must not be read as a change to the independent 46/60 assessment.

The import-free replay v2 supplies partial evidence for H7: it reconstructs the
fresh MIRA state/action trace from the sealed prior and manifest and rejects
reward/score mutations. It does not repair the mechanism design gap. The later
Shared-Credit run is not a valid wrong-credit control: its common posterior
term cancels in `argmax`, its initial ranking differs from Frozen, the card was
written post hoc, and its point estimates were paired with scaffold-mean
intervals. Consequently M5 and M13 remain **2** in the independent review,
and the method Gate remains **HOLD**. The raw run is retained as an exploratory
static-recentering diagnostic in
`task/task49-pharma-benchmark-baseline-freeze/fresh_mechanism_ablation_v1/report.md`.

The next mechanism test must freeze Local's initial actions and vary only
released-state consumption, with a single estimand used for both point
estimates and confidence intervals. Until that test is independently reviewed,
the paper may claim the declared controlled drift response against Frozen and
modern online controls, but not that selected-provider credit assignment has
been isolated.

## Independent follow-up re-review: fresh mechanism ablation v2 (2026-10-07)

The later v2 receipt was reviewed separately against the same method criteria.
This section does not rewrite the historical 46/60 judgment; it records the
new evidence and the resulting declared-scope score change.

- **M5: 2 → 3.** The v2 Local/Frozen/permutation contract holds the cold-start
  score and action fixed and changes only the released-state credit destination.
  In the controlled replay Local−Frozen is +14.40 pp with 95% scaffold CI
  [+9.79,+19.23], while Local−permutation is +15.67 pp
  [+10.65,+20.74]. The natural Local−Frozen interval crosses zero, so this is
  not a generic stationary-gain claim.
- **M7: 4 for the declared replay scope.** An import-free verifier reconstructs
  90 streams and 14,130 decisions, checks 56,520 state hashes and 14,130 score
  vectors, and binds input and implementation hashes. Provider predictions and
  fit-prior/offline fit remain sealed replay inputs.
- **M8: 2 → 3.** The v2 canonical trace and fixed no-update/permutation controls
  show released state changing later actions under the declared drift. No
  natural net improvement or broader environment claim follows.
- **M13: 2 → 3.** The source-anchor residual increment and fixed cyclic wrong-credit
  control identify the mechanism in the controlled evaluator-side replay. The
  result does not identify family sharing or end-to-end agent utility.
- **M14 remains 3.** Raw JSONL, config, source/input hashes, canonical trace,
  strict release timing/order, and mutation rejection are present; crash/restart,
  missing-packet, and broader disorder stress are still absent.

The declared-scope method total is therefore approximately **51/60**, with
**H7 = PARTIAL/PASS_FOR_REPLAY**. The method Gate remains **HOLD** for the
paper-level goal because the replay does not cover natural provider drift,
provider inference/offline-fit reconstruction, terminal/final-state hash fields,
or the broader agent/verifier/refusal claims. The v2 result is safe to use as a
matched mechanism result only after the main text binds the same receipt and
scope.

## Main-text binding follow-up (2026-10-08)

The paper now binds the v2 result in a separate mechanism-attribution paragraph.
The pooled confirmation tables remain unchanged, and the cyclic arm is labelled a
mechanism control rather than a deployable baseline. The source/build binding is
`paper/aamas2027/main.tex` SHA
`4272e3689ddd3c88be317c948b0608bae30b0d32c3d65e1be4a5ec4d1c7ac045` and
`paper/aamas2027/build/submission_20261007_161243/main.pdf` SHA
`788488c2c3d02e613f38cca0b91af6c2cbb7c10cbd19724e6b0ebe346669269a`.

The declared replay scope is therefore text-bound and page-verified. The
independent method score remains approximately **51/60**, H7 remains
**PARTIAL/PASS_FOR_REPLAY**, and the full method Gate remains **HOLD** because
provider inference/offline-fit reconstruction, terminal/final-state receipt
fields, natural provider drift, and broader agent/verifier/refusal claims are not
covered.


## Independent follow-up: H7 receipt closure v1 (2026-10-08)

This follow-up is the current evidence for the declared policy-replay slice; the historical scores above remain unchanged as an audit trail. The new receipt closure uses v3 events with canonical state bytes, terminal receipt hashes, a `stream_complete` marker, event-chain commitments, fixed logical-clock permutations, representative checkpoint suffix replay, and fail-closed mutation cases.

- Natural and controlled conditions each verify **90/90 streams and 42,480 events**. Each has 15 exact checkpoint suffix checks and 270 exact logical-clock reorder checks; all 7 mutations reject in each condition.
- Development provenance was actually reconstructed with the native forward/fit helpers: 2,888 provider probabilities and 4,332 original Frozen decisions; maximum logit and fit-rate errors are both `0.0` under the preregistered `1e-8` tolerance.
- Delete/reinsert from the immutable spool gives byte-identical event files and verified final receipts for both conditions. The earlier 471-event schema-drift failure is retained and excluded from the pass count.

Current declared-scope update: **M14 3→4** and **H7=PASS_FOR_DECLARED_POLICY_REPLAY**; M5/M7/M8/M13 remain 3/4,4/4,3/4,3/4 from the matched v2 review, for approximately **52/60** on this narrow evidence slice. This does not close the broader method Gate: natural lifecycle drift, family sharing, verification/refusal, end-to-end agent gain, training-data lineage, and any claim beyond policy replay remain HOLD.

Evidence: `task/task49-pharma-benchmark-baseline-freeze/fresh_receipt_closure_v1/report.md` and its `artifact_manifest.json`.
