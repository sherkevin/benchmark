# task21 · D2-① 方差分解可行性核验（BBB 族，零机时）

> 上游：owner 2026-09-14 批准（「可以」）。这是 ADR-0028 D2 五条零机时检验的第一条，
> 也是 ADR-0006 裁定的「GPU 闸门前置」中唯一一条当前数据就能做的。
> 目标文档「一句话」段的反跑偏判据（α_i 驱动、跨工具可迁移）正是本检验要量化的东西。

## 1. 目标

一句话：**用冻结的真值 + 冻结的分歧投票，量出血脑屏障（BBB）族上「分子主效应 α_i」与
「工具偏置 β_k」各占多少误差方差，回答「这数据支不支持进第三层」。**

判据（ADR-0028 D2-①）：

1. α_i 占比可计算、且显著大于 β_k 占比 → 训练目标「判断这组预测能不能用」有数据支撑；
2. 置换检验给出 α_i 占比的右尾 p 值 → 排除「α_i 占比只是自由度基线」的解释；
3. 伪共识分子（七族全票一致但判错）可计数 → 为 Step 6 捷径行为诊断预留样本量；
4. 产物确定性：同输入连跑两次 JSON 逐字节一致；
5. 全部 caveat 落盘：二值响应近似（GLMM 风险）、deeppurpose 泄漏敏感性、
   γ_ik 与 ε_ik 混杂、真值残余噪声。

## 2. 输入（全部只读冻结）

| 输入 | 路径 | 说明 |
|---|---|---|
| 分歧投票 | `data/tool_results/divergence/血脑屏障 BBB.csv` | 730 分子 × 7 族（admet_ai/admetlab/b3clf/deeppurpose/fpadmet/graphb3/lgbm_bbb），Step 9 后冻结 |
| 实验真值 | `data/bbb_census_v3/.../screening_ledger.jsonl` | `experimental_truth.label`，730/730 全有值（522 阳 208 阴） |
| 矛盾排除表 | `data/bbb_census_v3/.../truth_unresolved_bbb.json` | 11 个矛盾分子，其中 8 个有 entity_id（另 3 个不在池内），必须排除 |

样本口径：730 − 8 = **722 可用**；其中七族全票齐 = **652 平衡面板**（主运行）。

## 3. 交付物

- [x] `src/probe_headless/bbb_variance_decomposition.py` — 可重复执行的分解脚本
- [x] `data/tool_results/divergence/variance_decomposition_bbb.json` — 结果 JSON（带输入 md5）
- [x] `src/probe_headless/tests/test_bbb_variance_decomposition.py` — 11 条测试，三次变异验证
- [x] `task/task21-D2一方差分解BBB/report.md` — 结论与证据

## 4. 边界

- 只动 BBB 一个族；其余 8 个已计分族的真值未接，不在本任务。
- 二值标签下的 ANOVA 分解是近似，严格版是 GLMM（入库版提案 §3.4.7 风险 2），
  本任务只登记该风险，不做 GLMM。
- D2-① 的另一半「标签噪声翻转率」需要多源真值或重复测定，当前单一 Martins 源做不了，
  用 10 分子人工核验的 1/10 不一致率作上界登记，不冒充测量值。
- 不修改任何冻结输入；不新建 campaign；不申请 GPU。
