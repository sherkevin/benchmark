# 四工具的剩余机会与反馈覆盖

日期：2026-10-07。状态：真实缓存分析与独立复算完成，verified、0 issues。本轮不训练、不推理、不运行新策略，不读取confirmation。

## 结论

当前开发面板仍有选择空间，但大部分错误是四工具共同出错；可救错误又很少发生在正确工具完全没有历史反馈的时刻。因此，下一步优先检验**更新能否根据当前分子使用已有反馈**，不先增加随机探索。反馈数量并不能证明相似分子的反馈充分，这一点仍需干预实验检验。

这是对722个反复使用的development分子的诊断，不是最终效果，也不更换主方法。上轮query中心化没有达到原1pp升级线的结论保持不变。

## 1. 当前菜单有多少空间

以下每个分子只计一次。正确性由封存原生BBB概率以0.5阈值判标签，再与真实标签比较。旧33-provider输入里的预测列不参与计算。

| 对照/工具 | 正确数 / 722 | 准确率 |
|---|---:|---:|
| ECFP logistic | 580 | 80.33% |
| ECFP MLP | 564 | 78.12% |
| ECFP random forest | 571 | 79.09% |
| Graph message passing | 514 | 71.19% |
| Fit-only Best Fixed（每折只看fit选定工具） | 572 | 79.22% |
| Raw Frozen | 573 | 79.36% |
| Corrected Frozen | 579 | 80.19% |
| Retrospective Best Fixed（事后一个全局工具） | 580 | 80.33% |
| Per-query Oracle（评价器逐题看全表） | 618 | 85.60% |

722题中，**471题四工具全对，147题有对有错，104题四工具全错**。Raw Frozen的149个错误中，104个属于全菜单均错、45个存在正确候选；Corrected Frozen对应104和39。剩余Oracle空间分别为6.23和5.40个百分点。这是当前固定菜单的上界，不是不可约误差，也不保证能被合法调用前特征学到。

四provider来自已绑定checkpoint，不能视为四个独立错误来源。provider资格审计还记录了训练候选与development的118个scaffold重合；本轮不声称完整provider→selector链的scaffold外推。参见[菜单资格报告](../provider_fresh_menu_v2/report.md)。

## 2. 分数有一点上下文价值，但优势有限

只在147道mixed题上比较；fit-rate排序用于控制“某工具整体更好”带来的优势。top1按真实代码的首个argmax，成对排序的平分按0.5计。本面板没有这些排序tie。

| 分数 | mixed top1正确 / 147 | mixed top1 | 分子平均成对一致率 | 按pair加权一致率 |
|---|---:|---:|---:|---:|
| Fit rates | 101 | 68.71% | 71.66% | 69.98% |
| Raw | 102 | 69.39% | 66.84% | 66.60% |
| Corrected | 108 | 73.47% | 71.37% | 70.40% |

Corrected Frozen比fit-only多答对7题，整体+0.97pp；原raw只多1题。五折corrected相对fit-only的正确题数差为 **+4、+4、0、+1、−2**。这是有限的实际选择信号，不能写成各折稳定领先，也不能因成对一致率略低于fit-rate就否定真实top1差。

20个provider×fold的correctness AUROC从0.3795到0.7910不等；raw与corrected逐项相同。这符合常数logit平移保持工具内部排序的数学性质，不是校准改善了工具内部判别力。完整逐折结果在summary，未筛去低于0.5的项。

## 3. “正确工具从未被探索”只覆盖少量错误

以下各行来自同一cell的三种delay，共2166个实际决策；分子重复三次，不当独立样本。F/M/Q为Frozen、平均正确率中心化、调用时分数中心化。

