# 实施前独立方法审查

日期：2026-10-07。审查员：独立 `calibration_centering_design`。根协调者据审查补齐card后冻结；未把审查共识当实测证据。

## 结论

6格 × 5fold × 3delay × natural/controlled flip，是足以分离分数版本和在线中心化的最小闭环设计。同意development运行；0 API/GPU、新forward，180秒CPU预算。主方法和旧confirmation不变。

## 已采纳的实施前修订

- 每个regime单独聚合：fold按分子数合并、delay等权；原natural/stress不合成一个总分。
- 1pp资源线和5个leave-one-fold-out反号检查提前固定，不看结果换阈值。
- 旧行为动作/reward/时钟精确，M score容差1e-10；尾部沿旧N+6排空。
- Q状态含sum_q、pending decision_q；独立checker重算全部状态和动作。
- natural/stress首次真正受翻转影响的反馈释放前必须一致；按decision tick翻转，不按release tick。
- 固定后半段分母，同时报告绝对成绩、匹配Frozen差、target选择和首次受影响release，防止不同stress暴露率误导。
- OOF分数审计包含Brier、log loss、均值差；不事后拟合新校准器或调C。

## 保留的科学异议

1. 逆权重变换只在逐点weighted-risk最优下精确；当前有限维、C=1、正则化logistic不保证被修正后校准。[sklearn官方LogisticRegression](https://scikit-learn.org/1.6/modules/generated/sklearn.linear_model.LogisticRegression.html)也说明liblinear的synthetic intercept feature受正则。公式中的pi是provider正确率，不是BBB阳性率。
2. Q要求给定历史、当前query及选择的条件正确性匹配，普通分数分箱校准不足；greedy仍可能缺少未选provider反馈。
3. 不同分数版本改变静态偏好、score margin和stress暴露；匹配Frozen增益不能独自归因为在线机制更强。
4. label flip把p(x)变为1−p(x)，不是恒定加性漂移，某些原本易错样本可能受益。不得外推自然lifecycle。
5. development被多轮使用，fold训练集重叠且delay重复；本轮以配对描述、逐fold和leave-one-out为准，不做逐日志行bootstrap显著性主张。

## 结果解释约束

corrected Frozen提升而在线增量不升，只支持静态改进。自然提升而stress下降是取舍。Q−M集中一折不升级主张。分数审计没改善不能说校准成功，也不能据此否定全部Q方法。六格若没达到预先资源线，则停止扫计数/学习率，从任务headroom和反馈缺失解释下一步。

这些标准操作本身不承担创新；本轮贡献是验证当前机制疑问并决定后续值得投入的方向。

## 实施复核与执行边界

root逐段核对新runner：修正符号、provider正确率pi来源、调用时q保存、selected-only接口、decision-tick翻转、N+6尾部、每组对应Frozen和分regime聚合均与卡一致。准备期检查指出CARD需入provenance、OOF需log loss、首受影响release需真实tick；开跑前均补齐。6项合成测试通过，源码固定后只运行一次真实矩阵。独立checker不导入runner，40项合成/篡改测试通过，随后完整核验180流、77,976事件为verified。没有因结果改动卡、阈值、分母、方法或旧confirmation。
