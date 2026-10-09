# 测试 0002 — 三领域预测工具是否可用

## 目的

两种模式的「科学答案质量」只有在调真预测工具时才比得出来。若工具停摆,两模式都退化为 LLM 记忆+文献,数值精度无法区分模式。故在正式对比前,先探明超导/半导体/药物三领域的预测工具是否真在工作。

## 方法

跑各题 pipeline,检查 SSE 流里 `tool_call`→`tool_result` 的实际返回内容(而非模型口述),把返回里含 error/not found/unreachable/insufficient 等字样的判为不可用。

## 结果 — 预测工具全停摆,仅文献检索正常

| 工具 | 领域 | 实测状态 |
|---|---|---|
| `crystal_generate` (DiffCSP) | 超导/材料 | 报 DiffCSP 服务**离线** |
| `crystal_predict` | 超导/材料 | 找不到 cif —— demo cif 已失效(试过 `/data/cif/MgB2.cif`、`data/demo_pipeline/MgB2.cif` 均 File not found) |
| `predict_admet` | 药物 | 是 **mock 肽类过滤器**,小分子只回 "Insufficient data",不产 logS/logD/BBB/hERG |
| (带隙预测) | 半导体 | 平台**无**对应工具 |
| `literature_search` | 全部 | 正常 |

并行模式还观察到工具名翻倍 bug `crystal_predictcrystal_predict`(无效工具),仅在并行子 agent 出现。

## 结论

三领域预测后端整体不可用,**两种模式都只能靠 LLM 记忆+文献作答**。直接后果,贯穿后续所有结论:

- 「数值正确性」测不出模式差异 —— 易记事实(GaN 带隙、caffeine 溶解度、超导 Tc 排序)单步多数答对,难题(裸 SMILES 识别)两模式都会错。
- 真正能测到的模式差异落在**编排与交付层面**(完成率、报错率、能否产出成稿),不在科学预测精度。
- 若要纯比「科学答案质量」,须先恢复 DiffCSP/crystal_predict 等真实工具;在那之前,以编排/交付层面的差异为准(见 [0003](0003-single-vs-parallel-12-runs.md))。