| cell | 全菜单均错 | 可救错误 | 全部正确候选零released | 其中全部从未选过 | 其中至少一个pending | 至少一个正确候选已有released |
|---|---:|---:|---:|---:|---:|---:|
| raw/F | 312 | 135 | 1 | 0 | 1 | 134 |
| raw/M | 312 | 139 | 3 | 2 | 1 | 136 |
| raw/Q | 312 | 154 | 7 | 6 | 1 | 147 |
| corrected/F | 312 | 117 | 2 | 0 | 2 | 115 |
| corrected/M | 312 | 125 | 7 | 5 | 2 | 118 |
| corrected/Q | 312 | 114 | 5 | 3 | 2 | 109 |

排除各流最初D+1次必然没有反馈的决策后，原raw/M还有138个可救错误，其中**136个已有正确候选反馈，2个全部正确候选从未被选择**。corrected/Q为112个，其中109个已有反馈、3个从未选择；pending类均为0。全部六cell的这一分层均保存，没有把启动延迟算成长期回避。

该结果削弱了“主要问题是完全没试过正确工具”的解释。它不证明反馈足够、不排除探索作用，也不提供探索收益的因果上界；已有反馈可能来自不相关分子，更多采样还可能改变之后的选择。下一轮需要用保持表示、分数和时钟不变的干预来区别这些解释。

## 4. 下一步的唯一问题

**同样的已释放反馈，按当前分子条件化使用，是否比给每个工具统一加一个偏移更有效？**

候选诊断把现有scalar residual扩成正则化context residual，并用constant-only特征精确还原原Q作为控制；冻结corrected分数、原特征、先验强度、菜单、延迟和预算。标准ridge及秩一更新仅作诊断工具，不单列创新。先完成数学退化证明和审查，再冻结实验卡；此处没有新实验效果。

选择这一问题的理由是：当前有39/722个Corrected Frozen可救错误，已有分数相对fit-only只有7题净收益，而可救错误中的完全未反馈比例很低。若条件化更新也不能带来足够收益，保留负结果，再检查表示或离线排序；不同时改backbone、探索和训练目标。

## 5. 证据与复用

- [实验卡](experiment_card.md)，SHA `86b1524076b1efa46b508cc82fc289dd3922c6b2459c78b053ee7508b199fc5e`；[独立数学审查](math_review.md)已在执行前完成，保留top1/concordance反例与修改理由。
- 分析器：`src/probe_headless/analyze_headroom_feedback_v1.py`；10项必要合成测试通过，首次即通过，证据在`experiments/logs/task49_headroom_feedback_20261007/prepare_v1/`。
- 正式分析：`experiments/logs/task49_headroom_feedback_20261007/v1/`；90条自然流、12,996条逐决策记录、722条逐分子记录，2.514秒，0训练/推理/API/GPU。命令、源码及输入hash在config和上层launch日志。
- summary SHA `e1e8a7bafdc16042727ee76849dd9676240ae57e00839472a0b8c2bdb7615e68`；decision_rows SHA `c04d896f1e8e46a189dc3fc6a507b0d770b25faada516eb2d2926ca5cbe5e1f9`。
- 独立复算：`src/probe_headless/verify_headroom_feedback_v1.py`不导入分析器或生产runner，用独立rank-sum AUROC及原native/OOF/事件核对全部722分子字段、12,996决策字段、90条自然轨迹的选择/释放/pending身份及所有汇总分组；3.113秒，verified、0 issues。未重跑策略更新或provider；当前仅核对描述性分析。
- 验证回执：`experiments/logs/task49_headroom_feedback_verification_20261007/v1/verification.json`，SHA `bb39e6f9a19e592abd534c85ad5b913d340d797ac61f5f7790b746e61d4cf831`。两次执行均首次成功，命令、环境、源码hash和逐行证据保存。

可直接复用现有OOF分数、722×65特征、五折priors和已释放反馈；后续最小干预不需要重新调用科学工具。当前PDF仍使用上一轮已验证的正文8页版本；本诊断尚未加入正文。Benchmark E1/E5得到补充证据，但现代baseline完整性、机制增量和新候选独立确认仍未解决，完整科学gate保持HOLD。
