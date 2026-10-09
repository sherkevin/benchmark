# Expected matrix v02 multi-angle audit

日期：2026-10-09
对象：expected experiment matrix v02、当前 paper/aamas2027/main.tex 与 main.pdf、Guide v09 审查材料

## 总判定

v01 不能签收为 clean expected experiment matrix。v02 已按当前完整主稿逐表重写，状态仍是 planning-only。Guide v09 作为方向约束可以保留，但作为已经通过或可以关闭 benchmark+baseline 的验收合同，结论仍为 HOLD / PENDING。

## 一一对应审查

当前绑定的完整主稿为 9 页 main.pdf。它包含 Tables 1--8 和 Figure 3：

- Table 1：common routing contract 的 baseline roster；
- Table 2：E1--E6 experimental map；
- Table 3：natural/controlled selected correctness，Original 与 128-order mean；
- Table 4：Local/Frozen/Permutation source-aligned contrasts；
- Table 5：BBB cold-start；
- Table 6：score scale × update rule；
- Table 7：post-call refusal；
- Table 8：CPU policy overhead；
- Figure 3：delay-specific gain over Frozen。

v01 的 Table 8b、未来 sealed Table 9、Table 10 不存在于当前 main.pdf，已从 clean matrix 移除。v01 的 E6 map 指向未来 Frozen confirmation，也已改回当前 PDF 的 post-call refusal / Table 7。v02 的 Figure 3 data 保留原图的 gain-over-Frozen 指标，避免把 delay 曲线改成 absolute success。

## 数值与方法关系审查

- 自然 stationary：MIRA-Relay 与 Frozen 持平或略低；不宣称 generic stationary gain。MIRA-Relative 与 Frozen 相同，因为其 molecule-independent multiplicative factor 不改变 action sequence。
- Controlled correctness drift：MIRA-Relay 明显高于 Frozen，且高于 matched Online SGD、Greedy LinUCB、BayesianRouter；fixed-permutation credit 低于 Frozen，符合 source-aligned credit 的机制方向。
- E2 attribution：三行满足代数关系，natural planning bands 跨 0，controlled Local-minus-Permutation 为正；表注明确这是 scaffold-mean estimand，与 Table 3 pooled success 分开。
- E3 cold-start：BCE 与 BT 两个 seed 均低于 Corrected Frozen，保留正文“两个 offline objective 没有改善”的负结果；oracle 只作 evaluator-only 上界。
- E4 centering：query update 在 drift 下优于同 scale Frozen，natural 改善小；不把开发诊断升级为主方法证据。
- E5 cost：MIRA-Relay 的 selection 高于 Frozen、低于 LinUCB/BayesianRouter，update 保持轻量；不把分开的 mean-of-stream-p95 写成 joint p95 cap。
- E6 post-call：整数计数、coverage、accepted accuracy、utility 相互一致；仍标 secondary/reused holdout。

## Baseline 审查

正文 roster 保持 Random、Best Fixed、Retrospective Best Fixed、Frozen、MIRA-Relative、Online SGD、Greedy LinUCB、BayesianRouter、MIRA-Relay、cyclic mechanism 与 pooled audit。Replay-SGD、PILOT、GraphRouter、Laya/JEV 等没有被擅自加入主矩阵；它们若要成为 primary 必须同时修改正文、成本表、协议和 receipts。

## Guide v09 审查结论

Guide v09 的方向基本正确：自然场景允许持平或略逊，受控 drift 才要求候选优于 matched online baseline，逐 delay 与成本/暴露边界也有正确意图。但四个审查仍未闭合：

1. candidate/scope 混用（fresh MIRA-Relay 与 development MIRA-Relative/family-outage）；
2. E2/E3/E4 缺少同一 candidate、同一 intervention 下完整 paired CI，且 Guide 的 primary ablation（MIRA-Relative）不在当前 PDF 的 controlled 行；
3. E5 只有拆开的 timer，不能证明 joint p95、memory 与全成本；
4. E6 的 previously-inspected 157 cohort 仍缺足够的 sealed exposure receipt；
5. estimand、future-Y、独立统计单位、failure/unknown/refusal utility 与 clean replay 仍需机器合同。

因此 Guide v09 状态为 HOLD/PENDING，不能被“矩阵目标值与它一致”自动关闭。Expected matrix 只提供后续实验的对标目标，不替代 raw/recovery/exposure evidence。

## 变更清单

- 新文件：versions/v02_20261009/expected_experiment_table.tex
- 新文件：versions/v02_20261009/README.md
- 新文件：versions/v02_20261009/main_input_sha256.txt
- 本报告：expected_experiment_table/reviews/expected_matrix_v02_multiaudit_20261009.md