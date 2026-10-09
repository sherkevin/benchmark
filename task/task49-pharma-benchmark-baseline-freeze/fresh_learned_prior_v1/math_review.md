# 执行前独立方法审查

2026-10-07；审查者 `/root/headroom_diagnostic_math`；对象：准备卡、旧RewardHead/loss与拟议匹配训练。只读审查，未运行实验、未看confirmation。root据审查冻结experiment_card.md。

## 审查结论与采纳项

1. BT符号和配对方向正确；固定全pair平均，明确不同分歧形式的权重不同。all-agree仍通过CLS影响encoder，不作“消除无用梯度”的结论。
2. 同构双头、复制相同初始state；只比较主loss方案，排除bias/容量差。独立CLS只辅助，不在结果后择优改用CLS路由。
3. 两者共同按validation top1最多、最早epoch选型；各自loss只记录。validation无mixed统一epoch200并标uninformative。原准备卡的各自loss选型不采用。
4. 空pair函数可graph-zero，整折fit无pair必须fail closed；不把未受BT训练的score头算完整训练。
5. BT score非正确率，独立CLS不消除其平移不识别；后续更新接口需要新卡，不能直接继承Q。
6. 分开学习型prior有用与BT优越两层资源线；即使BT失败，BCE通过仍可推进。每条明确LOO差值，1pp非统计显著性。

## 剩余边界

固定200步只支持该配方比较，不证明各自充分优化；两seed共享分子，不能当独立重复。当前静态对照不能打开全部科学gate或取代新确认。原审查建议全部采纳，无未决超参；不增加无关矩阵。

## 冻结卡复核

审查者再次核对卡，核心设计通过；要求完整20fit才可go、分子加权LOO与未舍入1pp、tie首provider，并限定独立验证为best forward和按日志选epoch。这些已写入卡并传给实现者。代码尚未出现时未作代码通过结论。
