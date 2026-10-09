# CGEF 固定采集终局融合干预：负结果报告

日期：2026-10-08。状态：**已按 `experiment_card_v2.md` 执行；独立复算通过；候选停止。**

## 目的和边界

这次只测试一个单因素候选：在同一首工具、同一已取得的第二工具输出、同一动作阈值和同一已发生成本下，用 Coherent Gaussian Evidence Fusion（CGEF）替换当前 masked-logistic 终局融合。它不改变 selector、第二调用是否发起、gain learner、反馈时钟或训练权限。

它是开发集 B 的固定采集诊断，不是新的 confirmation，不读取 C、不调用 provider/API/GPU，也不把结果写入主版论文。卡内预先规定的通过线是：CGEF 相对当前规则和 `ignore-second` 的 pooled paired utility 均至少增加 `+0.01`，且至少 3/5 fold 非负；否则停止该候选。

实验卡 [`experiment_card_v2.md`](experiment_card_v2.md) SHA-256：
`a4b0361b6383ec3616f40b7279c01c26719196246aa533fca2e8ca61f48653a00`。

## 执行合同

- A 拟合分区只用四个 native provider probability 和 truth；按每折 `fit_indices` 计算类条件均值、pooled covariance，加固定 `0.10 I`，类别先验为 A 正例率。
- B 只用已经取得的 1,518 条 paired target；506 个 fold–molecule，每个有 3 个第二工具候选。
- 对每个候选比较三个终局规则：`ignore-second`、原 masked-logistic、CGEF。三者都按同一 post-paid 口径计入 $kappa=0.01$；拒答损失为 $ho=0.25$。
- 所有 native probability 先按卡内固定规则裁剪到 `[10^{-6},1-10^{-6}]` 再取 logit；不因结果改变裁剪、阈值或输入。
- 统计单位是 molecule within fold；先对每个 entity 的三个候选取均值，再计算 fold/all 的 paired utility delta。

执行命令：

```text
python3 src/probe_headless/run_cgef_intervention_v1.py --go \\
  --card-hash a4b0361b6383ec3616f40b7279c01c26719196246aa533fca2e8ca61f48653a00 \\
  --output experiments/logs/task49_conditional_fusion_intervention_20261008/cgef_v1
```

原始结果、参数快照、逐行输出和配置保存在 [`cgef_v1/`](../../../experiments/logs/task49_conditional_fusion_intervention_20261008/cgef_v1/)。输入 A SHA 为 `e1dabd7ff222b21b1319b73b52d2c47d3a5e861bd3d7231a4e38e9269528bf8b`，输入 B SHA 为 `6826f43960e2ceede02482b674e9fa0b4f3874c971824b85575ce4e79500585f`。

## 结果

表中数值是 CGEF 相对对应控制的 utility/题差值；正值表示 CGEF 更好。

| fold | entities | CGEF − masked-logistic | CGEF − ignore-second | 非负/原规则 | 非负/ignore |
|---:|---:|---:|---:|---:|---:|
| 0 | 133 | −0.002506 | −0.009398 | 124 | 125 |
| 1 | 94 | −0.005319 | +0.002660 | 84 | 89 |
| 2 | 86 | +0.000000 | +0.006783 | 77 | 80 |
| 3 | 95 | +0.014035 | +0.001754 | 81 | 86 |
| 4 | 98 | −0.005102 | −0.000851 | 83 | 88 |
| **all** | **506** | **+0.000000** | **−0.000659** | **449** | **468** |

CGEF 只在 fold 3 相对原规则超过 `+0.01`，相对 `ignore-second` 没有任何 fold 达到 `+0.01`。pooled 相对原规则为数值零（约 `8.8×10^{-19}`），相对 ignore 为 `−0.000659`，两项都没有通过预注册开发线。

## 独立复算

独立 verifier [`verify_cgef_intervention_v1.py`](../../../src/probe_headless/verify_cgef_intervention_v1.py) 不导入 runner，重新读取 A fit、B raw event 和每折模型元数据，重算：

1. 五折 CGEF 均值、协方差和先验；
2. 全部 1,518 个 B row 的 masked posterior、动作、损失和 utility；
3. 506 个 entity 的 paired mean；
4. 五折及 pooled 六行聚合。

五折参数最大绝对差为 `0`，逐行重算最大绝对差为 `0`。独立核验 JSON 为 [`independent_verification.json`](../../../experiments/logs/task49_conditional_fusion_intervention_20261008/cgef_v1/independent_verification.json)，SHA-256：
`e77d35e07a7dcd8197dcbbf60d383d44205530631c85a86bd17c5d9fe5957eb3`。

## 决定

这次结果反驳了当前候选的开发假设：在固定已取得的 pair 流上，共享协方差的 Gaussian marginal 没有带来 pooled utility 改善，也没有超过 `ignore-second`。因此按卡内停止条件，**不做 CGEF 闭环扩展、不调 covariance shrinkage、不扩展 Gaussian basis，也不把该结果写成方法效果**。

这不是“第二工具一定没有价值”的结论；它只说明当前瓶颈不能靠这一种终局分布假设修复。下一步应回到更有区分力的问题：同一 gain/value 模型下即时反馈与延迟反馈的动作变化，以及预算相同的闭环 acquisition；候选必须先写卡、做独立复算，再决定是否进入主稿。完整 benchmark/baseline/confirmation gate 仍保持 HOLD，主版 `paper/aamas2027/main.pdf` 未修改。
