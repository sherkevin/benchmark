# task34 — T3 Modern Safe/Calibration Closure

> 状态：⚡️进行中

## 目标

在 Task32 已否定 H1/H2、Task33 已关闭 risk-sensitive abstention 后，继续关闭两个可能被审稿人直接指出的近邻：

1. modern conservative contextual bandits / safe exploration；
2. few-shot / meta conformal calibration under auxiliary tasks and limited feedback。

本任务仍不直接设计 Ours。先回答：历史工具是否能迁移的不是 mean representation，而是 **one-sided error calibration / adoption risk certificate**。

## 验收标准

- 写清 ICLR 2025 Conservative Contextual Bandits、SEA、Cross-Domain OPE/L、Few-shot Conformal Prediction、Conformal Risk Control、2026 limited-feedback conformal 的覆盖与差异；
- 明确 frozen feedback 下哪些方法需要不可获得的 side observation / source action data；
- 用 train-only pseudo-arrival episodes 测试 historical one-sided calibration 是否能在新 challenger few-shot 下产生正 IOU + 高 precision；
- formal test partitions 不接触；
- 若失败，明确否定该方向，不为了 Ours 强行保留。

## 2026-09-22 — 隔离缺陷修复登记（运行前）

依赖审计发现 `precompute_aux_errors(pack)` 在真实 held-out j 之前为所有 k 计算 `Bh/Ih`；随后仅删掉 aux 的 j 行仍不足，其他 pseudo-k 的 baseline 可含 j。

本轮仅修复该实现缺陷：对每个真实 j，先从历史 tool matrix、baseline selection、baseline value prediction 中删除 j，再对 pseudo-k 做第二次排除。另建 `t34_meta_calibrated_gate_isolated.py` 与 `_isolated` 输出，保留原脚本及已存在原结果。

保持 B=4/8/16/32、TARGET_SEEDS=0..4、CAL_SEEDS=0..2、ALPHAS=0.20/0.10/0.05、Ridge alpha=1 不变；Task31 正式协议不变。实际 learner 只收当前选择的 challenger feedback，calibration 不得读取真实 j 的任何历史 outcome/fit column。

新增验收：poison j 历史及 prediction 列后所有 auxiliary error arrays/quantiles 不变；正向对照能检出旧路径泄漏；重跑全部 train-only episodes，并保留 probe-inclusive IOU 的原定义。

解释限制：pooled auxiliary error quantile 是启发式 calibration diagnostic，不是已实现/已证实的 distribution-free risk certificate；incumbent 的 in-sample auxiliary calibration 与 out-of-sample target 环境仍不同。本轮不把它冒充已发表 conformal 方法。

更正前提：Task32 r1 结论存在信息隔离问题；有效版本为 Task32 r2 serial。r2 仅显示指定 H1 构造缺乏稳定优势，H2 在 BBB 小预算有正例，不应继续写作普遍“已否定”。
