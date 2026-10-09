# ADR-0018：context-conditioned cell JEV 作为下一版 MVP

状态：Superseded by ADR-0019

日期：2026-09-25

## 背景

post-freeze BBB v2 对照显示，provider-marginal M2 在主成本下退化为 random probe，no-update 对照也没有显示稳定的在线更新收益。当前状态只按 provider 聚合，无法表示同一工具对不同分子上下文的 verification gain 差异。

仓库没有真正 fresh 的 BBB 分子池，所以这一步只能先做接口和回放 pilot，不能把结果写成未见分子泛化。

## 决定

下一版先实现一个低维 context-conditioned JEV MVP：

1. context 固定为 `(TPSA > 60, aromatic_ring_count > 0)`，共 4 个 cell；阈值在实验前注册。
2. 每个 `(context, provider)` 独立维护 eligible `N`、HT gain `H`、observed count `O` 和 deterministic Freedman 半径。
3. STOP 和未选中 provider 仍增加 `N`，只有实际选中 provider 才增加 `O` 和 HT gain。
4. 只有 `O >= 4` 且 LCB 高于调用成本时，利用分支才允许打开；探索分支保留正的 overlap floor。
5. global prior 只影响探索排序，不写进 certified HT 估计，也不能单独打开门。
6. 先完成独立状态、context、propensity 和 synthetic coverage 测试，再接入 M2 runner。

## 理由

4-cell 比 8-cell 稀疏程度低，仍能让状态依赖分子结构；HT/Freedman 合同可以直接复用已审查的 overlap 证明；不训练 backbone，先把“上下文是否带来可识别的增益”这个问题单独测清楚。

## 后果

- 这不是最终的神经 JEV，也不声称解决复杂上下文泛化。
- cell 稀疏时门会保持关闭，调用收益可能偏低，但不会用 global pseudo-count 伪造置信保证。
- 如果 synthetic coverage、cell support 或 fresh scaffold 上的 utility 不达预注册阈值，应停止扩展到神经 backbone，先重新审查 context 定义和数据池。

## 关联产物

- 规格：`research/AAMAS2027_ContextConditioned_JEV_MVP_Spec_v1.md`
- 初始状态实现：`src/probe_headless/context_dynamic_jev.py`
- 合同测试：`src/probe_headless/tests/test_context_dynamic_jev.py`
- v2 失败证据：`research/AAMAS2027_BBB_Formal_Evaluation_Report_v2.md`
