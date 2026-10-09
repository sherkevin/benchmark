# Post-call confidence screen v1

日期：2026-09-27

## 问题

工具已经返回结果后，provider 自带的 numeric probability 是否能预测本次端到端 `system_success`？这个信号在调用后才可见，因此只用于 verification/refusal 扩展，不回填 direct pre-call provider selection。

## 固定定义

对 kind=`prob` 的 provider，读取 cached `raw_value=p`，定义无标签的输出 confidence 为 $c=2|p-0.5|$。标签仍是 evaluator-side 的 `system_success`：实际返回失败记 0，返回标签与独立参考一致记 1。kind=`label` 的 fpadmet 不参加 numeric confidence 分析。

每个 outer fold 只用 fit 区确定 provider 可用性和样本支持；evaluation 只用于报告 AUC 与 risk-coverage。AUC 使用 `c` 预测 correctness，不用 evaluation 标签调阈值。对固定 `admetlab_3_0` 额外报告 coverage `0.5/0.8/0.9/1.0` 的 accepted accuracy，accepted 集按 evaluation 中 `c` 从高到低取前缀，coverage 网格在运行前固定。

## 判据

如果 numeric provider 的 evaluation AUC 中位数高于 `0.55`，且 `admetlab_3_0` 在至少 4/5 folds 的 0.8 coverage accuracy 高于全量 accuracy，则保留“post-call confidence verifier”作为下一候选；否则不实现在线 confidence calibrator。这个筛选不声称动态效果，不打开 confirmation，不申请 A800。

## 信息边界

confidence 只能来自已调用 provider 的返回值；选择前的 policy 看不到它。fit 标签只用于 support 统计，不训练任何 evaluation 阈值。保存每个 provider/fold 的原始计数、AUC、coverage 和输入/source hash。
