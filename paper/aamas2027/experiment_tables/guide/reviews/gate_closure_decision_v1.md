# Guide 与实验 Gate 的关系：判定记录 v1

日期：2026-10-08  
对象：Guide v07、当前生效的 Benchmark/Baseline 评价标准 v006 及现有 E1--E5 审计。  
结论性质：验收判定，不修改 benchmark、baseline、方法或正文结果。

## 结论

**仅当实验矩阵与 Guide 的目标/参考分布一致，不能关闭实验 Gate，也不能单独达到实验验收标准。**

Guide v07 通过的是三个更窄的事情：

1. 已有固定队列和已有策略的联合错误、配对差值、顺序波动、协方差和区间口径可复核；
2. 原始顺序上的条件误差预算可以从同一记录验算；
3. 这些参考值没有被伪装成未知未来候选的预测区间。

它没有证明一个新候选已经达到这些条件，也没有替代统一 baseline 合同、candidate-specific replay 或 untouched confirmation。

## Guide 当前审查状态

| 审查对象 | 结果 | 能证明什么 | 不能证明什么 |
|---|---|---|---|
| v07 固定队列顺序分布 | PASS / 22/24 | 固定157题、固定策略/seed、声明的随机到达与tick-78压力过程中，现有策略的共同分布可复核 | 新候选、新分子、自然部署漂移或泛化 |
| v06 原始顺序联合错误/条件预算 | PASS / 21/24 | 同一错误记录中的救回/误伤、条件响应和分层计算闭合 | 条件响应能被候选实时识别或达到 |
| v07 内部一致性 | PASS | 统计单位、分母、协方差、区间类型和正文 Table 3--8 映射一致 | 方法优越性 |
| 反方审查 | 58/100，Major Revision | 识别出 reference-only、baseline fidelity、practical margin 和 candidate replay 缺口 | 不能把 Guide 当作论文效果验收通过 |
| 未来候选预测 | UNESTABLISHED，2/24 | 说明目前没有校准的未来结果分布 | 不支持“按 Guide 数字对标就会达到” |

## 按 Gate C 逐项判断

生效标准要求 Gate C 的 C1--C4 每项至少 3/4、总分至少 12/16，E1--E5 不能缺；confirmation 前不能把开发结果当最终主结果。

| 子项 | Guide 一致时的作用 | 当前能否仅凭 Guide 关闭 |
|---|---|---|
| C1 矩阵覆盖 | 可以证明计划写全了 E1--E5 和声明的 feedback-adaptive scope | **不能**。还要有候选的冻结配置、每个实验实际收据和 E1--E5 角色合格主表；当前审计仍把 E1--E5 标为 partial |
| C2 baseline 同协议闭合 | Guide 列出 Frozen、SGD、LinUCB、BayesianRouter 等角色 | **不能**。需要每个 baseline 的版本、适配差异、可见字段、初始化、反馈、delay、预算、failure/unknown 和 raw recovery 一致；公开论文数字不替代本地同协议运行 |
| C3 方向、控制、成本和主张边界 | Guide 给出“受控时可能赢、自然时可能输”的合理关系 | **只能部分支持**。候选必须用自己的 matched Frozen/Online/Replay/Permutation 结果验证；旧 Relay 的 +14.18pp 或 +4.74pp 不能充当新候选的 superiority evidence |
| C4 paired uncertainty、分母、恢复和负结果 | Guide 对已有128 order reference 已做到配对 SD、分位数、均值区间和负 order | **不能**。候选的每条主比较要有独立 raw-event recovery、cluster/paired uncertainty、failure/unknown 分母、成本和负结果；不能继承旧 SD/covariance/win rate |

因此，即使候选数字“看起来和 Guide 一致”，最多说明它没有明显违背当前参考分布；只有 C1--C4 的候选证据全部闭合并重新评分，Gate C 才可能达到 12/16。

## 还要过的两道门

实验 Gate 不是总 Gate。当前最新记录的三门快照仍为：

| 闸门 | 当前记录 | 关闭条件 |
|---|---:|---|
| Gate A Benchmark | 11/16，HOLD | 至少12/16；A1/A3/A4各≥3；truth、冲突/重复、confirmation、provider lineage边界闭合 |
| Gate B Baseline | 11/16，HOLD | 至少12/16且四项各≥3；每个主 baseline 有 source/adapter/fairness/raw recovery |
| Gate C 实验矩阵与结果 | 10/16，HOLD | 至少12/16且 C1--C4 各≥3；E1--E5 全部 role-qualified |

三门不能互相抵消。Guide 与矩阵一致不会自动提高 Gate A 或 Gate B。

即使 Gate C 通过，若论文要声称新分子或未读数据上的效果，还要进入 Stage B：方法、菜单、阈值、epoch 和 baseline 冻结后，一次性读取 untouched confirmation，标签不回流。当前 Guide 的157题已经查看过，不能承担这个 E6 confirmation 角色。

## “一致”必须满足的严格含义

这里的一致不是把结果调到接近目标数，而是同时满足：

1. 同一候选、同一版本、同一输入 hash、同一 provider menu、同一 feature/prior、同一 delay 和 selected-only release；
2. 同一统计单位：128-order 结果按 order 推断，分子结果按预登记 molecule/scaffold 权重；不能把 4,710 records 或 61,440 streams 当独立样本；
3. 同一错误结构：报告 rescue、harm、both-wrong、failure、unknown 和拒答，而不是只对齐 pooled accuracy；
4. 同一比较合同：candidate 与自己的 Frozen、matched Online/Replay、自己的 permutation 和 evaluator-only control 分开；
5. 同一主张边界：受控 tick-78 correctness-drift 只能写成该压力法下的结果，自然场景若低于 Frozen 就保留为边界结果；
6. 同一不确定性对象：均值 Monte Carlo 区间、order 分位数、正收益频率区间分别解释，不能互换；
7. 同一恢复链：原始 JSONL、config、checkpoint/source/input hash 和独立 verifier 能重算主表；失败现场和负结果不删除。

若“对齐”只指均值、百分点或胜率相近，不算一致；这会重复旧版 Guide 的问题。

## 公开 baseline 结果的作用

公开文献结果已独立保存到 [`/Users/jingwu/work/reasonable_results/pharma_tool_selection_public_baselines_v1/`](file:///Users/jingwu/work/reasonable_results/pharma_tool_selection_public_baselines_v1/)。其中 LinUCB/LinTS 和 BayesianRouter 的数字可以证明这些 baseline 属于近期方法家族，并帮助校准方法选择；它们的任务、动作池、反馈和指标不同，不能拿论文中的 0.6166、0.6430 或 88.23% 直接填 BBB Guide，也不能替代本地同合同回放。

## 最终判定

- **现在：不能关闭实验 Gate，整体仍 HOLD。**
- **Guide 的正确定位：** 固定已查看队列的高质量参考分布和条件需求规格；不是未来候选的预期结果 PDF。
- **可以关闭 Gate C 的充分路径：** 完成 candidate-specific E1--E5，所有主 baseline 同协议闭合，C1--C4 逐项≥3，独立恢复/paired uncertainty/失败分母/成本/负结果齐全，然后重新评分达到≥12/16。
- **可以关闭完整实验验收的额外条件：** Gate A、Gate B 同时达到各自阈值；若保留新分子或泛化 claim，再完成冻结后的 Stage-B untouched confirmation。

这不是降低目标，而是把“目标分布”和“已证明的实验结果”分开。若真实候选在自然场景逊于 Frozen、在受控反馈释放后优于 matched online baseline，这种不对称结果反而符合当前机制假设，应该保留在论文中。
