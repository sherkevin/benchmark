# 叙事迭代第二轮终审记录

- 日期：2026-10-04
- 对象：`paper/aamas2027/main.tex`（本轮修订后）
- 依据：`research/2026_AI顶会最佳杰出论文叙事范式_六大范式_阅读整理.md`、`docs/决议/0008-论文叙事范式作为写作评审基准.md`、三份生效评价标准
- 审查方式：独立审查员逐行复核；随后由主协调者核对实现与排版
- 结论：**pre-results design CONDITIONAL；作为已有实证效果的最终论文 FAIL**

## 已通过或已修正

1. 主范式仍为“根因手术刀”：正常返回仍可能错误，动作先于当前输出，正确性只属于 selected call 且可能延迟；引言已经按 scene → concrete failure → root cause → insight → mechanism → falsifier 排列。
2. `z000282` 已是可审计的开发示例：TDC BBB label=1；`admet_ai_1_4_0` 返回 0、`b3clf_dtree_common` 返回 1；两次 runtime status 均为 successful。正文明确该例只证明选择分歧，不冒充 online effect 或 confirmation。
3. 方法合同已补齐固定 provider registry + 动态 eligible mask、同 tick 按 enqueue order 释放、`system_success` 与 `chemical_correct` 分账、空 challenger 时的完整 propensity、fallback hazard 参考和 Beta 上界计数规则。
4. cold-start 与 online state 已显式对齐：`beta_0=(u^0,b^0)` 且 `z_{a,0}=z_a^0`。invariants、falsification、science gate 已拆成独立段落。
5. E6 状态为 `BLOCKED / TBD`，没有把 confirmation 或开发诊断写成完成；PDF 使用官方 AAMAS class，7 pages。

## 仍然决定最终验收的缺口

1. E1–E5 结果单元仍为 TBD；没有 headline result、paired uncertainty 或成本结果，不能宣称 MIRA-Relay 已优于基线。
2. strict baseline 的 feature、prior、likelihood、warm-start、budget、cost 和 recovery schema 还需在同一冻结合同中闭合；GraphRouter 保持 audit-only，Replay-SGD 保持 separate-control。
3. E3 尚未证明 shared residual 的整体效用。必须比较同 schedule 的 `ProbeNoUpdate`、`ProbeIndependentResidual` 和 family/provider permutation，并报告 action changes、rescued/harmed 与 molecule-cluster interval。Graph Relay fold0 的负结果只能作为“动作变化不等于 utility”的边界证据。
4. provider lineage、端点资格和 molecule/scaffold-disjoint confirmation 尚未闭合；当前范围只能写成固定 BBB development menu 的在线选择协议。
5. hazard 常数 `kappa`、`delta_h`、`eta_h` 的具体冻结值和来源应进入 CPU preregistration/receipt；公式本身已可执行，但配置还不能由最终稿反推。

## 下一步

先完成唯一一套统一 raw-run/recovery schema 下的 E1/T1、E2/T2、E3/T4、E5/T5 CPU gate，再决定是否支付 A800 的完整矩阵成本。结果出来后重写摘要、贡献段、主表和结论；在此之前不把当前七页稿当作投稿实证稿。
