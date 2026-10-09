# 叙事迭代第三轮：合同措辞收口审查

- 日期：2026-10-04
- 对象：`paper/aamas2027/main.tex` 本轮释放时钟、收据边界和 pre-results 时态修订
- 依据：`research/2026_AI顶会最佳杰出论文叙事范式_六大范式_阅读整理.md`、`docs/决议/0008-论文叙事范式作为写作评审基准.md`、三份生效评价标准
- 审查方式：方法合同审查员与叙事审查员独立复核，主协调者核对编译产物
- 结论：**pre-results design CONDITIONAL；作为已有实证效果的最终论文 FAIL**

## 本轮已通过

1. 正文显式给出固定延迟合同：决策先执行，随后终止并入队，在
   `\(\tau_t=t+1+D\)`、`\(D\in\{0,1,4\}\)` 释放；`D=0` 也在下一决策 tick 的 release phase 处理，不会反向改变触发该事件的动作。
2. receipt verifier 的职责收窄为 read set、action、release queue、state hash、schema/counts；完整 score、propensity、MAP 和指标重算明确留给 E5 transition replayer。
3. 公平记录加入 checkpoint 或 sealed-fit-state hash；方法仍要求 source/input identity，便于复核 fit-only warm start。
4. 摘要和方法改为 planned evaluation / will evaluate，development-only verifier prototype 不再被读成独立恢复证据。
5. 叙事主线没有改变：根因手术刀为主，新基准暴露失效和反直觉重构为支撑；新增合同细节留在 Problem/Method，不打断引言的根因转折。

## 仍然阻止最终验收的事项

- E1–E5 主结果、paired cluster uncertainty、成本与失败/unknown/refusal 分母仍未闭合。
- transition replayer 尚未以独立实现验证 score、propensity、MAP 和从事件前缀恢复动作；当前 receipt 不能承担这个 claim。
- strict baseline 的统一 feature/prior/likelihood/warm-start/budget 合同、provider lineage 和 E6 confirmation 仍未完成。
- 负的 Graph Relay fold0 只能说明动作变化不等于 utility，不能替代 MIRA-Relay 的主结果。

## 编译核对

- `paper/aamas2027/build/main.pdf`：7 页，letter，692633 bytes。
- 单线程 clean build 通过，无 fatal error；保留模板的 `\ifx` incomplete warning 和少量排版 warning。
- 当前主稿仍是 internal pre-results draft，实验表格 TBD 不得写成结果。

## 下一步

继续唯一的统一 raw-run/recovery schema 下的 E1/T1、E2/T2、E3/T4、E5/T5 CPU gate；通过后再支付完整矩阵的 A800 成本，并据真实结果改写摘要与主表。
